---

copyright:
  years: 2024
lastupdated: "2024-11-06"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I delete my Private Path service?
{: #troubleshoot-pps-2}
{: troubleshoot}
{: support}

As a service provider, you attempted to delete your Private Path service, but encountered an error.
{: shortdesc}

You receive a `Private Path Service Gateway is in use` error message.
{: tsSymptoms}

The Private Path service is still permitting accounts associated with active VPE gateways.
{: tsCauses}

Follow these steps to troubleshoot this issue:
{: tsResolve}

1. Revoke accounts associated with any active VPE gateways. The customer receives a notification that their account is denied and the VPE gateway changes to a `failed` state. For more information, see [Revoking an account's access to a Private Path service](/docs/vpc?topic=vpc-pps-ui-revoke-account&interface=ui).

1. Delete the Private Path service. For more information, see [Updating and deleting a Private Path service](/docs/vpc?topic=vpc-pps-ui-updating-deleting&interface=ui).
