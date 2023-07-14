---

copyright:
  years: 2023
lastupdated: "2023-07-11"

keywords: file storage, file share, mount target, API change, access mode, vpc, security group

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# `2023-07-11` API migration (file shares)
{: #2023-07-11-migration-file-shares}

As described in the [Beta VPC API](/apidocs/vpc-beta) reference [versioning](/apidocs/vpc-beta#api-versioning-beta) policy, to more quickly respond to feedback as a feature progresses through its beta phase, support for older versions of the beta API is limited to 45 days. Therefore, beta API requests must specify a `version` query parameter date value within the last 45 days.

Before you adopt beta release version `2023-07-11` or later, be aware of the following changes, which might require you to update your client:

- In the [share](/apidocs/vpc-beta#create-share) methods, the `access_control_mode` property has been added. This property can have the values `vpc` or `security_group`. File shares that are based on the previous IOPS tiers or custom profiles and are created with version `2023-07-10` or earlier default to the `vpc` value for the `access_control_mode` property. File shares that are based on the `dp2` profile and are created with version `2023-07-11` or later, default to `security_group` value for the `access_control_mode` property unless specified otherwise.
- When [creating](/apidocs/vpc-beta#create-share) a file share, you must specify the `access_control_mode` as either `vpc` or `security_group`.
- In the [share mount targets](/apidocs/vpc-beta#list-share-mount-targets) methods, the `virtual_network_interface` property value has been added. Learn [about virtual network interfaces](/docs/vpc?topic=vpc-vni-about&interface=api).
- When [creating a mount target](/apidocs/vpc-beta#create-share-mount-target) for a file share with `access_control_mode` set to `security group`, you must specify the `virtual_network_interface` property to create a virtual network interface.

All requests that are using the following methods enforce the existing requirement that you provide the [`maturity=beta`](/apidocs/vpc-beta#maturity-query-parameter) query parameter:

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
{: #action-needed-access-mode}

Before specifying `version` query parameter of `2023-07-11` or later, follow these actions to avoid regressions in client functionality.

### Client migration
{: #client-migration-access-mode}

Before you migrate a client to API version `2023-07-11` or later, review your code for use of the `POST /share` and `POST /mount_targets` methods. Update your code to include the `access_control_mode` property for shares and the `virtual_network_interface` property for share mount targets in the manner appropriate for your programming language. For more information, see the [Beta API change log](/docs/vpc?topic=vpc-api-change-log-beta#version-2023-07-11-beta).

## Examples
{: #examples-access-mode}

These examples compare differences between before and after the `2023-07-11` versioned change.

### Creating a file share and mount target
{: #migration-access-mode-create-share-examples}

The following examples compare how to make a request to create a file share before and after the `2023-07-11` versioned change. The path of the API request is the same before and after the property change. However, the data section changes with the addition of the new `access_control_mode` and `virtual_network_interface` properties.

The following example request creates a file share and target, specifying API version `2023-07-10` or earlier. The data object specifies an earlier profile, which supports VPC-wide access to the file share.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-07-10&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "size": 100,
    "targets": [
      {
        "name": "my-target-1",
        "vpc": {
          "id": "a1fb6c4f-6a63-4d34-8bf6-55fab89e932a"
        }
      }
    ],
    "name": "my-share-name1",
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

The following example request creates a file share and mount target, specifying API version `2023-07-11` or later. The `access_control_mode` and `virtual_network_interface` properties are specified in the example. The new share is based on the `dp2` profile.

```sh
curl -X POST "$vpc_api_endpoint/v1/shares?version=2023-07-11&generation=2&maturity=beta"\
-H "Authorization: $iam_token" \
-d '{
    "size": 10,
    "name": "my-share-sc-2",
    "profile": {
        "name": "dp2"
    },
    "zone": {
        "name": "us-south-3"
    },
    "mount_targets": [
        "virtual_network_interface": {
            "primary_ip": {
                "name": "my-primary-ip1",
                "address": "192.0.2.0",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/aea5fe79f-52c3-4f05-86ae-9540a10489f5/reserved_ips/6fd4925d-7774-4e87-829e-7e5765d454ad",
                "id": "6fd4925d-7774-4e87-829e-7e5765d454ad",
                "name": "my-reserved-ip",
                "auto_delete": "false"
                }
            },
        "transit_encryption": {
            "user_managed"
          }
       ]
    }'
 ```
 {: pre}

### Creating a mount target for an existing share
{: #migration-access-mode-create-mount-target-examples}

The following examples compare how to create a mount target for an existing file share before and after the `2023-07-11` versioned change. The path of the API request is the same as before the change.

The following request creates a target for an existing file share, it specifies API version `2023-07-10` or earlier. The request specifies only the VPC that contains the virtual server instances that can mount and access the file share.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets?version=2023-07-10&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "name": "my-mount-target-4",
    "vpc": {
      "id": "549192f1-d238-42bd-8657-b6034a08f04e"
    }
   }'
```
{: pre}

The following request creates a mount target for an existing file share with the `access_control_mode` set to `vpc`, specifying API version `2023-07-11` or later. All virtual server instances within the specified VPC can mount and access the file share.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$share_id/mount_targets?version=2023-07-11&generation=2&maturity=beta"\
-H "Authorization: Bearer $iam_token"\
-H 'Content-Type: application/json'\
-d '{
    "name": "my-mount-target-name1",
    "vpc": {
      "id": "6e01bc24-4a6e-4a0c-a1bd-4caa0c8159e7"
    },
}
```
{: pre}

The following request creates a mount target for an existing file share with the `access_control_mode` set to `security group`, specifying API version `2023-07-11` or later. The request specifies the `virtual_network_interface` that's going to be attached to the mount target by providing a subnet and security group values. 

```sh
 curl -X POST "$vpc_api_endpoint/v1/shares/$share_id/mount_targets/?version=2023-07-11&generation=2&maturity=beta"\
 -d '{
     "virtual_network_interface": {
        "subnet": {
            "id": "1a0b3d75-8a62-4c78-9263-f9bcd25a8759"
            },
        "security_groups": [
            {
            "id": "b2599112-7027-480e-ad1b-fd917d2fcb84"
            }
        ]
     },
}'
```
{: pre}
