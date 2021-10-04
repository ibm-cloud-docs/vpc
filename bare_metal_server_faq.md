---

copyright:
  years: 2021
lastupdated: "2021-09-17"

keywords: bare metal servers, faq, bare metal faq

subcollection: vpc

---

{:beta: .beta}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}
{:note: .note}
{:important: .important}

# FAQ for bare metal servers (Beta)
{: #bare-metal-server-faq}

Bare Metal Servers on VPC is a Beta feature that requires special approval. Contact your IBM Sales representative if you're interested in getting access.
{: beta}

## What's the difference between Bare Metal Servers (classic infrastructure) and Bare Metal Servers for VPC?
{: #faq-bare-metal-0}
{: faq}

Compared to the classic bare metal infrastructure, Bare Metal Servers for VPC provides the following advantages:

* Security and performance of the private cloud with the flexibility and scalability of the public cloud.
* Better connectivity and networking throughput empowered by VPC concepts. 
* Faster provisioning and deployment.

Keep in mind that Bare Metal Servers for VPC is less customizable than classic bare metal servers. 

## What operating systems are supported?
{: #faq-bare-metal-1}
{: faq}

Currently, only VMware ESXi 7.0 is supported. Support for RHEL, Windows, Linux, and custom images is coming soon. 

## What storage options are available?
{: #faq-bare-metal-2}
{: faq}

For the beta version, you have two storage options that include secondary local NVMe drives: 

* The `bx2d-metal-192x768` profile provides mirrored 960 GB SATA M.2 drives as boot storage only. 
* The `bx2d-metal-192x768` profile provides mirrored 960 GB SATA M.2 drives as boot storage and 16 3.2 TB U.2 NVMe SSDs as secondary local storage to support vSAN, or user-managed RAID. 

## What is required to set up my Bare Metal Servers for VPC? 
{: #faq-bare-metal-3}
{: faq}

When you are planning to create the bare metal servers, you can go through the configuration checklist on [Planning for bare metal servers](/docs/vpc?topic=vpc-planning-for-bare-metal-servers). 

## What regions are Bare Metal Servers for VPC available?
{: #faq-bare-metal-4}
{: faq}

The beta offering is first available in the `eu-de` region and expand to other regions globally soon. 

## How does billing work?
{: #faq-bare-metal-5}
{: faq}

You are billed for Bare Metal Servers for VPC based on the server profile that you selected. Billing stops only when you delete the bare metal server. Powering off the server doesn't change billing. 

The Beta version of Bare Metal Servers for VPC is provided free of charge.
{: important}

See the following table for the available price plans. Billing and pricing are subject to change. For more information about billing and pricing, contact your IBM Sales representative.

| Profile | Price per hour |
|---------|---------|
| bx2-metal-192x768 | USD 9.219 |
| bx2d-metal-192x768 | USD 12.726 |
{: caption="Table 1. Price plans" caption-side="top"}

You are also billed for other VPC services and resources that are attached to any bare metal servers. For more information about pricing for Bare Metal Servers on VPC, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).

## How is billing for bare metal severs different from virtual server instances?
{: #faq-bare-metal-6}
{: faq}

The main difference between virtual server instances and the bare metal servers is that powering off a bare metal server has no effect on the billing cycle. Meaning that hourly billed servers still accrue at the normal rate whether the server is powered off or on. The billing stops only when the bare metal server is deleted. 

## How do I view my invoices?
{: #faq-bare-metal-7}
{: faq}

To view your account invoices follow these steps.

1. Go to the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), 
2. Then, click **Manage > Billing and Usage**.

Each account receives a single bill. If you need separate billing for different sets of resources, then you need to create multiple accounts.

For more information about invoices, see [Viewing your invoices](/docs/billing-usage?topic=billing-usage-managing-invoices).
