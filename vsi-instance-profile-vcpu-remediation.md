---

copyright:
  years: 2026
lastupdated: "2026-09-01"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Action required: Prepare your automation for instance profile vCPU count response changes
{: #vsi-instance-profile-vcpu-remediation}

## What are we changing?
{: #vcpu-what-changed}

As of 1 September 2026, the VPC API changed how virtual server instance profile vCPU counts are represented when retrieving an instance profile ([Get an instance profile](https://cloud.ibm.com/docs/apis/vpc/latest#get-instance-profile)) and listing instance profiles ([List instance profiles](https://cloud.ibm.com/docs/apis/vpc/latest#list-instance-profiles)). This applies only to versions dated `2026-09-01` or later.

* The `supported_vcpu_count` property has been removed from instance profile responses.
* The existing `vcpu_count` property no longer returns a single fixed-value response for the affected profiles.
* For profiles that previously returned supported values by using `supported_vcpu_count`, the `vcpu_count` property now returns those supported values directly as an array of enum values.
* Clients that already handle `vcpu_count` as an array of enum values are not impacted by this change. The change is specific to clients that rely on the fixed-value response or on the separate `supported_vcpu_count` property.

For detailed migration guidance and examples, see [Updating to the 2026-09-01 version (instance profile vCPU count change)](/docs/vpc?topic=vpc-2026-09-01-instance-profile-vcpu-count-change). Review this guidance before updating your clients to API version `2026-09-01`. For additional details on this change, see the [API change log](/docs/vpc?topic=vpc-api-change-log#1-september-2026).

## Why are we making this change?
{: #vcpu-why-change}

This change simplifies the instance profile response model by removing duplicated vCPU count information and returning supported vCPU values directly in the existing `vcpu_count` property. This enhancement improves consistency in how instance profile capabilities are represented across profiles.

## Who will be affected by this change?
{: #vcpu-those-affected-by-change}

Accounts with code or automation using API versions dated `2026-09-01` or later that retrieves or lists instance profiles are affected, particularly:

* Code that reads `supported_vcpu_count` from instance profile responses
* Code that requires `vcpu_count` to always contain a fixed `value`
* Code that maps instance profile sizing logic from `supported_vcpu_count.values` instead of inspecting `vcpu_count`
* Validation or parsing logic that does not support the enum-based `vcpu_count`

## What actions can you take to avoid disruption?
{: #vcpu-actions-to-avoid-disruption}

If you or other users in your account retrieve or list instance profiles, perform the following tasks to avoid disruption:

* Check for code or automation that reads `supported_vcpu_count` from instance profile responses, and update it to read `vcpu_count` from instance profile responses.
* Update that code to inspect `vcpu_count.type` and read `vcpu_count.values` when the response uses the enum values.
* Check for code or automation that is hardcoded to treat `vcpu_count` as a fixed `value`.
* Update parsing and validation logic so that it handles all supported `vcpu_count` responses, including enum-based, range-based, and dependent values.
* Test any profile selection, sizing, or provisioning workflows that currently derive supported vCPU choices from `supported_vcpu_count`.

## Hazardous migration considerations
{: #vcpu-migration-guidance}

If your client code currently treats `vcpu_count` as a fixed scalar-like property, or if it reads only `supported_vcpu_count.values`, updating to the new API version without revising that logic can cause incorrect profile selection, validation failures, or runtime errors.

### 1. Clients that read only `supported_vcpu_count`
{: #guidance-supported-vcpu-count}

If your existing automation requires `supported_vcpu_count` to always be present, the property removal can cause failures, such as missing-field errors, null dereferences, or empty option lists.

Evaluate any code paths that:

* Build allowed vCPU choices for a UI or CLI
* Validate a requested custom profile size
* Derive provisioning defaults from `supported_vcpu_count.values`

In these scenarios, update your logic to read `vcpu_count.values` directly instead of `supported_vcpu_count.values`.

### 2. Clients that require `vcpu_count` to be a fixed value
{: #guidance-fixed-value}

If your existing client code requires `vcpu_count.value` to always be present, code using the new API version will fail because `vcpu_count` is now returned as an enum with a `values` array for the affected profiles.

Evaluate the intent of any code that reads `vcpu_count.value` directly:

* If the intent is to display or select supported vCPU counts, read `vcpu_count.values` instead and process the array accordingly.
* If the intent is to choose a default custom profile size, use a value from the `vcpu_count.values` array rather than reading a single fixed `value` field.
* If the intent is only to detect whether a profile supports multiple vCPU choices, check whether `vcpu_count.type` is `enum` and whether `vcpu_count.values` contains more than one entry.

### 3. Clients that cache instance profile data or transform it into a standardized internal format
{: #guidance-cached-data}

If your automation stores instance profile data in a cached or standardized internal format, review any schema transformations that flatten these properties:

* `supported_vcpu_count.values`
* `vcpu_count.value`

These transforms can silently produce incomplete or misleading data after the change if they do not handle an enum-based `vcpu_count` response with a `values` array.

Before adopting the new API version, test any downstream systems that consume normalized instance profile data.
