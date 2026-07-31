---

copyright:
  years: 2026
lastupdated: "2026-07-31"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Action required: Prepare your automation for instance profile vCPU count response changes
{: #vsi-instance-profile-vcpu-remediation}

## What are we changing?
{: #vcpu-what-changed}

The VPC API version `2026-09-01` introduces changes to how instance profile vCPU counts are represented. Requests that use earlier API versions are not affected. For information about this upcoming change, see the [Upcoming changes](/docs/vpc?topic=vpc-api-change-log#upcoming-changes) section of the VPC API change log, and look for the `InstanceProfile` response change entry.

For API versions `2026-09-01` or later:

* The `supported_vcpu_count` property will be removed from instance profile responses.
* The existing `vcpu_count` property will no longer support a fixed-value response for the affected profiles.
* For profiles that previously returned supported values by using `supported_vcpu_count`, the `vcpu_count` property will instead return those supported values directly by using an enum-based (a list of allowed values) shape.
* Clients that already handle `vcpu_count` as a range-based or dependent shape are not affected by those existing shapes. Clients that use version dates before `2026-09-01` are not affected. The change is specific to clients that rely on the old fixed-value shape or on the separate `supported_vcpu_count` property and are using an API version of `2026-09-01` or later.

## Why are we making this change?
{: #vcpu-why-change}

This change simplifies the instance profile response model by removing duplicated vCPU count information and returning supported vCPU values directly in the existing `vcpu_count` property. This enhancement improves consistency in how instance profile capabilities are represented across profiles.

## Who will be affected by this change?
{: #vcpu-those-affected-by-change}

This change applies only to requests that use the API version in which this change is introduced and later versions. Accounts using earlier API versions are not affected and will continue to receive the existing response for instance profiles.

Accounts with code or automation that retrieves or lists instance profiles using API version `2026-09-01` or later will be affected, particularly:

* Code that reads `supported_vcpu_count` from instance profile responses
* Code that requires `vcpu_count` to always contain a fixed `value`
* Code that is hardcoded to treat `vcpu_count.type` as `fixed`
* Code that maps instance profile sizing logic from `supported_vcpu_count.values` instead of inspecting `vcpu_count`
* Validation or parsing logic that does not support the enum-based `vcpu_count`

## What actions can you take to avoid disruption?
{: #vcpu-actions-to-avoid-disruption}

If you or other users in your account retrieve or list instance profiles using API version `2026-09-01` or later, perform the following tasks before adopting that API version:

* Pin requests to an API version before `2026-09-01` if your code or automation cannot be updated before the change date. Requests that specify an earlier version date will not be affected.
* Check for code or automation that is hardcoded to treat `vcpu_count` as a fixed `value`.
* Update parsing and validation logic so that it handles all supported `vcpu_count` response shapes, including enum-based, range-based, and dependent values.
* Test any profile selection, sizing, or provisioning workflows that currently derive supported vCPU choices from `supported_vcpu_count`.

## Migration guidance
{: #vcpu-migration-guidance}

Detailed migration guidance and examples are available in [Updating to the `2026-09-01` version (instance profile vCPU count change)](/docs/vpc?topic=vpc-2026-09-01-instance-profile-vcpu-count-change). Review this guidance before updating your clients to API version `2026-09-01`.
