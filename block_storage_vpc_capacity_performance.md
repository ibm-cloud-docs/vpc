---

copyright:
  years: 2019, 2026
lastupdated: "2026-10-12"

keywords: Block Storage, virtual private cloud, IOPS, throughput, capacity, SSD, volume performance, sdp profile, storage profile, bandwidth, I/O size, tiered profile, custom profile

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Block Storage capacity and performance
{: #capacity-performance}

Choose the optimal {{site.data.keyword.block_storage_is_short}} volume capacity and performance for your workload. Customize capacity, IOPS, and throughput with the `sdp` profile, or select from predefined IOPS tiers and custom profiles.
{: shortdesc}


The `sdp` profile is available to select customers. To request access, submit an [allowlisting request](https://forms.monday.com/forms/6f855ea28400d75ef31e540e39c1d31a?r=use1&SDSallowlist=){: external}.
{: preview}


## Capacity
{: #block-storage-vpc-capacity}

{{site.data.keyword.block_storage_is_short}} offers a range of storage capacities to meet your requirements.

When you choose the second-generation `sdp` profile, you can provision second-generation data volumes up to 32,000 GB.

When you select a first-generation [storage profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers), you can specify 10-16,000 GB of capacity in 1 GB increments.

Boot volumes are 100 GB by default. You can install some Linux stock images on boot volumes as small as 10 GB, while Windows stock images require 40 GB or more. If you provision an instance with a custom image or a snapshot, you can specify a boot volume capacity in the range of 10 - 250 GB when you're using first-generation volume profiles, and in the range of 10 - 32,000 GB when you're using the `sdp` profile.

## Block Storage volume profiles
{: #iops-profiles}

When you provision {{site.data.keyword.block_storage_is_short}} volumes, you specify a [volume profile](/docs/vpc?topic=vpc-block-storage-profiles) that best meets your storage requirements.

You can provision volumes with the [SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-profiles#defined-performance-profile) with custom IOPS in the range of 3000 - 64,000 IOPS. An IOPS level of over 48,000 can be achieved when the new volume is attached to a virtual server instance with a [3rd generation instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). In addition to custom capacity and IOPS, you can also specify a custom throughput limit for a second-generation volume. The provisioned bandwidth limit can be set anywhere in the range of 125-1024 MBps (1000-8192 Mbps).

In the first-generation volume profiles, three predefined tiered profiles are available, or you can choose a custom profile. [Profiles in the tiered family](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provide predefined IOPS per GB performance for volumes up to 16,000 GB capacity. A [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile defines ranges of volume capacity and IOPS that you can select.

All of these profiles are backed by solid-state drives (SSDs).

## How volume bandwidth is allocated
{: #cp-storage-bandwidth-allocate}

The bandwidth that is available to the virtual server instance is split between attached storage volumes and networking. The initial volume and network bandwidth allocation depends on the [instance profile](/docs/vpc?topic=vpc-profiles). The allocation of the instance's total bandwidth can be adjusted, balancing between network bandwidth and volume bandwidth. If you do not specify the initial volume and network bandwidth allocation, then 25% of total instance bandwidth is allocated to volume bandwidth and 75% is allocated to network bandwidth. For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

The provisioned volume bandwidth is the highest potential bandwidth that can be allocated to the volume when it is attached to an instance. In cases where the total maximum bandwidth of attached volumes exceeds the amount that is available on the instance, the bandwidth for each volume attachment is set proportionally. The bandwidth is allocated based on the corresponding volume's maximum bandwidth. For more information, see [Bandwidth allocation for Block Storage volumes](/docs/vpc?topic=vpc-block-storage-bandwidth).

## How I/O size affects performance
{: #how-block-size-affects-performance}

The IOPS metric shows the number of read and write operations that a storage device can perform per second. The IOPS value of a volume is based on a 16 KB block size with a random workload of 50% reads and 50% writes for all the volume profiles. Each 16 KB of data that is read or written counts as one operation. A single write of less than 16 KB also counts as one operation.

The provisioned volume throughput limit is determined by the number of IOPS multiplied by the preset throughput multiplier. The preset value is 16 KB for the 3 and 5 IOPS per GB tiered profiles, and 256 KB for the 10 IOPS per GB tiered and custom profiles. The higher the IOPS that you specify, the higher the throughput. Maximum throughput is 1024 MBps (8192 Mbps). For more information about the maximum throughput values, see [Block storage profile families](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#block-storage-profile-overview).

Your application's I/O size can be different from the throughput multiplier of the storage volume profile, and it directly affects how well your workload performs. If the application I/O size is smaller than 16 KB, the IOPS limit is reached before the throughput limit. Conversely, if the application I/O size is larger than the preset throughput multiplier, the throughput limit can be reached before the IOPS limit.

The following table provides some examples of how application I/O size and provisioned IOPS affect the throughput, which is calculated as shown.

* The average application I/O size x IOPS = Throughput

| Average I/O size (KB) | IOPS | Throughput (MBps) |
|-----------------|------|-------------------|
| 4 (typical for Linux&reg;) | 1,000 | 4 |
| 8 (typical for Oracle) | 1,000  | 8 |
| 16 | 1,000 | 16 |
| 32 (typical for SQL Server) | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
{: caption="Examples of how application I/O size and IOPS affect the throughput" caption-side="bottom"}

In these examples, your performance caps are 1000 IOPS or 16 MBps throughput. You can achieve maximum IOPS when you use smaller I/O sizes, but throughput is less than what the volume can handle.

The following example shows how throughput decreases for smaller average I/O sizes, when the IOPS values are the same.

* 16 KB * 6000 IOPS == ~94 MBps
* 8 KB * 6000 IOPS == ~47 MBps
* 4 KB * 6000 IOPS == ~23 MBps

If you want to improve the performance of your storage volume without changing the I/O size, you can adjust the IOPS value. Increasing the IOPS value might require increasing the capacity of your volume, too. For more information, see [Adjusting IOPS of a block storage volume](/docs/vpc?topic=vpc-adjusting-volume-iops&interface=ui). You can't adjust the throughput limit of a first-generation volume directly.

When you create a second-generation volume or adjust its IOPS and throughput values, it is important to understand how I/O size, IOPS, and throughput relate to each other. The IOPS value that you set is based on an assumed I/O size of 16 KB. The preset throughput value is calculated by multiplying the specified IOPS value by the preset 16 KB I/O size. If your application uses an I/O size that is larger than 16 KB, you might not be able to achieve the maximum IOPS value because the throughput limit is reached first. In such cases, you can increase the throughput value of the volume to get more IOPS. For more information, see [Adjusting the throughput limit of a Block Storage for VPC volume](/docs/vpc?topic=vpc-adjusting-volume-throughput).

## How concurrency and queue depth affect performance
{: #concurrency-queue-depth}

Provisioned IOPS and throughput values represent the maximum capability of your {{site.data.keyword.block_storage_is_short}} volume, rather than guaranteed performance for every workload. Achieving these maximum values requires that the storage pipeline is kept saturated with sufficient outstanding I/O requests from the application and operating system layers.

Single-threaded workloads and workloads with limited concurrency often achieve less throughput than the provisioned maximum because they cannot generate enough concurrent requests to saturate the storage pipeline. Understanding the roles of queue depth and concurrency helps explain observed performance and how to tune workloads.

### Key concepts
{: #concurrency-queue-depth-concepts}

Queue depth (also called I/O queue depth or outstanding I/O)
:   The number of I/O requests that a single I/O path or thread submits to the storage subsystem and that are in flight simultaneously (issued but not yet acknowledged). The operating system, host bus adapter (HBA), or device drivers maintain a queue. The queue depth setting controls how many operations can be outstanding in that queue before the issuer must wait. Queue depth can be configured at multiple layers of the guest OS and within application benchmark tools (such as the `fio --iodepth` parameter).

Concurrency
:   The number of independent, parallel execution contexts (threads, processes, or application workers) that generate separate I/O streams simultaneously. Each concurrent worker might have its own queue depth (for example, a queue depth of 1 for synchronous I/O or a deeper queue for asynchronous I/O).

### Relationship between concurrency, queue depth, and throughput
{: #concurrency-queue-depth-relationship}

To drive throughput toward the provisioned ceiling, the workload must generate sufficient total outstanding I/O:

* Total outstanding I/O = Concurrency * Queue depth per stream

For example, a single application writer that issues synchronous I/O with a queue depth of 1 operates in a serial pattern (issue request, wait for acknowledgment, issue next request). Even with large I/O sizes, a single synchronous stream cannot saturate the storage volume's full provisioned bandwidth. To increase throughput toward the volume's limit, you can:

- Increase **concurrency** by using multi-threaded applications, parallel processes, or multiple worker streams.
- Increase **queue depth** by using asynchronous I/O and configuring guest OS or application queue depth parameters.

Combining parallel writers with appropriate queue depth allows the workload to achieve aggregate throughput closer to the volume's maximum provisioned limit.
