---

copyright:
  years: 2026
lastupdated: "2026-05-28"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# Attention: Changes to VPN gateway IKE and IPsec policy API
{: #vpn-hcr-introduction}

On 29 May 2026, a feature that adds support for multiple algorithms in IKE and IPsec policies for VPN gateways was made generally available in the VPC service. Because this feature changes the behavior of the related APIs, customers using automation to provision or manage these policies should complete the required remediation actions before the release becomes generally available.
{: shortdesc}

If you have already completed the required remediation actions, no further action is needed.

## What are we changing?
{: #vpn-hcr-whats-changing}

- The following single-value properties are deprecated, but continue to be supported for backward compatibility:

   - For IKE policies: `authentication_algorithm`, `dh_group`, and `encryption_algorithm` are deprecated in favor of `authentication_algorithms`, `dh_groups`, and `encryption_algorithms` arrays.
   - For IPsec policies: `authentication_algorithm`, `encryption_algorithm`, and `pfs` are deprecated in favor of `authentication_algorithms`, `encryption_algorithms`, and `pfs_groups` arrays.

- API responses will always include both the deprecated properties and the new array properties.

- When multiple algorithms are configured in an array-based property, the deprecated properties will return sentinel values:

   - `"multiple"` for string properties (`authentication_algorithm`, `encryption_algorithm`, `pfs`)
   - `65535` for the integer `dh_group` property

- PATCH operations (for both IKE and IPsec policies) will deviate from standard JSON Merge Patch (RFC 7396) semantics:

   - Updating a deprecated single-value property will automatically update the corresponding array property, and vice versa.

   For example:

   - A PATCH request with:

      ```json
      {"encryption_algorithm": "aes256"}
      ```

      Will set:

      ```json
      "encryption_algorithms": ["aes256"]
      ```

   - A PATCH request with:

      ```json
      {"encryption_algorithms": ["aes256", "aes128"]}
      ```

      Will set:

      ```json
      "encryption_algorithm": "multiple"
      ```

## Why are we making this change?
{: #vpn-hcr-why-change}

Allowing VPN gateways to negotiate across a prioritized list of cryptographic algorithms improves security and flexibility. This enhances interoperability with diverse on-premises VPN devices and enables configuration of fallback algorithms while maintaining strong security postures.

Deprecated properties remain supported to preserve backward compatibility.

## Who will be affected by this change?
{: #vpn-hcr-who-is-affected}

Accounts with client code or automation that retrieves, validates, or updates IKE or IPsec policies are  affected, particularly:

- Code that parses or validates deprecated single-value properties without accounting for sentinel values
- Code that relies on standard [JSON Merge Patch (RFC 7396)](https://datatracker.ietf.org/doc/html/rfc7396){: external} semantics for PATCH operations

## What actions can you take to avoid a disruption?
{: #vpn-hcr-actions}

As documented in the [property value expansion](/apidocs/vpc/latest#property-value-expansion) guidance, ensure that your client code or automation processes deprecated property values while accounting for the new sentinel values `multiple` and `65535`, according to [API best practices](/apidocs/vpc/latest#api-best-practices).

- For IKE policies:
   - `authentication_algorithm` and `encryption_algorithm` can return `multiple`
   - `dh_group` can return `65535`

- For IPsec policies:
   - `authentication_algorithm`, `encryption_algorithm`, and `pfs` can return `multiple`

- Deprecated properties will continue to return existing (non-sentinel) values when only one algorithm is used.

Multiple algorithms are not supported for IKEv1 policies. IKEv1 policies will remain limited to a single algorithm per category.
{: note}

If you or other users in your account are currently using IKE or IPsec policies, complete the following tasks:

- Review code or automation that retrieves IKE or IPsec policies and processes the following single-value properties:

   - IKE policies:
      - `authentication_algorithm`
      - `dh_group`
      - `encryption_algorithm`
   - IPsec policies:
      - `authentication_algorithm`
      - `encryption_algorithm`
      - `pfs`

- Update validation logic to follow [API best practices](/apidocs/vpc/latest#api-best-practices).
