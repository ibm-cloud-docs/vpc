---

copyright:
    years: 2025, 2026
lastupdated: "2025-09-16"

keywords: storage, File Storage, VPC, API, virtual private cloud, support 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the `2025-09-16` version (file shares, file share profiles, and file share mount targets)
{: #2025-09-16-migration-file-shares}

As described in the VPC API reference [versioning](/apidocs/vpc/latest#api-versioning) policy, most changes to the VPC APIs are fully compatible with earlier versions and are made available to all clients, regardless of the API version the client requests. However, the `2025-09-16` release of the VPC API necessitated incompatible changes in support of file shares, file share profiles, and file share mount target methods.

Before you adopt the release version `2025-09-16` or later, review the changes that are described in this migration guidance that might require you to update your client.
{: important}

## Changed properties
{: #changed-properties-2025-09-16}

### Updating `user_managed` transit encryption mode to `ipsec`
{: #updating-user-managed-transit-encryption-to-ipsec}

In this update, the value `user_managed` for the `allowed_transit_encryption_mode` of file shares and file share profiles, and `transit_encryption` property of file share mount targets (introduced in the [11 July 2023](/docs/vpc?topic=vpc-api-change-log-beta#version-2023-07-11-beta) beta release for file shares, file share profiles, and file share mount targets) is now replaced with `ipsec`.

| Property name                     | Old property value | New property value |
|-----------------------------------|--------------------|--------------------|
| `allowed_transit_encryption_mode` | `user_managed`     | `ipsec`            |
| `transit_encryption`              | `user_managed`     | `ipsec`            |
{: caption="Old and new properties for file shares, file share profiles, and file share mount targets" caption-side="bottom"}

### File share mount target `access_protocol` and `transit_encryption` requirement
{: #file-share-mount-target-access-protocol-transit-encryption-requirement}

In this update, the `access_protocol` and `transit_encryption` properties are now required for file share mount target creation requests.

### `zone` property no longer required
{: #zone-property-no-longer-required}

In this update, the `zone` property of `Share` requests is no longer required as part of the `Share` API request. This value must not be specified for regional file shares with the `rfs` profile, but the value is still required for the zonal file shares that use the `dp2` profile. Existing requests for file shares that use the `dp2` profile are not affected.

## Action needed
{: #action-needed-2025-09-16}

### Migrating from `user_managed` to `ipsec`
{: #migrating-from-user_managed-to-ipsec-2025-09-16}

Before you specify the version query parameter of `2025-09-16` or later, follow these actions to avoid regressions in client functionality.

If your clients continue to specify version `2025-09-15` or earlier, no changes are required.

### Client migration
{: #client-migration-2025-09-16}

Before you migrate a client to an API version `2025-09-16` or later, review your code that uses the following methods: 

- `POST /shares`
- `GET /shares`
- `PATCH /shares/{share_id}`
- `GET /shares/{share_id}`
- `POST /shares/{share_id}/mount_targets`
- `PATCH /shares/{share_id}/mount_targets`
- `GET /shares/{share_id}/mount_targets`
- `GET /shares/{share_id}/mount_targets/{id}` 

Verify that your code includes the `ipsec` value (instead of `user_managed`) for `allowed_transit_encryption_modes` for shares and for `transit_encryption` for share mount targets in the manner that is appropriate for your programming language. Additionally, verify that requests to create file share mount targets include an `access_protocol` value as specified by the share's `allowed_access_protocols`. For more information, see the [Beta VPC API change log](/docs/vpc?topic=vpc-api-change-log#version-2025-09-16).

## Examples
{: #examples-2025-09-16}

These examples compare differences between before and after the `2025-09-16` versioned change.

### Creating a file share
{: #creating-file-share-2025-09-16}

The following example uses API version `2025-09-15` or earlier to create a file share. The data object specifies `allowed_transit_encryption_modes` including `user_managed`.

```sh
curl --request POST \
  --url '$vpc_api_endpoint/v1/shares?version=2025-09-15&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
    "allowed_transit_encryption_modes": ["user_managed", "none"],
    "iops": 1000,
    "name": "my-file-share",
    "profile": {
      "name": "dp2"
    },
    "size": 10,
    "zone": {
      "name": "us-south-1"
    }
  }'
```
{: codeblock}

The following example uses API version `2025-09-16` or later to create a file share. The data object specifies `allowed_transit_encryption_modes` including `ipsec`.

```sh
curl --request POST \
  --url '$vpc_api_endpoint/v1/shares?version=2025-09-16&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
    "allowed_transit_encryption_modes": ["ipsec", "none"],
    "iops": 1000,
    "name": "my-file-share",
    "profile": {
      "name": "dp2"
    },
    "size": 10,
    "zone": {
      "name": "us-south-1"
    }
  }'
```
{: codeblock}

### Creating a regional file share
{: #creating-regional-file-share}

The following example creates a regional file share. The data object does not specify a `zone`.

```sh
curl --request POST \
  --url '$vpc_api_endpoint/v1/shares?version=2025-01-01&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
    "allowed_transit_encryption_modes": ["stunnel", "none"],
    "name": "my-file-share",
    "profile": {
      "name": "rfs"
    },
    "size": 10,
  }'
```
{: codeblock

The following example shows a response to an API request for a regional file share that uses the API version `2025-09-15` or earlier. The `zone` property indicates the first zone in the region.

```sh
{
  "access_control_mode": "security_group",
  "accessor_binding_role": "none",
  "allowed_access_protocols": [
    "nfs4"
  ],
  "allowed_transit_encryption_modes": [
    "stunnel",
    "none"
  ],
  "availability_mode": "regional",
  "bandwidth": 10,
  "created_at": "2025-09-16T13:07:54.000Z",
  "crn": "crn:v1:bluemix:public:is:us-south:a/df0564dd126042ebb03e0224728ce939::share:r006-7ee9c4d4-88ff-4b6c-8a8a-49aa096bf0d8",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-7ee9c4d4-88ff-4b6c-8a8a-49aa096bf0d8",
  "id": "r006-7ee9c4d4-88ff-4b6c-8a8a-49aa096bf0d8",
  "initial_owner": {
    "gid": 0,
    "uid": 0
  },
  "iops": 35000,
  "lifecycle_reasons": [],
  "lifecycle_state": "stable",
  "mount_targets": [],
  "name": "my-file-share",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/rfs",
    "name": "rfs",
    "resource_type": "share_profile"
  },
  "replication_role": "none",
  "replication_status": "none",
  "replication_status_reasons": [],
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
    "id": "678523bcbe2b4eada913d32640909956",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 100,
  "snapshot_count": 0,
  "snapshot_size": 0,
  "storage_generation": 2,
  "user_tags": [],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
      "name": "us-south-1"
  }
}
```
{: codeblock

The following example shows a response to an API request for a regional file share that uses the API version `2025-09-16` or later. The `zone` property is absent. 

```sh
{
  "access_control_mode": "security_group",
  "accessor_binding_role": "none",
  "allowed_access_protocols": [
    "nfs4"
  ],
  "allowed_transit_encryption_modes": [
    "stunnel",
    "none"
  ],
  "availability_mode": "regional",
  "bandwidth": 10,
  "created_at": "2025-09-16T13:07:54.000Z",
  "crn": "crn:v1:bluemix:public:is:us-south:a/df0564dd126042ebb03e0224728ce939::share:r006-7ee9c4d4-88ff-4b6c-8a8a-49aa096bf0d8",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/shares/r006-7ee9c4d4-88ff-4b6c-8a8a-49aa096bf0d8",
  "id": "r006-7ee9c4d4-88ff-4b6c-8a8a-49aa096bf0d8",
  "initial_owner": {
    "gid": 0,
    "uid": 0
  },
  "iops": 35000,
  "lifecycle_reasons": [],
  "lifecycle_state": "stable",
  "mount_targets": [],
  "name": "my-file-share",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/rfs",
    "name": "rfs",
    "resource_type": "share_profile"
  },
  "replication_role": "none",
  "replication_status": "none",
  "replication_status_reasons": [],
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
    "id": "678523bcbe2b4eada913d32640909956",
    "name": "Default"
  },
  "resource_type": "share",
  "size": 100,
  "snapshot_count": 0,
  "snapshot_size": 0,
  "storage_generation": 2,
  "user_tags": []
}
```
{: codeblock}

### Creating a file share mount target
{: #creating-file-share-mount-target-2025-09-16}

The following example uses API version `2025-09-15` or earlier to create a mount target for a file share. The data object specifies `transit_encryption` as `user_managed`.

```sh
curl --request POST \
  --url '$vpc_api_endpoint/v1/shares/$my_share_id/mount_targets?version=2025-09-15&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
    "transit_encryption": "user_managed",
    "virtual_network_interface": {
      "auto_delete": true,
      "subnet": {
        "id": "$my_subnet_id$"
      }
    }
  }'
```
{: codeblock}

The following example uses API version `2025-09-16` or later to create a mount target for a file share. The data object specifies `access_protocol` as `nfs4` `transit_encryption` as `ipsec` per the new requirements.

```sh
curl --request POST \
  --url '$vpc_api_endpoint/v1/shares/$my_share_id/mount_targets?version=2025-09-16&generation=2' \
  --header 'Authorization: Bearer $iam_token' \
  --header 'Content-Type: application/json' \
  --data '{
    "access_protocol": "nfs4",
    "transit_encryption": "ipsec",
    "virtual_network_interface": {
      "auto_delete": true,
      "subnet": {
        "id": "$my_subnet_id$"
      }
    }
  }'
```
{: codeblock}
