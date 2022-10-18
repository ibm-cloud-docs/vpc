---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-16"

keywords: vpc, vpc block storage, ISCSI, durability, availability, HA, high-availability, data loss, data integrity, uptime, five 9's, eleven 9's, data health, data corruption, data decay, encryption, security, integrity

subcollection: vpc

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}

# Availability and Durability of VPC storage
{: #storageavailability}

In today's fast-paced economy, companies rely on data in their decision-making. They need secure and immediate access to their data on a moment's notice. Data integrity is top priority because compromised or incomplete data is of no use. Not to mention the dangers that are presented if sensitive data goes missing. When you store your data in {{site.data.keyword.block_storage_is_short}} volumes, snapshots, or in File Storage for VPC file shares, it's durable, highly available, and encrypted.
{: shortdesc}

| Storage type | Use Case | Durability | Availability | Encryption |
|--------------|----------|------------|--------------|------------|
| 3 IOPS per GB tier| It's designed for general-purpose workloads such as workloads that host small databases for web applications or store virtual machine disk images for a hypervisor. |  99.999999999% \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
| 5 IOPS per GB tier| It's designed for high I/O intensity workloads that are characterized by a large percentage of active data, such as transactional and other performance-sensitive databases. |  99.999999999% \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
| 10 IOPS per GB tier| It's designed for demanding storage workloads such as data-intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics. |  99.999999999% \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
{: caption="Table 1. Storage durability and availability chart." caption-side="bottom"}

Block storage volumes are double-encrypted at rest. This includes the underlying volume that holds the customer volumes, and the customer volume. The customer volumes are encrypted using provider-managed encryption or customer-managed encryption keys. Customer file shares are encrypted using provider-managed encryption or customer-managed encryption keys, but there is no underlying encryption.
{: note}

## Durability
{: #stordurability}

Think of durability as a measurement of how healthy and resilient your data is. Durability in {{site.data.keyword.vpn_vpc_short}} means that your data is stored consistent and intact without any signs of data decay, influence of drive failures, or any other form of corruption. 99.999999999% (11 nines) durability means that if you store 10 million files, then you expect to lose one file every 10000 years.

When people hear the word durability, most of them think of hardware failures of storage, compute, and network components that could cause data loss. In {{site.data.keyword.vpn_vpc_short}}, your data is protected against drive failures and numerous type of disk errors that otherwise might negatively impact data durability and data integrity. The data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

Other than physical failure, a common source of data loss is accidental deletion or modifications of files by users. Block Storage for VPC, Snapshots for VPC, and File Storage for VPC are only accessible to authorized hosts within your virtual private network. You control who can access it. For more information, see [Managing security and compliance](/docs/vpc?topic=vpc-manage-security-compliance). 

Another measure to protect against accidental deletion and modification of files is a snapshot. If a user accidentally modifies or deletes crucial data from a volume, the data can be easily and quickly restored from a snapshot copy. For more information about this feature, see [About Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about).

The 11 nines durability target applies to a single Availability Zone. To protect against natural or man-made disasters that could destroy an entire Availability Zone, consider storing your most important data in multiple locations. For more information about this topic, see [Understanding high availability and disaster recovery](/docs/vpc?topic=vpc-ha-dr-vpc).

## High Availability
{: #storavailability}

VPC storage is built upon best-in-class, proven, enterprise-grade hardware and software to ensure high availability and uptime. As mentioned previously, the data is stored redundantly across multiple physical disks on HA paired nodes. Each storage node has multiple paths to its own Solid-State Drives and its partner node's SSDs as well. This configuration protects against path failure and controller failure because the node can still access its partner's disks for continued productivity. Redundant network ports and paths protect against network failures across the cloud connections. For more information, see [Understanding high availability and disaster recovery](/docs/vpc?topic=vpc-ha-dr-vpc).

## Encryption
{: #storencryption}

{{site.data.keyword.cloud}} provides full-disk encryption without compromising storage application performance. For more information about provider- and customer-managed encryption, see [About data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about).
