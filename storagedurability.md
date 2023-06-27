---

copyright:
  years: 2020, 2023
lastupdated: "2023-06-26"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Availability and Durability of VPC storage
{: #storageavailability}

In today's fast-paced economy, companies rely on data in their decision-making. They need secure and immediate access to their data on a moment's notice. Data integrity is of high priority because compromised or incomplete data is of no use. Not to mention the dangers that are presented if sensitive data goes missing. When you store your data in {{site.data.keyword.block_storage_is_short}} volumes, snapshots, backups, or in {{site.data.keyword.filestorage_vpc_short}} shares, it's durable, highly available, and encrypted.
{: shortdesc}

| {{site.data.keyword.block_storage_is_short}} Storage type | Use Case | Durability | Availability | Encryption |
|--------------|----------|------------|--------------|------------|
| 3 IOPS per GB tier| It is designed for general-purpose workloads such as workloads that host small databases for web applications or store virtual machine disk images for a hypervisor. |  99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
| 5 IOPS per GB tier| It is designed for high I/O intensity workloads that are characterized by a large percentage of active data, such as transactional and other performance-sensitive databases. |  99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
| 10 IOPS per GB tier| It is designed for demanding storage workloads such as data-intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics. |  99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
| custom | Customers can specify capacity between 10 - 16000 MB with IOPS ranging 100 - 48000. | 99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
{: caption="Table 1. {{site.data.keyword.block_storage_is_short}} Storage durability and availability chart." caption-side="bottom"}

{{site.data.keyword.block_storage_is_short}} volumes are double-encrypted at rest. The double-encryptions includes the underlying volume that holds the customer volumes, and the customer volume. The customer volumes are encrypted by using provider-managed encryption or customer-managed encryption keys. 

| {{site.data.keyword.filestorage_vpc_short}} Storage type | Use Case | Durability | Availability | Encryption |
|--------------|----------|------------|--------------|------------|
| `dp2` | The most flexible share profile option. Customers can specify capacity between 10 - 32000 MB with IOPS ranging 100 - 96000. | 99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
| 3 IOPS per GB tier| It is designed for general-purpose workloads such as workloads that host small databases for web applications or store virtual machine disk images for a hypervisor. |  99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
| 5 IOPS per GB tier| It is designed for high I/O intensity workloads that are characterized by a large percentage of active data, such as transactional and other performance-sensitive databases. |  99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
| 10 IOPS per GB tier| It is designed for demanding storage workloads such as data-intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics. |  99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
| custom | Customers can specify capacity between 10 - 16000 MB with IOPS ranging 100 - 48000. | 99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption, Customer-managed encryption |
{: caption="Table 2. {{site.data.keyword.filetorage_is_short}} Storage durability and availability chart." caption-side="bottom"}

{{site.data.keyword.filestorage_vpc_short}} shares are encrypted by using provider-managed encryption or customer-managed encryption keys.

## Durability
{: #stordurability}

Think of durability as a measurement of how healthy and resilient your data is. Durability in VPC storage means that your data is stored consistent and intact without any signs of data decay, influence of drive failures, or any other form of corruption. 99.999999999% (11 nines) durability means that if you store 10 million files, then you expect to lose one file every 10000 years.

When people hear the word durability, most of them think of hardware failures of storage, compute, and network components that might cause data loss. In VPC storage, your data is protected against drive failures and numerous type of disk errors that otherwise might negatively impact data durability and data integrity. The data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

Other than physical failure, a common source of data loss is accidental deletion or modifications of files by users. {{site.data.keyword.block_storage_is_short}}, Snapshots for VPC, and {{site.data.keyword.filestorage_vpc_short}} are only accessible to authorized hosts within your virtual private network. You control who can access it. For more information, see [Managing security and compliance](/docs/vpc?topic=vpc-manage-security-compliance).

Another measure to protect against accidental deletion and modification of files is a snapshot. If a user accidentally modifies or deletes crucial data from a volume, the data can be easily and quickly restored from a snapshot or a backup. For more information about this feature, see [About Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about).

The 11 nines durability target applies to a single Availability Zone. To protect against natural or man-made disasters that might destroy an entire Availability Zone, consider storing your most important data in multiple locations. For more information, see [Understanding high availability and disaster recovery](/docs/vpc?topic=vpc-ha-dr-vpc).

## High Availability
{: #storavailability}

VPC storage is built upon best-in-class, proven, enterprise-grade hardware and software to ensure high availability and uptime. The data is stored redundantly across multiple physical disks on HA paired nodes. Each storage node has multiple paths to its own Solid-State Drives and its partner node's SSDs as well. This configuration protects against path failure and controller failure because the node can still access its partner's disks for continued productivity. Redundant network ports and paths protect against network failures across the cloud connections. For more information, see [Understanding high availability and disaster recovery](/docs/vpc?topic=vpc-ha-dr-vpc).

## Encryption
{: #storencryption}

{{site.data.keyword.cloud}} provides full-disk encryption without compromising storage application performance. For more information about provider- and customer-managed encryption, see [About data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about).
