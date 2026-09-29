---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: byoip, troubleshooting, authorized CIDR, bring your own IP, CIDR not visible, ipv4

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my IPv4 authorized CIDR not visible after the support case was closed?
{: #ts-byoip-cidr-not-visible}
{: troubleshoot}
{: support}

[BYOIP for IPv4 is a beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You submitted a support case to provision a custom authorized CIDR and received confirmation that it was completed, but the CIDR is not visible in your account.
{: shortdesc}

After you receive confirmation that the support case is closed, the authorized CIDR does not appear in your account.
{: tsSymptoms}

The provisioning process involves multiple IBM teams. The CIDR might not yet be visible if the final configuration step in the VPC control plane has not been completed.
{: tsCauses}

Check the following:
{: tsResolve}

- Allow several hours after the support case is closed before checking again.
- Confirm that you are looking in the correct region. Authorized CIDRs are scoped to a single region.
- If the CIDR is still not visible after 24 hours, reply to the support case to request a status update.
