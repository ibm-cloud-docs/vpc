---

copyright:
years: 2021
lastupdated: "2021-09-23"

keywords: file storage, virtual private cloud, shares, profile

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:preview: .preview}
{:table: .aria-labeledby="caption"}
{:note: .note}

# File storage profiles
{: #file-storage-profiles}

When you provision File Storage for VPC shares by using the {{site.data.keyword.cloud_notm}} console, CLI, or API, you specify a profile that best meets your file storage requirements. Profiles are available as three predefined IOPS tiers or a custom IOPS you can tailor to your needs.
{: shortdesc}

File Storage for VPC is available to customers with special approval to preview this service in the Washington, Dallas, and Frankfurt regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

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

## Custom IOPS profile
{: #custom}
Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier. You can customize the IOPS by specifying the total IOPS for the file share within the range for its size. You can provision shares with IOPS performance from 100 IOPS to 48,000 IOPS, based on the size.

Table 2 shows the available IOPS ranges based on file share size.

| File Share size (GB) | IOPS range |
|-------------|--------------|
| 10 - 39   | 100 - 1,000 |
| 40 - 79 | 100 -2,000 |
| 80 - 99 | 100 - 4,000 |
| 100 - 499 | 100 - 6,000 |
| 500 - 999 | 100 - 10,000 |
| 1,000 - 1,999 | 100 - 20,000 |
| 2000 - 3,999 | 200 - 40,000 |
| 4000 - 7,999 | 300 - 40,000 |
| 8000 - 9,999 | 500 - 48,000 |
| 10,000 - 16,000 | 1,000 - 48,000 |
{: caption="Table 2. Available IOPS based on file share size" caption-side="top"}

### Profile Block size
{: #fs-profiles-block-size}

The IOPS value for File Storage for VPC profiles is based on a 16-KB block size with a 50/50 read/write 50/50 random/sequential workload. A 16-KB block is the equivalent of one write to the storage system.

The block size used by your application directly impacts the storage performance. If the block size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the block size that is used by your application is larger than 16 KB, the throughput limit is realized before to the IOPS limit.

Table 1 shows examples of how block size affects throughput, calculated as Average I/O size x IOPS = Throughput in Mbps.

| Block Size (KB) | IOPS | Throughput (Mbps) |
|-----|-----|-----|
| 4 | 1,000 | 16 |
| 8 | 1,000 | 16 |
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
{: pre}

### Using the API
{: #fs-using-api-iops-profiles}

Use the `GET /share/profiles` request to retrieve information for all share profiles.

```
curl -X GET \
$vpc_api_endpoint/v1/share/profiles?$api_version&generation=2 \
-H "Authorization: $iam_token"
```
{: codeblock}

## Next steps
{: #fs-next-steps}

[Start creating file shares](/docs/vpc?topic=vpc-file-storage-create).
