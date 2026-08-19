---

copyright:
  years: 2026
lastupdated: "2026-08-19"

keywords: instance profile migration, vcpu count, instance profiles, versioned change, migration

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating to the 2026-09-01 version (instance profile vCPU count change)
{: #2026-09-01-instance-profile-vcpu-count-change}

As described in the VPC API reference [versioning](/apidocs/vpc/latest#api-versioning) policy, most changes to the VPC APIs are fully backward compatible and are made available to all clients, regardless of the API version the client requests. However, the `2026-09-01` release of the VPC API will necessitate incompatible changes in support of updated modeling of instance profile vCPU count values.

Before you adopt the release version `2026-09-01` or later, review the changes described in this migration guidance that might require you to update your client.

## Changed instance profile properties
{: #changed-properties-instance-profile-api-version-2026-09-01}

The following properties will change for API requests that use a version query parameter of `2026-09-01` or later.

When [retrieving](/apidocs/vpc/2026-08-31#get-instance-profile) or [listing](/apidocs/vpc/2026-08-31#list-instance-profiles) instance profiles, the following properties will change for API requests that use a `version` query parameter of `2026-09-01` or later.

| Changed property        | Change |
|-------------------------|--------|
| `vcpu_count`            | The fixed, single value will be removed and replaced with an array. |
| `supported_vcpu_count`  | Will be removed from the response. |
{: caption="Changed properties when retrieving or listing instance profiles." caption-side="bottom"}

For API versions before `2026-09-01`, `vcpu_count` can be returned as a fixed-value and `supported_vcpu_count` contains the supported enum values. For API version `2026-09-01` or later, `supported_vcpu_count` will be removed from the response and `vcpu_count` will contain the permitted values directly when the profile returns an enum-based value.

## Action needed
{: #action-needed-instance-profile-api-version-2026-09-01}

Before you specify the `version` query parameter of `2026-09-01` or later, follow these actions to avoid regressions in client functionality.

If your client reads `supported_vcpu_count.values`, update it to read `vcpu_count.values` instead.

If your client assumes that `vcpu_count` always contains a fixed `value`, update it to handle the supported response for `vcpu_count`, including enum-based, range-based, and dependent values.

If your clients continue to specify a version earlier than `2026-09-01`, no changes are required.
{: note}

### Client migration
{: #client-migration-instance-profile-api-version-2026-09-01}

Before you migrate a client to API version `2026-09-01` or later, review your code that uses the following methods:

- `GET /instance/profiles/{name}`
- `GET /instance/profiles`

Review the changes that will be announced in the API change log, and verify that your code adopts these changes in a manner that is appropriate for your programming language.

## Example
{: #example-instance-profile-api-version-2026-09-01}

The following examples show the response before and after the `2026-09-01` API version change.

### Retrieving an instance profile
{: #retrieve-instance-profile-api-version-2026-09-01}

The following response is from a request that uses an API version earlier than `2026-09-01`. The `vcpu_count` property contains a fixed value, and `supported_vcpu_count` contains the supported values.

```json
{
  "name": "bx2-custom",
  "resource_type": "instance_profile",
  "vcpu_count": {
    "type": "fixed",
    "value": 4
  },
  "supported_vcpu_count": {
    "type": "enum",
    "values": [2, 4]
  }
}
```
{: codeblock}
