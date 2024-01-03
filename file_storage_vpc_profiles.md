---

copyright:
  years: 2021, 2024
lastupdated: "2023-12-18"

keywords: file storage, file share, performance, IOPS, block size, capacity, range

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.filestorage_vpc_short}} profiles
{: #file-storage-profiles}

When you provision {{site.data.keyword.filestorage_vpc_short}} file shares by using the {{site.data.keyword.cloud_notm}} console, CLI, or API, you specify capacity and performance within a file share profile. Available performance levels vary based on the file share size. All file shares are backed by solid-state drives (SSDs).
{: shortdesc}

## File Storage profiles overview
{: #file-storage-profile-overview}

When you [create a file share](/docs/vpc?topic=vpc-file-storage-create), you select the share size and IOPS performance that is available, based on a file storage profile. All new file shares are created based on the high-performance [dp2](#dp2-profile) profile. 

File shares that were created during the beta and limited availability phases with either the [IOPS tier](#fs-tiers) profiles or [custom IOPS](#fs-custom) profiles can continue to operate based on these profiles. You can also update these file shares to use the `dp2` profile or switch to another previous generation profile. However, you cannot use the previous profiles when you create a file share, and only the file shares with the `dp2` profile can use the new security features.

Table 1 shows the dp2 profile performance levels compared to the earlier profiles.

| Family   | Profile         | IOPS^1^    | IOPS per share | Max throughput^2^ | Share size   |
|----------|-----------------|--------------:|---------------:|---------------------|-------------:|
| `defined_performance`|`dp2`| 1-100 IOPS/GB |     100-96,000 |           1024 MB/s | 10-32,000 GB | 
| `tiered` | `tier-3iops`    |     3 IOPS/GB |   3,000-96,000 |            670 MB/s | 10-32,000 GB | 
| `tiered` | `tier-5iops`    |     5 IOPS/GB |   3,000-48,000 |            768 MB/s |  10-9,600 GB | 
| `tiered` | `tier-10iops`   |    10 IOPS/GB |   3,000-48,000 |           1024 MB/s |  10-4,800 GB | 
| `custom` | `custom`        | 1-100 IOPS/GB |   3,000-48,000 |           1024 MB/s | 10-16,000 GB | 
{: caption="Table 1. Comparison of file share profiles and performance levels." caption-side="top"}

^1^ IOPS values are based on 16k IO size.

^2^ Baseline throughput is determined by the number of IOPS multiplied by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB or custom IOPS tiers. The higher the IOPS that you specify, the higher the throughput. Maximum throughput is 1024 MBps.

The application I/O size directly impacts storage performance. If the application I/O size is smaller than the throughput multiplier that is used by the profile to calculate the bandwidth, the IOPS limit is reached before the throughput limit. Conversely, if the application I/O size is larger, the throughput limit is reached before the IOPS limit.

## Defined performance profile
{: #dp2-profile}

With the `dp2` profile, you can specify the total IOPS for the file share within the range for a specific file share size, 10 GB (default minimum) to 32,000 GB. You can provision shares with IOPS performance from 100 IOPS (the default minimum) to 96,000 IOPS, based on share size. The `dp2` profile is based on a IO size of 256 KB. Maximum throughput is 1024 MB/s.

Table 2 shows the available IOPS ranges, based on share size.

| Share size (GB) | IOPS range (IOPS) |
|-----------------|-------------------|
| 10 - 39         | 100 - 1,000 |
| 40 - 79         | 100 - 2,000 |
| 80 - 99         | 100 - 4,000 |
| 100 - 499       | 100 - 6,000 |
| 500 - 999       | 100 - 10,000 |
| 1,000 - 1,999   | 100 - 20,000 | 
| 2,000 - 3,999   | 200 - 40,000 |
| 4,000 - 7,999   | 300 - 40,000 |
| 8,000 - 15,999  | 500 - 64,000 | 
| 16,000 - 32,000  | 2,000 - 96,000^1^|
{: caption="Table 2. dp2 file share profile IOPS and capacity ranges." caption-side="top"}

^1^ For the 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share that is accessed by one client is limited to 48,000 IOPS.

## Previous version file storage profiles
{: #fs-v2-profiles}

This section is about the previous generation of file share profiles (general purpose, 5-iops, 10-iops, or custom) that were used in the Beta release. New file shares can be provisioned with only the dp2 profile. To access the latest features, you must change the IOPS profile of your share to dp2.
{: deprecated}

### IOPS tiers
{: #fs-tiers}

Existing file shares can be based on IOPS tiers that you selected when you created the file share. Table 3 describes the IOPS performance for the IOPS tier profile.

| IOPS Tier | Workload | Share size (GB) | Max IOPS (IOPS) |
|-----------|----------|-----------------|-----------------|
| 3 IOPS/GB | General-purpose workloads  | 10-32,000 | 48,000-96,000¹ |
| 5 IOPS/GB | High I/O intensity workloads | 10-9,600 | 48,000 |
| 10 IOPS/GB | Demanding storage workloads | 10-4,800 | 48,000 |
{: caption="Table 3. IOPS tier profiles and performance levels for each tier" caption-side="bottom"}

¹For the 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share that is accessed by only one client is limited to 48,000 IOPS.

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS. The total maximum IOPS is rounded up to the next multiple of 100 for IOPS calculations that result in IOPS greater than 48,000 IOPS up to 96,000 IOPS.
{: note}

### Custom IOPS profile
{: #fs-custom}

Custom IOPS profiles specify the total IOPS for the file share within the range for its size. File shares that use a custom IOPS profile can have an IOPS performance level in the range of 100-48000 IOPS.

Table 4 shows the available IOPS ranges based on file share size.

| File Share size (GB) | IOPS range (IOPS) |
|----------------------|-------------------|
| 10 - 39   | 100 - 1,000 |
| 40 - 79   | 100 - 2,000 |
| 80 - 99   | 100 - 4,000 |
| 100 - 499 | 100 - 6,000 |
| 500 - 999 | 100 - 10,000 |
| 1,000 - 1,999 | 100 - 20,000 |
| 2,000 - 3,999 | 200 - 40,000 |
| 4,000 - 7,999 | 300 - 40,000 |
| 8,000 - 9,999 | 500 - 48,000 |
| 10,000 - 16,000 | 1,000 - 48,000 |
{: caption="Table 4. Available IOPS based on file share size." caption-side="bottom"}

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS.
{: note}

## View profiles in the UI
{: #fs-using-ui-iops-profile}
{: ui}

When you [create a file share in the UI](/docs/vpc?topic=vpc-file-storage-create&interface=ui#file-storage-create-ui), you can see the dp2 profile in the table in the **Profiles** section.

## Viewing profiles from the CLI
{: #fs-using-cli-iops-profiles}
{: cli}

To view the list of available profiles from the CLI, run the `ibmcloud is share-profiles` command.

```sh
$ ibmcloud is share-profiles
Listing file share profiles in region us-south under account Test Account as user test.user@ibm.com...
Name   Family   
dp2    defined_performance   
```
{: screen}

For more information about the command options, see [`ibmcloud is share-profiles`](/docs/vpc?topic=vpc-vpc-reference#share-profiles).

## Viewing profiles with the API
{: #fs-using-api-iops-profiles}
{: api}

Use the `GET /share/profiles` request to retrieve information for all share profiles.

```sh
curl -X GET \
$vpc_api_endpoint/v1/share/profiles?$api_version&generation=2\
-H "Authorization: $iam_token"
```
{: pre}

The response returns the following profiles and related information:

```json
{
  "first": {"href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles?limit=50"},
  "limit": 50,
  "profiles": [
    {
      "capacity": {
        "max": 32000,
        "min": 10,
        "step": 1,
        "type": "dependent_range"
      },
      "family": "defined_performance",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/dp2",
      "iops": {
        "default": 100,
        "max": 96000,
        "min": 100,
        "step": 1,
        "type": "range"
      },
      "name": "dp2",
      "resource_type": "share_profile"
    }
  ],
  "total_count": 4
}
```
{: codeblock}

## Viewing profiles with Terraform
{: #fs-using-terraform-iops-profiles}
{: terraform}

1. To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).

2. VPC infrastructure services use a regional specific endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file. See the following example of targeting a region other than the default `us-south`.

   ```terraform 
   provider "ibm" {
      region = "eu-de"
   }
   ```
   {: codeblock}

3. Import the list of available volume profiles as a read-only data source. 

   ```terraform
   data "ibm_is_share_profiles" "example" {
   }
   ```
   {: codeblock}

   For more information, see [ibm_is_share_profiles](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_share_profiles){: external}.
   
## How IO size affects file share performance
{: #fs-profiles-block-size}

IOPS values are based on a 16 KB block size for all profiles, with a 50-50 read/write random workload. Each 16 KB of data that is read or written counts as one read/write operation. A single write of less than 16 KB counts as a single write operation.

Maximum throughput for a file share is calculated by taking the file share's IOPS and multiplying it by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB, custom IOPS, and `dp2` tiers.

The higher the IOPS that you specify, the higher the throughput. Maximum throughput is 1024 MBps.

The application I/O size directly impacts storage performance. If the application I/O size is smaller than the throughput multiplier that is used by the profile to calculate the bandwidth, the IOPS limit is reached before the throughput limit. Conversely, if the application I/O size is larger, the throughput limit is reached before the IOPS limit.

Table 5 provides some examples of how block size and IOPS affect the throughput, calculated average I/O block size x IOPS = Throughput in MBps.

| Block Size (KB) | IOPS | Throughput (MBps) |
|-----------------|------|-------------------|
| 4 | 1,000 | 4^1^|
| 8 | 1,000 | 8^1^|
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1,024 | 16 | 16 |
{: caption="Table 5. How block size and IOPS affect throughput." caption-side="bottom"}

^1^ If your cap is 1000 IOPS or 16 KB block size, the throughput caps at whatever limit is reached first.

Maximum IOPS can still be obtained when you use smaller block sizes, but throughput is less. The following example shows how throughput decreases for smaller block sizes, when max IOPS is maintained.

* 16 KB * 6,000 IOPS == ~94 MBps
* 8 KB * 6,000 IOPS == ~47 MBps
* 4 KB * 6,000 IOPS == ~23 MBps

## Next steps
{: #fs-next-steps}

* [Creating file shares](/docs/vpc?topic=vpc-file-storage-create).
* [Managing file shares](/docs/vpc?topic=vpc-file-storage-managing)
