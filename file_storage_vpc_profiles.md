---

copyright:
years: 2021, 2022
lastupdated: "2022-05-26"

keywords:

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

# File Storage for VPC profiles
{: #file-storage-profiles}

When you provision File Storage for VPC file shares by using the {{site.data.keyword.cloud_notm}} console, CLI, or API, you specify a profile that best meets your file storage requirements. Profiles are available as three predefined IOPS tiers or a custom IOPS you can tailor to your needs.
{: shortdesc}

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, London, Sydney, Sao Paulo, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## IOPS tiers
{: #fs-tiers}

File file shares are based on IOPS tiers you can select when creating a file share. Choose the profile that provides optimal performance for your compute workloads. Table 1 describes the IOPS performance you can expect for file shares you create in your zone.

| IOPS Tier | Workload | Share size | Max IOPS |
|-----------|----------|-------------|--------------|
| 3 IOPS/GB | General-purpose workloads - Workloads that host small databases for web applications or store virtual machine disk images for a hypervisor | 10 GB to 32,000 GB | 48,000/96,000&sup1; IOPS |
| 5 IOPS/GB | High I/O intensity workloads - Workloads characterized by a large percentage of active data, such as transactional and other performance-sensitive databases| 10 GB to 9,600 GB | 48,000 IOPS|
| 10 IOPS/GB | Demanding storage workloads - Data intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics | 10 GB to 4,800 GB | 48,000 IOPS |
{: caption="Table 1. IOPS tier profiles and performance levels for each tier" caption-side="bottom"}

&sup1; For 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share/instance is limited to 48,000 IOPS.

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS. The total maximum IOPS is rounded up to the next multiple of 100 for IOPS calculations results in IOPS greater than 48,000 IOPS up to 96,000 IOPS.
{: note}

## Custom IOPS profile
{: #custom}

Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier. You can customize the IOPS by specifying the total IOPS for the file share within the range for its size. You can provision file shares with IOPS performance from 100 IOPS to 48,000 IOPS, based on the size.

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
{: caption="Table 2. Available IOPS based on file share size." caption-side="bottom"}

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS.
{: note}

### How block size affects file share performance
{: #fs-profiles-block-size}

IOPS are based on either a 16 KB for all profiles, with a 50-50 read/write random workload. Each 16 KB of data read/written counts as one read/write operation; a single write of less than 16 KB counts as a single write operation.

Baseline throughput is determined by the amount of IOPS multiplied by the 16 KB block size. The higher the IOPS you specify, the higher the throughput. Maximum throughput is 1024 Mbps.

The block size that you choose for I/O from your application directly impacts storage performance. If the block size is smaller than the block size used by the profile to calculate the share’s bandwidth limit, the IOPS limit is reached before the throughput limit. Conversely, if the block size is larger, the throughput limit is reached before the IOPS limit. 

Table 1 provides some examples of how block size and IOPS affect the throughput, calculated average I/O block size x IOPS = Throughput in Mbps.

| Block Size (KB) | IOPS | Throughput (Mbps) |
|-----|-----|-----|
| 4 | 1,000 | 4&sup1;|
| 8 | 1,000 | 8&sup1;|
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Table 1. How block size and IOPS affect throughput." caption-side="bottom"}

&sup1;If you cap is 1000 IOPS or 16K block size, throughput will cap at whatever limit is reached first.

Maximum IOPS can still be obtained when you use smaller block sizes, but throughput is less. The following example shows how throughput decreases for smaller block sizes, when max IOPS is maintained.

* 16 KB * 6000 IOPS == ~94 Mbpsec
* 8 KB * 6000 IOPS == ~47 Mbpsec
* 4 KB * 6000 IOPS == ~23 Mbpsec

## View IOPS profiles
{: #fs-view-iops-profiles}

You can view available IOPS profiles the {{site.data.keyword.cloud_notm}} console, CLI, or API.

### Using the UI
{: #fs-using-ui-iops-profile}

When you create a file share in the UI, under **Profiles**, select an **IOPS Tiers**.

### Using the CLI
{: #fs-using-cli-iops-profiles}

To view the list of available profiles from the CLI, run the following command:

```zsh
ibmcloud is share-profiles
```
{: pre}

### Using the API
{: #fs-using-api-iops-profiles}

Use the `GET /share/profiles` request to retrieve information for all share profiles.

```curl
curl -X GET \
$vpc_api_endpoint/v1/share/profiles?$api_version&generation=2 \
-H "Authorization: $iam_token"
```
{: codeblock}

## Next steps
{: #fs-next-steps}

[Start creating file shares](/docs/vpc?topic=vpc-file-storage-create).
