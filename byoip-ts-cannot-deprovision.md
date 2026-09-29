---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: byoip, troubleshooting, authorized CIDR, deprovision, bring your own IP

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I deprovision my authorized CIDR?
{: #ts-byoip-cannot-deprovision}
{: troubleshoot}
{: support}

[IPv4 BYOIP is an allowlisted beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

The deprovision request for your authorized CIDR is blocked.
{: shortdesc}

Your deprovision request is blocked, and the console displays the message: *Delete associated public address ranges before deprovisioning CIDR.*
{: tsSymptoms}

An authorized CIDR cannot be deprovisioned if it still has active public address range allocations.
{: tsCauses}

Delete all public address ranges that are allocated from the CIDR before you submit the deprovision request. After all allocations are removed, retry the deprovision. For more information, see [Deprovisioning a custom authorized CIDR](/docs/vpc?topic=vpc-deprovision-custom-authorized-cidr&interface=ui).
{: tsResolve}
