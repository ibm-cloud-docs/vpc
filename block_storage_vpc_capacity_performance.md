---

copyright:
  years: 2019, 2020
lastupdated: "2020-11-09"

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
{:shortdesc}

## Capacity
{: #block-storage-vpc-capacity}

{{site.data.keyword.block_storage_is_short}} offers a range of storage capacities to meet your requirements.
You can specify 10 - 2000 GB (2 TB) of capacity per block storage data volume in 1 GB increments. This capacity depends on the [block storage IOPS profile](#iops-profiles) you're using. Boot volumes are always 100 GB.

Data volumes are also available with capacities greater than 2000 GB. This is a beta feature that is available for evaluation and testing purposes. For information, see [Expanded capacity IOPS tiers (Beta)](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta).
{:beta}

## Block storage IOPS profiles
{: #iops-profiles}

When you provision {{site.data.keyword.block_storage_is_short}} volumes, you specify an [IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles) that best meets your storage requirements. Three predefined tiered profiles are available, or you can choose a custom profile. [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provide guaranteed IOPS/GB performance for volumes up to 2 TB capacity. A [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile defines ranges of volume capacity and IOPS that you can select. These profiles are backed by solid-state drives (SSDs). 

## How block size affects performance
{: #how-block-size-affects-performance}

IOPS is based on a 16 KB block size with a 50-50 read/write random workload. Each 16 KB of data read/written counts as one read/write operation; a single write of less than 16 KB counts as a single write operation.

Baseline throughput is determined by the amount of IOPS multiplied by the 16 KB block size. The higher the IOPS you specify, the higher the throughput. Maximum throughput for a block storage volume is 750 MB/s.

The block size that you choose for your application directly impacts storage performance. If the block size is smaller than 16 KB, the IOPS limit is reached before the throughput limit. Conversely, if the block size is larger than 16 KB, the throughput limit is reached before the IOPS limit. The following table provides some examples of how block size and IOPS affect the throughput, calculated average I/O block size x IOPS = Throughput in MB/s.

| Block Size (KB) | IOPS | Throughput (MB/s) |
|-----------------|------|-------------------|
| 4 (typical for Linux&reg;) | 1,000 | 4 |
| 8 (typical for Oracle) | 1,000  | 8 |
| 16 | 1,000 | 16 |
| 32 (typical for SQL Server) | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
{: caption="Table 1. Examples of how block size and IOPS affect the throughput" caption-side="top"}

Maximum IOPS can still be obtained when you use smaller block sizes, but throughput is less. The following example shows how throughput decreases for smaller block sizes, when max IOPS is maintained.

* 16 KB * 6000 IOPS == ~93.75 MB/sec
* 8 KB * 6000 IOPS == ~46.88 MB/sec
* 4 KB * 6000 IOPS == ~23.44 MB/sec

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
