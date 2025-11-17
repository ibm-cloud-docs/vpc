---

copyright:
  years: 2019, 2025
lastupdated: "2025-11-17"

keywords: vsi, virtual server instances, profile, profiles, balanced, compute, memory, very high memory, ultra high memory, gpu storage optimized, confidential compute

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# x86-64 instance profiles
{: #profiles}

When you provision {{site.data.keyword.vsi_is_full}}, you can select from seven families of profiles: Balanced, Compute, Memory, Ultra High Memory, Very High Memory, Storage Optimized, and GPU.

A profile is a combination of instance attributes, such as the number of vCPUs, amount of RAM, network bandwidth, and default bandwidth allocation. The attributes define the size and capabilities of the virtual server instance that is provisioned. In the {{site.data.keyword.Bluemix_notm}} console, you can select the most recently used profile or click **View All Profiles** to choose the profile that best fits your needs.
{: shortdesc}

For more information about SAP profiles, see [Intel Virtual Server certified profiles on VPC infrastructure for SAP HANA](/docs/sap?topic=sap-hana-iaas-offerings-profiles-intel-vs-vpc) and [Intel Virtual Server certified profiles on VPC infrastructure for SAP NetWeaver](/docs/sap?topic=sap-nw-iaas-offerings-profiles-intel-vs-vpc).

## Before you begin
{: #x86-64-instance-profiles-before-you-begin}

Some profiles might not be available because of one of the following reasons:
   - The number of network interfaces in the virtual server exceeds profile limits. You can remove network interfaces to select from more profiles. For more information, see [Resizing a virtual server](/docs/vpc?topic=vpc-resizing-an-instance).
   - The image selected contains an allowed-use expression that is not compatible with the profile. In these cases, select an image with an allowed-use expression that is compatible with the desired profile. For more infomation, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).

## Profile families
{: #x86-64-instance-profile-families}

The following profile families are available when you provision a virtual server instance.

| Family | Description |
| -------- | ----------- |
| [Balanced](#balanced) | Balanced profiles offer a core to RAM ratio that is best for midsize databases and common cloud applications with moderate traffic. |
| [Compute](#compute)  | Compute profiles offer a core to RAM ratio that is best for moderate to high web traffic workloads. Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers. |
| [Memory](#memory) | Memory profiles offer a core to RAM ratio that is best for memory caching and real-time analytics workloads. Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. |
| [Very High Memory](#vhmemory) | Very High Memory profiles offer a core to RAM ratio of 1 vCPU to 14 GiB of RAM. This family is optimized for running small to medium in-memory databases and OLAP workloads, such as SAP BW/4 HANA. |
| [Ultra High Memory](#uhmemory) | Ultra High Memory profiles offer the most memory per core with 1 vCPU to 28 GiB of RAM. These profiles are optimized to run large in-memory databases and OLTP workloads, such as SAP S/4 HANA.|
| [GPU](#gpu) | GPU-enabled profiles provide on-demand access to GPUs and accelerators to facilitate AI, high-performance computing, data science, and graphics workloads.|
| [Storage Optimized](#storageopt) | Storage Optimized profiles offer temporary SSD instance storage disks at a ratio of 1 vCPU to 300 GB instance storage with a smaller price point per GB. These profiles are designed for storage-dense workloads and offer `virtio` interface type for attached disks. |
| [Confidential Compute](#confidential-computing-profiles) | Confidential Compute-supported profiles use processor reserved memory called EPC (Enclave Page Cache) to encrypt application data. Processor reserved memory EPC maintains confidentiality and integrity. |
| [Flex profiles](#flexible-profiles) | Flex profiles offer a cost-effective option to help improve and mainstream capacity and scalability where and when you need it. |
| [Burstable Flex profiles](#burstable-supported-flex-profiles) (beta) | Burstable profiles are designed to provide flexible CPU performance so workloads can operate at a smaller baseline level and burst to higher performance when needed. |
{: caption="Virtual server family selections" caption-side="bottom"}

2nd generation profiles with instance storage and 2nd generation profiles with 64 or more vCPUs are deployed exclusively on the Intel&reg;'s second-generation quad processor Xeon&reg; Platinum 8260 Cascade Lake with 96 cores that are running at a base speed of 2.4 GHz and an all-core turbo frequency of 3.1 GHz or Intel&reg;'s quad processor Xeon&reg; Gold 6248 Cascade Lake with 80 cores that are running at a base speed of 2.5 GHz and an all-core turbo frequency of 3.1 GHz.
{: note}

For more information about LinuxONE (s390x processor architecture) profiles, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles&interface=ui).

Profiles with AMD-manufactured processors are available in the Toronto region.
{: preview}

## Balanced
{: #balanced}

Balanced profiles provide a mix of performance and scalability for more common workloads. The Balanced profile family includes profiles with and without [instance storage](/docs/vpc?topic=vpc-instance-storage). The following table shows all Balanced profiles that are available for Intel&reg; x86-64, and AMD x86-64 processors.

The following table shows all balance profiles that are available for x86-64.

| Instance profile | vCPU / Cores | NUMA count | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) |
|---------|---------|-------|-------|---------|---------|
| bx3d-2x10 | 2 / 1 | 1 | 10 | 4 | 1x65 |
| bx3d-4x20 | 4 / 2 | 1 | 20 | 8 | 1x130 |
| bx3d-8x40 | 8 / 4 | 1 | 40 | 16 | 1x260 |
| bx3d-16x80 | 16 / 8 |  1 | 80 | 32 | 1x520 |
| bx3d-24x120 | 24 / 12 | 1 | 120 | 48 | 1x780 |
| bx3d-32x160 | 32 / 16 | 2 | 160 | 64 | 2x520 |
| bx3d-48x240 | 48 / 24 | 2 | 240 | 96 | 2x780 |
| bx3d-64x320 | 64 / 32 | 2 | 320 | 128 | 2x1024 |
| bx3d-96x480 | 96 / 48 | 2 | 480 | 192 | 2x1560 |
| bx3d-128x640 | 128 / 64 | 2 | 640 | 200 | 2x2080 |
| bx3d-176x880 | 176 / 88 | 2 | 880 | 200 | 2x2860 |
{: caption="Balanced bx3d beta profile options for Intel x86-64 instances" caption-side="bottom"}
{: #balanced-intel-x86-64-spr}
{: tab-title="bx3d"}
{: tab-group="Balanced"}
{: class="simple-tab-table"}
{: summary="Balanced Beta profile options for Intel x86-64 virtual server instances."}

| Instance profile | vCPU / Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|
| bx2-2x8 | 2 / 1 | 8 | 4 | - |
| bx2d-2x8 | 2 / 1 | 8 | 4 | 1x75 |
| bx2-4x16 | 4 / 2 | 16 | 8 | - |
| bx2d-4x16 | 4 / 2 | 16 | 8 | 1x150 |
| bx2-8x32 | 8 / 4 | 32 | 16 | - |
| bx2d-8x32 | 8 / 4 | 32 | 16 | 1x300 |
| bx2-16x64 | 16 / 8 | 64 | 32 | - |
| bx2d-16x64 | 16 / 8 | 64 | 32 | 1x600 |
| bx2-32x128 | 32 / 16 | 128 | 64 | - |
| bx2d-32x128 | 32 / 16 | 128 | 64 | 2x600 |
| bx2-48x192 | 48 / 24 | 192 | 80 | - |
| bx2d-48x192 | 48 / 24 | 192 | 80 | 1x900 |
| bx2-64x256 | 64 / 32 | 256| 80 | - |
| bx2d-64x256 | 64 / 32 | 256 | 80 | 2x1200 |
| bx2-96x384 | 96 / 48 | 384 | 80 | - |
| bx2d-96x384 | 96 / 48 | 384 | 80 | 2x1800 |
| bx2-128x512 | 128 / 64 | 512 | 80 | - |
| bx2d-128x512 | 128 / 64 | 512 | 80 | 2x2400 |
| bx2a-2x8 | 2 / 1 | 8 | 2 | - |
| bx2a-4x16 | 4 / 2 |  16 | 4 | - |
| bx2a-8x32 | 8 / 4 | 32 | 8 | - |
| bx2a-16x64 | 16 / 8 | 64 | 16 | - |
| bx2a-32x128 | 32 / 16 | 128 | 32 | - |
| bx2a-48x192 | 48 / 24 | 192 | 48 | - |
| bx2a-64x256 | 64 / 32 | 256 | 64 | - |
| bx2a-96x384 | 96 / 48 | 384 | 80 | - |
| bx2a-128x512 | 128 / 64 | 512 | 80 | - |
| bx2a-228x912 | 228 / 114 | 912 | 80 | - |
{: caption="Balance profiles options for Intel x86-64 instances" caption-side="bottom"}
{: #balanced-intel-x86-64}
{: tab-title="bx2"}
{: tab-group="Balanced"}
{: class="simple-tab-table"}
{: summary="Balanced bx2 profile options for Intel x86-64 virtual server instances."}

AMD-based virtual machines use AMD EPYC Milan processors. Compute capabilities are limited to AMD EPYC Rome capabilities.
{: important}

## Compute
{: #compute}

Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers. The Compute profile family includes profiles with and without [instance storage](/docs/vpc?topic=vpc-instance-storage). The following table shows all Compute profiles that are available for &reg; x86-64.

The following table shows all compute profiles that are available for x86-64.

| Instance profile | vCPU / Cores | NUMA count | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|---------|
| cx3d-2x5 | 2 / 1 | 1 | 5 | 4 | 1x65 |
| cx3d-4x10 | 4 / 2 | 1 | 10 | 8 | 1x130 |
| cx3d-8x20 | 8 / 4 | 1 | 20 | 16 | 1x260 |
| cx3d-16x40 | 16 / 8 | 1 | 40 | 32 | 1x520 |
| cx3d-24x60 | 24 / 12 | 1 | 60 | 48 | 1x780 |
| cx3d-32x80 | 32 / 16 | 2 | 80 | 64 | 2x520 |
| cx3d-48x120 | 48 / 24 | 2 |  120 | 96 | 2x780 |
| cx3d-64x160 | 64 / 32 | 2 | 160 | 128 | 2x1024 |
| cx3d-96x240 | 96 / 48 | 2 | 240 | 192 | 2x1560 |
| cx3d-128x320 | 128 / 64  | 2 | 320 | 200 | 2x2080 |
| cx3d-176x440 | 176 / 88 | 2 | 440 | 200 | 2x2860 |
{: caption="Compute profile options for x86-64 instances" caption-side="bottom"}
{: #compute-intel-x86-64}
{: tab-title="cx3d"}
{: tab-group="Compute"}
{: class="simple-tab-table"}
{: summary="3rd generation Compute profile options for Intel x86-64 virtual server instances."}

| Instance profile | vCPU / Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|
| cx2-2x4 | 2 / 1 | 4 | 4 | - |
| cx2d-2x4 | 2 / 1 | 4 | 4 | 1x75 |
| cx2-4x8 | 4 / 2 | 8 | 8 | - |
| cx2d-4x8 | 4 / 2 | 8 | 8 | 1x150 |
| cx2-8x16 | 8 / 4 | 16 | 16 | - |
| cx2d-8x16 | 8 / 4 | 16 | 16 | 1x300 |
| cx2-16x32 | 16 / 8 |  32 | 32 | - |
| cx2d-16x32 | 16 / 8 | 32 | 32 | 1x600 |
| cx2-32x64 | 32 / 16 | 64 | 64 | - |
| cx2d-32x64 | 32 / 16 | 64 | 64 | 2x600 |
| cx2-48x96 | 48 / 24 | 96 | 80 | - |
| cx2d-48x96 | 48 / 24 | 96 | 80 | 1x900 |
| cx2-64x128 | 64 / 32 | 128 | 80 | - |
| cx2d-64x128 | 64 / 32 | 128 | 80 | 2x1200 |
| cx2-96x192 | 96 / 48 | 192 | 80 | - |
| cx2d-96x192 | 96 / 48 | 192 | 80 | 2x1800 |
| cx2-128x256 | 128 / 64 | 256 | 80 | - |
| cx2d-128x256 | 128 / 64 | 256 | 80 | 2x2400 |
{: caption="Compute profile options for x86-64 instances" caption-side="bottom"}
{: #compute-intel-x86-64b}
{: tab-title="cx2"}
{: tab-group="Compute"}
{: class="simple-tab-table"}
{: summary="Compute profile options for Intel x86-64 virtual server instances."}

## Memory
{: #memory}

Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. The Memory profile family includes profiles with and without [instance storage](/docs/vpc?topic=vpc-instance-storage). The following table shows all Memory profiles that are available for Intel&reg; x86-64.

| Instance profile | vCPU / Cores | NUMA count | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) |
|---------|-------|-------|---------|---------|---------|
| mx3d-2x20 | 2 / 1 | 1 | 20 | 4 | 1x65 |
| mx3d-4x40 | 4 / 2 | 1 | 40 | 8 | 1x130 |
| mx3d-8x80 | 8 / 4 | 1 | 80 | 16 | 1x260 |
| mx3d-16x160 | 16 / 8 |  1 | 160 | 32 | 1x520 |
| mx3d-24x240 | 24 / 12 | 1 | 240 | 48 | 1x780 |
| mx3d-32x320 | 32 / 16 | 2 | 320 | 64 | 2x520 |
| mx3d-48x480 | 48 / 24 | 2 | 480 | 96 | 2x780 |
| mx3d-64x640 | 64 / 32 | 2 | 640 | 128 | 2x1024 |
| mx3d-96x960 | 96 / 48 | 2 | 960 | 192 | 2x1560 |
| mx3d-128x1280 | 128 / 64 | 2 | 1280 | 200 | 2x2080 |
| mx3d-176x1760 | 176 / 88 | 2 | 1760 | 200 | 2x2860 |
{: caption="Memory mx3d Beta profile options for x86-64 instances " caption-side="bottom"}
{: #memory-intel-x86-64}
{: tab-title="mx3d"}
{: tab-group="Memory"}
{: class="simple-tab-table"}
{: summary="Memory mx3d profile options for Intel x86-64 virtual server instances."}

| Instance profile | vCPU / Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|
| mx2-2x16 | 2 / 1 | 16 | 4 | - |
| mx2d-2x16 | 2 / 1 | 16 | 4 | 1x75 |
| mx2-4x32 | 4 / 2 | 32 | 8 | - |
| mx2d-4x32 | 4 / 2 | 32 | 8 | 1x150 |
| mx2-8x64 | 8 / 4 | 64 | 16 | - |
| mx2d-8x64 | 8 / 4 | 64 | 16 | 1x300 |
| mx2-16x128 | 16 / 8 | 128 | 32 | - |
| mx2d-16x128 | 16 / 8 | 128 | 32 | 1x600 |
| mx2-32x256 | 32 / 16 | 256 | 64 | - |
| mx2d-32x256 | 32 / 16 | 256 | 64 | 2x600 |
| mx2-48x384 | 48 / 24 | 384 | 80 | - |
| mx2d-48x384 | 48 / 24 | 384 | 80 | 2x900 |
| mx2-64x512| 64 / 32 | 512 | 80 | - |
| mx2d-64x512| 64 / 32 | 512 | 80 | 2x1200 |
| mx2-96x768| 96 / 48 | 768 | 80 | - |
| mx2d-96x768| 96 / 48 | 768 | 80 | 2x1800 |
| mx2-128x1024| 128 / 64 | 1024 | 80 | - |
| mx2d-128x1024| 128 / 64 | 1024 | 80 | 2x2400 |
{: caption="Memory mx2 profile options for x86-64 instances " caption-side="bottom"}
{: #memory-intel-x86-64b}
{: tab-title="mx2"}
{: tab-group="Memory"}
{: class="simple-tab-table"}
{: summary="Memory mx2 profile options for Intel x86-64 virtual server instances."}

## Very High Memory
{: #vhmemory}

Very High Memory profiles offer 1 vCPU to 14 GiB of RAM to host small to medium-sized in-memory databases, OLAP services such as SAP NetWeaver, and other memory intensive applications. All Very High Memory profiles are provisioned with temporary SSD-backed [instance storage](/docs/vpc?topic=vpc-instance-storage) at no additional charge. The following Very High Memory profiles are available on Intel&reg; x86 processors.

The 3rd generation profile with the vx3d prefix is available in the Chennai (`in-che`), Toronto (`ca-tor`) and Washington DC (`us-east`) regions to provision virtual server instances on 4th Generation Intel&reg; Xeon&reg; Scalable processors, the Intel 8474C processor (previously code named Sapphire Rapids). For more information about the capabilities of the new profiles, see [Next generation instance profiles](#next-gen-profiles).
{: preview}

- The vx2d profiles are on the Cascade Lake processors.
- The vx3d profiles are on the Sapphire Rapids processors.

| Instance profile | vCPU | Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|---------|
| vx3d-2x32 | 2 | 1 | 32 | 4 | 1x65 |
| vx3d-4x64| 4 |  2 | 64 | 8 | 1x130 |
| vx3d-8x128 | 8 | 4 | 128 | 16 | 1x260 |
| vx3d-16x256| 16 | 8 | 256 | 32 | 1x520 |
| vx3d-24x384| 24 | 12 | 384 | 48 | 1x780 |
| vx3d-32x512 | 32 | 16 | 512 | 64 | 2x520 |
| vx3d-48x768 | 48 | 24 | 768 | 96 | 2x780 |
| vx3d-64x1024 | 64 | 32 | 1024 | 128 | 2x1024 |
| vx3d-88x1408 | 88 | 44 | 1408 | 176 | 2x1430 |
| vx3d-96x1536 | 96 | 48 | 1536 | 200 | 2x1560 |
| vx3d-128x2048 | 128 | 64 | 2048 | 200 | 2x2080 |
| vx3d-176x2816| 176 | 88 | 2816 | 200 | 2x2860 |
{: caption="Very High Memory profiles options for x86-64 instances" caption-side="bottom"}
{: #vhmemory-intel-x86-64}
{: tab-title="vx3d"}
{: tab-group="Very High Memory"}
{: class="simple-tab-table"}
{: summary="Very High Memory profiles options for Intel x86-64 virtual server instances."}

| Instance profile | vCPU | Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|---------|
| vx2d-2x28 | 2 | 1 | 28 | 4 | 1x60 |
| vx2d-4x56 | 4 |  2 | 56 | 8 | 1x120 |
| vx2d-8x112 | 8 | 4 | 112 | 16 | 1x240 |
| vx2d-16x224| 16 | 8 | 224 | 32 | 1x480 |
| vx2d-44x616 | 44 | 22 | 616 | 80 | 1x1320 |
| vx2d-88x1232 | 88 | 44 | 1232 | 80 | 2x1320 |
| vx2d-144x2016 | 144 | 72 | 2016 | 80 | 2x2160 |
| vx2d-176x2464 | 176 | 88 | 2464 | 80 | 2x2640 |
{: caption="Very High Memory profiles options for x86-64 instances" caption-side="bottom"}
{: #vhmemory-intel-x86-64b}
{: tab-title="vx2d"}
{: tab-group="Very High Memory"}
{: class="simple-tab-table"}
{: summary="Very High Memory profiles options for Intel x86-64 virtual server instances."}

## Ultra High Memory
{: #uhmemory}

Ultra High Memory profiles are hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts. This profile family offers our highest vCPU to memory ratio with 28 GiB of memory for every 1 vCPU of compute and up to 5.7 TiB of available RAM and is optimized for running memory intensive applications and in-memory database such as SAP HANA, Memcached, or Redis. All Very High Memory profiles are provisioned with temporary SSD-backed [instance storage](/docs/vpc?topic=vpc-instance-storage) at no additional charge.

The following Ultra High Memory profiles are available for x86-64 processors:

| Instance profile | vCPU / Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|
| ux2d-2x56 | 2 / 1 | 56 | 4 | 1x60 |
| ux2d-4x112 | 4 / 2 | 112 | 8 | 1x120 |
| ux2d-8x224 | 8 / 4 | 224 | 16 | 1x240 |
| ux2d-16x448 | 16 / 8 | 448 | 32 | 1x480 |
| ux2d-36x1008 | 36 / 18 | 1008 | 64 | 1x1080 |
| ux2d-48x1344| 48 / 24 | 1344 | 80 | 2x720 |
| ux2d-72x2016 | 72 / 36 |  2016 | 80 | 2x1080 |
| ux2d-100x2800 | 100 / 50 | 2800 | 80 | 2x1500 |
| ux2d-200x5600 | 200 / 100 | 5600 | 80 | 2x3000 |
{: caption="Ultra High Memory profiles options for x86-64 instances" caption-side="bottom"}
{: #uhmemory-intel-x86-64}
{: tab-title="ux2d"}
{: tab-group="Ultra High Memory"}
{: class="simple-tab-table"}
{: summary="Ultra High Memory profiles options for Intel x86-64 virtual server instances."}

## GPU
{: #gpu}

The GPU and accelerated profile family includes profiles with and without [instance storage](/docs/vpc?topic=vpc-instance-storage).

- GPU `-v100` profiles include 1 or 2 NVIDIA V100 PCIe 16 GB GPUs. All OS images are supported on these GPU profiles.
- GPU `-l4` profiles include NVIDIA L4 24 GB GPUs.
- GPU `-l40S` profiles include NVIDIA L40S 48 GB GPUs.
- [Select availability]{: tag-green} GPU `-a100p` profiles include NVIDIA A100 Tensor Core 80 GB GPUs.
- GPU `-h100` profiles include [NVIDIA](https://www.nvidia.com/en-us/data-center/hgx/){: external} GPUs. The system is an HGX design. The H100 offering is available in the following regions and zones: London (eu-gb-2), Sydney (au-syd-2), Toronto (ca-tor-3), Madrid (eu-es-3), Washington DC (us-east-3), Tokyo (jp-tok-3), Sao Paulo (br-sao-1), Dallas (us-south-1), and Frankfurt (eu-de-2).
- [Select availability]{: tag-green} GPU `-h200` profiles include [NVIDIA](https://www.nvidia.com/en-us/data-center/hgx/){: external} GPUs. The system is an HGX design. The H200 offering is available in Washington DC (us-east-3), Toronto (ca-tor-3), Frankfurt (eu-de-2), London (eu-gb-2), and Sydney (au-syd-3).
- [Select availability]{: tag-green} GPU `-gaudi3` profiles include the [Intel® Gaudi® 3 AI Accelerator](https://www.intel.com/content/www/us/en/products/details/processors/ai-accelerators/gaudi.html). The Intel Gaudi 3 offering is available in Dallas (us-south-dal12-a), Washington DC (us-east-wdc06-a, us-east-wdc07-a) and Frankfurt (eu-de-fra02-a).
- [Select availability]{: tag-green} GPU `mi300x` profiles include the [AMD Instinct™ MI300X Accelerator](https://www.amd.com/en/products/accelerators/instinct/mi300/mi300x.html).  The AMD Instinct MI300X offering is available for select customers in Washington DC (us-east-wdc06-a, us-east-wdc07-a) and Frankfurt (eu-de-fra02-a, eu-de-fra05-a). Create a [support case](/docs/account?topic=account-open-case&interface=ui) if you are interested in purchasing and using this offering.

Make sure to install the appropriate driver and software for the profile you select:
- [NVIDIA Drivers](https://www.nvidia.com/en-us/drivers/){: external}
- [Intel Gaudi Driver and Software Installation](https://docs.habana.ai/en/latest/Installation_Guide/Driver_Installation.html){: external}
- [Installing AMD ROCm and machine learning frameworks](https://rocm.docs.amd.com/en/latest/how-to/rocm-for-ai/install.html){: external}

| Instance profile | vCPU / Cores | GiB RAM | Type / Number of GPUs | Bandwidth Cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|---------|
| gx3-16x80x1l4 | 16 / 8 | 80 | l4 / 1 | 32 | - |
| gx3-32x160x2l4 | 32 / 16 | 160 | l4 / 2 | 64 | - |
| gx3-64x320x4l4 | 64 / 32 | 320 | l4 / 4 | 128 | - |
| gx3-24x120x1l40s | 24 / 12 | 120 | l40s / 1 | 50 | - |
| gx3-48x240x2l40s | 48 / 24 | 240 | l40s / 2  | 100 | - |
| gx3d-24x120x1a100p | 24 / 12 | 120 | a100p / 1  | 48 | 780 |
| gx3d-48x240x2a100p | 48 / 24 | 240 | a100p / 2 | 96 | 1560 |
| gx3d-160x1792x8h100 | 160 / 80 | 1792 | h100 / 8 | 200 | 8x7680 |
| gx3d-160x1792x8h200 | 160 / 80 | 1792 | h200 / 8 | 200 | 8x7680 |
| gx3d-160x1792x8gaudi3 | 160 / 80 | 1792 | Gaudi-3 / 8 | 200 | 8x3200 |
| gx3d-208x1792x8mi300x | 208 / 104 | 1792 | MI300X / 8 | 200 | 8x3200 |
{: caption="GPU gx3 profile options for x86-64 instances" caption-side="bottom"}
{: #gpu-intel-x86-64}
{: tab-title="gx3"}
{: tab-group="GPU"}
{: class="simple-tab-table"}
{: summary="GPU gx3 profile options for x86-64 virtual server instances."}

| Instance profile | vCPU / Cores | GiB RAM | Type / Number of GPUs | Bandwidth Cap (Gbps) | Instance storage (GB) |
|---------|---------|---------|---------|---------|---------|
| gx2-8x64x1v100 | 8 / 4 | 64 | v100 / 1 | 16 | - |
| gx2-16x128x1v100 | 16 / 8 | 128 | v100 / 1 | 32 | - |
| gx2-16x128x2v100 | 16 / 8 | 128 | v100 / 2 | 32 | - |
| gx2-32x256x2v100 | 32 / 16 | 256 | v100 / 2 | 64 | - |
{: caption="GPU gx2 16 GB profile options for Intel x86-64 instances" caption-side="bottom"}
{: #gpu-intel-x86-64b}
{: tab-title="gx2"}
{: tab-group="GPU"}
{: class="simple-tab-table"}
{: summary="GPU gx2 16 GB profile options for Intel x86-64 virtual server instances."}

### Considerations for GPU profiles
{: #considerations-gpu-profiles}

When you create a virtual server by using a GPU or accelerated profile, keep the following recommendations in mind.

- During {{site.data.keyword.Bluemix_notm}} periodic maintenance, GPU workloads aren't secure live migrated. Instead, the virtual server instance is restarted. You are notified 30 days in advance of any maintenance where the virtual server instance restarts. For more information, see [Understanding cloud maintenance operations](/docs/vpc?topic=vpc-about-cloud-maintenance).
- If you are using GPU profiles, you need to install the required software and drivers onto your virtual server instance. For more information, see [Managing GPUs and accelerators](/docs/vpc?topic=vpc-managing-gpus).
- For more information about persistent storage options, see [Storage notes for profiles](#block-storage-notes-for-profiles).
- For GPU profiles that contain instance storage, those block devices are attached as native NVMe drives instead of virtio block devices. The ordering of the instance storage drives with respect to storage volumes might differ.

## Storage Optimized
{: #storageopt}

Storage Optimized profiles are hosted exclusively on Intel® Xeon® Platinum Cascade Lake servers. This profile family offers our highest vCPU to [instance storage](/docs/vpc?topic=vpc-instance-storage) ratio with 300 GB of storage for every 1 vCPU and is optimized for running data lake and other workloads that require more intensive data capabilities. All storage optimized profiles are provisioned with temporary SSD-backed instance storage at no additional charge. For more information, see [Lifecycle of instance storage](/docs/vpc?topic=vpc-instance-storage#instance-storage-lifecycle).

Storage Optimized profiles use the `Storage optimized (ox2) instance storage` quota, for instance, storage quota tracking. Unlike other profiles, which use the `Instance storage` quota. For more information, see [Quotas](/docs/vpc?topic=vpc-quotas#vpcquotas).

The following Storage Optimized profiles are available for x86-64 processors:

| Instance profile | vCPU / Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) | Interface type |
|---------|---------|---------|---------|---------|---------|
| ox2-2x16     | 2 / 1 | 16    | 4     | 1x600   | virtio_blk   |
| ox2-4x32     | 4 / 2  | 32    | 8     | 1x1200  | virtio_blk   |
| ox2-8x64     | 8 / 4  | 64    | 16    | 2x1200  | virtio_blk   |
| ox2-16x128   | 16 / 8  | 128   | 32    | 2x2400  | virtio_blk   |
| ox2-32x256   | 32 / 16 | 256   | 64    | 3x3200  | virtio_blk   |
| ox2-64x512   | 64 / 32 | 512   | 80    | 6x3200  | virtio_blk   |
| ox2-96x768   | 96 / 48 | 768   | 80    | 9x3200  | virtio_blk   |
| ox2-128x1024 | 128 / 64 | 1024  | 80    | 12x3200 | virtio_blk   |
{: caption="Storage Optimized profiles options for x86-64 instances" caption-side="bottom"}
{: #storageopt-intel-x86-64}
{: tab-title="ox2"}
{: tab-group="Storage Optimized"}
{: class="simple-tab-table"}
{: summary="Storage Optimized profiles options for Intel x86-64 virtual server instances."}

## Confidential computing profiles
{: #confidential-computing-profiles}

[Select availability]{: tag-green}

Confidential computing profiles are available in the Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de) regions. Confidential computing with Intel SGX for VPC is Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de). Confidential computing with Intel TDX for VPC is available only in the Washington DC (us-east) region. If you want to create a virtual server instance with a confidential computing profile and TDX, you can create that virtual server instance only in the Washington DC (us-east) region. You can’t create a virtual server instance with TDX in any other region, including Dallas (us-south) and Frankfurt (eu-de). For more information, see [Confidential computing known issues](/docs/vpc?topic=vpc-known-issues#confidential-computing-vpc-known-issues).
{: preview}

The following profiles support Intel SGX, Intel TDX, and secure boot. If you prefer to use Intel SGX or Intel TDX without secure boot, you can disable the secure boot option. For more information, see [Secure boot for Virtual Servers for VPC](/docs/vpc?topic=vpc-confidential-computing-with-secure-boot-vpc).

For more information about confidential computing, see [Confidential computing for x86 Virtual Servers for VPC](/docs/vpc?topic=vpc-about-confidential-computing-vpc).

| Instance profile | vCPU / Cores | GiB RAM | EPC (SGX) capacity (GiB)  | Bandwidth cap (Gbps) | Instance storage (GB) |
| ---------------- | ---- | ------- | ------------------------- |------- | ------------------------- |
| bx3dc-2x10 | 2 / 1 | 10 | 4 | 4 | 1x65 |
| bx3dc-4x20 | 4 / 2 | 20 | 8 | 8 | 1x130 |
| bx3dc-8x40 | 8 / 4 | 40 | 16 | 16 | 1x260 |
| bx3dc-16x80 | 16 / 8 | 80 | 32 | 32 | 1x520 |
| bx3dc-24x120 | 24 / 12 | 120 | 48 | 48 | 1x780 |
| bx3dc-32x160 | 32 / 16 | 160 | 64 | 64 | 2x520 |
| bx3dc-48x240 | 48 / 24 | 240 | 96 | 96 | 2x780 |
| bx3dc-64x320 | 64 / 32 | 320 | 128 | 128 | 2x1024 |
| bx3dc-96x480 | 96 / 48 | 480 | 192 | 480 | 2x1560 |
{: caption="Balanced profile options for confidential computing compatible virtual server instances" caption-side="bottom"}
{: #balanced-cc-x86-64}
{: tab-title="bx3"}
{: tab-group="Confidential compute"}
{: class="simple-tab-table"}
{: summary="Balanced bx3 profiles for confidential compute compatible virtual server instances."}

| Instance profile | vCPU / Cores | GiB RAM | EPC (SGX) capacity (GiB)  | Bandwidth cap (Gbps) | Instance storage (GB) |
| ---------------- | ---- | ------- | ------------------------- |------- | ------------------------- |
| cx3dc-2x5 | 2 / 1 | 5 | 2 | 4 | 1x65 |
| cx3dc-4x10 | 4 / 2 | 10 | 4 | 8 | 1x130 |
| cx3dc-8x20 | 8 / 4 | 20 | 8 | 16 | 1x260 |
| cx3dc-16x40 | 16 / 8 | 40 | 16 | 32 | 1x520 |
| cx3dc-24x60 | 24 / 12 | 60 | 24 | 48 | 1x780 |
| cx3dc-32x80 | 32 / 16 | 80 | 32 | 64 | 2x520 |
| cx3dc-48x120 | 48 / 24 | 120 | 48 | 96 | 2x780 |
| cx3dc-64x160 | 64 / 32 | 160 | 64 | 128 | 2x1024 |
| cx3dc-96x240 | 96 / 48 | 240 | 96 | 192 | 2x1560 |
| cx3dc-128x320 | 128 / 64 | 320| 128 | 200 | 2x2860|
{: caption="Compute profile options for confidential computing compatible virtual server instances" caption-side="bottom"}
{: #compute-cc-x86-64}
{: tab-title="cx3"}
{: tab-group="Confidential compute"}
{: class="simple-tab-table"}
{: summary="Compute cx3 profile options for confidential compute compatible virtual server instances."}

## Flex profiles
{: #flexible-profiles}

Flex profiles are a cost-effective option to help improve mainstream capacity where and when its needed. As a dedicated core offering, you have cpu-to-memory ratio options for compute, balanced, memory, and two low-memory variants (small and nano). The Flex profile family doesn't include instance storage.

The following flex profiles are available.

| Instance profile | vCPU | Memory (GiB) | Bandwidth cap (Gbps)|
|------------------|------|--------------|---------------------|
| nxf-2x1          | 2    | 1            | 2   |
| nxf-2x2          | 2    | 2            | 2   |
| bxf-2x8          | 2    | 8            | 4   |
| bxf-4x16         | 4    | 16           | 8   |
| bxf-8x32         | 8    | 32           |  16  |
| bxf-16x64        | 16   | 64           | 32  |
| bxf-24x96        | 24   | 96           | 48  |
| bxf-32x128       | 32   | 128          | 64  |
| bxf-48x192       | 48   | 192          | 80  |
| bxf-64x256       | 64   | 256          | 80  |
| cxf-2x4          | 2    | 4            | 4   |
| cxf-4x8          | 4    | 8            | 8   |
| cxf-8x16         | 8    | 16           | 16   |
| cxf-16x32        | 16   | 32           | 32  |
| cxf-24x48        | 24   | 48           | 48  |
| cxf-32x64        | 32   | 64           | 64  |
| cxf-48x96        | 48   | 96           | 80  |
| cxf-64x128       | 64   | 128          | 80  |
| mxf-2x16         | 2    | 16           | 4   |
| mxf-4x32         | 4    | 32           | 8   |
| mxf-8x64         | 8    | 64           | 16   |
| mxf-16x128       | 16   | 128          | 32  |
| mxf-24x192       | 24   | 192          | 64  |
| mxf-48x384       | 48   | 384          | 80  |
| mxf-64x512       | 64   | 512          | 80  |
{: caption="Flex profile options for virtual servers" caption-side="bottom"}

### Burstable-supported Flex profiles
{: #burstable-supported-flex-profiles}

[Beta]{: tag-blue}

With supported Flex profiles, you can enable Burstable CPU capability. Burstable means that Flex virtual server CPUs are configured with the baseline CPUs and can "burst" beyond the baseline when idle host CPU capacity is available. For more information, see [Burstable virtual servers](/docs/vpc?topic=vpc-burstable-virtual-servers).

The following Burstable profiles are available.

| Name      | vCPU | Memory (GiB) | % vCPU share |
|:----------|:-----|:-------------|:------------|
| nxf-1x1   | 1    | 1            | 10% 25% 50% |
| nxf-1x2   | 1    | 2            | 10% 25% 50% |
| nxf-1x4   | 1    | 4            | 25% 50%     |
| nxf-2x1   | 2    | 1            | 10% 25% 50% |
| nxf-2x2   | 2    | 2            | 10% 25% 50% |
| cxf-2x4   | 2    | 4            | 25% 50%     |
| bxf-2x8   | 2    | 8            | 50%         |
| cxf-4x8   | 4    | 8            | 25% 50%     |
| bxf-4x16  | 4    | 16           |  50%        |
| cxf-8x16  | 8    | 16           | 25% 50%     |
| bxf-8x32  | 8    | 32           |  50%        |
| cxf-16x32 | 16   | 32           | 25% 50%     |
| bxf-16x64 | 16   | 64           |  50%        |
{: caption="Burstable-supported Flex profile options for virtual servers" caption-side="bottom"}

## Bandwidth allocation
{: #bandwidth-allocation}

Instance bandwidth is allocated between volume bandwidth and networking bandwidth. The bandwidth capacity (Bandwidth Cap) is determined by the virtual server profile that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a bandwidth cap of 4 Gbps.

### Bandwidth allocation between storage and networking
{: #bandwidth-storage-network}

The initial volume and network bandwidth allocation depends on the bandwidth that is set by the instance profile that you selected. You can also see the bandwidth allocations in the profile information during instance creation in the console. The bandwidth allocation between Storage and Network can be changed on the instance details page after you provision an instance.

For example, for the bx2-2x8 profile, you might have the following bandwidth configuration:

- Storage: 1 Gbps
- Network: 3 Gbps

You can adjust the amount of overall bandwidth that is provided to storage volumes within the overall instance limits. However, both volume and network bandwidth must be at least 500 MBps each. For example, to allow more bandwidth for volumes, you can apportion the bx2-2x8 example in equal allocations:
   - Storage: 2 Gbps
   - Network: 2 Gbps

For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles) and [Adjusting bandwidth allocation by using the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).
{: ui}

For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles) and [Adjusting bandwidth allocation by using the CLI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=cli#adjusting-bandwidth-allocation-cli).
{: cli}

For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles) and [Adjusting total storage bandwidth allocation from the API](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api#adjusting-bandwidth-allocation-api).
{: api}

### Bandwidth allocation with multiple data volumes
{: #bandwidth-multi-volumes}

By default, the storage bandwidth is divided between all attached volumes. To help ensure reasonable boot times, a minimum of 393 MBps is allocated to the primary boot volume. The remaining volume bandwidth can be divided between the attached data volumes proportionally (weighted bandwidth allocation) or shared between the attached data volumes (pooled bandwidth allocation).

When weighted bandwidth allocation is used, the data volumes are assigned bandwidth that is proportional to their maximum bandwidth. The weights are based on the size, IOPS, and bandwidth values of the storage volumes. Each volume can use only the bandwidth that is assigned to it at the time of attachment.

Review the [compute profiles details](/docs/vpc?group=profile-details) to see the volume bandwidth allocation capabilities available. For more information about the bandwidth allocation, see the [Bandwidth allocation for Data Volumes](/docs/vpc?topic=vpc-block-storage-bandwidth&interface=ui#attached-block-vol-bandwidth).

### Bandwidth allocation with multiple network interfaces
{: #bandwidth-multi-vnic}

You can add up to 15 network interfaces for your virtual server instance, depending on the vCPU count that is included in the instance profile.

* 2-16 vCPUs: Up to five network interfaces
* 17-48 vCPUs: Up to 10 network interfaces
* 49 or more vCPUs: Up to 15 network interfaces

With [flex profiles](#flexible-profiles), you can add up to 2 network interfaces for a virtual server instance, depending on the vCPU count that is included in the profile.

* 2-4 vCPUs: 1 network interface
* 8 or more vCPUs: 2 network interfaces

With multiple network interfaces, bandwidth is distributed evenly across the network interfaces that are attached to the virtual server instance.

For more information, see [Managing network interfaces](/docs/vpc?topic=vpc-using-instance-vnics).

## Block Storage volume notes for profiles
{: #block-storage-notes-for-profiles}

When you create the data volumes, you can select a volume profile that best meets your requirements. The second-generation volume profile, `sdp` provides the most flexibility, as you can specify your capacity, IOPS, and throughput maximum values. For more information, see the [SSD defined performance profile](/docs/vpc?topic=vpc-block-storage-profiles#defined-performance-profile). First-generation volume profiles are available as three predefined [tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) or as a [custom profile](/docs/vpc?topic=vpc-block-storage-profiles#custom). These volume profiles relate to virtual server instance profiles:

- A [3 IOPS general-purpose tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provides IOPS/GB performance that is suitable for a virtual server instance Balanced profile.
- A [5-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance that is suitable for a virtual server instance Compute profile.
- A [10-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance that is suitable for a virtual server instance Memory profile.

## Understanding profile naming
{: #profiles-naming-rule}

The following information describes the profile naming rules.

The first character represents the profile families. Different profile families have different ratios of vCPU to memory and other characteristics that are designed for different workloads.

- "n" nano family of profiles
- "b": balanced family of profiles
- "c": compute family of profiles (higher on the CPUs)
- "m": memory family of profiles (higher on the memory)
- "u": ultra high memory family of profiles, 1 vCPU to 28 GiB of memory ratio
- "v": very high memory family of profiles, 1 vCPU to 14 GiB of memory ratio
- "g": GPU profiles, which is a 1:8 or 1:16 ratio
- "o": storage optimized family of profiles, 1 vCPU to 8 GiB memory ratio and 1 vCPU to 300 GB instance storage ratio
- "f": flex profiles offer a broad set of capabilities and scale from 2 vCPUs (1 physical core) up to 64 vCPUs (64 physical cores).

The second character represents the CPU architecture.

- "x": x86_64
- "z": s390x

The third character represents the generation of the IBM Cloud infrastructure where the profile is provisioned, such as a "2" or "3". A value of "f" indicates that the profile is flexible and can provision on any infrastructure generation.

If the fourth character is a "d", such as bx3d, then a defined quantity of instance storage is provisioned with the virtual server.

The characters after "-" represents the number of vCPUs and the size of RAM (GiB). For example, "2x4" means that this profile has 2 vCPU and 4 GiB of RAM.

Using “bx2-4x16” as an example, you can know from the name that it is a balanced profile that provides 4 vCPUs of compute and 16 GiB of memory. The profile is deployed on an x86-based host and is for the second-generation VPC.

## Viewing profile configurations
{: #popular-profiles}

You can view available profile configurations by using the {{site.data.keyword.cloud_notm}} console, CLI, or the API. In the {{site.data.keyword.cloud_notm}} console, you can select from popular profile configurations that support the most common use cases.

### Viewing instance profiles in the console
{: #profiles-using-console}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
2. From the Virtual server instances page, click **New instance**.
3. You can either select a profile configuration from **Popular profiles** or click **All profiles** to view more configurations.

### Viewing instance profiles from the CLI
{: #profiles-using-cli}
{: cli}

To view the list of available instance profiles by using the CLI, run the following command:

```sh
ibmcloud is instance-profiles
```
{: pre}

### Viewing instance profiles with the API
{: #profiles-using-api}
{: api}

To view the list of available instance profiles by using the API, you can call the [List all instance profiles API](/apidocs/vpc#list-instance-profiles).

The following request example lists the available instance profiles. When you call the API, replace the API endpoint and IAM token with the values from your enterprise. For more information about the `$vpc_api_endpoint` and `$iam_token` variables, see the Authentication and Endpoint URLs sections in [Virtual Private Cloud API Introduction](/apidocs/vpc/latest#about-vpc-api).

```sh
curl -X GET \
"$vpc_api_endpoint/v1/instance/profiles?version=2021-02-23&generation=2" \
-H "Authorization: $iam_token"
```
{: codeblock}

## Next generation instance profiles
{: #next-gen-profiles}

The 3rd generation of {{site.data.keyword.cloud_notm}} {{site.data.keyword.vsi_is_short}} are available to provision in all regions. This new generation features virtual server profile families that are hosted exclusively on Intel 4th Generation Xeon Scalable processors to provide the most powerful and performant general-purpose profiles available. These 3rd generation profiles provide the following enhancements:

- Improved performance with DDR 5 memory DIMMs, PCI Gen 5 interconnects, and more memory per vCPU than prior generation profiles.
- A wide variety of profiles sizes with core to memory ratios optimized to maximize performance and economics for intensive workloads.
- Enhanced integrated accelerators that feature AMX-512, AVX, and enhanced crypto acceleration.
- Instances are started by default with Open Virtual Machine Format (OVMF), and run in Unified Extensible Firmware Interface (UEFI) mode for enhanced security.
- Local instance storage is included with all profiles for ease of access to temporary storage and swap space. For more information about the temporary nature of instance storage, see [Lifecycle of instance storage](/docs/vpc?topic=vpc-instance-storage#instance-storage-lifecycle).
- Optional dynamic bandwidth allocation for volume attachments.
- A 3rd generation profile can be resized to a 2nd generation profile. A 2nd generation profile can be resized to a 3rd generation profile. For more information, see [Resizing between Gen 2 and Gen 3 profiles](/docs/vpc?topic=vpc-resizing-an-instance&interface=ui#resizing-instance-generations).

## Intel Hyper-Threading Technology
{: #vpc-intel-hyper-threading-technology}

All Intel&reg; x86-64 servers have Hyper-Threading enabled by default. Intel&reg; Hyper-Threading Technology is a term that describes simultaneous multithreading (SMT). Hyper-Threading Technology splits each physical core into two virtual processors. Hyper-Threading Technology is like taking a wide road with a single lane and making it into two relatively narrower lanes. The two-lane highway provides better service over the single-lane road if traffic is moving slow and fast. Hyper-Threading Technology provides better application performance for File I/O, Network I/O, and other slower operations mixed with CPU-intensive operations. The performance advantage of Hyper-Threading Technology typically ranges in the range 0 - 30% over a single-thread mode. Some applications might also see a drop in performance.

If you want to disable Intel&reg; Hyper-Threading, see [Disabling Intel Hyper-Threading Technology](/docs/vpc?topic=vpc-disabling-hyper-threading).

## Next steps
{: #nextsteps-profiles}

After you choose a profile, you're ready to create an instance.

- [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui)
- [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli#creating-virtual-servers-cli)
- [Creating an instance by using the API](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api)
- [Creating an instance by using Terraform](/docs/vpc?topic=vpc-creating-virtual-servers&interface=terraform#create-instance-terraform)
- [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus)
