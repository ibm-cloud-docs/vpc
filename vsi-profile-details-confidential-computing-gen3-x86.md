---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-19"

keywords: vsi, virtual server, virtual server instances, profile, profiles, balanced, compute, gen 3, intel, confidential computing

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Confidential Computing instance profiles - x86 Gen 3
{: #confidential-computing-vsi-profiles-gen3-x86}

The confidential computing family of 3rd generation {{site.data.keyword.cloud}} VPC virtual server profiles (balanced and compute) are built atop the 4th Generation Intel® Xeon® Scalable processors. The confidential computing profiles provide a broad set of capabilities and scale from 2 vCPUs (1 physical core) up to 128 vCPUs (64 physical cores). These profiles support the Intel SGX confidential
computing capability.
{: shortdesc}

## Operating systems
{: #cc-os-gen3-intel}

- Linux

## Processor generation
{: #cc-processor-gen3-intel}

- Intel 8474C - 4th Generation Xeon Scalable processor

## Availability
{: #cc-availability-gen3-intel}

- SGX status: Select availability
- Regions:
   - Dallas (`us-south`)
   - Washington DC (`us-east`)
   - Frankfurt (`eu-de`)

- TDX Status: Select availability
- Regions:
   - Washington DC (`us-east`)

## Capabilities
{: #cc-capabilities-gen3-intel}

- Core type: Dedicated
- Dedicated host: No
- Hyperthreading: Yes (SMT-2)
- Secure boot: Yes
- Confidential computing: SGX, TDX
- Live migration: No
- Instance storage: Yes
- [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth): `weighted` by default, it can be updated to `pooled`.

## VM configuration
{: #cc-vm-config-gen3-intel}

- Hardware type: q35
- Cloud networking: virtio
- Block boot volume: virtio
- Block data volumes: virtio
- Instance storage: virtio

## Instance profiles
{: #cc-vsi-profiles-gen3-intel}

### Balanced
{: #cc-balanced-profiles-gen3-intel}

| Profile      | vCPUs / Cores / NUMA domains | Memory (GiB) | SGX mode | TDX mode| Bandwidth cap (Gbps) | Instance storage (Qty x GB)|
| ------------ | ---------------------------- | ------------ | ---------|-------- | -------------------- | ---------------------------|
| bx3dc-2x10   | 2 / 1 / 1                    | 10           | 4 GB EPC| 0 GB EPC | 4                    | 1 x 65                     |
| bx3dc-4x20   | 4 / 2 / 1                    | 20           | 8 GB EPC| 0 GB EPC | 8                    | 1 x 130                    |
| bx3dc-8x40   | 8 / 4 / 1                    | 40           | 16 GB EPC| 0 GB EPC | 16                   | 1 x 260                    |
| bx3dc-16x80  | 16 / 8 / 1                   | 80           | 32 GB EPC| 0 GB EPC | 32                   | 1 x 520                    |
| bx3dc-24x120 | 24 / 12 / 1                  | 120          | 48 GB EPC| 0 GB EPC | 48                   | 1 x 780                    |
| bx3dc-32x160 | 32 / 16 / 2                  | 160          | 64 GB EPC| 0 GB EPC | 64                   | 2 x 520                    |
| bx3dc-48x240 | 48 / 24 / 2                  | 240          | 96 GB EPC| 0 GB EPC | 96                   | 2 x 780                    |
| bx3dc-64x320 | 64 / 32 / 2                  | 320          | 128 GB EPC| 0 GB EPC | 128                  | 2 x 1024                   |
| bx3dc-96x480 | 96 / 48 / 2                  | 480          | 192 GB EPC| 0 GB EPC | 192                  | 2 x 1560                   |
{: caption="Confidential computing balanced instance profile options for x86 architecture, Gen 3" caption-side="bottom"}

### Compute
{: #cc-compute-profiles-gen3-intel}

| Profile       | vCPUs / Cores / NUMA domains | Memory (GiB) | SGX mode | TDX mode| Bandwidth cap (Gbps) | Instance storage (Qty x GB)|
| ------------- | ---------------------------- | ------------ | ---------|-------- | -------------------- | ---------------------------|
| cx3dc-2x5     | 2 / 1 / 1                    | 5            | 2 GB EPC| 0 GB EPC |  4                   | 1 x 65                     |
| cx3dc-4x10    | 4 / 2 / 1                    | 10           | 4 GB EPC| 0 GB EPC |  8                   | 1 x 130                    |
| cx3dc-8x20    | 8 / 4 / 1                    | 20           | 8 GB EPC| 0 GB EPC |  16                  | 1 x 260                    |
| cx3dc-16x40   | 16 / 8 / 1                   | 40           | 16 GB EPC| 0 GB EPC |  32                  | 1 x 520                    |
| cx3dc-24x60   | 24 / 12 / 1                  | 60           | 24 GB EPC| 0 GB EPC |  48                  | 1 x 780                    |
| cx3dc-32x80   | 32 / 16 / 2                  | 80           | 32 GB EPC| 0 GB EPC |  64                  | 2 x 520                    |
| cx3dc-48x120  | 48 / 24 / 2                  | 120          | 48 GB EPC| 0 GB EPC |  96                  | 2 x 780                    |
| cx3dc-64x160  | 64 / 32 / 2                  | 160          | 64 GB EPC| 0 GB EPC |  128                 | 2 x 1024                   |
| cx3dc-96x240  | 96 / 48 / 2                  | 240          | 96 GB EPC| 0 GB EPC |  192                 | 2 x 1560                   |
| cx3dc-128x320 | 128 / 64 / 2                 | 320          | 128 GB EPC| 0 GB EPC |  200                 | 2 x 2860                   |
{: caption="Confidential computing compute instance profile options for x86 architecture, Gen 3" caption-side="bottom"}

- These profiles configure EPC memory when used in SGX mode only. In TDX mode the EPC memory is not configured.
- Any profile with more than 120 GB memory does not support TDX mode.

## Limits
{: #cc-vsi-limits-gen3-intel}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="Confidential computing profile family limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

### Boot volume profiles
{: #cc-volume-profiles}

In the current release of {{site.data.keyword.block_storage_is_short}} offering, only first-generation volumes from the tiered and custom volume profile families can be used as boot volumes for the Confidential Computing instances if secure boot is needed. Second-generation boot volumes with the `sdp` profile do not support secure boot.

## SGX limitations
{: #cc-vsi-sgx-limitations}

- Windows guest not supported

## TDX limitations
{: #cc-vsi-tgdx-limitations}

- Windows guest not supported

   Windows guest operating systems do not support TDX natively.

- VNC not supported

   The data that flows to the VNC console from the TDX virtual server is facilitated by the cloud provider. However, the cloud provider is not a trusted entity from the customer point of view. Since the data is exposed to the cloud provider, the TDX virtual server disables VNC.

- Forced reboot leads to virtual server shutdown

   For security reasons TDX virtual servers cannot be reset without terminating the virtual server. A forced reboot that is invoked from the control plane resets the virtual server, effectively terminating it. However, this behavior can be masked by the control plane by automatically starting the virtual server. The control plane is enhanced to run the automatic restart.
