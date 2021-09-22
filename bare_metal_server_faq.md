---

copyright:
  years: 2021
lastupdated: "2021-07-20"

keywords: bare metal servers, faq

subcollection: vpc

---

{:beta: .beta}
{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# FAQ for bare metal servers (beta)
{: #bare-metal-server-faq}

## What's the difference between bare metal servers (classic infrastructure) and Bare Metal Servers for VPC?
{: #diffs-with-classic-bm}

With Bare Metal Servers for VPC, you can enjoy the security and performance of the private cloud with the flexibility and scalability of the public cloud. Compared to classic bare metal infrastructures, Bare Metal Servers for VPC provides better connectivity and networking throughput empowered by VPC concepts. It also takes less time on provisioning and deploying.

It is important to notice that Bare Metal Servers for VPC is less customizable than classic bare metal servers. Most apparently, Bare Metal Servers for VPC currently only supports the VMware use cases with ESXi.

## What OS options are supported?
{: #supported-os}

Beta only supports VMware ESXi 7.0, but will support RHEL, Windows, Linux, and Custom Image later this year. 

## What storage options are available for Bare Metal Servers for VPC?
{: #supported-os}

For beta, the offering provides two storage options that includes secondary local NVMe drives: 

* The bx2d-metal-192x768 profile provides mirrored 960 GB SATA M.2 drives as boot storage only. 
* The bx2d-metal-192x768 profile provides mirrored 960 GB SATA M.2 drives as boot storage and 16 3.2 TB U.2 NVMe SSDs as secondary local storage to support vSAN, or customer managed RAID. 

## What billing options are available?
{: #billing-options}

Beta of Bare Metal Servers for VPC is provided free of charge.
{: important}

You will be billed for Bare Metal Servers for VPC based on the server profile you've selected. Charging stops only when you delete the bare metal server. Power off will have no impact on billing. 

## What is required to set up your Bare Metal Servers for VPC? 
{: #config-checklist}

When you are planning to create the bare metal servers to host your VMware environments in VPC, go through the configuration checklist on [this page](/docs/vpc?topic=vpc-planning-for-bare-metal-servers). 

## In which regions is Bare Metal Servers for VPC available?
{: #available-zone}

The beta offering will first be available in the `eu-de` region, then we will expand to other regions globally throughout the year. 
