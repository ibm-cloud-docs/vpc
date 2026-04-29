---

copyright:
  years: 2026
lastupdated: "2026-04-29"

keywords: vsi, virtual server, virtual server instances, profile, profiles, gpu, accelerated, b300, blackwell, gen 4

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Accelerated (GPU) instance profiles - Gen 4
{: #accelerated-profile-family-gen4}

The Gen 4 accelerated (GPU) family of profiles provides on-demand, cost-effective access to the latest generation accelerators for {{site.data.keyword.vsi_is_full}} (VPC). These accelerators help to reduce the processing time that is required for compute intensive workloads such as AI, machine learning, inferencing, and more.
{: shortdesc}

NVIDIA HGX B300 accelerated virtual server profiles are available for select customers. Create a [support case](/docs/account?topic=account-open-case&interface=ui) if you are interested in purchasing and using this offering.
{: preview}

## NVIDIA B300 instance profile
{: #nvidia-b300-profiles}

The Blackwell-based Accelerated virtual server profiles are built atop the NVIDIA B300 accelerator. These accelerators are tuned for AI workloads, including inferencing, fine-tuning, and large-scale training. The solution is paired with the 6th Generation Intel Xeon Scalable processors.

### Operating systems
{: #nvidia-b300-os}

- Linux

### Processor generation
{: #nvidia-b300-processor}

- Intel® Xeon® 6776P Processor

### Accelerator
{: #nvidia-b300-accelerator}

- NVIDIA B300 SXM6 (288 GB) based on the Blackwell Ultra chip

### Availability
{: #nvidia-b300-availability}

Status: Select Availability

The following table lists the available regions and universal zones for the NVIDIA B300 accelerated virtual server profiles.

| Region                    | Universal zone    |
| ------------------------  | -------------     |
| Washington DC (`us-east`) | `us-east-wdc07-a` |
{: caption="Supported regions and zones" caption-side="bottom"}

For more information about regions and universal zones, see [Regions](/docs/overview?topic=overview-locations#regions). You can review the assigned zone mapping for an account on the [VPC Infrastructure Overview](/infrastructure/overview#endpoints) page in the Endpoint section. The zone mapping shows how the zone corresponds to the universal zone name that represents the physical location.

### Capabilities
{: #nvidia-b300-capabilities}

- Core type: Dedicated
- Dedicated host: No
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: No
- Instance storage: Yes
- NVLink: Yes (1.8 TBps)
- [NVIDIA GPUDirect Capable](https://developer.nvidia.com/gpudirect){: external}: Yes
- NIC capabilities:
    - Max single NIC throughput: up to 200 Gbps VPC traffic and 32 Gbps external
    - Bandwidth Pooling: Yes
- [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth): `pooled` by default; it can be updated to `weighted`.

### VM configuration
{: #nvidia-b300-vm-config}

- Hardware type: q35
- Cloud networking: virtio
- Block boot volume: virtio
- Block data volumes: virtio
- Instance storage: NVMe

### Instance profiles
{: #nvidia-b300-vsi-profiles}

The following table lists the NVIDIA B300 accelerated virtual server profile.

| Instance profile | vCPU / Cores / NUMA | Memory (GiB) | Bandwidth cap (Gbps) | Accelerators | Instance storage (GB) |
|---------|---------|---------|---------|---------|---------|
| gx4d-232x3840x8b300 | 232 / 116 / 2 | 3840 | 200 | 8x NVIDIA B300 (288 GB) | 4 x 7.68 TB |
{: caption="Accelerated NVIDIA B300 profile options" caption-side="bottom"}

This large profile likely requires that you open a support ticket to request a [quota increase](/docs/vpc?topic=vpc-quotas). Review your quota levels, and determine whether the account that's provisioning the resource requires a change to the quotas. This server uses vCPU, RAM, instance storage, and GPU quotas.
{: important}

### Limits
{: #nvidia-b300-vsi-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 49+             | 12          | 15        |
{: caption="Accelerated NVIDIA B300 limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

#### Boot volume profiles
{: #nvidia-b300-volume-profiles}

In the current release of {{site.data.keyword.block_storage_is_short}} offering, only first-generation volumes from the tiered and custom volume profile families can be used as boot volumes for the NVIDIA B300 instances.
