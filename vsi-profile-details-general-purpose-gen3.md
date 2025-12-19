---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-19"

keywords: vsi, virtual server, virtual server instances, profile, profiles, balanced, compute, memory, dedicated host, gen 3

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# General purpose instance profiles - Intel Gen 3
{: #general-purpose-vsi-profiles-gen3-intel}

The general purpose 3rd generation virtual server profiles (balanced, compute, and memory) are built atop the 4th Generation Intel® Xeon® Scalable processors. This generation provides NUMA pinning and increased overall instance throughput up to 200 Gbps. These profiles also include secure boot virtual servers as an option.
{: shortdesc}

The general purpose profiles can also be paired with a corresponding dedicated host. This solution allows
for single-tenant hypervisors running workloads from a single {{site.data.keyword.cloud}} account.

## Operating Systems
{: #general-purpose-os-gen3}

- Linux
- Windows

## Processor Generation
{: #general-purpose-processor-gen3}

- Intel 8474C - 4th Generation Xeon® Scalable processor

## Availability
{: #general-purpose-availability-gen3}

- Status: Generally available
- Regions: All

## Capabilities
{: #general-purpose-capabilities-gen3}

- Core type: Dedicated
- Dedicated host: Yes
- Hyperthreading: Yes (SMT-2)
- Secure boot: Yes, select profiles
- Confidential computing: No
- Live migration: Yes
- Instance storage: Yes
- NUMA Pinning: Yes
- [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth): `weighted` by default, it can be updated to `pooled`.

## VM Configuration
{: #general-purpose-vm-config-gen3}

- Hardware type: i440fx
   - Utilizes Q35 hardware type when running in secure boot mode
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based virtual server instances
- Block data volumes: virtio
- Instance storage: virtio

## Instance profiles
{: #general-purpose-profiles-gen3-intel}

### Balanced
{: #balanced-profiles-gen3-intel}

| Profile      | vCPUs / Cores / NUMA Domains | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
| ------------ | ---------------------------- | ------------ | -------------------- | --------------------------- |
| bx3d-2x10    | 2 / 1 / 1                    | 10           | 4                    | 1x65                        |
| bx3d-4x20    | 4 / 2 / 1                    | 20           | 8                    | 1x130                       |
| bx3d-8x40    | 8 / 4 / 1                    | 40           | 16                   | 1x260                       |
| bx3d-16x80   | 16 / 8 / 1                   | 80           | 32                   | 1x520                       |
| bx3d-24x120  | 24 / 12 / 1                  | 120          | 48                   | 1x780                       |
| bx3d-32x160  | 32 / 16 / 2                  | 160          | 64                   | 2x520                       |
| bx3d-48x240  | 48 / 24 / 2                  | 240          | 96                   | 2x780                       |
| bx3d-64x320  | 64 / 32 / 2                  | 320          | 128                  | 2x1024                      |
| bx3d-96x480  | 96 / 48 / 2                  | 480          | 192                  | 2x1560                      |
| bx3d-128x640 | 128 / 64 / 2                 | 640          | 200                  | 2x2080                      |
| bx3d-176x880 | 176 / 88 / 2                 | 880          | 200                  | 2x2860                      |
{: caption="Balanced instance profile options, Gen 3 Intel" caption-side="bottom"}

### Compute
{: #compute-profiles-gen3-intel}

| Profile      | vCPUs / Cores / NUMA domains | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
| ------------ | ---------------------------- | ------------ | -------------------- | --------------------------- |
| cx3d-2x5     | 2 / 1 / 1                    | 5            | 4                    | 1x65                        |
| cx3d-4x10    | 4 / 2 / 1                    | 10           | 8                    | 1x130                       |
| cx3d-8x20    | 8 / 4 / 1                    | 20           | 16                   | 1x260                       |
| cx3d-16x40   | 16 / 8 / 1                   | 40           | 32                   | 1x520                       |
| cx3d-24x60   | 24 / 12 / 1                  | 60           | 48                   | 1x780                       |
| cx3d-32x80   | 32 / 16 / 2                  | 80           | 64                   | 2x520                       |
| cx3d-48x120  | 48 / 24 / 2                  | 120          | 96                   | 2x780                       |
| cx3d-64x160  | 64 / 32 / 2                  | 160          | 128                  | 2x1024                      |
| cx3d-96x240  | 96 / 48 / 2                  | 240          | 192                  | 2x1560                      |
| cx3d-128x320 | 128 / 64 / 2                 | 320          | 200                  | 2x2080                      |
| cx3d-176x440 | 176 / 88 / 2                 | 440          | 200                  | 2x2860                      |
{: caption="Compute instance profile options, Gen 3 Intel" caption-side="bottom"}

### Memory
{: #memory-profiles-gen3-intel}

| Profile      | vCPUs / Cores / NUMA domains | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
| ------------- | ------------- | ------------ | -------------------- | --------------------------- |
| mx3d-2x20     | 2 / 1 / 1     | 20           | 4                    | 1x65                        |
| mx3d-4x40     | 4 / 2 / 1     | 40           | 8                    | 1x130                       |
| mx3d-8x80     | 8 / 4 / 1     | 80           | 16                   | 1x260                       |
| mx3d-16x160   | 16 / 8 / 1    | 160          | 32                   | 1x520                       |
| mx3d-24x240   | 24 / 12 / 1   | 240          | 48                   | 1x780                       |
| mx3d-32x320   | 32 / 16 / 2   | 320          | 64                   | 2x520                       |
| mx3d-48x480   | 48 / 24 / 2   | 480          | 96                   | 2x780                       |
| mx3d-64x640   | 64 / 32 / 2   | 640          | 128                  | 2x1024                      |
| mx3d-96x960   | 96 / 48 / 2   | 960          | 192                  | 2x1560                      |
| mx3d-128x1280 | 128 / 64 / 2  | 1280         | 200                  | 2x2080                      |
| mx3d-176x1760 | 176 / 88 / 2  | 1760         | 200                  | 2x2860                      |
{: caption="Memory instance profile options, Gen 3 Intel" caption-side="bottom"}

## Limits
{: #general-purpose-vsi-limits-gen3}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="General purpose profile family limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

### Boot volume profiles
{: #general-purpose-volume-profiles}

In the current release of {{site.data.keyword.block_storage_is_short}} offering, only first-generation volumes from the tiered and custom volume profile families can be used as boot volumes for the Intel Gen 3 instances if secure boot is needed. Second-generation boot volumes with the `sdp` profile do not support secure boot.

## Dedicated host profiles
{: #general-purpose-dh-profiles-gen3}

Dedicated hosts allow users to provision a single-tenant host server and then provision virtual server instances of
the corresponding family on the dedicated host.

| Profile            | vCPUs / Cores | Memory (GiB) | Instance storage | Sockets per host (NUMA) | Supported instance types |
| ------------------ | ------------- | ------------ | ---------------- | ----------------------- | ------------------------ |
| bx3d-host-176x880  | 176 / 88      | 608          | 2x 3.2 TB        | 2                       | bx3d (all)               |
| cx3d-host-176x440  | 176 / 88      | 608          | 2x 3.2 TB        | 2                       | cx3d (all)               |
| mx3d-host-176x1760 | 176 / 88      | 1760         | 2x 3.2 TB        | 2                       | mx3d (all)               |
{: caption="General purpose dedicated host profile options, Gen 3 Intel" caption-side="bottom"}
