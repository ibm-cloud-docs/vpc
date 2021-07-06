---

copyright:
years: 2021
lastupdated: "2021-06-29"

keywords: file storage, virtual private cloud, shares, profile

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# File storage profiles
{: #file-storage-profiles}

When you provision File Storage for VPC shares by using the {{site.data.keyword.cloud_notm}} console, CLI, or API, you specify a profile that best meets your storage requirements. Profiles are available as three predefined IOPS tiers.
{:shortdesc}

File Storage for VPC is available to customers with special approval to preview this service in the Washington, Dallas, and Frankfurt regions. Contact your IBM Sales representative if you are interested in getting access.
{:note}

## IOPS tiers
{: #fs-tiers}

File shares are based on IOPS tiers you can select when creating a share. Choose the profile that provides optimal performance for your compute workloads. Table 1 describes the IOPS performance you can expect for shares you create in your zone.

| IOPS Tier | Workload | Share size | Max IOPS |
|-----------|----------|-------------|--------------|
| 3 IOPS/GB | General-purpose workloads - Workloads that host small databases for web applications or store virtual machine disk images for a hypervisor | 10 GB to 32,000 GB | 48,000/96,000<sup>1</sup> IOPS |
| 5 IOPS/GB | High I/O intensity workloads - Workloads characterized by a large percentage of active data, such as transactional and other performance-sensitive databases| 10 GB to 9,600 GB | 48,000 IOPS|
| 10 IOPS/GB | Demanding storage workloads - Data intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics | 10 GB to 4,800 GB | 48,000 IOPS |
{: caption="Table 1. IOPS tier profiles and performance levels for each tier" caption-side="top"}

<sup>1</sup>For 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share/instance is limited to 48,000 IOPS.

### Profile Block size
{: #fs-profiles-block-size}

The IOPS value for File Storage for VPC profiles is based on a 16-KB block size with a 50/50 read/write 50/50 random/sequential workload. A 16-KB block is the equivalent of one write to the storage system.

The block size used by your application directly impacts the storage performance. If the block size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the block size that is used by your application is larger than 16 KB, the throughput limit is realized before to the IOPS limit.

Table 1 shows examples of how block size affects throughput, calculated as Average I/O size x IOPS = Throughput in MB/s.

| Block Size (KB) | IOPS | Throughput (MB/s) |
|-----|-----|-----|
| 4 | 1,000 | 4 |
| 8 | 1,000 | 8 |
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Table 1. How block size and IOPS affects throughput" caption-side="top"}

## Viewing IOPS profiles
{: #fs-view-iops-profiles}

You can view available IOPS profiles the {{site.data.keyword.cloud_notm}} console, CLI, or API.

### Using the UI
{: #fs-using-ui-iops-profile}

When you create a share using the UI, under **Profiles**, select an **IOPS Tiers**.

### Using the CLI
{: #fs-using-cli-iops-profiles}

To view the list of available profiles by using the CLI, run the following command:

```
ibmcloud is share-profiles
```
{:pre}

### Using the API
{: #fs-using-api-iops-profiles}

Use the `GET /share/profiles` request to retrieve information for all share profiles.

```
curl -X GET \
$vpc_api_endpoint/v1/share/profiles?$api_version&generation=2 \
-H "Authorization: $iam_token"
```
{:codeblock}

## Next steps
{: #fs-next-steps}

[Start creating file shares](/docs/vpc?topic=vpc-file-storage-create).
