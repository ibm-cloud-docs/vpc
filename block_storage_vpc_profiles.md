---

copyright:
  years: 2019, 2024
lastupdated: "2024-09-27"

keywords: Block Storage profiles, Block Storage for VPC, IOPS tiers, custom IOPS, storage performance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.block_storage_is_short}} profiles
{: #block-storage-profiles}

When you provision {{site.data.keyword.block_storage_is_short}} volumes by using the {{site.data.keyword.cloud_notm}} console, CLI, API, or Terraform you specify a volume profile that best meets your storage requirements. Profiles are generally available as three predefined IOPS levels or with custom IOPS. The volume profiles from the _tiered_ family provide reliable IOPS/GB performance for volumes up to 16,000 GB capacity. With a _custom_ volume profile, you can specify your own IOPS value in a range that is appropriate for your selected volume capacity.
{: shortdesc}

## Block Storage profile families
{: #block-storage-profile-overview}

When you create a Block Storage volume, you can select from various profiles. 
- Select a profile from the _tiered_ profile family when you want to pick a profile where performance scales with capacity of the volume.
- Select the profile from the _custom_ profile family if your performance requirements don't fall within any of the predefined IOPS tiers. When you select the custom profile, you can define your IOPS within a range that depends on the capacity that you specified.

The following table shows the available storage profiles with their different properties.

| Family name  | Profile name      | Capacity range \n (GB) | IOPS rate \n (IOPS/GB)  | IOPS range [^ttext1] \n (IOPS)| Max throughput[^ttext2] \n (MBps)|
|--------------|-------------------|---------------:|----------------------:|------------------:|-----------------------:|
| tiered       | `general-purpose` |    10 - 16,000 |  3        | 3,000 - 48,000 |  670 | 
| tiered       | `5iops-tier`      |    10 -  9,600 |  5        | 3,000 - 48,000 |  768 |
| tiered       | `10iops-tier`     |    10 -  4,800 | 10        | 3,000 - 48,000 | 1024 | 
| custom       | `custom`          |    10 - 16,000 | 10 - 100  | 100 - 48,000[^ttext3] | 1024 |
{: caption="Table 1. Block Storage profiles and performance levels." caption-side="bottom"}

[^ttext1]: Nominal IOPS values are based on 16k I/O size.
[^ttext2]: Max throughput is determined by the number of IOPS multiplied by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers. The throughput multiplier for the 10 IOPS/GB  tier and the custom profile is 256 KB. The higher the IOPS that you specify, the higher the throughput limit becomes.
[^ttext3]: The IOPS range that is available is dependent on the volume capacity. For more information, see [Table 3](#custom).

Nominal IOPS values are based on 16k I/O size. The maximum throughput value is determined by the number of IOPS multiplied by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB or custom IOPS tiers. The higher the IOPS that you specify, the higher the throughput the volume can handle.

The application I/O size directly impacts storage performance. If the application I/O size is smaller than the throughput multiplier that is used by the profile to calculate the volume’s bandwidth limit, the IOPS limit is reached before the throughput limit. Conversely, if the application I/O size is larger, the throughput limit is reached before the IOPS limit. For more information, see [How I/O size affects performance](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).
{: note}

Moving volumes across volume-profiles that belong to different families is not allowed.
{: restriction}

### Tiered volume profiles
{: #tiers}

When you create your storage volume, you can select from three predefined IOPS tiers. Choose the profile that provides optimal performance for your Compute workloads. Table 2 describes the IOPS performance that you can expect for each tier.

| Intended workload | Capacity range (GB)	| IOPS rate (IOPS/GB)	|  IOPS range	| Throughput multiplier (KB) |
|------------------|------------:|----------------:|-------------:|----------------------:|
| `general-purpose` - Workloads that host small databases for web applications or store virtual machine disk images for a hypervisor. |  10 - 16,000 | 3 IOPS/GB  | 3,000	- 48,000 | 16  |
| `5iops-tier` - High I/O intensity workloads - Workloads characterized by a large percentage of active data, such as transactional and other performance-sensitive databases.|  10 - 9,600 | 5 IOPS/GB | 3,000 - 48,000 | 16  |
| `10iops-tier` - Demanding storage workloads - Data intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics.| 10 - 4,800 | 10 IOPS/GB | 3,000 - 48,000 | 256  | 
{: caption="Table 2. IOPS tier profiles and performance levels for each tier" caption-side="bottom"}

Max IOPS for all volume profiles from the _tiered_ family starts at 3,000 IOPS. Max IOPS then increases, based on the storage tier and volume size, up to the Max IOPS in Table 2. While you can't customize the IOPS value of a volume with a tiered profile, you can change the volume to another tiered profile and adjust the IOPS that way.

### Custom volume profile
{: #custom}

Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier. You can customize the IOPS by specifying the total IOPS for the volume within the range for its volume size. You can provision volumes with IOPS performance from 100 IOPS to 48,000 IOPS, based on volume size.

The following table shows the available IOPS ranges based on volume capacity for the custom profile. 

| Intended workload | Capacity range (GB)	| IOPS range    | Throughput multiplier (KB)| 
|-------------------|--------------------:|--------------:|--------------------------:|
| Custom            | 10 - 39             | 100 - 1,000   |                       256 |
|                   | 40 - 79             | 100 - 2,000   |                       256 |
|                   | 80 - 99             | 100 - 4,000   |                       256 |
|                   | 100 - 499           | 100 - 6,000   |                       256 |
|                   | 500 - 999           | 100 - 10,000  |                       256 |
|                   | 1,000 - 1,999       | 100 - 20,000  |                       256 |
|                   | 2,000 - 3,999       | 200 - 40,000  |                       256 |
|                   | 4,000 - 7,999       | 300 - 40,000  |                       256 |
|                   | 8,000 - 9,999       | 500 - 48,000  |                       256 |
|                   | 10,000 - 16,000     | 1,000 - 48,000|                       256 |
{: caption="Table 3. Available IOPS based on volume size" caption-side="bottom"}

If your application needs more IOPS and throughput, you can increase the volume size and specify a new IOPS value in a higher range. Capacity and IOPS can be modified only when the volume is attached to a running instance.
{: tip}

## Profiles for boot volumes
{: #vsi-profiles-boot}

By default, boot volumes are created with the `general-purpose` volume profile with 100 GB capacity during instance provisioning. [Boot volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes) can be increased by modifying the boot volume, up to 250 GB. Boot volume IOPS and bandwidth are never reduced to be less than 3000 IOPS or 393 Mbps.

## How virtual server profiles relate to volume profiles
{: #vsi-profiles-relate-to-storage}

Virtual server profiles are a combination of vCPU and RAM that can be instantiated quickly to start a virtual server instance. Select from [three families of instance profiles](/docs/vpc?topic=vpc-profiles#profiles) based on your workload requirements. These requirements can range from common workloads to CPU-intensive or memory-intensive workloads.

Similarly, volume profiles provide a range of capacity and performance for secondary volumes. By default, a 100 GB primary boot volume is created when you create a virtual server instance. You can also create and attach secondary volumes. When you create a data volume as part of instance creation, you can select a volume profile that best meets your storage requirements for your Compute workloads. In general, as your Compute requirements increase, you need higher IOPS performance. Table 4 shows this relationship.

| IOPS tier storage profile | Virtual server profile                                             |
|-----------------|------------------------------------------------------------------------------|
| 3 IOPS/GB       | [Balanced](/docs/vpc?topic=vpc-profiles#balanced) for common workloads.      |
| 5 IOPS/GB       | [Compute](/docs/vpc?topic=vpc-profiles#compute) for intensive CPU demands.   |
| 10 IOPS/GB      | [Memory](/docs/vpc?topic=vpc-profiles#memory) for memory-intensive workloads.|
{: caption="Table 4. Relationship of Block Storage volume profiles to virtual server instance profiles" caption-side="bottom"}



## Storage capacity
{: #note-about-measurement-unit}

In this documentation, we refer to storage capacity by using the unit GB (Gigabytes) to align with the industry standard terminology. However, the actual provisioning and billing of storage are based on GiB (Gibibytes).

The difference between GB and GiB lies in their numerical representation:
- GB (Gigabyte) is a decimal unit, where 1 GB equals 1,000,000,000 bytes
- GiB (Gibibyte), is a binary unit, where 1 GiB equals 1,073,741,824 bytes

To ensure transparency, please note that your provisioned storage and associated charges are calculated based on GiB. Rest assured that you receive the exact amount of storage you expect and are billed accurately for the GiB you use. For more information, see the [FAQs](/docs/vpc?topic=vpc-block-storage-vpc-faq#faq-storage-units).

## Viewing available volume profiles
{: #view-iops-profiles}

You can view available volume profiles by using the {{site.data.keyword.cloud_notm}} UI, CLI, API, or Terraform.

### In the console
{: #using-console-iops-profile}
{: ui}

When you [create a Block Storage volume in the {{site.data.keyword.cloud_notm}} console](/docs/vpc?topic=vpc-creating-block-storage), you can see the available profiles on two tabs. 
- For [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers), you can see 3 tiles with the different performance levels. Select the one that best suits your needs. 
- For [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) IOPS, you can specify the size of your volume and IOPS range based on the size of the volume. As you type the IOPS value, the console shows the acceptable range. You can also click the **storage size** link to see [Table 3](#custom).

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
```
{: screen}

For more information about available command options, see [`ibmcloud is volume-profile`](/docs/vpc?topic=vpc-vpc-reference&interface=ui#volume-profile-view).

### With the API
{: #using-api-iops-profiles}
{: api}

To see the available profiles, make a `GET /volume/profiles` request.

```sh
curl -X GET \
$vpc_api_endpoint/v1/volume/profiles?$api_version&generation=2 \
-H "Authorization: $iam_token"
```
{: pre}

Before 24 September 2024, the API response included the fields `name`, `href`, `family`. Now the response is enhanced to include the following fields:

- `boot_capacity` denotes the capacity values that are permissible for boot volumes for each profile. The returned value is a range with minimum and maximum values that are specified for each profile.
   - For `custom` and `tiered` profiles, the range is 10 GB - 250 GB.
- `capacity` denotes the capacity values that are permissible for data volumes for each profile. The returned value is a range with minimum and maximum values that are specified for each profile.
   - For `custom` and `tiered` profiles, the range is 10 GB - 16 TB.
- `adjustable_capacity_states` indicates whether the capacity for the volume can be changed when the volume is not attached to a running virtual server instance. This field is informational. It describes the characteristics of the volume profile and cannot be changed.
   - For `custom` and `tiered` profiles, this value is `attached`.
- `adjustable_iops_states` indicates whether the IOPS for the volume can be changed when the volume is not attached to a running virtual server instance. This field is informational. It describes the characteristics of the volume profile and cannot be changed.
   - For the `custom` profiles, this value is `attached`.
   - For the `tiered` profiles, this value is empty because changes to IOPS are not supported in any state. If you want to change the IOPS value of a volume, you can change to another `tiered` profile.

To see details of a specific profile, make a `GET /volume/profiles/` request with the profile name.

```sh
curl -X GET "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/10iops-tier?version=2024-09-24&generation=2"\
 -H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example.   

```json
   {
  "boot_capacity": {
    "max": 250,
    "min": 10,
    "step": 1,
    "type": "dependent_range"
  },
  "capacity": {
    "max": 16000,
    "min": 10,
    "step": 1,
    "type": "dependent_range"
  },
  "family": "tiered",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/10iops-tier",
  "iops": {
    "max": 48000,
    "min": 100,
    "step": 1,
    "type": "dependent_range"
  },
  "name": "10iops-tier",
  "adjustable_capacity_states": {
    "type": "fixed",
    "value": "attached"
  },
  "adjustable_iops_states": {
    "type": "fixed",
    "value": ""
  }
}
```
{: screen}



For more information about this method, see the API reference for [listing all volume profiles](/apidocs/vpc/latest#list-volume-profiles) and [retrieving a volume profile](/apidocs/vpc/latest#get-volume-profile).

### With Terraform
{: #using-terraform-iops-profiles}
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
   data "ibm_is_volume_profiles" "example" {
   }
   ```
   {: codeblock}

   For more information, see [ibm_is_volume_profile](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_volume_profiles){: external}.

## Next Steps
{: #profile-next-steps}

[New]{: tag-new} For more information about the pricing of the volume profiles, see the [FAQs](/docs/vpc?topic=vpc-block-storage-vpc-faq#faq-bs-pricing).

For more information about how to expand volume capacity, see [expanding Block Storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

For more information about how to change the IOPS tier or Custom IOPS for an existing volume, see [Adjusting IOPS of a Block Storage for VPC volume](/docs/vpc?topic=vpc-adjusting-volume-iops).

For more information about Balanced, Compute, and Memory profiles for {{site.data.keyword.vsi_is_short}}, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles).
