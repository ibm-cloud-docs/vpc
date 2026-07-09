---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot virtual server instance, network interfaces, more than 5 network interfaces, network interface limit, vCPU

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I add more than 5 network interfaces to my virtual server instance?
{: #error-above-5-network-interfaces}
{: troubleshoot}
{: support}

You receive an error when you try to add more than five network interfaces to an existing virtual server instance.
{: shortdesc}

You receive an error when you try to add more than five network interfaces to an existing virtual server instance.
{: tsSymptoms}

The network interface limit was previously set to five interfaces. Virtual server instances that were provisioned before this limit was increased require a stop and start cycle to apply the new limit.
{: tsCauses}

If the [x86 instance profile](/docs/vpc?topic=vpc-profiles) that you used to provision your virtual server includes 17 or more vCPUs, you can add more than five network interfaces. To take advantage of this capability on a virtual server that was provisioned before the limit increased, stop the running virtual server and then start it again. For more information about multiple network interfaces, see [Managing network interfaces](/docs/vpc?topic=vpc-using-instance-vnics).
{: tsResolve}
