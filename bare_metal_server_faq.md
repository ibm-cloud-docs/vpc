---

copyright:
  years: 2021, 2023
lastupdated: "2023-08-25"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for bare metal servers
{: #bare-metal-server-faq}

You might encounter the following frequently asked questions when you use a Bare Metal Server for VPC.

## What's the difference between Bare Metal Servers (Classic infrastructure) and Bare Metal Servers for VPC?
{: #faq-bare-metal-0}
{: faq}

Compared to the classic bare metal infrastructure, Bare Metal Servers for VPC provides the following advantages:

* Security and performance of the private cloud with the flexibility and scalability that you expect of the public cloud.
* Integrates with other VPC services such as virtual server, security groups, networking, and more. 
* Faster provisioning and deployment and simple hourly billing.

Keep in mind that Bare Metal Servers for VPC is less customizable than classic bare metal servers.

For more information about the differences between the Classic infrastructure and VPC, see [
Comparing IBM Cloud Classic and VPC infrastructure environments](/docs/cloud-infrastructure?topic=cloud-infrastructure-compare-infrastructure). 

## What operating systems are supported?
{: #faq-bare-metal-1}
{: faq}

Currently, VMware ESXi, Windows, RHEL, RHELfor SAP, Debian GNU, SUSE Linux Enterprise, Ubuntu Linux, and custom images are supported.

## What storage options are available?
{: #faq-bare-metal-2}
{: faq}

Two storage options are available that include secondary local NVMe drives.

* The `bx2-metal-192x768` profile provides mirrored 960 GB SATA M.2 drives as boot storage only. 
* The `bx2d-metal-192x768` profile provides mirrored 960 GB SATA M.2 drives as boot storage and 16 3.2 TB U.2 NVMe SSDs as secondary local storage to support vSAN, or user-managed RAID. 

{{site.data.keyword.block_storage_is_short}} is not supported. {{site.data.keyword.filestorage_vpc_short}} is compatible. However, security groups access mode, mounts with virtual network interfaces, and encryption in transit are not supported between {{site.data.keyword.filestorage_vpc_short}} and {{site.data.keyword.bm_is_short}}. For more information about file storage, see [About {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-file-storage-vpc-about).

## What is required to set up my Bare Metal Servers for VPC? 
{: #faq-bare-metal-3}
{: faq}

When you are planning to create the bare metal servers, you can go through the configuration checklist on [Planning for bare metal servers](/docs/vpc?topic=vpc-planning-for-bare-metal-servers). 

## What regions are Bare Metal Servers for VPC available?
{: #faq-bare-metal-4}
{: faq}

Bare metal servers are available in eu-de, us-east, and us-south regions.

## Do I need to configure multiple network interfaces on a bare metal server to support the full 100 Gbps bandwidth?
{: #faq-bare-metal-5}
{: faq}

No. You can add as many or as few vNICs as you need. Every interface can take advantage of the 100 Gbps bandwidth, but the total aggregate is 100 Gbps for each server. Meaning that the bandwidth is shared by all the vNICs. For more information about networking on bare metal, see [Networking overview for bare metal servers](/docs/vpc?topic=vpc-bare-metal-servers-network).

## How many NVMe drives does a bare metal server support? 
{: #faq-bare-metal-6}
{: faq}

The number NVMe drives that are supported depends on the profile that you select. The 4-socket profile with drives supports 16 3.2 TB NVMe drives and the planned 2-socket profile with drives supports eight 3.2 TB NVMe drives. These NVMe drives are in addition to the 960 Gb boot drive. 

No other drive configurations are supported and drive size, type, and quantity can't be changed.
{: note} 

For more information about profiles, see [Profiles for Bare Metal Servers for VPC](/docs/vpc?topic=vpc-bare-metal-servers-profile).

## Does Bare Metal Servers for VPC support RAID?
{: #faq-bare-metal-7}
{: faq}

The boot disk supports RAID 1 by using a hardware RAID controller. If you use a profile with NVMe drives, it is recommended that you use a software RAID option.  
 
Secondary drives use a JBOD configuration and aren't supported by a hardware RAID controller. 

## Can I enable dual uplinks (uplink redundancy)?  
{: #faq-bare-metal-8}
{: faq}

No. The uplinks (PCI network interfaces) are redundant by design. The VLAN network that you create are on that default, redundant uplink. You don’t need to manage uplink redundancy because redundancy is automatic.   

For more information, see [Networking overview for Bare Metal Servers on VPC](/docs/vpc?topic=vpc-bare-metal-servers-network). 

## What storage replication is supported for Bare Metal Servers for VPC? 
{: #faq-bare-metal-9}
{: faq}

Replication isn't supported. 

## What is the size of the boot drive for Bare Metal Servers for VPC? 
{: #faq-bare-metal-10}
{: faq}

The boot disk is 960 GB. You can configure each image differently for its partition sizes.  
 
Bare Metal Servers for VPC supports only UEFI images. 
{: important}

## How does billing work?
{: #faq-bare-metal-11}
{: faq}

You are billed for Bare Metal Servers for VPC based on the server profile that you selected. Billing stops only when you delete the bare metal server. Powering off the server doesn't change billing. For more information about billing and pricing, contact your IBM Sales representative.

You are also billed for other VPC services and resources that are attached to any bare metal servers. For more information about pricing for Bare Metal Servers on VPC, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).

## How is billing for bare metal severs different from virtual server instances?
{: #faq-bare-metal-12}
{: faq}

The main difference between virtual server instances and the bare metal servers is that powering off a bare metal server has no effect on the billing cycle. Meaning that hourly billed servers still accrue at the normal rate whether the server is powered off or on. The billing stops only when the bare metal server is deleted. 

## How do I view my invoices?
{: #faq-bare-metal-13}
{: faq}

To view your account invoices, follow these steps.

1. Go to the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](/login), 
2. Then, click **Manage > Billing and Usage**.

Each account receives a single bill. If you need separate billing for different sets of resources, then you need to create multiple accounts.

For more information about invoices, see [Viewing your invoices](/docs/billing-usage?topic=billing-usage-managing-invoices).
