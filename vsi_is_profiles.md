---
copyright:
  years: 2019, 2021

lastupdated: "2021-11-29"

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
{:preview: .preview}
{:external: target="_blank" .external}
{:download: .download}
{:beta: .beta}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Instance Profiles
{: #profiles}

When you provision {{site.data.keyword.vsi_is_full}}, you can select from six families of profiles: Balanced, Compute, Memory, Very High Memory, Ultra High Memory, and GPU.

A profile is a combination of instance attributes, such as the number of vCPUs, amount of RAM, and network bandwidth. The attributes define the size and capabilities of the virtual server instance that is provisioned. In the {{site.data.keyword.Bluemix_notm}} console, you can select the most recently used profile or click **View All Profiles** to choose the profile that best fits your needs.
{: shortdesc}

The following profile families are available:

| Families | Description |
| -------- | ----------- |
| [Balanced](#balanced) | Best for midsize databases and common cloud applications with moderate traffic. |
| [Compute](#compute)  | Best for moderate to high web traffic workloads. Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers. |
| [Memory](#memory) | Best for memory caching and real-time analytics workloads. Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. |
| [Very High Memory](#vhmemory) | Very High Memory profiles offer a core to RAM ratio of 1 vCPU to 14 GiB of RAM. This family is optimized for running high-compute-intensity in-memory workloads like SAP BW/4 HANA |
| [Ultra High Memory](#uhmemory) | Ultra High Memory profiles offer the highest vCPU to memory ratios with 1 vCPU to 28 GiB of RAM to serve in-memory OLTP databases, such as SAP. |
| [GPU](#gpu) | GPU enabled profiles provide on-demand access to NVIDIA V100 GPUs to accelerate AI,high performance computing, data science and graphics workloads. |
{: caption="Table 1. Virtual server family selections" caption-side="top"}

Profiles with instance storage are deployed exclusively on the second-generation Intel&reg; Xeon&reg; Platinum 8272 (Cascade Lake) running at a base speed of 2.4 GHz and an all-core turbo frequency of 3.1 GHz.
{: note}

## Bandwidth allocation using the UI
{: #bandwidth-allocation-ui}
{: ui}

Instance bandwidth is allocated between volume bandwidth and networking bandwidth. The bandwidth capacity (Bandwidth Cap) is determined by the virtual server profile that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a bandwidth cap of 4 Gbps. The initial volume and network bandwidth allocation depends on the bandwidth set by the instance profile you selected. You can also see the bandwidth allocations in the profile information during instance creation in the UI. The bandwidth allocation can be changed on the instance details page after provisioning an instance.

For example, for the bx2-2x8 profile you might have:

* Storage: 1 Gbps
* Network: 3 Gbps

For a cx2-8x16 profile:

* Storage: 4 Gbps
* Network: 12 Gbps

The amount of overall bandwidth provided to volume bandwidth can be adjusted within the overall instance limits. A default amount of volume bandwidth is set on each instance profile.

For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles) and [Adjusting bandwidth allocation using the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).

## Bandwidth allocation using the API
{: #bandwidth-allocation-api}
{: api}

Instance bandwidth is allocated between volume bandwidth and networking bandwidth. The bandwidth capacity (Bandwidth Cap) is determined by the virtual server profile that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a bandwidth cap of 4 Gbps. The initial volume and network bandwidth allocation depends on the bandwidth you set by using the API or by the instance profile you selected. You can see bandwidth allocations with the `/instance/profiles` endpoint in the API. The bandwidth can be changed during or after provisioning an instance.

For example, for the bx2-2x8 profile you might have:

* Storage: 1 Gbps
* Network: 3 Gbps

For a cx2-8x16 profile:

* Storage: 4 Gbps
* Network: 12 Gbps

The amount of overall bandwidth provided to volume bandwidth can be adjusted within the overall instance limits. A default amount of volume bandwidth is set on each instance profile.

For more information, see [Using the REST APIs to create VPC resources](https://test.cloud.ibm.com/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis#select-profile-and-image) and [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

## Balanced
{: #balanced}

Balanced profiles provide a mix of performance and scalability for more common workloads with a ratio of 4 GiB of memory for every 1 vCPU of compute.

The Balanced profile family includes both profiles that are provisioned with and without [instance storage](/docs/vpc?topic=vpc-instance-storage).

### Balanced profiles for x86_64 processors
{: #balanced-x86-profiles}

The following Balanced profiles are available for x86_64 processors:

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Instance Storage (GB) |
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

### Balanced profiles for s390x processors
{: #balanced-s390x-profiles}

The following Balanced profiles are available for LinuxONE (s390x processor architecture):

LinuxONE (s390x processor architecture) profiles can be used to provision virtual server instances in the Tokyo region.
{: preview}

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Instance Storage (GB) |
|---------|---------|---------|---------|---------|
| bz2-1x2 | 1 | 2 | 2 | - |
| bz2-1x4 | 1 | 4 | 2 | - |
| bz2-2x8 | 2 | 8 | 4 | -
| bz2-4x16 | 4 | 16 | 8 | -
| bz2-8x32 | 8 | 32 | 16 | - |
| bz2-16x64 | 16 | 64 | 32 | - |
{: caption="Table 3. Balance profiles options for s390x instances" caption-side="top"}

## Compute
{: #compute}

Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers that can benefit from 2 GiB of memory for every 1 vCPU of compute.

The Compute profile family includes both profiles that are provisioned with and without [instance storage](/docs/vpc?topic=vpc-instance-storage).

### Compute profiles for x86_64 processors
{: #compute-x86-profiles}

The following Compute profiles are available for instances with x86_64 processors:

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Instance Storage (GB) |
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

### Compute profiles for s390x processors
{: #compute-s390x-profiles}

The following Compute profiles are available for LinuxONE (s390x processor architecture):

LinuxONE (s390x processor architecture) profiles can be used to provision virtual server instances in the Tokyo region.
{: preview}

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Instance Storage (GB) |
|---------|---------|---------|---------|---------|
| cz2-2x4 | 2 | 4 | 2 | - |
| cz2-4x8 | 4 | 8 | 8 | - |
| cz2-8x16 | 8 | 16 | 16 | - |
| cz2-16x32 | 16 | 32 | 32 | - |
{: caption="Table 5. Balance profiles options for s390x instances" caption-side="top"}

## Memory
{: #memory}

Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads with 8 GiB of memory for every 1 vCPU of compute.

The Memory profile family includes both profiles that are provisioned with and without [instance storage](/docs/vpc?topic=vpc-instance-storage).

### Memory profiles for x86_64 processors
{: #memory-x86-profiles}

The following memory profiles are available for instances with x86_64 processors:

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Instance Storage (GB) |
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

### Memory profiles for s390x processors
{: #memory-s390x-profiles}

The following Memory profiles are available for LinuxONE (s390x processor architecture):

LinuxONE (s390x processor architecture) profiles can be used to provision virtual server instances in the Tokyo region.
{: preview}

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Instance Storage (GB) |
|---------|---------|---------|---------|---------|
| mz2-2x16 | 2 | 16 | 4 | - |
| mz2-4x32 | 4 | 32 | 8 | - |
| mz2-8x64 | 8 | 64 | 16 | - |
| mz2-16x128 | 16 | 128 | 32 | - |
{: caption="Table 7. Balance profiles options for s390x instances" caption-side="top"}

{: #callout-note}

Profiles with 64 or more vCPUs are deployed exclusively on the second-generation Intel&reg; Xeon&reg; Platinum 8272 (Cascade Lake) running at a base speed of 2.4 GHz and an all-core turbo frequency of 3.1 GHz.  
{: note}

{: #callout-beta}

## Very High Memory
{: #vhmemory}

Very High Memory profiles offer 1 vCPU to 14 GiB of RAM to server OLAP databases, such as SAP NetWeaver. All Very High Memory profiles are provisioned with temporary SSD-backed [instance storage](/docs/vpc?topic=vpc-instance-storage) at no additional charge.

Very High Memory profiles are available in the US South (Dallas), US East (Washington DC), United Kingdom (London), Canada (Toronto), EU Germany (Frankfurt), and Japan (Tokyo), Japan (Osaka), and Australia (Sydney) regions. Additional regions will be added.
{: preview}

{: #callout-note}

### Very High Memory profiles for x86_64 processors
{: vhmemory-x86-profiles}

The following Very High Memory profiles are available for x86_64 processors:

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|
| vx2d-2x28 | 2 | 28 | 4 | 1x60 |
| vx2d-4x56 | 4 | 56 | 8 | 1x120 |
| vx2d-8x112 | 8 | 112 | 16 | 1x240 |
| vx2d-16x224| 16 | 224 | 32 | 1x480 |
| vx2d-44x616 | 44 | 616 | 80 | 1x1320 |
| vx2d-88x1232 | 88 | 1232 | 80 | 2x1320 |
| vx2d-72x2016 | 144 | 2016 | 80 | 2x2160 |
| vx2d-176x2464 | 176 | 2464 | 80 | 2x2640 |
{: caption="Table 8. Very High Memory profiles options for x86-64 instances" caption-side="top"}

## Ultra High Memory
{: #uhmemory}

Ultra High Memory profiles are hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and are optimized for running memory intensive applications and in-memory database such as SAP HANA, Memcached, or Redis. This profile family offers our highest vCPU to memory ratio with 28 GiB of memory for every 1 vCPU of compute and up to 5.7 TiB of available RAM. All Ultra High Memory profiles are provisioned with temporary SSD-backed [Instance Storage](/docs/vpc?topic=vpc-instance-storage) at no additional charge.

{: #callout-note}

### Ultra High Memory profiles for x86_64 processors
{: #uhmemory-x86-profiles}

The following Ultra High Memory profiles are available for x86_64 processors:

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Instance Storage (GB) |
|---------|---------|---------|---------|---------|
| ux2d-2x56 | 2 | 56 | 2 | 1x60 |
| ux2d-4x112 | 4 | 112 | 4 | 1x120 |
| ux2d-8x224 | 8 | 224 | 8 | 1x240 |
| ux2d-16x448 | 16 | 448 | 16 | 1x480 |
| ux2d-36x1008 | 36 | 1008 | 36 | 1x1080 |
| ux2d-48x1344| 48 | 1344 | 80 | 2x720 |
| ux2d-72x2016 | 72 | 2016 | 72 | 2x1080 |
| ux2d-100x2800 | 100 | 2800 | 80 | 2x1500 |
| ux2d-200x5600 | 200 | 5600 | 80 | 2x3000 |
{: caption="Table 9. Ultra High Memory profiles options for x86-64 instances" caption-side="top"}



## GPU
{: #gpu}

GPU profiles include 1 or 2 NVIDIA V100 PCIe 16GB GPUs. All OS images are supported on the GPU profiles. NVIDIA GPU drivers must be installed separately.

If you are using GPU profiles, you need to install the NVIDA driver onto your virtual server instance. For more information, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).
{: tip}

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Number of GPUs |
|---------|---------|---------|---------|---------|
| gx2-8x64x1v100 | 8 | 64 | 16 | 1 |
| gx2-16x128x1v100 | 16 | 128 | 32 | 1 |
| gx2-16x128x2v100 | 16 | 128 | 32 | 2 |
| gx2-32x256x2v100 | 32 | 256 | 64 | 2 |
{: caption="Table 10. GPU profile options" caption-side="top"}

If you are using GPU profiles, you might need to install the CUDA toolkit onto your virtual server instance. For more information, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).
{: tip}

For more information about persistent storage options, see [Storage notes for profiles](#storage-notes-for-profiles).

For information about storage, see [Storage notes for profiles](#storage-notes-for-profiles).

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
- "u": ultra high memory family of profiles, 1 vCPU to 28 GiB of memory ratio
-  "g" is GPU, which is a 1:8 or 1:16 ratio

The second character represents the CPU architecture.
- "x": x86_64
- "z": s390x

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
{: codeblock}

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
{: codeblock}

## Block storage volume notes for profiles
{: #block-storage-notes-for-profiles}

When you create secondary data volumes, you select a volume profile that best meets your requirements. Volume profiles are available as three predefined [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) or as a [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom). These volume profiles relate to virtual server instance profiles:

* A [3 IOPS general-purpose tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provides IOPS/GB performance suitable for a virtual server instance Balanced profile.
* A [5-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance suitable for a virtual server instance Compute profile.
* A [10-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance suitable for a virtual server instance Memory profile.

## Network performance notes for profiles
{: #network-perf-notes-for-profiles}

Every profile has a maximum bandwidth cap of 80 Gbps. Network bandwidth is distributed evenly across network interfaces, and each network interface has a cap of 16 Gbps. You might need to attach multiple network interfaces to your virtual server instance to optimize network performance.

For example, if you choose a profile with 16 vCPU, the bandwidth cap for the profile is 32 Gbps. The default network cap will be 24 Gbps, but can be adjusted up to a maximum of 31.5 Gbps. You need to attach more network interfaces (16 Gbps each) to reach the profile cap of 80 Gbps.

The network bandwidth cap applies separately to egress (transmitted) and ingress (received) traffic. That is, even if an instance capped at 4 Gbps reaches its transmit cap of 4 Gbps, that does not impact its ability to receive up to its cap of 4 Gbps.

## Next steps
{: nextsteps-profiles}

After you choose a profile, it's time to create an instance.

* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli)
* [Creating an instance by using the API](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis#select-profile-and-image)
* [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus)
