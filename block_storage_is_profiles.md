---

copyright:
  years: 2019, 2023
lastupdated: "2023-07-27"

keywords: block storage profiles, Block Storage for VPC, IOPS tiers, custom IOPS, storage performance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.block_storage_is_short}} profiles
{: #block-storage-profiles}

When you provision {{site.data.keyword.block_storage_is_short}} volumes by using the {{site.data.keyword.cloud_notm}} console, CLI, API, or Terraform you specify an IOPS profile that best meets your storage requirements. Profiles are generally available as three predefined IOPS levels or with custom IOPS. IOPS tiers provide reliable IOPS/GB performance for volumes up to 16,000 GB capacity. You can also specify a custom IOPS profile and define volume capacity and IOPS within a range.
{: shortdesc}

## Block storage profiles overview
{: #block-storage-profile-overview}

When you create a block storage volume, you can select between custom and tiered-IOPS profiles. All these profiles are backed by solid-state drives (SSDs). The following table shows the available storage profiles.

| Profile family | Profile name      | IOPS&sup1;      | IOPS per volume| Max throughput&sup2;| Volume size  |   
|----------------|-------------------|----------------:|---------------:|---------------:|------------------:|
| `tiered`       | `general-purpose` | 3 IOPS/GB       | 3,000 - 48,000 | 670 MB/s       | 10 GB - 16,000 GB | 
| `tiered`       | `5iops-tier`      | 5 IOPS/GB       | 3,000 - 48,000 | 768 MB/s       | 10 GB - 9,600 GB  | 
| `tiered`       | `10iops-tier`     | 10 IOPS/GB      | 3,000 - 48,000 | 1024 MB/s      | 10 GB - 4,800 GB  |
| `custom`       | `custom`          | 1 - 100 IOPS/GB |   100 - 48,000 | 1024 MB/s      | 10 GB - 16,800 GB |
{: caption="Table 1. Block storage profiles and performance levels." caption-side="bottom"}

&sup1; IOPS values are based on 16k IO size. 

&sup2; Baseline throughput is determined by the number of IOPS multiplied by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB or custom IOPS tiers. The higher the IOPS that you specify, the higher the throughput. Maximum throughput is 1024 MBps.

The application I/O size directly impacts storage performance. If the application I/O size is smaller than the throughput multiplier that is used by the profile to calculate the volume’s bandwidth limit, the IOPS limit is reached before the throughput limit. Conversely, if the application I/O size is larger, the throughput limit is reached before the IOPS limit.

For more information, see [How block size affects performance](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

Moving volumes across volume-profiles that belong to different families is not allowed.
{: restriction}

### IOPS tiers
{: #tiers}

When you create your storage volume, you can select from three predefined IOPS tiers. Choose the profile that provides optimal performance for your compute workloads. Table 2 describes the IOPS performance that you can expect for each tier.

|  Name	| Purpose | IOPS rate	| Capacity range (GB)	| Min IOPS	Max IOPS	| IO Size |
|--------|---------|------------:|----------------------:|-------------------:|----------:|
| 	`general-purpose`	| Workloads that host small databases for web applications or store virtual machine disk images for a hypervisor. | 3 IOPS/GB  | 10 -	16,000 | 3,000	- 48,000 | 16 KB |
| 	`5iops-tier`	|High I/O intensity workloads - Workloads characterized by a large percentage of active data, such as transactional and other performance-sensitive databases.| 5 IOPS/GB | 10 - 9,600 | 3,000 - 48,000 | 16 KB |
| 	`10iops-tier`	| Demanding storage workloads - Data intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics. Bandwidth limits are calculated by using a 256 KB block size.| 10 IOPS/GB | 10 - 4,800 | 3,000 - 48,000 | 256 KB | 
{: caption="Table 2. IOPS tier profiles and performance levels for each tier" caption-side="bottom"}

Max IOPS for all tiered profiles starts at 3,000 IOPS. Max IOPS then increases, based on the storage tier and volume size, up to the Max IOPS in Table 2.

### Custom IOPS profiles
{: #custom}

Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier. You can customize the IOPS by specifying the total IOPS for the volume within the range for its volume size. You can provision volumes with IOPS performance from 100 IOPS to 48,000 IOPS, based on volume size.

The following table shows the available IOPS ranges based on volume capacity for the custom profile. 

| Volume size (GB) | IOPS range    |
|------------------|---------------|
| 10 -39           | 100 - 1,000   |
| 40 - 79          | 100 - 2,000   |
| 80 - 99          | 100 - 4,000   |
| 100 - 499        | 100 - 6,000   |
| 500 - 999        | 100 - 10,000  |
| 1,000 - 1,999    | 100 - 20,000  |
| 2,000 - 3,999    | 200 - 40,000  |
| 4,000 - 7,999    | 300 - 40,000  |
| 8,000 - 9,999    | 500 - 48,000  |
| 10,000 - 16,000  | 1,000- 48,000 |
{: caption="Table 3. Available IOPS based on volume size" caption-side="bottom"}

## Profiles and boot volumes
{: #vsi-profiles-boot}

By default, boot volumes are created based on the `general-purpose` IOPS profile with 100 GB capacity during instance provisioning. [Boot volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes) can be increased by modifying the boot volume, up to 250 GB. 

## How virtual server profiles relate to storage profiles
{: #vsi-profiles-relate-to-storage}

Virtual server profiles are a combination of vCPU and RAM that can be instantiated quickly to start a virtual server instance. Select from [three families of profiles](/docs/vpc?topic=vpc-profiles#profiles) based on your workload requirements. These requirements can range from common workloads to CPU-intensive or memory-intensive workloads.

Similarly, storage profiles (IOPS tiers or custom) provide a range of capacity and performance for secondary volumes. By default, a 100 GB primary boot volume is created when you create a virtual server instance. You can also create and attach secondary volumes. When you create a secondary data volume as part of instance creation, you select a storage profile that best meets your storage requirements for your compute workloads. In general, as your compute requirements increase, you need higher IOPS performance. Table 3 shows this relationship.

| IOPS tier storage profile | Virtual server profile                                             |
|-----------------|------------------------------------------------------------------------------|
| 3 IOPS/GB       | [Balanced](/docs/vpc?topic=vpc-profiles#balanced) for common workloads.      |
| 5 IOPS/GB       | [Compute](/docs/vpc?topic=vpc-profiles#compute) for intensive CPU demands.   |
| 10 IOPS/GB      | [Memory](/docs/vpc?topic=vpc-profiles#memory) for memory-intensive workloads.|
{: caption="Table 4. Relationship of block storage profiles to virtual server profiles" caption-side="top"}

## Viewing available IOPS profiles
{: #view-iops-profiles}

You can view available IOPS profiles the {{site.data.keyword.cloud_notm}} UI, CLI, API, or Terraform.

### In the UI
{: #using-console-iops-profile}
{: ui}

When you [create a block storage volume from the {{site.data.keyword.cloud_notm}} console](/docs/vpc?topic=vpc-creating-block-storage), select **Tiers**.

Alternately, select **Custom** and then select an IOPS value within the range for that volume size. Click the storage size link to see a table of size and IOPS ranges.

### From the CLI
{: #using-cli-iops-profiles}
{: cli}

To view the list of available profiles by using the CLI, run the following command:
```sh
ibmcloud is volume-profiles
```
{: pre}

```sh
$ ibmcloud is volume-profiles
Listing volume profiles in region us-east under account TEST as user test.user@ibm.com...
Name              Family   
general-purpose   tiered   
5iops-tier        tiered   
10iops-tier       tiered   
custom            custom 
```
{: codeblock}

To view details of the profile, run the `ibmcloud is volume-profile` command with the name of the profile that you are interested in seeing.

The following example shows the details of the `10iops-tier`.

```sh
$ ibmcloud is volume-profile 10iops-tier
Getting volume profile 10iops-tier under account Test Account as user test.user@ibm.com...
                                          
Name                                   10iops-tier   
Family                                 tiered   
Adjustable IOPS                        false   
Boot capacity                          Max   Min      
                                       250   10      
                                          
Capacity                               Max    Min   Default   Step      
                                       4800   10    10        1      
                                          
IOPS                                   Max    Min   Default   Step      
                                       48000  10    10        1      
                                          
Unattached capacity update supported   false   
Unattached iops update supported       false  
```
{: screen}

For more information about available command options, see [`ibmcloud is volume-profile`](/docs/cli?topic=cli-vpc-reference#volume-profile).

### With the API
{: #using-api-iops-profiles}
{: api}

To see the available profiles, make a `GET /volume/profiles` call.

```sh
curl -X GET \
$vpc_api_endpoint/v1/volume/profiles?$api_version&generation=2 \
-H "Authorization: $iam_token"
```
{: pre}

To see details of a specific profile, make a `GET /volume/profile` request and specify the name of the profile.

```sh
curl -X GET "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/custom?version=2023-07-24&generation=2"\
 -H "Authorization: $iam_token"
```
{: pre}

### With Terraform
{: #using-terraform-iops-profiles}
{: terraform}

1. To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).

2. VPC infrastructure services use a region-specific endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the right region in the provider block in the `provider.tf` file. See the following example of targeting a region other than the default `us-south`.

   ```terraform 
   provider "ibm" {
      region = "eu-de"
   }
   ```
   {: codeblock}

3. Import the list of available volume profiles as a read-only data source. 

   ```terraform
   data "ibm_is_volume_profiles" "example" {
   }
   ```
   {: codeblock}

   For more information, see [ibm_is_volume_profile](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_volume_profiles){: external}.

## Next Steps
{: #profile-next-steps}

To expand volume capacity, see [expanding block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

To [change the IOPS tier or Custom IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) for an existing volume that is attached to a virtual server instance.

For more information about Balanced, Compute, and Memory profiles for {{site.data.keyword.vsi_is_short}}, see [Profiles](/docs/vpc?topic=vpc-profiles).
