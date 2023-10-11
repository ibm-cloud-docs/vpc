---

copyright:
  years: 2023
lastupdated: "2023-08-08"

keywords: file storage, file share, API change, replication, fallback plan, fallback poicy, failover

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# `2023-08-08` API migration (file shares)
{: #2023-08-08-migration-file-shares}

As described in the [Beta VPC API](/apidocs/vpc-beta) reference [versioning](/apidocs/vpc-beta#api-versioning-beta) policy, to more quickly respond to feedback as a feature progresses through its beta phase, support for older versions of the beta API is limited to 45 days. Therefore, beta API requests must specify a `version` query parameter date value within the last 45 days.

Before you adopt beta release version `2023-08-08` or later, be aware of the following changes, which might require you to update your client:

- When initiating a request to [fail over to replica file share](/apidocs/vpc-beta#failover-share), you must provide a value for the `fallback_policy` property, either `split` or `fail`. If the value of the `fallback_policy` property is not specified, the system now defaults to `split` instead of `fail`.
- When initiating a request to [retrieve the source file share for a replica file share](/apidocs/vpc-beta#get-share-source). The request now returns a more concise `source_share` reference.

All requests that are using the following methods enforce the existing requirement that you provide the [`maturity=beta`](/apidocs/vpc-beta#maturity-query-parameter) query parameter:

- [Retrieve the source file share for a replica file share](/apidocs/vpc-beta#get-share-source)
- [Fail over to replica file share](/apidocs/vpc-beta#failover-share)

## Action needed
{: #action-needed-share-replication}

Before specifying `version` query parameter of `2023-08-08` or later, follow these actions to avoid regressions in client functionality.

### Client migration
{: #client-migration-share-replication}

Before you migrate a client to API version `2023-08-08` or later, review your code for use of the `POST /shares/{share_id}/failover` and `GET /shares/$share_id/source` methods. Ensure that your code includes the `fallback_policy` property for failovers in the manner appropriate for your programming language. For more information, see the [Beta API change log](/docs/vpc?topic=vpc-api-change-log-beta#version-2023-08-08-beta).

## Examples
{: #examples-share-replication}

These examples compare differences between before and after the `2023-08-08` versioned change.

### Initiating a failover to a replica share
{: #migration-replica-fallback-policy-examples}

The following examples show how to make a request to fail over to a replica file share before and after the `2023-08-08` versioned change. The change affects requests that do not have the `fallback_policy` explicitly specified.

The following example requests initiate a failover, specifying API version `2023-08-07` or earlier. When the `fallback_policy` is omitted, the result of a failover failure is the same as when the `fallback_policy` is specified as `fail`. The `$share_id` value is the share ID of the replica share.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/failover?version=2023-08-07-1&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-d '{
     "fallback_policy": "fail",
      "timeout": 600
    }'
```
{: pre}


```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/failover?version=2023-08-07&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
```
{: pre}

If the failover operation fails, both these requests have the same result, the replication relationship remains unchanged.

The following example requests initiate a failover, specifying API version `2023-08-08` or later. The `$share_id` value is the share ID of the replica share.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/failover?version=2023-08-08&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-d '{
     "fallback_policy": "split",
      "timeout": 600
    }'
```
{: pre}

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/failover?version=2023-08-08&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-d '{
      "timeout": 600
    }'
```
{: pre}

If the failover operation fails, both these requests have the same result, the replication relationship is severed. The two shares become independent of one another, and can be managed separately.

### Retrieving the source file share for a replica file share
{: #migration-retrieve-replication-source-share-examples}

The following examples show how the response to a request to retrieve the source file share for a replica file share looks before and after the `2023-08-08` versioned change. 

The following example request retrieves information about the source file share of a replica share, specifying API version `2023-08-07` or earlier. The `$share_id` value is the share ID of the replica share.

```sh
curl -X GET "$vpc_api_endpoint/v1/shares/$share_id/source?generation=2&version=2023-08-07&maturity=beta\
-H "Authorization: Bearer $iam_token"\
```
{: pre}

The response shows detailed information about the source share.

```json
`{
  "created_at": "2023-08-08T23:26:35Z",
  "crn": "crn:v1:bluemix:public:is:us-south-3:a/123456",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-6428d8d6-ff21-4b83-b4a1-70e7e55e8226",
  "id": "r006-6428d8d6-ff21-4b83-b4a1-70e7e55e8226",
  "initial_owner": {
    "gid": 0,
    "uid": 0
  },
  "iops": 3000,
  "latest_job": {
    "status": "succeeded",
    "status_reason": {
      "code": "",
      "message": "",
      "more_info": ""
    },
    "type": "replication_init"
  },
  "lifecycle_state": "stable",
  "name": "test-share-with-replica",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/tier-3iops",
    "name": "tier-3iops",
    "resource_type": "share_profile"
  },
  "replica_share": {
    "crn": "crn:v1:bluemix:public:is:us-south-1:a/123456::share:r006-45bb2104-85c5-4fae-93b1-d97060fac032",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-45bb2104-85c5-4fae-93b1-d97060fac032",
    "id": "r006-45bb2104-85c5-4fae-93b1-d97060fac032",
    "name": "test-replica-name",
    "resource_type": "share"
  },
  "replication_role": "source",
  "replication_status": "active",
  "replication_status_reasons": [],
  "resource_group": {
    "crn": "crn:v1:bluemix:public:resource-controller::a/123456::resource-group:678523bcbe2b4eada913d32640909956",
    "href": "https://resource-controller.test.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
    "id": "678523bcbe2b4eada913d32640909956",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 100,
  "targets": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-6428d8d6-ff21-4b83-b4a1-70e7e55e8226/targets/r006-d750b617-a82b-487a-a26b-0de1824dec8e",
      "id": "r006-d750b617-a82b-487a-a26b-0de1824dec8e",
      "name": "test-share-test-target-6",
      "resource_type": "share_target"
    }
  ],
  "user_tags": [
    "test-tag1:test-value1"
  ],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3",
    "name": "us-south-3"
  }
}`
```
{: screen}

The following example request retrieves information about the source file share of a replica share, specifying API version `2023-08-08` or later. The `$share_id` value is the share ID of the replica share.

```sh
curl -X GET "$vpc_api_endpoint/v1/shares/$share_id/source?version=2023-08-08&generation=2&maturity=beta"\ 
-H "Authorization: Bearer $iam_token"
```
{: pre}

The response shows more concise reference about the source share.

```json
`{
  "crn": "crn:v1:bluemix:public:is:us-south-3:a/123456",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-6428d8d6-ff21-4b83-b4a1-70e7e55e8226",
  "id": "r006-6428d8d6-ff21-4b83-b4a1-70e7e55e8226",
  "name": "test-share-with-replica",
  "resource_type": "share"
}`
```
{: codeblock}

For more details of the source share, make a `GET /shares/{share_id}` request with the `id` that is returned in the share reference. 

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/r006-6428d8d6-ff21-4b83-b4a1-70e7e55e8226?version=2023-08-08&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"
```
{: pre}
