---

copyright:
  years: 2019, 2023
lastupdated: "2023-01-25"

keywords: block storage profiles, Block Storage for VPC

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Block storage profiles
{: #block-storage-profiles}

When you provision {{site.data.keyword.block_storage_is_short}} data volumes by using the {{site.data.keyword.cloud_notm}} console, CLI, or API, you specify an IOPS profile that best meets your storage requirements. Profiles are available as three predefined IOPS tiers or as custom IOPS. IOPS tiers provide reliable IOPS/GB performance for volumes up to 16,000 GB capacity. You can also specify a custom IOPS profile and define volume capacity and IOPS within a range.
{: shortdesc}

IOPS values are based on either a 16 KB or 256 KB block size with a 50-50 read/write random workload. The 16-KB block size is used for the 3-GB/IOPS and 5-GB/IOPS tiers, the 256 KB block size is used for the 10-GB/IOPS tier and the custom profile. Each 16/256 KB of data that is read or written counts as one operation. A single write of less than 16/256 KB counts as a single write operation. Baseline throughput is determined by the amount of IOPS multiplied by the 16 KB block size. Maximum throughput is 1024 MBps. For more information, see [How block size affects performance](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

## IOPS tiers
{: #tiers}

Block storage data volumes have three predefined IOPS tiers that you can select when you create a volume. These profiles are backed by solid-state drives (SSDs).

Choose the profile that provides optimal performance for your compute workloads. Table 1 describes the IOPS performance that you can expect when create volumes in your availability zone.

Max IOPS for all tiers starts at 3,000 IOPS. Max IOPS then increases, based on the storage tier and volume size, up to the Max IOPS in Table 1.
{: note}

| IOPS Tier | Workload | Volume size | Max IOPS |
|-----------|----------|-------------|--------------|
| 3 IOPS/GB | General-purpose workloads - Workloads that host small databases for web applications or store virtual machine disk images for a hypervisor | 10 GB to 16,000 GB |  48,000 IOPS |
| 5 IOPS/GB | High I/O intensity workloads - Workloads characterized by a large percentage of active data, such as transactional and other performance-sensitive databases| 10 GB to 9,600 GB | 48,000 IOPS|
| 10 IOPS/GB | Demanding storage workloads - Data intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics. Bandwidth limits are calculated by using a 256 KB block size. | 10 GB to 4,800 GB | 48,000 IOPS |
{: caption="Table 1. IOPS tier profiles and performance levels for each tier" caption-side="top"}

## Custom IOPS profile
{: #custom}

Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier. You can customize the IOPS by specifying the total IOPS for the volume within the range for its volume size. You can provision volumes with IOPS performance from 100 IOPS to 48,000 IOPS, based on volume size. Custom IOPS profiles are backed by solid-state drives (SSDs).

Table 2 shows the available IOPS ranges based on volume size.

| Volume size (GB) | IOPS range |
|-------------|--------------|
| 10 -39   | 100 - 1,000 |
| 40 - 79 | 100 -2,000 |
| 80 - 99 | 100 - 4,000 |
| 100 - 499 | 100 - 6,000 |
| 500 - 999 | 100 - 10,000 |
| 1,000 - 1,999 | 100 - 20,000 |
| 2000 - 3999 GB |200	- 40000 |
| 4000 - 7999 GB |300	- 40000 |
| 8000 - 9999 GB |500	- 48000 |
| 10,000 - 16,000 GB | 1000	- 48000 |
{: caption="Table 2. Available IOPS based on volume size" caption-side="top"}

## Profiles and boot volumes
{: #vsi-profiles-boot}

Boot volumes are created based on a general-purpose IOPS profile with 100 GB capacity. [Boot volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes) can be increased during instance provisioning or later, by directly modifying the boot volume, up to 250 GB.

## How virtual server profiles relate to storage profiles
{: #vsi-profiles-relate-to-storage}

Virtual server profiles are a combination of vCPU and RAM that can be instantiated quickly to start a virtual server instance. Select from [three families of profiles](/docs/vpc?topic=vpc-profiles#profiles) based on your workload requirements. These requirements can range from common workloads to CPU-intensive or memory-intensive workloads.

Similarly, storage profiles (IOPS tiers or custom) provide a range of capacity and performance for secondary volumes. By default, a 100 GB primary boot volume is created when you create a virtual server instance. You can also create and attach secondary volumes. When you create a secondary data volume as part of instance creation, you select a storage profile that best meets your storage requirements for your compute workloads. In general, as your compute requirements increase, you need higher IOPS performance. Table 3 shows this relationship.

| IOPS tier storage profile | Virtual server profile |
|-----------------|------------------------|
| 3 IOPS/GB       | [Balanced](/docs/vpc?topic=vpc-profiles#balanced) for common workloads |
| 5 IOPS/GB       | [Compute](/docs/vpc?topic=vpc-profiles#compute) for intensive CPU demands |
| 10 IOPS/GB      | [Memory](/docs/vpc?topic=vpc-profiles#memory) for memory-intensive workloads |
{: caption="Table 3. Relationship of block storage profiles to virtual server profiles" caption-side="top"}

## Viewing IOPS profiles
{: #view-iops-profiles}

You can view available IOPS profiles the {{site.data.keyword.cloud_notm}} console, CLI, or API.

### Using the IBM Cloud console
{: #using-console-iops-profile}
{: ui}

When you [create a block storage volume from the {{site.data.keyword.cloud_notm}} console](/docs/vpc?topic=vpc-creating-block-storage), select **Tiers**.

Alternately, select **Custom** and then select an IOPS value within the range for that volume size. Click the storage size link to see a table of size and IOPS ranges.

### Using the CLI
{: #using-cli-iops-profiles}
{: cli}

To view the list of available profiles by using the CLI, run the following command:
```sh
ibmcloud is volume-profiles
```
{: codeblock}

### Using the API
{: #using-api-iops-profiles}
{: api}

The following cURL API request retrieves all volume profiles.

```curl
curl -X GET \
$vpc_api_endpoint/v1/volume/profiles?$api_version&generation=2 \
-H "Authorization: $iam_token"
```
{: codeblock}

## Next Steps
{: #profile-next-steps}

To expand volume capacity, see [expanding block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

To [change the IOPS tier or Custom IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) for an existing volume that is attached to a virtual server instance.

For more information about Balanced, Compute, and Memory profiles for {{site.data.keyword.vsi_is_short}}, see [Profiles](/docs/vpc?topic=vpc-profiles).
