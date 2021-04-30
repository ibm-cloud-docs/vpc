---
copyright:
  years: 2019, 2021
lastupdated: "2021-04-30"

keywords: vsi, virtural server instances, profiles, balanced, compute, memory, generation 2, gen 2

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:download: .download}
{:beta: .beta}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Instance Profiles
{: #profiles}

When you provision {{site.data.keyword.vsi_is_full}}, you can select from three families of profiles: Balanced, Compute, and Memory.
<!-- add Ultra High Memory after 3/26--->

A profile is a combination of instance attributes, such as the number of vCPUs, amount of RAM, and network bandwidth. The attributes define the size and capabilities of the virtual server instance that is provisioned. In the {{site.data.keyword.Bluemix_notm}} console, you can select the most recently used profile or click **View All Profiles** to choose the profile that best fits your needs.
{: shortdesc}

The following profile families are available:

| Families | Description |
| -------- | ----------- |
| [Balanced](#balanced) | Best for midsize databases and common cloud applications with moderate traffic. |
| [Compute](#compute)  | Best for moderate to high web traffic workloads. Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers. |
| [Memory](#memory) | Best for memory caching and real-time analytics workloads. Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. |

<!---| [Ultra High Memory](#uhmemory) | Ultra High Memory profiles offer the highest vCPU to memory ratios with 1 vCPU to 28 GiB of RAM to serve in-memory OLTP databases, such as SAP. |--->

<!---| [GPU](#gpu) | Best for artificial intelligence (AI) and deep learning workloads. Available for POWER processing architecture only. | -->
{: caption="Table 1. Virtual server family selections" caption-side="top"}

## Balanced
{: #balanced}

Balanced profiles provide a mix of performance and scalability for more common workloads with a ratio of 4 GiB of memory for every 1 vCPU of compute. 

The Balanced profile family includes both profiles that are provisioned with and without [instance storage](/docs/vpc?topic=vpc-instance-storage).

The following Balanced profiles are available for x86_64 processors:

| Instance profile | vCPU | GiB RAM | Network Bandwidth Cap (Gbps) | Instance Storage (GB) |
|---------|---------|---------|---------|---------|
| bx2-2x8 | 2 | 8 | 4 | - |
| bx2d-2x8 | 2 | 8 | 4 | 1x75 |
| bx2-4x16 | 4 | 16 | 8 | - |
| bx2d-4x16 | 4 | 16 | 8 | 1x150 |
| bx2-8x32 | 8 | 32 | 16 | - |
| bx2d-8x32 | 8 | 32 | 16 | 1x300 |
| bx2-16x64 | 16 | 64 | 32 | - |
| bx2d-16x64 | 16 | 64 | 32 | 1x600 | 
| bx2-32x128 | 32  | 128 | 64 | - |
| bx2d-32x128 | 32  | 128 | 64 | 2x600 |
| bx2-48x192 | 48 | 192 | 80 | - |
| bx2d-48x192 | 48 | 192 | 80 | 1x900 |
| bx2-64x256 | 64 | 256| 80 | - |
| bx2d-64x256 | 64 | 256 | 80 | 2x1200 |
| bx2-96x384 | 96 | 384 | 80 | - |
| bx2d-96x384 | 96 | 384 | 80 | 2x1800 |
| bx2-128x512 | 128 | 512 | 80 | - |
| bx2d-128x512 | 128 | 512 | 80 | 2x400 |
{: caption="Table 2. Balance profiles options for x86-64 instances" caption-side="top"}

## Compute
{: #compute}

Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers that can benefit from 2 GiB of memory for every 1 vCPU of compute.

The Compute profile family includes both profiles that are provisioned with and without [instance storage](/docs/vpc?topic=vpc-instance-storage).

The following Compute profiles are available for instances with x86_64 processors:

| Instance profile | vCPU | GiB RAM | Network Bandwidth Cap (Gbps) | Instance Storage (GB) |
|---------|---------|---------|---------|---------|
| cx2-2x4 | 2 | 4 | 4 | - |
| cx2d-2x4 | 2 | 4 | 4 | 1x75 |
| cx2-4x8 | 4 | 8 | 8 | - |
| cx2d-4x8 | 4 | 8 | 8 | 1x150 | 
| cx2-8x16 | 8 | 16 | 16 | - |
| cx2d-8x16 | 8 | 16 | 16 | 1x300 |
| cx2-16x32 | 16 | 32 | 32 | - |
| cx2d-16x32 | 16 | 32 | 32 | 1x600 |
| cx2-32x64 | 32  | 64 | 64 | - |
| cx2d-32x64 | 32  | 64 | 64 | 2x600 |
| cx2-48x96 | 48  | 96 | 80 | - |
| cx2d-48x96 | 48  | 96 | 80 | 1x900 |
| cx2-64x128 | 64 | 128 | 80 | - |
| cx2d-64x128 | 64 | 128 | 80 | 2x1200 |
| cx2-96x192 | 96 | 192 | 80 | - |
| cx2d-96x192 | 96 | 192 | 80 | 2x1800 |
| cx2-128x256 | 128 | 256 | 80 | - |
| cx2d-128x256 | 128 | 256 | 80 | 2x2400 |
{: caption="Table 4. Compute profile options for x86-64 instances" caption-side="top"}

## Memory
{: #memory}

Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads with 8 GiB of memory for every 1 vCPU of compute.

The Memory profile family includes both profiles that are provisioned with and without [instance storage](/docs/vpc?topic=vpc-instance-storage).

The following memory profiles are available for instances with x86_64 processors:

| Instance profile | vCPU | GiB RAM | Network Bandwidth Cap (Gbps) | Instance Storage (GB) |
|---------|---------|---------|---------|---------|
| mx2-2x16 | 2 | 16 | 4 | - |
| mx2d-2x16 | 2 | 16 | 4 | 1x75 |
| mx2-4x32 | 4 | 32 | 8 | - |
| mx2d-4x32 | 4 | 32 | 8 | 1x150 |
| mx2-8x64 | 8 | 64 | 16 | - |
| mx2d-8x64 | 8 | 64 | 16 | 1x300 |
| mx2-16x128 | 16 | 128 | 32 | - |
| mx2d-16x128 | 16 | 128 | 32 | 1x600 |
| mx2-32x256 | 32 | 256 | 64 | - |
| mx2d-32x256 | 32 | 256 | 64 | 2x600 |
| mx2-48x384 | 48 | 384 | 80 | - |
| mx2d-48x384 | 48 | 384 | 80 | 2x900 |
| mx2-64x512| 64 | 512 | 80 | - |
| mx2d-64x512| 64 | 512 | 80 | 2x1200 |
| mx2-96x768| 96 | 768 | 80 | - |
| mx2d-96x768| 96 | 768 | 80 | 2x1800 |
| mx2-128x1024| 128 | 1024 | 80 | - |
| mx2d-128x1024| 128 | 1024 | 80 | 2x2400|
{: caption="Table 5. Memory profile options for x86-64 instances " caption-side="top"}

{: #callout-note}

Profiles with 64 or more vCPUs are deployed exclusively on the second-generation Intel&reg; Xeon&reg; Platinum 8272 (Cascade Lake) running at a base speed of 2.4 GHz and an all-core turbo frequency of 3.1 GHz.  
{: note}

{: #callout-beta}

<!-- ## GPU
{: #gpu}

GPU profiles are best for AI and deep learning workloads.

The following GPU profiles, available for POWER-based instances, are provisioned with Ubuntu 18.04. The profiles feature NVLink 2.0 and PCIe Gen4 interconnects for faster CPU to GPU bandwidth and 50% faster networking I/O. Each GPU is a NVIDIA Tesla v100 and each includes 32 GBs of memory that contributes to the overall memory the VSI reports.

** (GMW 10/20/20 - pulled out the table coding after the table header as it was causing a chkpii problem with the comment coding)

| Profile | vCPU | GB RAM | Network Performance Cap (Gbps) | Number of GPUs |
| gp2-24x224x2 | 24 |224 | 72 | 2 |
| gp2-32x256x4 | 32 | 256 | 96 | 4 |
| gp2-56x448x4 | 56 | 448 | 100 | 4 |
| gp2-56x896x4 | 56 | 896 | 100 | 4 |
{: caption="Table 6. Power GPU profile options" caption-side="top"}

GPU profiles are supported by Ubuntu 18.04 only. For more information about supported operating systems, see [Images](/docs/vpc?topic=vpc-about-images). A few of the larger profile sizes might require you to increase your quota limit. To increase a quota for a particular resource, [contact support](/docs/get-support?topic=get-support-getting-customer-support).

If you are using GPU profiles, you might need to install the NVIDA kernel driver, the CUDA toolkit, or both onto your virtual server instance. For more information, see [Setting up GPU drivers for POWER-based instances](/docs/vpc?topic=vpc-setup-gpus).
{:tip} -->

For information about storage, see [Storage notes for profiles](#storage-notes-for-profiles). 

The network bandwidth cap applies separately to egress (transmitted) and ingress (received) traffic. That is, even if an instance capped at 4 Gbps reaches its transmit cap of 4 Gbps, that does not impact its ability to receive up to its cap of 4 Gbps. For information about network performance, see [Network performance notes for profiles](#network-perf-notes-for-profiles). 

## Viewing profile configurations
{: #popular-profiles}

You can view available profile configurations by using the {{site.data.keyword.cloud_notm}} console or the CLI. In the {{site.data.keyword.cloud_notm}} console, you can select from popular profile configurations that support most common use cases.

### Understanding the naming rule of the profiles
{: #profiles-naming-rule}

The following information describes the naming rule of the profiles.

The first character represents the profile families. Different profile families have different ratios of vCPU to memory and other characteristics that are designed for different workloads.
-	"b": balanced family of profiles, 1 vCPU to 4 GiB of memory ratio
-	"c": compute family of profiles (higher on the CPUs), 1 vCPU to 2 GiB of memory ratio
-	"m": memory family of profiles (higher on the memory), 1 vCPU to 8 GiB of memory ratio
<!-- *  "g" is GPU, which is a 1:8 or 1:16 ratio -->

The second character represents the CPU architecture.
- "x": x86_64
<!-- * "z": System Z -->
<!-- * POWER will deprecate on Aug. 22 -->

The third character represents the generation of the IBM Cloud infrastructure where the profile is provisioned.
-	"1": IBM Cloud Virtual Servers for Classic
-	"2": IBM Cloud Virtual Servers for VPC

If the fourth character is a "d", such as bx2d, then a defined quantity of instance storage is provisioned with the virtual server.

The characters after "-" represents the number of vCPUs and the size of RAM (GiB). For example, "2x8" means that this profile has 2 vCPU and 8 GiB of RAM.

Using “bx2-4x16” as an example, you can know from the name that it is a balanced profile that provides 4 vCPUs of compute and 16 GiB of memory. The profile is deployed on an x86-based host and is for the second-generation VPC. 

### Viewing instance profiles in the UI
{: #profiles-using-console}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. From the Virtual server instances page, click **New instance**.
3. You can either select a profile configuration from **Popular profiles** or click **All profiles** to view more configurations.

### Viewing instance profiles from the CLI
{: #profiles-using-cli}
{: cli}

To view the list of available instance profiles by using the CLI, run the following command:
```
$ ibmcloud is instance-profiles
```
{:codeblock}

### Viewing instance profiles with the API
{: #profiles-using-api}
{: api}

To view the list of available instance profiles by using the API, you can call the [List all instance profiles API](/apidocs/vpc#list-instance-profiles). 

The following request example lists the available instance profiles. When you call the API, replace the API endpoint and IAM token with the values from your enterprise. For more information about the `$vpc_api_endpoint` and `$iam_token` variables, see the Authentication and Endpoint URLs sections in [Virtual Private Cloud API Introduction](/apidocs/vpc#about-vpc-api).

```
curl -X GET \
"$vpc_api_endpoint/v1/instance/profiles?version=2021-02-23&generation=2" \
-H "Authorization: $iam_token"
```
{:codeblock}

## Storage notes for profiles
{: #storage-notes-for-profiles}

When you create secondary data volumes, you select a volume profile that best meets your requirements. Volume profiles are available
as three predefined [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) or as a [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom). These volume profiles relate to virtual server instance profiles:

* A [3 IOPS general-purpose tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provides IOPS/GB performance suitable for a virtual server instance Balanced profile.
* A [5-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance suitable for a virtual server instance Compute profile.
* A [10-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance suitable for a virtual server instance Memory profile.

## Network performance notes for profiles
{: #network-perf-notes-for-profiles}

Every profile has a maximum network bandwidth of 2 Gbps per vCPU, with a cap of 80 Gbps. Network bandwidth is distributed evenly across network interfaces, and each network interface has a cap of 16 Gbps that might limit the overall performance. You might need to attach multiple network interfaces to your virtual server instance to optimize network performance.

For example, if you choose a profile with 16 vCPU, the network cap for the profile is 32 Gbps. If you have just one network interface, the maximum network performance is 16 Gbps due to the network interface cap. You need to attach two network interfaces (16 Gbps each) to reach the profile cap of 32 Gbps.

## Next steps
{: nextsteps-profiles}

After you choose a profile, it's time to create an instance. 

* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli)
* [Creating an instance by using the API](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis#select-profile-and-image)
