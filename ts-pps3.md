---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-21"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How do I migrate to a newer version of my target service without any disruption of existing VPE connections?
{: #troubleshoot-pps-3}
{: troubleshoot}
{: support}

As a service provider, you want to migrate to a newer version of your target service without disrupting connectivity for your current customers.
{: shortdesc}

A newer, updated version of your target service is available.
{: tsSymptoms}

A possible cause is that the provider needs to update their target service without deleting or disrupting the service they provide to their current consumers.
{: tsCauses}

Follow these steps to troubleshoot this Private Path issue:
{: tsResolve}

If you’re updating the actual target service without changing the load balancer, you don’t need to modify Private Path service. Instead, update the Private Path network load balancer. For more information, see [Updating and deleting a Private Path service](/docs/vpc?topic=vpc-pps-ui-updating-deleting&interface=ui#pps-ui-update-target-private-path-service).
