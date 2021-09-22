---

copyright:
  years: 2019 - 2021
lastupdated: "2021-08-30"

keywords: block storage, volume, data storage, volume capacity, classic, virtual server

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:important: .important}
{:note: .note}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Block storage capacity and performance
{: #capacity-performance}

Choosing the optimal block storage volume size and performance level for your workloads is important. When you provision {{site.data.keyword.block_storage_is_short}}, you can specify the size of your volume and performance level you require.
{: shortdesc}

## Capacity
{: #block-storage-vpc-capacity}

{{site.data.keyword.block_storage_is_short}} offers a range of storage capacities to meet your requirements.
Based on the [storage profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers). you chose, you can specify 10 - 16,000 GB of capacity per block storage data volume in 1 GB increments. This capacity depends on the [block storage IOPS profile](#iops-profiles) you're using. Boot volumes by default are 100 GB. If you provision an instance from a custom image, you can specify boot volume capacity up to 250 GB.

## Block storage IOPS profiles
{: #iops-profiles}

When you provision {{site.data.keyword.block_storage_is_short}} volumes, you specify an [IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles) that best meets your storage requirements. Three predefined tiered profiles are available, or you can choose a custom profile. [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provide guaranteed IOPS/GB performance for volumes up to 16,000 capacity. A [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile defines ranges of volume capacity and IOPS that you can select. These profiles are backed by solid-state drives (SSDs). 

## How volume bandwidth is allocated
{: #cp-storage-bandwidth-allocate}

Bandwidth is split between attached block storage volumes and networking. The initial volume and network bandwidth allocation depends on what you set by using the API or by the instance profile you selected.

The maximum volume bandwidth is the highest potential bandwidth that can be allocated to the volume when attached to an instance. In cases where the total maximum bandwidth of attached volumes exceeds the amount available on the instance, the bandwidth for each volume attachment is set proportionally, based on the corresponding volume's maximum bandwidth.

You can reallocate volume and network bandwidth when creating a new instance or for an existing instance. Bandwidth can't be shared between volumes and networking. The [instance profile](/docs/vpc?topic=vpc-profiles) you choose affects total bandwidth. For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

## How block size affects performance
{: #how-block-size-affects-performance}

IOPS is are based on either a 16 KB (for the 3 GB/ IOPS and 5 GB/IOPS tiers) or 256 KB block size (for the 10 GB/IOPS tier) with a 50-50 read/write random workload. Each 16 KB of data read/written counts as one read/write operation; a single write of less than 16 KB counts as a single write operation.

Baseline throughput is determined by the amount of IOPS multiplied by the 16 KB or 256 KB block size. The higher the IOPS you specify, the higher the throughput.

The block size that you choose for I/O from your application directly impacts storage performance. If the block size is smaller than the block size used by the profile to calculate the volume’s bandwidth limit, the IOPS limit is reached before the throughput limit. Conversely, if the block size is larger, the throughput limit is reached before the IOPS limit. 

The following table provides some examples of how block size and IOPS affect the throughput, calculated average I/O block size x IOPS = Throughput in Mbps.

| Block Size (KB) | IOPS | Throughput (Mbps) |
|-----------------|------|-------------------|
| 4 (typical for Linux&reg;) | 1,000 | 4 |
| 8 (typical for Oracle) | 1,000  | 8 |
| 16 | 1,000 | 16 |
| 32 (typical for SQL Server) | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
{: caption="Table 1. Examples of how block size and IOPS affect the throughput" caption-side="top"}

Maximum IOPS can still be obtained when you use smaller block sizes, but throughput is less. The following example shows how throughput decreases for smaller block sizes, when max IOPS is maintained.

* 16 KB * 6000 IOPS == ~93.75 Mbpsec
* 8 KB * 6000 IOPS == ~46.88 Mbpsec
* 4 KB * 6000 IOPS == ~23.44 Mbpsec

## Storage-compute performance metrics 
{: #storage-performance-metrics}

Network policies that control your block storage server and virtual server instance traffic are critical to ensuring optimal performance and avoiding bottlenecks. IBM Cloud VPC uses rate limiting at the hypervisor level to establish optimal bandwidth between the hypervisor and storage. You can expect IOPS, latency, and bandwidth performance within a predefined range that guarantees performance.

Table 2 describes baseline metrics you can expect for read and write operations between your compute instances and block storage volume. These are averages; your performance might be slightly higher or lower. Wide swings from this mean might indicate issues with your environment. The table shows random reads and writes at different block sizes. For related information on how block size affects performance for typical 16 K block volumes, see [How block size affects performance](#how-block-size-affects-performance).

| Block Size | IOPS | Latency | Bandwidth |
|------------|------|---------|-----------|
| Random read 4K | 2.5K | <.5 ms | 10 MBs |
| Random write 4K | 1.5K | < 1 ms | < 10 MBs |
| Random read 16K | 2.2K | < .5 ms| 35 MBs |
| Random write 16K | 1.2K | < 1 ms| 17 MBs |
| Random read 128K | 1.5K | < 1 ms | 162 MBs |
| Random write 128K | < 1K | < 1.5 ms | 87 MBs |
{: caption="Table 2. Performance metrics" caption-side="top"}
