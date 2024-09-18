---

copyright:
  years: 2024
lastupdated: "2024-09-17"

keywords: storage, Block Storage, VPC, API, virtual private cloud, support

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the `2024-09-17` version (volumes and volume profiles)
{: #2024-09-17-migration-volumes}

As described in [Versioning](/apidocs/vpc/latest#api-versioning), most changes to the VPC APIs are fully compatible with earlier versions and are made available to all clients, regardless of the API version the client requests. However, the `2024-09-17` release of the VPC API necessitated incompatible changes in support of volumes and volume profiles methods.

Before you adopt the release version `2024-09-17` or later, review the changes described in this migration guidance that might require you to update your client.

Also before you proceed, familiarize yourself with another incompatible change introduced in beta API version `2024-09-17` for  [updating to the `2024-09-17` version (Private Path service gateway)](/docs/vpc?topic=vpc-2024-09-17-migration-private-path-service-gateway).
{: important}


## Changed properties
{: #changed-properties-2024-09-17-beta}

The properties `unattached_capacity_update_supported` and `unattached_iops_update_supported` that were introduced in the [19 September 2023](/docs/vpc?topic=vpc-api-change-log-beta&interface=api#19-september-2023-beta) release for volumes and volume profiles are now replaced with `adjustable_capacity_states` and `adjustable_iops_states` in this beta update.

| Old property                           | New property                 |
|----------------------------------------|------------------------------|
| `unattached_capacity_update_supported` | `adjustable_capacity_states` |
| `unattached_iops_update_supported`     | `adjustable_iops_states`     |
{: caption="Table 1. Old and new properties for volumes and volume profiles" caption-side="bottom"}

## Action needed
{: #action-needed-2024-09-17-beta}

If your clients are using properties `unattached_capacity_update_supported` and `unattached_iops_update_supported` that were introduced in the [19 September 2023](/docs/vpc?topic=vpc-api-change-log-beta&interface=api#19-september-2023-beta) beta release, follow these actions to avoid regressions in client functionality.

The following migration guidance applies to both volumes and volume profiles methods.
{: note}

### Migrating from `unattached_capacity_update_supported` to `adjustable_capacity_states`
{: #migrating-to-adjustable-capacity-states-2024-09-17-beta}

In the [19 September 2023](/docs/vpc?topic=vpc-api-change-log-beta&interface=api#19-september-2023-beta) beta release, the `unattached_capacity_update_supported` property returned a value of `true` to denote a volume or volume profile's ability to support updates to capacity when not attached to a running virtual server instance. If not supported, the value was `false`. In this release, the new `adjustable_capacity_states` property returns an array of enumerated values that denotes the different states in which the volume or volume profile can support updates to capacity, such as `attached`, `unattached`, `attached, unattached`, or an empty array `[]`.  A returned empty array indicates that a volume or volume profile does not support updates to capacity.

Before you migrate a client to API version 2024-09-17 or later, review your code for the use of `unattached_capacity_update_supported` property, and replace it with the `adjustable_capacity_states` property.

### Migrating from `unattached_iops_update_supported` to `adjustable_iops_states`
{: #migrating-to-adjustable-iops-states-2024-09-17-beta}

In the [19 September 2023](/docs/vpc?topic=vpc-api-change-log-beta&interface=api#19-september-2023-beta) beta release, the `unattached_iops_update_supported` property returned a value of `true` to denote a volume or volume profile's ability to support updates to IOPS when not attached to a running virtual server instance. If not supported, the value was `false`. In this release, the new `adjustable_iops_states` property returns an array of enumerated values that denotes the different states in which the volume or volume profile can support updates to IOPS, such as `attached`, `unattached`, `attached, unattached`, or an empty array `[]`. A returned empty array indicates that a volume or volume profile does not support updates to IOPS.

Before you migrate a client to API version 2024-09-17 or later, review your code for the use of `unattached_iops_update_supported` property, and replace it with the `adjustable_iops_states` property.

## Examples
{: #volumes-volume-profiles-examples-2024-09-17-beta}


These examples compare differences between before and after the 2024-09-17 versioned change.

### Retrieving a volume profile
{: #retrieving-volume-profile-example-2024-09-17-beta}

The following example uses API version `2024-09-16` or earlier  to retrieve a volume profile. The response includes the profile definition with properties `unattached_capacity_update_supported` and `unattached_iops_update_supported`.

```sh
curl -s -X GET "$vpc_api_endpoint/v1/volume/profiles/sdp?version=2024-09-16&generation=2&maturity=beta" -H "Authorization: Bearer $iam_token" | jq

{
  "boot_capacity": {
    "max": 32000,
    "min": 1,
    "step": 1,
    "type": "dependent_range"
  },
  "capacity": {
    "default": 1,
    "max": 32000,
    "min": 1,
    "step": 1,
    "type": "range"
  },
  "family": "defined_performance",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/sdp",
  "iops": {
    "default": 100,
    "max": 64000,
    "min": 100,
    "step": 1,
    "type": "range"
  },
  "name": "sdp",
  "unattached_capacity_update_supported": {
    "type": "fixed",
    "value": true
  },
  "unattached_iops_update_supported": {
    "type": "fixed",
    "value": true
  }
}
```
{: pre}

The following example uses API version `2024-09-17` or later to retrieve a volume profile. The response includes the profile definition with new properties `adjustable_capacity_states` and `adjustable_iops_states`.  


```sh
% curl -s -X GET "$vpc_api_endpoint/v1/volume/profiles/sdp?version=2024-09-17&generation=2&maturity=beta" -H "Authorization: Bearer $iam_token" | jq

{
  "adjustable_capacity_states": {
    "type": "enum",
    "values": [
      "attached",
      "unattached"
    ]
  },
  "adjustable_iops_states": {
    "type": "enum",
    "values": [
      "attached",
      "unattached"
    ]
  },
  "boot_capacity": {
    "max": 32000,
    "min": 1,
    "step": 1,
    "type": "dependent_range"
  },
  "capacity": {
    "default": 1,
    "max": 32000,
    "min": 1,
    "step": 1,
    "type": "range"
  },
  "family": "defined_performance",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/sdp",
  "iops": {
    "default": 100,
    "max": 64000,
    "min": 100,
    "step": 1,
    "type": "range"
  },
  "name": "sdp"
}
```
{: pre}

### Retrieving a volume
{: #retrieving-volume-example-2024-09-17-beta}

The following example uses API version `2024-09-16` or earlier to retrieve a volume. The response includes the volume definition with properties `unattached_capacity_update_supported` and `unattached_iops_update_supported`.

```sh
curl -s -X GET "$vpc_api_endpoint/v1/volumes/r134-844af68c-a928-4960-a725-f8702c80d0f6?version=2024-09-16&generation=2&maturity=beta" -H "Authorization: Bearer $iam_token" | jq

{
  "id": "844af68c-a928-4960-a725-f8702c80d0f6",
  "crn": "crn:[...]",
  "name": "my-volume",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/844af68c-a928-4960-a725-f8702c80d0f6",
  "attachment_state": "unattached",
  "capacity": 25,
  "iops": 1000,
  "encryption": "provider_managed",
  "status": "available",
  "zone": {
    "name": "us-south-3",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3"
  },
  "profile": {
    "name": "sdp",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/sdp"
  },
  "resource_group": {
    "id": "85cbbee12af748eabb6513cf311d4e2d",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/85cbbee12af748eabb6513cf311d4e2d",
    "name": "Default"
  },
  "volume_attachments": [],
  "created_at": "2024-09-16T12:06:42Z",

  "status_reasons": [],
  "active": false,
  "busy": false,
  "bandwidth": 131,
  "user_tags": [],
  "resource_type": "volume",
  "health_state": "ok",
  "health_reasons": [],
  "adjustable_iops_supported": true,
  "unattached_capacity_update_supported": true,
  "unattached_iops_update_supported": true
}
  ```
{: pre}

The following example uses API version `2024-09-17` or later to retrieve a volume. The response includes the volume definition with new properties `adjustable_capacity_states` and `adjustable_iops_states`. 

```sh
curl -s -X GET "$vpc_api_endpoint/v1/volumes/r134-844af68c-a928-4960-a725-f8702c80d0f6?version=2024-09-17&generation=2&maturity=beta" -H "Authorization: Bearer $iam_token" | jq

{
  "id": "844af68c-a928-4960-a725-f8702c80d0f6",
  "crn": "crn:[...]",
  "name": "my-volume",

  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/844af68c-a928-4960-a725-f8702c80d0f6",
  "attachment_state": "unattached",
  "capacity": 25,
  "iops": 1000,
  "encryption": "provider_managed",
  "status": "available",
  "zone": {
    "name": "us-south-3",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3"
  },
  "profile": {
    "name": "sdp",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/sdp"
  },
  "resource_group": {
    "id": "85cbbee12af748eabb6513cf311d4e2d",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/85cbbee12af748eabb6513cf311d4e2d",
    "name": "Default"
  },
  "volume_attachments": [],
  "created_at": "2024-09-17T12:06:42Z",

  "status_reasons": [],
  "active": false,
  "busy": false,
  "bandwidth": 131,
  "user_tags": [],
  "resource_type": "volume",
  "health_state": "ok",
  "health_reasons": [],
  "adjustable_iops_supported": true,
  "adjustable_capacity_states": [
    "attached",
    "unattached"
  ],
  "adjustable_iops_states": [
    "attached",
    "unattached"
  ]
}
  ```
{: pre}
