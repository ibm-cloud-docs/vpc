---

copyright:
  years: 2021, 2025
lastupdated: "2025-11-19"

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

[Select availability]{: tag-green} Customers with special access to preview the new regional file share offering can use the **rfs** profile to create file shares with regional availability and adjustable bandwidth values.

File shares that were created during the beta and limited availability phases of zonal file shares, with either [tiered](#fs-tiers) profiles or the [custom](#fs-custom) profile can continue to operate based on those profiles. You can also update these file shares to use the `dp2` profile or switch to another previous generation profile. However, previous profiles are not supported for creating new file shares. These profiles do not support newer features such as encryption-in-transit, cross-zone mounting, cross-account sharing, and snapshots.

The following tables show the characteristics and performance levels of the available profiles.

Current file share profiles:

| Family   | Profile         | Availability  | Share size   |IOPS per share | Max bandwidth[^tabletext1]| 
|----------|-----------------|--------------:|-------------:|---------------|-------------:|
| `defined_performance`|`dp2`|         Zonal | 10-32,000 GB |    100-96,000 |    8192 Mbps |
| `defined_performance`|`rfs`|      Regional |  1-32,000 GB |        35,000 |    8192 Mbps |
{: caption="Comparison of file share profiles and performance levels." caption-side="top"}

[^tabletext1]: For the `dp2` profile, the maximum allowable bandwidth is determined by the number of IOPS multiplied by 256 KB. For the `rfs` profile, the baseline bandwidth is 800 Mbps. You can increase the bandwidth value by increasing the capacity and IOPS of the `dp2` shares, and by increasing the bandwidth value of the `rfs` profile-based shares. Maximum bandwidth is 8192 Mbps for all defined performance profiles. This bandwidth value represents the maximum allowed combined throughput for read and write operations.

First- and second-generation profiles in the defined performance profile family are not interchangeable. You can't convert a zonal file share to a regional share, or a regional share to a zonal share.
{: important}

Deprecated file share profiles:

| Family   | Profile         | Availability  | Share size   |IOPS per share | Max bandwidth| 
|----------|-----------------|--------------:|-------------:|---------------|-------------:|
| `tiered` | `tier-3iops`    |         Zonal | 10-32,000 GB |  3,000-96,000 |    5360 Mbps |
| `tiered` | `tier-5iops`    |         Zonal |  10-9,600 GB |  3,000-48,000 |    6144 Mbps |
| `tiered` | `tier-10iops`   |         Zonal |  10-4,800 GB |  3,000-48,000 |    8192 Mbps |
| `custom` | `custom`        |         Zonal | 10-16,000 GB |  3,000-48,000 |    8192 Mbps |
{: caption="Comparison of file share profiles and performance levels." caption-side="top"}

The maximum allowable bandwidth for zonal shares is determined by the number of IOPS multiplied by the bandwidth multiplier, which is specific to the profile. This bandwidth value represents the maximum allowed combined throughput for read and write operations.

[Select availability]{: tag-green} The bandwidth value of regional file shares is adjustable. For the `rfs` profile, the baseline bandwidth is 800 Mbps. This value can be increased up to 8192 Mbps in the console, from the CLI, and with the API.

The application I/O size directly impacts storage performance. If the application I/O size is smaller than the bandwidth multiplier that is used by the profile to calculate the bandwidth, the IOPS limit is reached before the bandwidth limit. Conversely, if the application I/O size is larger, the bandwidth limit is reached before the IOPS limit.

A single session can achieve up to 64 KB block size transfer. To use the max allowable bandwidth, you need multiple concurrent sessions to the share.

## Defined performance profiles
{: #defined-performance-family}

Both profiles in the defined performance family provide resilient file storage with flexible performance characteristics and different data availability.

### Regional availability profile
{: #rfs-profile}

[Select availability]{: tag-green} The `rfs` profile is the second-generation profile in the defined performance profile family. With this profile, you can create file shares with regional availability. 

Regional data availability means that data is replicated across all 3 zones within the region, offering higher availability and fault tolerance. Due to the synchronous replication between the zones and the need to ensure data durability, you might experience increased latency during write operations. For workloads where latency performance is less critical than durability, or higher and more consistent IOPS is preferred over low latency, the regional shares can be a better choice.

When you create a regional file share, you can specify its capacity between 1 GiB to 32,000 GiB. 800 Mbps is the default allocation for all regional file shares. You can increase the bandwidth from 800 Mbps up to 8192 Mbps for extra cost. After the file share is created, you can adjust the bandwidth between the minimum and the maximum values anytime.

In the select availability release, cross-region asynchronous replication is not supported.

### Zonal availability profile
{: #dp2-profile}

With the `dp2` profile, you can provision zonal file shares, and specify your capacity and total IOPS values.

Zonal data availability means that data is available within a single availability zone, offering faster responses with less delays, but less resilience. For latency-sensitive applications that can tolerate a 1-hour RPO for replication, zonal shares can be a better choice. 

File shares can be created with capacity in the range of 10 GB (preset minimum) to 32,000 GB. You can start small, and if you need more storage capacity later, you can increase the size of the file share. When you create a file share, you can select an IOPS value between 100 IOPS (the preset minimum) to 96,000 IOPS, based on share size. You can also adjust the IOPS later. 

For the `dp2` profile, the baseline bandwidth is determined by the number of IOPS multiplied by 256 KB. So when you increase the capacity and the IOPS, you also increase the bandwidth limit. The maximum bandwidth value for this profile is 8192 Mbps.

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

In the following section, you can find information about the file share profiles (general purpose, 5-iops, 10-iops, or custom) that were used in the beta release of zonal file shares. New file shares can be provisioned with only the defined performance profiles. To access the newest features, you must change the IOPS profile of your share to dp2.
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

When you [create a file share in the console](/docs/vpc?topic=vpc-file-storage-create&interface=ui#file-storage-create-ui), you can see the available profiles in the table in the **Profiles** section.

## Viewing profiles from the CLI
{: #fs-using-cli-iops-profiles}
{: cli}

To view the list of available profiles from the CLI, run the `ibmcloud is share-profiles` command. [Select availability]{: tag-green} Customers with special access to preview the new regional file share offering, can also see the `rfs` profile when listing the share profiles.

```sh
ibmcloud is share-profiles
```
{: pre}

```sh
Listing file share profiles in region us-south under account Test Account as user test.user@ibm.com... 
Name   Family                Allowed Access Protocols   Allowed transit encryption modes   Availability Modes   Bandwidth(Mbps)
dp2    defined_performance   nfs4                       none,ipsec                         zonal                -
rfs    defined_performance   nfs4                       none,stunnel                       regional             1
```
{: screen}

For more information about the command options, see [`ibmcloud is share-profiles`](/docs/vpc?topic=vpc-vpc-reference#share-profiles-list).

To see the details of the share profile, use the `ibmcloud is share-profile` command and specify the name of the profile. See the following example.

```sh
ibmcloud is share-profile dp2
```
{: pre}

```sh
Getting file share profile dp2 under account Test Account as user test.user@ibm.com...

Name                               dp2
Family                             defined_performance
IOPS                               Default   Max     Min   Step   Type
                                   100       96000   100   1      range

Capacity                           Default   Max     Min   Step   Type
                                   10        32000   10    1      range

Bandwidth(Mbps)                    Default   Max   Min   Step   Type
                                   -         -     -     -      dependent

Availability Modes                 Default   Type    Value   Values
                                   -         fixed   zonal   -

Allowed Access Protocols           Default   Type     Values
                                   nfs4      subset   nfs4

Allowed transit encryption modes   Default   Type     Values
                                   none      subset   none,ipsec    
```
{: screen}

```sh
ibmcloud is share-profile rfs
```
{: pre}

```sh
Getting file share profile rfs in region us-south under account Test Account as user test.user@ibm.com...
Name                               rfs   
Family                             defined_performance   
IOPS                               Default   Max   Min   Step   Type      
                                   -         -     -     -      fixed      
                                      
Capacity                           Default   Max     Min   Step   Type      
                                   1         32000   1     1      range      
                                      
Bandwidth(Mbps)                    Default   Max    Min   Step   Type      
                                   800       8096   1     1      range      
                                      
Availability Modes                 Default   Type    Value      Values      
                                   -         fixed   regional   -      
                                      
Allowed Access Protocols           Default   Type     Values      
                                   nfs4      subset   nfs4      
                                      
Allowed transit encryption modes   Default   Type     Values      
                                   stunnel   subset   none,stunnel      
```
{: screen}

For more information about the command options, see [`ibmcloud is share-profile`](/docs/vpc?topic=vpc-vpc-reference#share-profile-view).

## Viewing profiles with the API
{: #fs-using-api-iops-profiles}
{: api}

Use the `GET /share/profiles` request to retrieve information about the generally available file share profiles.

```sh
curl -X GET $vpc_api_endpoint/v1/share/profiles?$api_version&generation=2\
-H "Authorization: $iam_token"
```
{: pre}

[Select availability]{: tag-green} Customers with special access to preview the new regional file share offering, can list the new profile with the following API request:

The response looks like the following example.

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles?limit=50"
  },
  "limit": 50,
  "profiles": [
    {    
    "allowed_access_protocols": {
      "default": [
        "nfs4"
      ],
      "type": "subset",
      "values": [
        "nfs4"
      ]
    },
    "allowed_transit_encryption_modes": {
      "default": [
        "none",
        "stunnel"
      ],
      "type": "subset",
      "values": [
        "none",
        "stunnel"
      ]
    },
    "availability_modes": {
      "type": "fixed",
      "value": "zonal"
    },
    "bandwidth": {
      "type": "fixed",
      "value": 100
    },
    "capacity": {
      "max": 32000,
      "min": 10,
      "step": 1,
      "type": "dependent_range"
    },
    "family": "defined_performance",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/dp2",
    "iops": {
      "max": 96000,
      "min": 100,
      "step": 1,
      "type": "dependent_range"
    },
    "name": "dp2",
    "resource_type": "share_profile"
},
    {
      "allowed_access_protocols": {
        "default": [
          "nfs4"
        ],
        "type": "subset",
        "values": [
          "nfs4"
        ]
      },
      "allowed_transit_encryption_modes": {
        "default": [
          "none"
        ],
        "type": "subset",
        "values": [
          "none"
        ]
      },
      "availability_modes": {
        "type": "fixed",
        "value": "regional"
      },
      "bandwidth": {
        "max": 8192,
        "min": 800,
        "step": 1,
        "type": "dependent_range"
      },
      "capacity": {
        "max": 32000,
        "min": 10,
        "step": 1,
        "type": "dependent_range"
      },
      "family": "defined_performance",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/share/profiles/rfs",
      "iops": {
        "type": "dependent"
      },
      "name": "rfs",
      "resource_type": "share_profile"
    }
  ],
  "total_count": 2
}
```
{: codeblock}

The 2025-09-16 version of the API is enhanced to include the following fields:

- `allowed_access_protocols` denotes which access protocol is allowed for use with the file share. The current default value is `nsf4`.
- `availability_modes` denotes the data availability. 
    - The `dp2` profile supports zonal data availability.
    - The `rfs` profile supports regional data availability.
- `bandwidth` denotes the available maximum bandwidth value that the file share can handle.
    - For the `dp2` profile, the bandwidth is calculated by multiplying the number of IOPS by 256 KB.
    - For the `rfs` profile, the preset value is 800 Mbps. However, you can increase this value up to 8192 Mbps.
- `storage_generation` denotes the generation of the file share profile within the Defined performance profile family. 
    - For the `dp2` profile, the value is `1`.
    - For the `rfs` profile, the value is `2`.

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

Maximum bandwidth for a file share is calculated by taking the file share's IOPS and multiplying it by the bandwidth multiplier. The bandwidth multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB, custom IOPS, and `dp2` profiles. The higher the IOPS that you specify, the higher the bandwidth. Maximum bandwidth is 8192 Mbps.

[Select availability]{: tag-green} For the `rfs` profile, the bandwidth is directly [adjustable](/docs/vpc?topic=vpc-file-storage-adjusting-bandwidth). The preset value is 800 Mbps. You can increase this value up to 8192 Mbps, or reduce it back to the preset value. The IOPS value for a regional file share is 35000. 

The application I/O size directly impacts storage performance. If the application I/O size is smaller than the bandwidth multiplier that is used by the profile to calculate the bandwidth, the IOPS limit is reached before the bandwidth limit. Conversely, if the application I/O size is larger, the bandwidth limit is reached before the IOPS limit.

Table 5 provides some examples of how block size and IOPS affect the bandwidth, calculated average I/O block size x IOPS = Bandwidth in MBps.

| Block Size (KB) | IOPS | Bandwidth (MBps) |
|-----------------|------|-------------------|
| 4 | 1,000 | 4¹|
| 8 | 1,000 | 8¹|
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1,024 | 16 | 16 |
{: caption="How block size and IOPS affect bandwidth." caption-side="bottom"}

¹ If your cap is 1000 IOPS or 16 KB block size, the bandwidth caps at whatever limit is reached first.

Maximum IOPS can still be obtained when you use smaller block sizes, but bandwidth is less. The following example shows how bandwidth decreases for smaller block sizes, when max IOPS is maintained.

* 16 KB * 6000 IOPS == ~94 MBps
* 8 KB * 6000 IOPS == ~47 MBps
* 4 KB * 6000 IOPS == ~23 MBps

## Next steps
{: #fs-next-steps}

* [Create a file shares](/docs/vpc?topic=vpc-file-storage-create).
* [Manage file shares](/docs/vpc?topic=vpc-file-storage-managing).
* For more information about the pricing, see the [FAQs](/docs/vpc?topic=vpc-file-storage-vpc-faqs#faq-fs-pricing).
