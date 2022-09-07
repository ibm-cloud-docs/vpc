---

copyright:
  years: 2022
lastupdated: "2022-09-12"

keywords: s390x bare metal faq, linuxone bare metal server faq

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for LinuxONE bare metal servers
{: #linuxone-bare-metal-server-faq}

You might encounter the following frequently asked questions when you use a LinuxONE bare metal server.

## What operating systems are supported?
{: #faq-bare-metal-1}
{: faq}

Currently, only SUSE Linux Enterprise Server 15 SP3 and Red Hat Enterprise Linux 8.x (minimal installation) for are supported.

## What storage options are available?
{: #faq-bare-metal-2}
{: faq}

Storage is provided by Fibre Channel Protocol (FCP) that is connected to IBM FlashSystems 9200.

## What is required to set up my LinuxONE Bare Metal servers? 
{: #faq-bare-metal-3}
{: faq}

When you are planning to create a LinuxONE bare metal server, you can go through the configuration checklist on [Planning for LinuxONE bare metal servers](/docs/vpc?topic=vpc-planning-for-bare-metal-servers). 

## What regions are LinuxONE Bare Metal servers available?
{: #faq-bare-metal-4}
{: faq}

The LinuxONE bare metal servers are available in the Toronto region with plans to expand to other regions. 

## Do I need to configure multiple network interfaces on a LinuxONE bare metal server to support the full 10 Gbps bandwidth?
{: #faq-bare-metal-5}
{: faq}

Yes. The bandwidth that you get depends on by the profile that you select.

## Does LinuxONE Bare Metal servers support RAID?
{: #faw-bare-metal-6}
{: faq}

Yes. RAID is supported and configured on IBM FlashSystem 9200 systems.

## Can I enable dual uplinks (uplink redundancy)?
{: faq-bare-metal-7}
{: faq}

No.

## What storage replication is supported for LinuxONE Bare Metal servers?
{: faq-bare-metal-8}
{: faq}

Replication isn't supported.

## What is the size of the boot drive for LinuxONE Bare Metal servers?
{: faq-bare-metal-9}
{: faq}

The boot disk is 100 GB. You can configure each image differently for its partition sizes.  

LinuxONE Bare Metal servers support only FCP images.
{: important}

## How does billing work?
{: #faq-bare-metal-10}
{: faq}

You are billed for LinuxONE bare metal servers based on the server profile that you selected. Billing stops only when you delete the bare metal server. Powering off the server doesn't change billing. For more information about billing and pricing, contact your IBM Sales representative.

You are also billed for other VPC services and resources that are attached to any bare metal servers. For more information about pricing for Bare Metal Servers on VPC, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).

## How is billing for LinuxONE bare metal severs different from LinuxONE virtual server instances?
{: #faq-bare-metal-11}
{: faq}

The main difference between LinuxONE virtual server instances and the LinuxONE bare metal servers is that powering off a bare metal server has no effect on the billing cycle. Meaning that hourly billed servers still accrue at the normal rate whether the server is powered off or on. The billing stops only when the bare metal server is deleted.

## How do I view my invoices?
{: #faq-bare-metal-12}
{: faq}

To view your account invoices, follow these steps.

1. Go to the [{{site.data.keyword.cloud_notm}} console ![External link icon](images/launch-glyph.svg "External link icon")](https://{DomainName}).
2. Then, click **Manage > Billing and Usage**.

Each account receives a single bill. If you need separate billing for different sets of resources, then you need to create multiple accounts.

For more information about invoices, see [Viewing your invoices](/docs/billing-usage?topic=billing-usage-managing-invoices).
