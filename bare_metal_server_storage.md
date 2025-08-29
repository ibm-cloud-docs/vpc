---

copyright:
  years: 2021, 2023
lastupdated: "2023-10-03"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Storage overview for Bare Metal Servers for VPC
{: #bare-metal-servers-storage}

All profiles of {{site.data.keyword.bm_is_short}} provide one 0.96 TB SATA M.2 mirrored SSD as the boot disk. Profile `bx2d-metal-96x384` provides an extra set of NVMe (Non-Volatile Memory Express) U.2 solid-state drives (SSD) as secondary local storage. NVMe SSDs provides fast and affordable storage to support options such as VMware vSAN, or customer-managed RAID.
{: shortdesc}

Storage for {{site.data.keyword.bm_is_short}} is unmanaged. You are responsible for encryption and backing up your data.
{: important}



{{site.data.keyword.filestorage_vpc_short}} is a compatible network-attached storage solution for {{site.data.keyword.bm_is_short}} that are provisioned after 31 August 2023. For more information, see [About {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-file-storage-vpc-about).

The following network-attached storage is not supported:
* {{site.data.keyword.block_storage_is_short}} 
