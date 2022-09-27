---

copyright:
  years: 2022
lastupdated: "2022-09-27"

keywords: s390x bare metal faq, linuxone bare metal server faq

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for s390x bare metal servers
{: #s390x-bare-metal-server-faq}

You might encounter the following frequently asked questions when you use a s390x bare metal server.

## What operating systems are supported?
{: #faq-s390x-bare-metal-1}
{: faq}

Currently, only SUSE Linux Enterprise Server 15 SP3 and Red Hat Enterprise Linux 8.x (minimal installation) are supported.

## What storage options are available?
{: #faq-s390x-bare-metal-2}
{: faq}

Storage is provided by Fibre Channel Protocol (FCP) that is connected to IBM FlashSystems 9200.

## What is required to set up a s390x bare metal server? 
{: #faq-s390x-bare-metal-3}
{: faq}

When you are planning to create a s390x bare metal server, you can go through the configuration checklist on [Planning for s390x bare metal servers](/docs/vpc?topic=vpc-planning-for-bare-metal-servers). 

## What regions are s390x bare metal servers available?
{: #faq-s390x-bare-metal-4}
{: faq}

The s390x bare metal servers are available in the US East (Washington DC) region with plans to expand to other regions. 

## Do I need to configure multiple network interfaces on a s390x bare metal server to support the full 10 Gbps bandwidth?
{: #faq-s390x-bare-metal-5}
{: faq}

Yes. The bandwidth that you get depends on by the profile that you select.

## Does s390x bare metal servers support RAID?
{: #faw-s390x-bare-metal-6}
{: faq}

Yes. RAID is supported and configured on IBM FlashSystem 9200 systems.

## Can I enable dual uplinks (uplink redundancy)?
{: faq-s390x-bare-metal-7}
{: faq}

No.

## What storage replication is supported for s390x bare metal servers?
{: faq-s390x-bare-metal-8}
{: faq}

Replication isn't supported.

## What is the size of the boot drive for a s390x bare metal server?
{: faq-s390x-bare-metal-9}
{: faq}

The boot disk is 100 GB. You can configure each image differently for its partition sizes.  

s390x bare metal servers support only FCP images.
{: important}

## How does billing work?
{: #faq-s390x-bare-metal-10}
{: faq}

You are billed for s390x bare metal servers based on the server profile that you selected. Billing stops only when you delete the bare metal server. Powering off the server doesn't change billing. For more information about billing and pricing, contact your IBM Sales representative.

You are also billed for other VPC services and resources that are attached to any bare metal servers. For more information about pricing for Bare Metal Servers on VPC, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).

## How is billing for s390x bare metal severs different from s390x virtual server instances?
{: #faq-s390x-bare-metal-11}
{: faq}

The main difference between s390x virtual server instances and the s390x bare metal servers is that powering off a bare metal server has no effect on the billing cycle. Meaning that hourly billed servers still accrue at the normal rate whether the server is powered off or on. The billing stops only when the bare metal server is deleted.

## How do I view my invoices?
{: #faq-s390x-bare-metal-12}
{: faq}

To view your account invoices, follow these steps.

1. Go to the [{{site.data.keyword.cloud_notm}} console ![External link icon](images/launch-glyph.svg "External link icon")](https://{DomainName}).
2. Then, click **Manage > Billing and Usage**.

Each account receives a single bill. If you need separate billing for different sets of resources, then you need to create multiple accounts.

For more information about invoices, see [Viewing your invoices](/docs/billing-usage?topic=billing-usage-managing-invoices).
