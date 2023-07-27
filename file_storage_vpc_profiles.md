---

copyright:
  years: 2021, 2023
lastupdated: "2023-07-27"

keywords: file storage, file share, performance, IOPS, block size, capacity, range

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.filestorage_vpc_short}} profiles
{: #file-storage-profiles}

When you provision {{site.data.keyword.filestorage_vpc_short}} file shares by using the {{site.data.keyword.cloud_notm}} console, CLI, or API, you specify a file share profile. The profile defines the performance level that you require based on the file share size.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Madrid, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## File storage profiles overview
{: #file-storage-profile-overview}

When you [create a file share](/docs/vpc?topic=vpc-file-storage-create) in your availability zone, you select share size and IOPS performance based on a file storage profile. New file shares are created based on the high performance, [dp2](#dp2-profile) profile. 

Earlier file shares that were created by using either the [IOPS tier](#fs-tiers) profiles or [custom IOPS](#fs-custom) profile can continue to operate based on these profiles. You can also update these file shares to use the dp2 profile or switch back to a previous profile. However, you cannot use the previous profiles when you create a new file share.

Table 1 shows the dp2 profile performance levels compared to the earlier profiles. [Table 2](#dp2-profile) presents the IOPS and capacity ranges for the dp2 profile. For more information about the earlier IOPS tiers and custom profiles, see [previous version file storage profiles](#fs-v2-profiles).

Table 1 shows the dp2 profile performance levels compared to the earlier profiles.

| Family   | Profile         | IOPS&sup1;    | IOPS per share | Max throughput&sup2;| Share size   |
|----------|-----------------|--------------:|---------------:|---------------------|-------------:|
| `defined_performance`|`dp2`| 1-100 IOPS/GB |     100-96,000 |           1024 MB/s | 10-32,000 GB | 
| `tiered` | `tier-3iops`    |     3 IOPS/GB |   3,000-96,000 |            670 MB/s | 10-32,000 GB | 
| `tiered` | `tier-5iops`    |     5 IOPS/GB |   3,000-48,000 |            768 MB/s |  10-9,600 GB | 
| `tiered` | `tier-10iops`   |    10 IOPS/GB |   3,000-48,000 |           1024 MB/s |  10-4,800 GB | 
| `custom` | `custom`        | 1-100 IOPS/GB |   3,000-48,000 |           1024 MB/s | 10-16,000 GB | 
{: caption="Table 1. Comparison of file share profiles and performance levels." caption-side="top"}

&sup1; IOPS values are based on 16k IO size.

&sup2; Baseline throughput is determined by the number of IOPS multiplied by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB or custom IOPS tiers. The higher the IOPS that you specify, the higher the throughput. Maximum throughput is 1024 MBps.

## Defined performance profile
{: #dp2-profile}

By using the dp2 profile, you can specify the total IOPS for the file share within the range for a specific file share size, 10 GB (default minimum) to 32,000 GB. You can provision shares with IOPS performance from 100 IOPS (default minimum) to 96,000 IOPS, based on share size. The dp2 profile is based on a block size of 256 KB. Maximum throughput is 1024 MB/s. This profile is backed by solid-state drives (SSDs).

Table 2 shows the available IOPS ranges, based on share size.

| Share size (GB) | IOPS range (IOPS) |
|-----------------|-------------------|
| 10-39 | 100-1,000 |
| 40-79 | 100-2,000 |
| 80-99 | 100-4,000 |
| 100-499 | 100-6,000 |
| 500-999 | 100-10,000 |
| 1,000-1,999 | 100-20,000 | 
| 2,000-3,999  | 200-40,000 |
| 4,000-7,999 | 300-40,000 |
| 8,000-15,999  | 500-64,000 | 
| 16,000-32,000 | 2,000-96,000&sup1; |
{: caption="Table 2. dp2 file share profile IOPS and capacity ranges" caption-side="bottom"}

&sup1;For the 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances.

## Previous version file storage profiles
{: #fs-v2-profiles}

### IOPS tiers
{: #fs-tiers}

Existing file shares can be based on IOPS tiers that you selected when you created the file share. Table 3 describes the IOPS performance for the IOPS tier profile.

| IOPS Tier | Workload | Share size (GB) | Max IOPS (IOPS) |
|-----------|----------|-----------------|-----------------|
| 3 IOPS/GB | General-purpose workloads | 10-32,000 | 96,000&sup1; |
| 5 IOPS/GB | High I/O intensity workloads | 10-9,600 | 48,000 |
| 10 IOPS/GB | Demanding storage workloads | 10-4,800 | 48,000 |
{: caption="Table 3. IOPS tier profiles and performance levels for each tier" caption-side="bottom"}

&sup1;For the 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances.

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS. The total maximum IOPS is rounded up to the next multiple of 100 for IOPS calculations that result in IOPS greater than 48,000 IOPS up to 96,000 IOPS.
{: note}

### Custom IOPS profile
{: #fs-custom}

Custom IOPS profiles specify the total IOPS for the file share within the range for its size. File shares that use a custom IOPS profile can have IOPS performance level in the range 100-48000 IOPS.

Table 4 shows the available IOPS ranges based on file share size.

| File Share size (GB) | IOPS range (IOPS) |
|----------------------|-------------------|
| 10 - 39 | 100 - 1,000 |
| 40 - 79 | 100 - 2,000 |
| 80 - 99 | 100 - 4,000 |
| 100 - 499 | 100 - 6,000 |
| 500 - 999 | 100 - 10,000 |
| 1,000 - 1,999 | 100 - 20,000 |
| 2,000 - 3,999 | 200 - 40,000 |
| 4,000 - 7,999 | 300 - 40,000 |
| 8,000 - 9,999 | 500 - 48,000 |
| 10,000-16,000 | 1000 - 48,000 |
{: caption="Table 4. Available IOPS based on file share size." caption-side="bottom"}

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS.
{: note}

## View profiles in the UI
{: #fs-using-ui-iops-profile}
{: ui}

When you [create a file share in the UI](/docs/vpc?topic=vpc-file-storage-create&interface=ui#file-storage-create-ui), under **Profiles**, select a profile.

## View profiles from the CLI
{: #fs-using-cli-iops-profiles}
{: cli}

To view the list of available profiles from the CLI, run the following command.

```sh
ibmcloud is share-profiles
```
{: pre}

The command returns the following profiles:

```text
ibmcloud is share-profiles
Listing file share profiles in region us-south under account VPC1 as user user@mycompany.com...
Name          Family
custom-iops   custom
dp2           defined-performance
tier-10iops   tiered
tier-3iops    tiered
tier-5iops    tiered
```
{: screen}

## View profiles with the API
{: #fs-using-api-iops-profiles}
{: api}

Use the `GET /share/profiles` request to retrieve information for all share profiles.

As described in the [Beta VPC API](/apidocs/vpc-beta) reference [versioning](/apidocs/vpc-beta#api-versioning-beta) policy, support for older versions of the beta API is limited to 45 days. Therefore, beta API requests must specify a `version` query parameter date value within the last 45 days. You must also provide `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc#api-generation-parameter).
{: requirement}

```sh
curl -X GET \
$vpc_api_endpoint/v1/share/profiles?$api_version&generation=2&maturity=beta\
-H "Authorization: $iam_token"
```
{: pre}

The response returns the following profiles:

```json
{
"profiles": [
    {
      "family": "custom",
      "href": "$vpc_api_endpoint/v1/share/profiles/custom-iops",
      "name": "custom-iops",
      "resource_type": "share_profile"
    },
    {
      "family": "defined_performance",
      "href": "$vpc_api_endpoint/v1/share/profiles/dp2",
      "name": "dp2",
      "resource_type": "share_profile"
    },
    {
      "family": "tiered",
      "href": "$vpc_api_endpoint/v1/share/profiles/tier-10iops",
      "name": "tier-10iops",
      "resource_type": "share_profile"
    },
    {
      "family": "tiered",
      "href": "$vpc_api_endpoint/v1/share/profiles/tier-3iops",
      "name": "tier-3iops",
      "resource_type": "share_profile"
    },
    {
      "family": "tiered",
      "href": "$vpc_api_endpoint/v1/share/profiles/tier-5iops",
      "name": "tier-5iops",
      "resource_type": "share_profile"
    }
  ]
}
```
{: codeblock}

## How block size affects file share performance
{: #fs-profiles-block-size}

IOPS is based on a 16 KB block size for all profiles, with a 50-50 read/write random workload. Each 16 KB of data that is read or written counts as one read/write operation. A single write of less than 16 KB counts as a single write operation.

Maximum throughput for a file share is calculated by taking the file share's IOPS and multiplying it by the block size given for the file share's profile in [Table 1](#file-storage-profile-overview). The resulting throughput is subject to the limit of Max throughput specified in Table 1.

The block size that you choose for I/O from your application directly impacts storage performance. If the block size is smaller than the block size used by the profile to calculate the share’s bandwidth limit, the IOPS limit is reached before the throughput limit. Conversely, if the block size is larger, the throughput limit is reached before the IOPS limit.

Table 5 provides some examples of how block size and IOPS affect the throughput, calculated average I/O block size x IOPS = Throughput in MBps.

| Block Size (KB) | IOPS | Throughput (MBps) |
|-----------------|------|-------------------|
| 4 | 1000 | 4&sup1;|
| 8 | 1000 | 8&sup1;|
| 16 | 1000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Table 5. How block size and IOPS affect throughput" caption-side="bottom"}

&sup1;If your cap is 1000 IOPS or 16 KB block size, the throughput caps at whatever limit is reached first.

Maximum IOPS can still be obtained when you use smaller block sizes, but throughput is less. The following example shows how throughput decreases for smaller block sizes, when max IOPS is maintained.

* 16 KB * 6000 IOPS == ~94 MBps
* 8 KB * 6000 IOPS == ~47 MBps
* 4 KB * 6000 IOPS == ~23 MBps

## Next steps
{: #fs-next-steps}

[Start creating file shares](/docs/vpc?topic=vpc-file-storage-create).
