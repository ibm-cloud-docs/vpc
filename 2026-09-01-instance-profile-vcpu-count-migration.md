---

copyright:
  years: 2026
lastupdated: "2026-09-01"

keywords: instance profile migration, vcpu count, instance profiles, versioned change, migration

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the 2026-09-01 version (instance profile vCPU count change)
{: #2026-09-01-instance-profile-vcpu-count-change}

As described in the VPC API reference [versioning](/docs/apis/vpc/latest#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the `2026-09-01` release of the VPC API necessitated incompatible changes in support of updated modeling of instance profile vCPU count values.

Before you adopt the release version `2026-09-01` or later, review the changes described in this migration guidance that might require you to update your client.

## Changed instance profile properties
{: #changed-properties-instance-profile-api-version-2026-09-01}

The following properties have changed for API requests that use a `version` query parameter of `2026-09-01` or later.

When [retrieving](/docs/apis/vpc/latest#get-instance-profile) or [listing](/docs/apis/vpc/latest#list-instance-profiles) instance profiles, the following properties have changed for API requests that use a `version` query parameter of `2026-09-01` or later.

| Changed property        | Behavior change |
|-------------------------|-------------------------------------------------|
| `vcpu_count`            | fixed value response will be replaced by array. |
| `supported_vcpu_count`  | Removed from the response. |
{: caption="Changed properties when retrieving or listing instance profiles." caption-side="bottom"}

For API `version` query parameter of `2026-08-31` or earlier, `vcpu_count` is returned as a fixed-value, and `supported_vcpu_count` contains the supported enum values. For API version `2026-09-01` or later, `supported_vcpu_count` is removed from the response and the `vcpu_count` property now returns those supported values directly as an array of enum values.

## Action needed
{: #action-needed-instance-profile-api-version-2026-09-01}

Before you specify the `version` query parameter of `2026-09-01` or later, follow these actions to avoid regressions in client functionality.

if your client reads `supported_vcpu_count.values`, update your client code to read `vcpu_count.values` instead.

If your client implementation is based on `vcpu_count` containing a fixed `value`, update the code to support the documented response forms for `vcpu_count`, including enum-based, range-based, and dependent values.

If your clients continue to specify version `2026-08-31` or earlier, no changes are required.
{: note}

### Client migration
{: #client-migration-instance-profile-api-version-2026-09-01}

Before you migrate a client to API version `2026-09-01` or later, review your code that uses the following methods:

- `GET /instance/profiles/{name}`
- `GET /instance/profiles`

Review the changes that were announced in the [API change log](/docs/vpc?topic=vpc-api-change-log#1-september-2026), and verify that your code adopts these changes in a manner that is appropriate for your programming language.

## Example
{: #example-instance-profile-api-version-2026-09-01}

These examples compare differences between before and after the `2026-09-01` versioned change.

### Retrieving an instance profile
{: #retrieve-instance-profile-api-version-2026-09-01}

The example response is from a request made using API version `2026-08-31` or earlier. The response includes `vcpu_count` as a fixed value and `supported_vcpu_count` as the list of supported values.

```json
{
  "name": "bx2-custom",
  "resource_type": "instance_profile",
  "vcpu_count": {
    "type": "fixed",
    "value": 4
  },
  "supported_vcpu_count": {
    "default": 4,
    "type": "enum",
    "values": [4]
  }
}
```
{: codeblock}

The example below shows a response for requests made using API version 2026-09-01 or later.In this version, vcpu_count is returned as an enum value, and supported_vcpu_count has been removed from the response.

```json
{
  "name": "bx2-custom",
  "resource_type": "instance_profile",
  "vcpu_count": {
    "default": 4,
    "type": "enum",
    "values": [4]
  }
}
```
{: codeblock}
