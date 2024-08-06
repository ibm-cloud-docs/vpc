---

copyright:
  years: 2019, 2024
lastupdated: "2024-08-06"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Block Storage capacity and performance
{: #capacity-performance}

Choosing the optimal Block Storage volume size and performance level for your workloads is important. When you provision {{site.data.keyword.block_storage_is_short}}, you can specify the size of your volume and the performance level that you require.
{: shortdesc}

## Capacity
{: #block-storage-vpc-capacity}

{{site.data.keyword.block_storage_is_short}} offers a range of storage capacities to meet your requirements.

Based on the [storage profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers) that you chose for your data volume, you can specify 10-16,000 GB of capacity per Block Storage data volume in 1 GB increments.

Boot volumes are 100 GB by default. If you provision an instance from a custom image, you can specify a boot volume capacity up to 250 GB.



## Block Storage IOPS profiles
{: #iops-profiles}

When you provision {{site.data.keyword.block_storage_is_short}} volumes, you specify an [IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles) that best meets your storage requirements. Three predefined tiered profiles are available, or you can choose a custom profile. [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provide guaranteed IOPS/GB performance for volumes up to 16,000 GB capacity. A [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile defines ranges of volume capacity and IOPS that you can select. These profiles are backed by solid-state drives (SSDs).



## How volume bandwidth is allocated
{: #cp-storage-bandwidth-allocate}

Bandwidth that is available to the VSI is split between attached Block Storage volumes and networking. The initial volume and network bandwidth allocation depends on the [instance profile](/docs/vpc?topic=vpc-profiles) and the bandwidth ratio that you selected for your instance.

The allocation of the instance's total bandwidth can be adjusted, balancing between network bandwidth and volume bandwidth. If you do not specify the initial volume and network bandwidth allocation, then 25% of total instance bandwidth is allocated to volume bandwidth and 75% is allocated to network bandwidth.

The maximum volume bandwidth is the highest potential bandwidth that can be allocated to the volume when it is attached to an instance. In cases where the total maximum bandwidth of attached volumes exceeds the amount that is available on the instance, the bandwidth for each volume attachment is set proportionally. The bandwidth is allocated based on the corresponding volume's maximum bandwidth.

The volume bandwidth available to the instance is apportioned on a per-volume basis. The bandwidth is assigned per volume, not shared between volumes.
{: note}

For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

## How I/O size affects performance
{: #how-block-size-affects-performance}

The IOPS value is based on a 16 KB block size (for all the tiers) with a 50-50 read/write random workload. Each 16 KB of data read/written counts as one read/write operation; a single write of less than 16 KB counts as a single write operation.

Baseline throughput is determined by the number of IOPS multiplied by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB or custom IOPS tiers. The higher the IOPS that you specify, the higher the throughput. Maximum throughput is 1024 MBps.

The application I/O size directly impacts storage performance. If the application I/O size is smaller than the throughput multiplier that is used by the profile to calculate the volume’s bandwidth limit, the IOPS limit is reached before the throughput limit. Conversely, if the application I/O size is larger, the throughput limit is reached before the IOPS limit.

The following table provides some examples of how application I/O size and provisioned IOPS affect the throughput, which is calculated as average application I/O size x IOPS = Throughput in MBps.

| Average I/O Size (KB) | IOPS | Throughput (MBps) |
|-----------------|------|-------------------|
| 4 (typical for Linux&reg;) | 1,000 | 4 |
| 8 (typical for Oracle) | 1,000  | 8 |
| 16 | 1,000 | 16 |
| 32 (typical for SQL Server) | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
{: caption="Table 1. Examples of how application I/O size and IOPS affect the throughput" caption-side="top"}

In these examples, your performance caps are 1000 IOPS or 16 MBps throughput. You can achieve maximum IOPS when you use smaller I/O sizes, but throughput is less than what the volume can handle. The following example shows how throughput decreases for smaller average I/O sizes, when max IOPS is maintained.

* 16 KB * 6000 IOPS == ~94 MBps
* 8 KB * 6000 IOPS == ~47 MBps
* 4 KB * 6000 IOPS == ~23 MBps
