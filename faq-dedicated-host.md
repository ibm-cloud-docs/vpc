---

copyright:
  years: 2021
lastupdated: "2021-02-23"

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}

# FAQs for dedicated hosts
{: #faqs-dedicated-host}

## What elements do I need to create a dedicated host?
{: #faq-dedicated-host-0}
{: faq}

Dedicated hosts in {{site.data.keyword.vpc_short}} are created as part of a dedicated group. When you use {{site.data.keyword.cloud_notm}} 
console to create a dedicated host, you create an initial dedicated group as part of the dedicated host provisioning process. 
If you are using the {{site.data.keyword.cloud_notm}} CLI to create a dedicated host, you must first create a dedicated group. 
For more information, see [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances). 

## How much am I charged for a dedicated host?
{: #faq-dedicated-host-1}
{: faq}

When you create a dedicated host, you are billed by the usage of the host on an hourly basis. You are not billed for the vCPU and RAM associated with instances that are running on the host. 
 
## Why can I only create one dedicated host per region?
{: #faq-dedicated-host-2}
{: faq}

When you provision dedicated hosts, the vCPU associated with your dedicated hosts counts toward the total vCPU for virtual 
server instances per region. The standard quota for virtual servers vCPU is 200. The dedicated host profile uses 152 vCPUs. 
To increase your vCPU quota, [contact Support](/docs/get-support?topic=get-support-using-avatar). For more information, see [Quotas](/docs/vpc?topic=vpc-quotas#vpcquotas).  

## What are the advantages of provisioning instances on a dedicated group versus a dedicated host?
{: #faq-dedicated-host-3}
{: faq}

Provisioning instances on a dedicated group allows your instances to move between hosts if the need ever occurs. For example, if you want to decrease the size of your dedicated host group, you can stop instances on one of the hosts and disable placement for the host that you want to decomission. Then, when you restart the instances they are started on another host in the group if capacity is available. 
