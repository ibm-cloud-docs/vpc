---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-12"

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

When you choose the second-generation `sdp` profile, you can provision second-generation data volumes up to 32,000 GB.

When you select a first-generation [storage profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers), you can specify 10-16,000 GB of capacity in 1 GB increments.

Boot volumes are 100 GB by default. If you provision an instance with a custom image or a snapshot, you can specify a boot volume capacity up to 250 GB.

## Block Storage volume profiles
{: #iops-profiles}

When you provision {{site.data.keyword.block_storage_is_short}} volumes, you specify a [volume profile](/docs/vpc?topic=vpc-block-storage-profiles) that best meets your storage requirements.

You can provision volumes with the [SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-profiles#defined-performance-profile) with custom IOPS in the range of 3000 - 64,000 IOPS. An IOPS level of over 48,000 can be achieved when the new volume is attached to a virtual server instance with a [3rd generation instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In addition to custom capacity and IOPS, you can also specify a custom throughput limit for a second-generation volume. The provisioned bandwidth limit can be set anywhere in the range of 125-1024 MBps (1000-8192 Mbps).

In the first-generation volume profiles, three predefined tiered profiles are available, or you can choose a custom profile. [Profiles in the tiered family](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provide pre-defined IOPS/GB performance for volumes up to 16,000 GB capacity. A [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile defines ranges of volume capacity and IOPS that you can select.

All of these profiles are backed by solid-state drives (SSDs).

## How volume bandwidth is allocated
{: #cp-storage-bandwidth-allocate}

The bandwidth that is available to the VSI is split between attached storage volumes and networking. The initial volume and network bandwidth allocation depends on the [instance profile](/docs/vpc?topic=vpc-profiles). The allocation of the instance's total bandwidth can be adjusted, balancing between network bandwidth and volume bandwidth. If you do not specify the initial volume and network bandwidth allocation, then 25% of total instance bandwidth is allocated to volume bandwidth and 75% is allocated to network bandwidth. For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

The provisioned volume bandwidth is the highest potential bandwidth that can be allocated to the volume when it is attached to an instance. In cases where the total maximum bandwidth of attached volumes exceeds the amount that is available on the instance, the bandwidth for each volume attachment is set proportionally. The bandwidth is allocated based on the corresponding volume's maximum bandwidth. For more information, see [Bandwidth allocation for Block Storage volumes](/docs/vpc?topic=vpc-block-storage-bandwidth).

## How I/O size affects performance
{: #how-block-size-affects-performance}

The IOPS metric shows how many read and/or write operations a storage device can perform per second. The IOPS value of a volume is based on a 16 KB block size with a 50-50 read/write random workload for all the volume profiles. Each 16 KB of data read/written counts as one read/write operation; a single write of less than 16 KB counts as a single write operation.

The provisioned volume throughput limit is determined by the number of IOPS multiplied by the preset throughput multiplier. The preset value is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiered profiles, or 256 KB for 10 IOPS/GB tiered or custom volume profiles. The higher the IOPS that you specify, the higher the throughput. Maximum throughput is 1024 MBps (8192 Mbps). For more information about the maximum throughput values, see [Block storage profile families](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#block-storage-profile-overview).

Your application's I/O size can be different than the throughput multiplier of the storage volume profile and it directly impacts how well your workload performs. If the application I/O size is smaller than 16 KB, the IOPS limit is reached before the throughput limit. Conversely, if the application I/O size is larger than the preset throughput multiplier, the throughput limit can be reached before the IOPS limit.

The following table provides some examples of how application I/O size and provisioned IOPS affect the throughput, which is calculated as shown.

* The average application I/O size x IOPS = Throughput

| Average I/O Size (KB) | IOPS | Throughput (MBps) |
|-----------------|------|-------------------|
| 4 (typical for Linux&reg;) | 1,000 | 4 |
| 8 (typical for Oracle) | 1,000  | 8 |
| 16 | 1,000 | 16 |
| 32 (typical for SQL Server) | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
{: caption="Examples of how application I/O size and IOPS affect the throughput" caption-side="top"}

In these examples, your performance caps are 1000 IOPS or 16 MBps throughput. You can achieve maximum IOPS when you use smaller I/O sizes, but throughput is less than what the volume can handle. 

The following example shows how throughput decreases for smaller average I/O sizes, when the IOPS values are the same.

* 16 KB * 6000 IOPS == ~94 MBps
* 8 KB * 6000 IOPS == ~47 MBps
* 4 KB * 6000 IOPS == ~23 MBps

If you want to improve the performance of your storage volume without changing the IO size, you can adjust the IOPS value. Increasing the IOPS value might require increasing the capacity of your volume, too. For more information, see [Adjusting IOPS of a block storage volume](/docs/vpc?topic=vpc-adjusting-volume-iops&interface=ui). You can't adjust the throughput limit of a first-generation volume directly.

Second-generation volumes: Understanding the relationship between the IO size, IOPS, and Throughput is especially important when you create a second-generation volume or adjust its IOPS and Throughput values. The IOPS value that you set is based on an assumed IO size of 16 KB. The preset throughput value is calculated by multiplying the specified IOPS value with the preset 16 KB IO size. If your application uses an IO size that is bigger than 16 KB, you might not be able to achieve the maximum IOPS value due to reaching the Throughput limit. In such cases, you can increase the Throughput value of the volume to get more IOPS. For more information, see [Adjusting the throughput limit of a Block Storage for VPC volume](/docs/vpc?topic=vpc-adjusting-volume-throughput)
