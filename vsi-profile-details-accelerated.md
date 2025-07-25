---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-21"

keywords: vsi, virtual server, virtual server instances, profile, profiles, gpu, accelerated, h100, h200, l4, l40s

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Accelerated instance profiles - Gen 3
{: #accelerated-profile-family}

The accelerated family of profiles provides on-demand, cost-effective access to accelerators and GPUs. GPUs and accelerators help to accelerate the processing time that is required for compute intensive workloads such as AI, machine learning, inferencing, and more.
{: shortdesc}

## AMD MI300X instance profiles
{: #amd-mi300x-profiles}

The AMD MI300X accelerated virtual server profiles are built atop 192 GB OAM-based AMD Instinct™ MI300X Accelerators. These accelerators are tuned for AI workloads, including inferencing and fine tuning. The solution is paired with the 5th Generation Intel® Xeon® Scalable processors.

### Operating systems
{: #amd-mi300x-os}

- Linux

### Processor generation
{: #amd-mi300x-processor}

- Intel® 8570 - 5th Generation Xeon® Scalable processor

### Accelerator
{: #amd-mi300x-accelerator}

- AMD Instinct MI300X Accelerators (192 GB OAM)

### Availability
{: #amd-mi300x-availability}

Status: Select Availability

| Region                    | Universal zone    |
| ------------------------  | -------------     |
| us-east  | `us-east-wdc06-a` |
| us-east  | `us-east-wdc07-a` |
| eu-de    | `eu-de-fra02-a` |
| eu-de    | `eu-de-fra05-a` |
{: caption="Table 1. Supported regions and zones" caption-side="bottom"}

For more information about regions and universal zones, see [Regions](/docs/overview?topic=overview-locations#regions). You can review the assigned zone mapping for an account on the [VPC Infrastructure Overview](/infrastructure/overview#endpoints) page in the Endpoint section. The zone mapping shows how the zone corresponds to the universal zone name that represents the physical location.

### Capabilities
{: #amd-mi300x-capabilities}

- Core type: Dedicated
- Dedicated host: No
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: No
- Instance storage: Yes
- Internal AI Fabric: Yes
- Infinity Fabric™ (XGMI) 128 GB/s GPU-to-GPU connections
- Cluster network capable: No

### VM configuration
{: #amd-mi300x-vm-config}

- Hardware type: q35
- Cloud networking: virtio
- Block boot volume: virtio
- Block data volumes: virtio
- Instance storage: NVMe

### Instance profiles
{: #amd-mi300x-vsi-profiles}

| Instance profile | vCPU / Cores | Memory (GiB)  | Bandwidth cap (Gbps) | Accelerators | Instance storage (GB) |
|---------|---------|---------|---------|---------|--------- |
| gx3d-208x1792x8mi300x | 208 / 104 | 1792 | 200 | 8x AMD MI300X (192 GB) | 8 x 3.2 TB |
{: caption="Accelerated AMD profile options" caption-side="bottom"}

This large profile likely requires that you open a support ticket to request a
[quota increase](/docs/vpc?topic=vpc-quotas). Please review your quota levels,
and determine if the account provisioning the resource requires a change to the quotas. Note that
this server utilizes vCPU, RAM, instance storage and GPU quotas.
{: important}

### Limits
{: #amd-mi300x-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Profile | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| gx3d-208x1792x8mi300x | 12          | 15        |
{: caption="Accelerated AMD family limits for maximum volumes, and maximum network interfaces" caption-side="bottom"}

## Intel Gaudi 3 instance profiles
{: #gaudi-3-profiles}


The Intel Gaudi 3 accelerated virtual server profiles are built atop 128 GB OAM-based Intel Gaudi 3 AI Accelerators. These accelerators are tuned for AI workloads, including inferencing and fine tuning. The solution is paired with the 5th Generation Intel® Xeon® Scalable processors.

### Operating systems
{: #gaudi-3-os}

- Linux

### Processor generation
{: #gaudi-3-processor}

- Intel 8568Y+ - 5th Generation Xeon® Scalable processor

### Accelerator
{: #gaudi-3-accelerator}

- Intel Gaudi 3 AI Accelerator (128 GB OAM)


### Availability
{: #gaudi-3-availability}


Status: Select Availability

| Region                    | Universal zone    |
| ------------------------  | -------------     |
| us-south | `us-south-dal12-a`                 |
| us-east  | `us-east-wdc06-a`, `us-east-wdc07-a` |
| eu-de    | `eu-de-fra02-a`                    |
{: caption="Table 1. Supported regions and zones" caption-side="bottom"}

For more information about regions and universal zones, see [Regions](/docs/overview?topic=overview-locations#regions). You can review the assigned zone mapping for an account on the [VPC Infrastructure Overview](/infrastructure/overview#endpoints) page in the Endpoint section. The zone mapping shows how the zone corresponds to the universal zone name that represents the physical location.

### Capabilities
{: #gaudi-3-capabilities}

- Core type: Dedicated
- Dedicated host: No
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: No
- Instance storage: Yes
- Internal AI Fabric: Yes
   - 21 x 200 GbE for OAM-to-OAM connections
- Cluster network capable: No

### VM configuration
{: #gaudi-3-vm-config}

- Hardware type: q35
- Cloud networking: virtio
- Block boot volume: virtio
- Block data volumes: virtio
- Instance storage: NVMe

### Instance profiles
{: #gaudi-3-vsi-profiles}

| Instance profile | vCPU / Cores | Memory (GiB)  | Bandwidth cap (Gbps) | Accelerators | Instance storage (GB) |
|---------|---------|---------|---------|---------|--------- |
| gx3d-160x1792x8gaudi3 | 160 / 80 | 1792 | 200 | 8x Intel Gaudi-3 (128 GB) | 8 x 3.2 TB |
{: caption="Accelerated Intel profile options" caption-side="bottom"}

This large profile likely requires that you open a support ticket to request a
[quota increase](/docs/vpc?topic=vpc-quotas). Please review your quota levels,
and determine if the account provisioning the resource requires a change to the quotas. Note that
this server utilizes vCPU, RAM, instance storage and GPU quotas.
{: important}

### Limits
{: #gaudi-3-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Profile | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| gx3d-160x1792x8gaudi3 | 12          | 15        |
{: caption="Accelerated Intel family limits for maximum volumes, and maximum network interfaces" caption-side="bottom"}


## NVIDIA Hopper HGX instance profiles
{: #hopper-hgx-profiles}

The Hopper-based Accelerated virtual server profiles are built atop NVIDIA H100 and H200 accelerators. These
accelerators are tuned for AI workloads, including inferencing, fine-tuning, and large-scale
training. The solution is paired with the 4th Generation Intel® Xeon® Scalable processors.

This solution also runs with {{site.data.keyword.cloud}} cluster networks. The cluster network implementation for the
Hopper generation of accelerators runs atop eight accelerated NICs, providing a total aggregate
cluster throughput of 3.2 Tbps. The solution also provides RoCEv2 to support RDMA-based workloads. For more information, see [About cluster networks](/docs/vpc?topic=vpc-about-cluster-network).

### Operating systems
{: #hopper-hgx-os}

- Linux

### Processor generation
{: #hopper-hgx-processor}

- Intel 8474C - 4th Generation Xeon® Scalable processor

### Accelerator
{: #hopper-hgx-accelerator}

- NVIDIA H100 SXM5 (80 GB)
- NVIDIA H200 SXM5 (141 GB)

### Availability
{: #hopper-hgx-availability}

#### NVIDIA H100 SXM5 (80 GB)
{: #hopper-hgx-availability-H100}

Status: Select Availability

| Region                    | Universal zone    | Cluster network |
| ------------------------  | -------------     | --------------- |
| Dallas (`us-south`)       | `us-south-dal10-a` | No             |
| Washington DC (`us-east`) | `us-east-wdc07-a` | Yes             |
| Toronto (`ca-tor`)        | `ca-tor-tor05-a`  | No              |
| Sao Paulo (`br-sao`)      | `br-sao-sao01-a`  | No              |
| Frankfurt (`eu-de`)       | `eu-de-fra04-a`   | Yes             |
| London (`eu-gb`)          | `eu-gb-lon05-a`   | No              |
| Madrid (`eu-es`)          | `eu-es-mad05-a`   | No              |
| Sydney (`au-syd`)         | `au-syd-syd04-a`  | No              |
| Tokyo (`jp-tok`)          | `jp-tok-tok05-a`  | No              |
| Osaka (`jp-osa`)          | Not Available     | No              |
{: caption="Table 1. Supported regions and zones" caption-side="bottom"}

For more information about regions and universal zones, see [Regions](/docs/overview?topic=overview-locations#regions). You can review the assigned zone mapping for an account on the [VPC Infrastructure Overview](/infrastructure/overview#endpoints) page in the Endpoint section. The zone mapping shows how the zone corresponds to the universal zone name that represents the physical location.

#### NVIDIA H200 SXM5 (141 GB)
{: #hopper-hgx-availability-H200}

Status: Select Availability

| Region                    | Universal zone    | Cluster network |
| ------------------------  | -------------     | --------------- |
| Washington DC (`us-east`) | `us-east-wdc07-a` | Yes             |
| Toronto (`ca-tor`)        | `ca-tor-tor05-a`  | No              |
| Frankfurt (`eu-de`)       | `eu-de-fra04-a`   | Yes             |
| London (`eu-gb`)          | `eu-gb-lon05-a`   | No              |
| Sydney (`au-syd`)         | `au-syd-syd04-a`  | No              |
{: caption="Table 1. Supported regions and zones" caption-side="bottom"}

For more information about regions and universal zones, see [Regions](/docs/overview?topic=overview-locations#regions). You can review the assigned zone mapping for an account on the [VPC Infrastructure Overview](/infrastructure/overview#endpoints) page in the Endpoint section. The zone mapping shows how the zone corresponds to the universal zone name that represents the physical location.

### Capabilities
{: #hopper-hgx-capabilities}

- Core type: Dedicated
- Dedicated host: No
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: No
- Instance storage: Yes
- NVLink: Yes (900 GB/s)
- [NVIDIA GPUDirect Capable](https://developer.nvidia.com/gpudirect): Yes
- Cluster network capable: Yes (limited regions)
   - Bandwidth: 3.2 Tbps (8x 400 Gbps)
   - Type: Dedicated

### VM configuration
{: #hopper-hgx-vm-config}

- Hardware type: q35
- Cloud networking: virtio
- Cluster networking: SR-IOV
   - Type: NVIDIA CX-7 – Virtual Function
   - Quantity: 8x Dedicated 400 Gbps Physical NICs
- Block boot volume: virtio
- Block data volumes: virtio
- Instance storage: NVMe

### Instance profiles
{: #hopper-hgx-vsi-profiles}

| Instance profile | vCPU / Cores | Memory (GiB)  | Bandwidth cap (Gbps) | Dedicated cluster network bandwidth | Accelerators | Instance storage (GB) |
|---------|---------|---------|---------|---------|--------- | ---------|
| gx3d-160x1792x8h100 | 160 / 80 | 1792 | 200 | 3.2 Tbps (8x 400 Gbps Dedicated NVIDIA CX-7) | 8x NVIDIA H100 (80 GB) | 8 x 7.68 TB |
| gx3d-160x1792x8h200 | 160 / 80 | 1792 | 200 | N/A | 8x NVIDIA H200 (141 GB) | 8 x 7.68 TB |
{: caption="Accelerated NVIDIA Hopper HGX profile options" caption-side="bottom"}

The large profiles likely require that you open a support ticket to request a
[quota increase](/docs/vpc?topic=vpc-quotas). Please review your quota levels,
and determine if the account provisioning the resource requires a change to the quotas. Note that
this server utilizes vCPU, RAM, instance storage and GPU quotas.
{: important}

### Limits
{: #hopper-hgx-vsi-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="Accelerated NVIDIA Hopper HGX limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

If you configure an RDMA-enabled cluster network, you must have either 8, 16 or 32 cluster
network interfaces available. Having the correct number of cluster network interfaces available helps ensure proper distribution of the network interfaces across the underlying
physical infrastructure. Most users typically use only 8. The cluster network interfaces can be configured only when the instance is powered off.
{: note}

## NVIDIA L4 instance profiles
{: #l4-profiles}

The virtual server profiles are built atop NVIDIA L4 accelerators. These accelerators are tuned for graphics
workloads. The solution is paired with the 4th Generation Intel® Xeon® Scalable processors.

### Operating systems
{: #l4-os}

- Windows
- Linux

### Processor generation
{: #l4-processor}

- Intel 8474C - 4th Generation Xeon® Scalable processor

### Accelerator
{: #l4-accelerator}

- NVIDIA L4 GPU (24 GB)

### Availability
{: #l4-availability}

Status: Generally Available

Regions:
- Americas
   - Sao Paulo (`br-sao`)
   - Toronto (`ca-tor`)
   - Dallas (`us-south`)
   - Washington DC (`us-east`)
- Europe
   - Frankfurt (`eu-de`)
   - London (`eu-gb`)
   - Madrid (`eu-es`)
- Asia Pacific
   - Sydney (`au-syd`)
   - Tokyo (`jp-tok`)

### Capabilities
{: #l4-capabilities}

- Core type: Dedicated
- Dedicated host: No
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: No
- Instance storage: No
- NVLink: No

### VM configuration
{: #l4-vm-config}

- Hardware type: i440fx
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based instances
- Block data volumes: virtio

### Instance profiles
{: #l4-vsi-profiles}

|                | vCPUs / Cores | Memory (GiB) | Bandwidth Cap (Gbps) | Accelerators         |
| -------------- | ------------- | ------------ | -------------------- | -------------------- |
| gx3-16x80x1l4  | 16 / 8        | 80           | 32                   | 1x NVIDIA L4 (24 GB) |
| gx3-32x160x2l4 | 32 / 16       | 160          | 64                   | 2x NVIDIA L4 (24 GB) |
| gx3-64x320x4l4 | 64 / 32       | 320          | 128                  | 4x NVIDIA L4 (24 GB) |
{: caption="Accelerated l4 profile options" caption-side="bottom"}

### Limits
{: #l4-vsi-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="Accelerated L4 limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

## NVIDIA L40S instance profiles
{: #l40s-profiles}

The L40s profiles are built atop NVIDIA L40s accelerators. These accelerators are tuned for
graphics and inferencing workloads. The solution is paired with the 4th Generation Intel® Xeon®
Scalable processors.

### Operating systems
{: #l40s-os}

- Windows
- Linux

### Processor generation
{: #l40s-processor}

- Intel 8474C - 4th Generation Xeon® Scalable processor

### Accelerator
{: #l40s-accelerator}

- NVIDIA L40s GPU (48 GB)

### Availability
{: #l40s-availability}

Status: Generally Available

Regions:
- Americas
   - Sao Paulo (`br-sao`)
   - Toronto (`ca-tor`)
   - Dallas (`us-south`)
   - Washington DC (`us-east`)
- Europe
   - Frankfurt (`eu-de`)
   - London (`eu-gb`)
   - Madrid (`eu-es`)
- Asia Pacific
   - Sydney (`au-syd`)
   - Tokyo (`jp-tok`)

### Capabilities
{: #l40s-capabilities}

- Core type: Dedicated
- Dedicated host: No
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: No
- Instance storage: No
- NVLink: No

### VM configuration
{: #l40s-vm-config}

- Hardware type: i440fx
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based instances
- Block data volumes: virtio

### Instance profiles
{: #l40s-vsi-profiles}

| Instance profile  | vCPUs / Cores | Memory (GiB) | Bandwidth Cap (Gbps) | Accelerators           |
| ----------------- | ------------- | ------------ | -------------------- | ---------------------- |
| gx3-24x120x1l40s  | 24 / 12       | 120          | 48                   | 1x NVIDIA L40s (48 GB) |
| gx3-48x240x-2l40s | 48 / 24       | 240          | 96                   | 2x NVIDIA L40s (48 GB) |
{: caption="Accelerated L40s profile options" caption-side="bottom"}

### Limits
{: #l40s-vsi-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="Accelerated L40s limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

## NVIDIA A100 instance profiles
{: #a100-profiles}

The A100 profiles are built atop NVIDIA A100 80 GB accelerators. These accelerators are tuned for
HPC and inferencing workloads. The solution is paired with the 4th Generation Intel® Xeon®
Scalable processors.

### Operating systems
{: #a100-os}

- Windows
- Linux

### Processor generation
{: #a100-processor}

- Intel 8474C - 4th Generation Xeon® Scalable processor


### Accelerator
{: #a100-accelerator}

- NVIDIA A100 Tensor Core GPU (80 GB)

### Availability
{: #a100-availability}

Status: Select Availability

Regions:
- Americas
   - Washington DC (`us-east`)
- Europe
   - Frankfurt (`eu-de`)
   - London (`eu-gb`)
- Asia Pacific
   - Tokyo (`jp-tok`)

### Capabilities
{: #a100-capabilities}

- Core type: Dedicated
- Dedicated host: No
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: No
- Instance storage: Yes
- NVLink: No

### VM configuration
{: #a100-vm-config}

- Hardware type: i440fx
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based instances
- Block data volumes: virtio

### Instance profiles
{: #a100-vsi-profiles}

| Instance profile  | vCPUs / Cores | Memory (GiB) | Bandwidth Cap (Gbps) | Accelerators           |Instance storage (GB) |
| ----------------- | ------------- | ------------ | -------------------- | ---------------------- | -------------------- |
| gx3d-24x120x1a100p | 24 / 12 | 120 |  48 | 1x NVIDIA A100 PCIe (80 GB) | 780 GB |
| gx3d-48x240x2a100p | 48 / 24 | 240 |  96 | 2x NVIDIA A100 PCIe (80 GB) | 1560 GB |
{: caption="Accelerated A100 profile options" caption-side="bottom"}

### Limits
{: #a100-vsi-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="Accelerated A100 limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}
