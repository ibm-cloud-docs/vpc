---

copyright:
  years: 2021, 2025
lastupdated: "2025-06-18"

keywords: file storage, file share, performance, IOPS, block size, capacity, range

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.filestorage_vpc_short}} profiles
{: #file-storage-profiles}

When you provision {{site.data.keyword.filestorage_vpc_short}} file shares by using the {{site.data.keyword.cloud_notm}} console, CLI, or API, you specify capacity and performance within a file share profile. Available performance levels vary based on the file share size. All file shares are backed by solid-state drives (SSDs).
{: shortdesc}

## File Storage profile overview
{: #file-storage-profile-overview}

When you [create a file share](/docs/vpc?topic=vpc-file-storage-create), you select the share size and IOPS performance that is available, based on a file storage profile. Currently, all file shares are created based on the high-performance [dp2](#dp2-profile) profile.

File shares that were created during the beta and limited availability phases with either one of the [tiered](#fs-tiers) profiles or the [custom](#fs-custom) profile can continue to operate based on these profiles. You can also update these file shares to use the `dp2` profile or switch to another previous generation profile. However, you cannot use the previous profiles when you create a file share, and only the file shares with the `dp2` profile can use new features like encryption-in-transit, cross-zone mounting, cross-account sharing, and snapshots.

The following tables show the characteristics and performance levels of the available profiles.

Current file share profile:

| Family   | Profile         | IOPS | IOPS per share | Max throughput  | Share size   |
|----------|-----------------|--------------:|---------------:|---------------------|-------------:|
| `defined_performance`|`dp2`| 1-100 IOPS/GB |     100-96,000 |  1024 MBps (8192 Mbps) | 10-32,000 GB |
{: caption="Comparison of file share profiles and performance levels." caption-side="top"}

Previous file share profiles:

| Family   | Profile         | IOPS[^tabletext1] | IOPS per share | Max throughput[^tabletext2]  | Share size   |
|----------|-----------------|--------------:|---------------:|---------------------|-------------:|
| `tiered` | `tier-3iops`    |     3 IOPS/GB |   3,000-96,000 |  670 MBps (5360 Mbps) | 10-32,000 GB |
| `tiered` | `tier-5iops`    |     5 IOPS/GB |   3,000-48,000 |  768 MBps (6144 Mbps) |  10-9,600 GB |
| `tiered` | `tier-10iops`   |    10 IOPS/GB |   3,000-48,000 |  1024 MBps (8192 Mbps)|  10-4,800 GB |
| `custom` | `custom`        | 1-100 IOPS/GB |   3,000-48,000 |  1024 MBps (8192 Mbps) | 10-16,000 GB |
{: caption="Comparison of file share profiles and performance levels." caption-side="top"}

[^tabletext1]: IOPS values are based on 16 KB I/O size.
[^tabletext2]: The maximum allowable throughput is determined by the number of IOPS multiplied by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB or custom IOPS tiers. The higher the IOPS that you specify, the higher the throughput limit. Maximum throughput is 1024 MBps (8192 Mbps).

The maximum allowable throughput is determined by the number of IOPS multiplied by the throughput multiplier, which is specific to the profile.

The application I/O size directly impacts storage performance. If the application I/O size is smaller than the throughput multiplier that is used by the profile to calculate the bandwidth, the IOPS limit is reached before the throughput limit. Conversely, if the application I/O size is larger, the throughput limit is reached before the IOPS limit.

A single session can achieve up to 64 KB block size transfer. To utilize the max allowable bandwidth, you need multiple concurrent sessions to the share.

## Defined performance profile
{: #dp2-profile}

With the `dp2` profile, you can specify the total IOPS for the file share within the range for a specific file share size, 10 GB (default minimum) to 32,000 GB. You can provision shares with IOPS performance from 100 IOPS (the default minimum) to 96,000 IOPS, based on share size. The `dp2` profile is based on an I/O size of 256 KB. Maximum throughput is 1024 MBps (8192 Mbps).

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
| 16,000 - 32,000  | 2,000 - 96,000¹|
{: caption="dp2 file share profile IOPS and capacity ranges." caption-side="top"}

¹ For the 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share that is accessed by one client is limited to 48,000 IOPS.

## Tiered and custom file storage profiles
{: #fs-v2-profiles}

In the following section, you can find information about the file share profiles (general purpose, 5-iops, 10-iops, or custom) that were used in the Beta release. New file shares can be provisioned with only the defined performance profile. To access the newest features, you must change the IOPS profile of your share to dp2.
{: deprecated}

### IOPS tiers
{: #fs-tiers}

Existing file shares can be based on IOPS tiers that you selected when you created the file share. Table 3 describes the IOPS performance for the IOPS tier profile.

| IOPS Tier | Workload | Share size (GB) | Max IOPS (IOPS) |
|-----------|----------|-----------------|-----------------|
| 3 IOPS/GB | General-purpose workloads  | 10-32,000 | 48,000-96,000¹ |
| 5 IOPS/GB | High I/O intensity workloads | 10-9,600 | 48,000 |
| 10 IOPS/GB | Demanding storage workloads | 10-4,800 | 48,000 |
{: caption="IOPS tier profiles and performance levels for each tier" caption-side="bottom"}

¹ For the 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share that is accessed by only one client is limited to 48,000 IOPS.

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS. The total maximum IOPS is rounded up to the next multiple of 100 for IOPS calculations that result in IOPS greater than 48,000 IOPS up to 96,000 IOPS.
{: note}

### Custom share profile
{: #fs-custom}

The Custom IOPS profile specifies the total IOPS for the file share within the range for its size. File shares that use a custom IOPS profile can have an IOPS performance level in the range of 100-48000 IOPS.

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
{: caption="Available IOPS based on file share size." caption-side="bottom"}

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS.
{: note}

## View profiles in the console
{: #fs-using-ui-iops-profile}
{: ui}

When you [create a file share in the console](/docs/vpc?topic=vpc-file-storage-create&interface=ui#file-storage-create-ui), you can see the availale profiles in the table in the **Profiles** section.

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

For more information about the command options, see [`ibmcloud is share-profiles`](/docs/vpc?topic=vpc-vpc-reference#share-profiles-list).

To see the details of the share profile, use the `ibmcloud is share-profile` command and specify the name of the profile. See the following example.

```sh
$ ibmcloud is share-profile dp2
Listing file share profiles in region us-south under account Test Account as user test.user@ibm.com...
Name       dp2   
Family     defined_performance   
IOPS       Default   Max     Min   Step   Type      
           100       96000   100   1      range      
              
Capacity   Default   Max     Min   Step   Type      
           10        32000   10    1      range     
```
{: screen}

For more information about the command options, see [`ibmcloud is share-profile`](/docs/vpc?topic=vpc-vpc-reference#share-profile-viewt).

## Viewing profiles with the API
{: #fs-using-api-iops-profiles}
{: api}

Use the `GET /share/profiles` request to retrieve information for all share profiles.

```sh
curl -X GET $vpc_api_endpoint/v1/share/profiles?$api_version&generation=2\
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

2. VPC infrastructure services use a specific regional endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file. See the following example of targeting a region other than the default `us-south`.

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
   
## How I/O size affects file share performance
{: #fs-profiles-block-size}

IOPS values are based on a 16 KB block size for all profiles, with a 50-50 read/write random workload. Each 16 KB of data that is read or written counts as one read/write operation. A single write of less than 16 KB counts as a single write operation.

Maximum throughput for a file share is calculated by taking the file share's IOPS and multiplying it by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB, custom IOPS, and `dp2` profiles. The higher the IOPS that you specify, the higher the throughput. Maximum throughput is 1024 MBps.

The application I/O size directly impacts storage performance. If the application I/O size is smaller than the throughput multiplier that is used by the profile to calculate the bandwidth, the IOPS limit is reached before the throughput limit. Conversely, if the application I/O size is larger, the throughput limit is reached before the IOPS limit.

Table 5 provides some examples of how block size and IOPS affect the throughput, calculated average I/O block size x IOPS = Throughput in MBps.

| Block Size (KB) | IOPS | Throughput (MBps) |
|-----------------|------|-------------------|
| 4 | 1,000 | 4¹|
| 8 | 1,000 | 8¹|
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1,024 | 16 | 16 |
{: caption="How block size and IOPS affect throughput." caption-side="bottom"}

¹ If your cap is 1000 IOPS or 16 KB block size, the throughput caps at whatever limit is reached first.

Maximum IOPS can still be obtained when you use smaller block sizes, but throughput is less. The following example shows how throughput decreases for smaller block sizes, when max IOPS is maintained.

* 16 KB * 6000 IOPS == ~94 MBps
* 8 KB * 6000 IOPS == ~47 MBps
* 4 KB * 6000 IOPS == ~23 MBps

## Next steps
{: #fs-next-steps}

* [Create a file shares](/docs/vpc?topic=vpc-file-storage-create).
* [Manage file shares](/docs/vpc?topic=vpc-file-storage-managing).
* For more information about the pricing, see the [FAQs](/docs/vpc?topic=vpc-file-storage-vpc-faqs#faq-fs-pricing).
