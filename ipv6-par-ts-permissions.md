---

copyright:
  years: 2026
lastupdated: "2026-10-28"

keywords: troubleshoot, public address range, ipv6, authorized cidr, permissions error, IAM

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I create or delete an IPv6 authorized CIDR due to a permissions error?
{: #troubleshoot-ipv6-par-permissions}
{: troubleshoot}
{: support}

[Public IPv6 address ranges](/docs/vpc?topic=vpc-release-notes&interface=ui#vpc-oct2726) is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

Creating or deleting an IPv6 authorized CIDR fails with an authorization error.
{: shortdesc}

Creating or deleting an IPv6 authorized CIDR fails with an authorization error.
{: tsSymptoms}

You do not have the required IAM actions for the IPv6 authorized CIDR resource.
{: tsCauses}

To create an IPv6 authorized CIDR, your IAM policy must include the `is.public-address-range-authorized-cidr.authorized-cidr.create` action. To delete one, you need `is.public-address-range-authorized-cidr.authorized-cidr.delete`. To rename one, you need `is.public-address-range-authorized-cidr.authorized-cidr.update`. These actions require at least the **Editor** IAM role on the Public Address Range service.
{: tsResolve}

For more information, see [IAM roles and actions for public address ranges](/docs/iam?topic=iam-iam-service-roles-actions#is.public-address-range-roles).

## Related links
{: #ipv6-par-ts-permissions-related}

- [About public IPv6 address ranges](/docs/vpc?topic=vpc-ipv6-par-about)
- [Creating and managing IPv6 authorized CIDRs](/docs/vpc?topic=vpc-ipv6-par-authorized-cidr)
- [Creating IPv6 public address ranges and binding to a VNI](/docs/vpc?topic=vpc-ipv6-par-creating-binding)
- [Known issues for public address ranges](/docs/vpc?topic=vpc-par-known-issues)
