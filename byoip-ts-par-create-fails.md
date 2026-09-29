---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: byoip, troubleshooting, authorized CIDR, public address range, create fails, bring your own IP

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I create a public address range from my authorized CIDR?
{: #ts-byoip-par-create-fails}
{: troubleshoot}
{: support}

[IPv4 BYOIP is an allowlisted beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

When you try to create a public address range by specifying a CIDR block from your authorized CIDR, the request fails.
{: shortdesc}

An error is returned when you submit a request to create a public address range that specifies a CIDR block from your authorized CIDR.
{: tsSymptoms}

One or more of the following might be the cause:
{: tsCauses}

- The specified CIDR block overlaps with an existing public address range already allocated from the authorized CIDR.
- The specified CIDR block is not within the range of any authorized CIDR in your account.
- You do not have the required IAM action (`is.public-address-range-authorized-cidr.authorized-cidr.operate`) to allocate from the authorized CIDR.
- The authorized CIDR might still be in a pending state and not yet fully provisioned.

Check the following:
{: tsResolve}

- List your authorized CIDRs and their current allocations to confirm the CIDR block is available and within range.
- Verify that you have the `is.public-address-range-authorized-cidr.authorized-cidr.operate` IAM action on the authorized CIDR.
- If the CIDR was recently provisioned, wait a few minutes and retry.
