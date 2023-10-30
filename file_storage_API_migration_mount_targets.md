---

copyright:
  years: 2023
lastupdated: "2023-10-26"

keywords: file storage, file share, mount target, API change

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the version `2023-05-30` (file shares, mount targets)
{: #2023-05-30-migration-file-shares}

As described in the [Beta VPC API](/apidocs/vpc-beta) reference [versioning](/apidocs/vpc-beta#api-versioning-beta) policy, to more quickly respond to feedback as a feature progresses through its beta phase, support for older versions of the beta API is limited to 45 days. Therefore, beta API requests must specify a `version` query parameter date value within the last 45 days.

Before you adopt the beta release version `2023-05-30` or later, be aware of the following changes, which might require you to update your client:

- In the [Shares](/apidocs/vpc-beta#list-share-profiles) methods, the `targets` property was changed to `mount_targets`.
- In the [share mount targets](/apidocs/vpc-beta#list-share-mount-targets) methods, the `resource_type` property value was changed to `share_mount_target`.
- When [creating](/apidocs/vpc-beta#create-share) a file share, you must specify the `mount_targets` property instead of the `targets` property
- When making [share mount targets](/apidocs/vpc-beta#list-share-mount-targets) method requests, you must use the method URL `/shares/{id}/mount_targets` instead of `/shares/{id}/targets`.

All requests that use the following methods enforce the existing requirement that you provide the [`maturity=beta`](/apidocs/vpc-beta#maturity-query-parameter) query parameter:

- [Create a file share](/apidocs/vpc-beta#create-share)
- [Create a mount target for a file share](/apidocs/vpc-beta#create-share-mount-target)
- [List all file shares](/apidocs/vpc-beta#list-shares)
- [Retrieve a file share](/apidocs/vpc-beta#get-share)
- [List all mount targets for a file share](/apidocs/vpc-beta#list-share-mount-targets)
- [Retrieve a share mount target](/apidocs/vpc-beta#get-share-mount-target)
- [Retrieve the source file share for a replica share](/apidocs/vpc-beta#get-share-source)
- [Update a file share](/apidocs/vpc-beta#update-share)
- [Update a share mount target](/apidocs/vpc-beta#update-share-mount-target)
- [Delete a file share](/apidocs/vpc-beta#delete-share)
- [Delete a share mount target](/apidocs/vpc-beta#delete-share-mount-target)

## Action needed
{: #action-needed-mount-target}

Before you specify `version` query parameter of `2023-05-30` or later, follow these actions to avoid regressions in client functionality.

### Client migration
{: #client-migration-mount-target}

Before you migrate a client to API version `2023-05-30` or later, review your code for use of the `targets` property. Change all instances of `targets` to `mount_targets` in the manner appropriate for your programming language. Changes are required in all relevant request paths, and request and response JSON field name formats. For more information, see the [Beta API change log](/docs/vpc?topic=vpc-api-change-log-beta#version-2023-05-30-beta).

## Examples
{: #examples-mount-target}

These examples compare differences between before and after the `2023-05-30` versioned change. All requests that use a `version` query parameter of `2023-05-30` or later must use `/shares/{share_id}/mount_targets` (instead of `/shares/{share_id}/targets`) in the request URL.

### Listing a file share and mount targets
{: #migration-list-share-examples}

The following examples compare how to make a request to list a file share and mount targets before and after the `2023-05-30` versioned change. The path of the API request is different before and after the change.

This request lists a file share and mount targets, specifying API version `2023-05-29` or earlier. The path of the API request includes `/targets`.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/{share_id}/targets?version=2023-05-29&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'
```
{: pre}

This request lists a file share and mount targets, specifying API version `2023-05-30` or later. The path of the API request includes `/mount_targets`.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/{share_id}/mount_targets?version=2023-05-30&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'
```
{: pre}

### Creating a file share and mount target
{: #migration-create-share-examples}

The following examples compare how to make a request to create a file share and mount target before and after the `2023-05-30` versioned change. The path of the API request is the same before and after the property change. However, the data section changes with the new `mount_targets` property.

This request creates a file share and target, specifying API version `2023-05-29` or earlier. The `targets` property is specified in the example.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-05-29&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json' \
-d '{
    "size": 100,
    "targets": [
      {
        "name": "target-1",
        "vpc": {
          "id": "a1fb6c4f-6a63-4d34-8bf6-55fab89e932a"
        }
      }
    ],
    "name": "share-name1",
    "profile": {
      "name": "tier-5iops"
    },
    "user_tags": [],
    "zone": {
      "name": "us-south-1"
    }
  }'
```
{: pre}

This request creates a file share and mount target, specifying API version `2023-05-30` or later. The `mount_targets` property is specified in the example.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-05-30&generation=2&maturity=beta"\

-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json' \
-d '{
    "size": 300,
    "mount_targets": [
      {
        "name": "mount-target-2",
        "vpc": {
          "id": "d716af94-b1be-4705-9f70-a6771d622f7d"
        }
      }
    ],
    "name": "share-name1",
    "profile": {
      "name": "tier-5iops"
    },
    "user_tags": [],
    "zone": {
      "name": "us-south-1"
    }
  }'
```
{: pre}

### Creating a mount target for an existing share
{: #migration-create-mount-target-examples}

The following examples compare how to create a mount target for an existing file share before and after the `2023-05-30` versioned change. The path of the API request is different before and after the change.

This request creates a target for an existing file share, specifying API version `2023-05-29` or earlier. The path of the API includes `/targets`.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/targets?version=2023-05-29&generation=2&maturity=beta"\

-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "name": "target-3",
    "vpc": {
      "id": "6e01bc24-4a6e-4a0c-a1bd-4caa0c8159e7"
    }
  }'
```
{: pre}

This request creates a mount target for an existing file share, specifying API version `2023-05-30` or later. The path of the API includes `/mount_targets`.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets?version=2023-05-30&generation=2&maturity=beta"\

-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "name": "mount-target-4",
    "vpc": {
      "id": "549192f1-d238-42bd-8657-b6034a08f04e"
    }
   }'
```
{: pre}

### Updating a file share mount target
{: #migration-update-mount-target-example}

The following examples compare how to make a request to update a file share mount target before and after the `2023-05-30` versioned change. The path of the API request is different before and after the change.

This request updates a mount target, specifying API version `2023-05-29` or earlier. The path of the API includes `/targets`.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id/targets/$id?version=2023-05-29&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json' \
-d '{
    "name": "target-1-updated"
}'
```
{: pre}

This request updates a file share mount target, specifying API version `2023-05-30` or later. The path of the API includes `/mount_targets`.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets/$id?version=2023-05-30&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json' \
-d '{
    "name": "target-2-updated"
}'
```
{: pre}

### Deleting a file share mount target
{: #migration-delete-mount-target-example}

The following examples compare how to make a request to delete a file share mount target before and after the `2023-05-30` versioned change. The path of the API request is different before and after the change.
  
This request deletes a mount target, specifying API version `2023-05-29` or earlier. The path of the API includes `/targets`.
  
```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id/targets/$id?version=2023-05-29&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'
```
{: pre}
  
This request deletes a file share mount target, specifying API version `2023-05-30` or later. The path of the API includes `/mount_targets`.
  
```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets/$id?version=2023-05-30&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'
```
{: pre}

## Changed methods
{: #changed-methods-mount-targets}

Table 1 lists the methods in which the `targets` property was changed to the `mount_targets` property in `hrefs`, request bodies, and response bodies.

| Method   | Original request path                   | New request path                        | Changed request payload    | Changed response payload |
|----------|-----------------------------------------|-----------------------------------------|----------------------------|------------------------------|
| `GET`    | `/shares`                               | `/shares`                               | No change                  | `mount_targets`              |
|          | `/shares/{id}`                         | `/shares/{id}`                         | No change                  | `mount_targets`              |
|          | `/shares/{id}/targets`                 | `/shares/{id}/mount_targets`           | No change                  | `mount_targets`              |
|          | `/shares/{share_id}/source`             | `/shares/{share_id}/source`             | No change                  | `mount_targets`              |
| `POST`   | `/shares`                               | `/shares`                               | `mount_targets`            | `mount_targets`              |
|          | `/shares/{id}/targets`                  | `/shares/{id}/mount_targets`            | No change                  | No change                    |
|          | `/shares/{id}/failover`                 | `/shares/{id}/failover`                 | No change                  | `mount_targets`              |
| `PATCH`  | `/shares/{id}`                          | `/shares/{id}`                          | No change                  | `mount_targets`              |
|          | `/shares/{share_id}/targets/{id}`       | `/shares/{share_id}/mount_targets/{id}` | No change                  | No change                    |
| `DELETE` | `/shares/{id}`                          | `/shares/{id}/`                         | No change                  | `mount_targets`              |
|          | `/shares/{id}/targets/{id}`             | `/shares/{id}/mount_targets/{id}`       | No change                  | No change                    |
{: caption="Table 1. List of API shares methods that have changed properties" caption-side="bottom"}
