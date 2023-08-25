---

copyright:
  years: 2021, 2023
lastupdated: "2023-08-25"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Storage overview for Bare Metal Servers for VPC
{: #bare-metal-servers-storage}

All profiles of Bare Metal Servers for VPC provide one 0.96 TB SATA M.2 mirrored SSD as the boot disk. Profile `bx2d-metal-192x768` provides an extra set of NVMe (Non-Volatile Memory Express) U.2 solid-state drives (SSD) as secondary local storage. NVMe SSDs provides fast and affordable storage to support options such as VMware vSAN, or customer-managed RAID.
{: shortdesc}

Storage for Bare Metal Servers for VPC is unmanaged. You are responsible for encryption and backing up your data.
{: important}

<!--The total size of the NVMe SSD set varies depending on the profile you select. The NVMe drives are empty by default.-->

The following network-attached storage is not supported:
* Block Storage for VPC

VPC File Storage is compatible. However, security groups access mode, mounts with virtual network interfaces, and encryption in transit are not supported between {{site.data.keyword.filestorage_vpc_short}} and {{site.data.keyword.bm_is_short}}. For more information about file storage, see [About {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-file-storage-vpc-about).
