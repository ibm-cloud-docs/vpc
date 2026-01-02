---

copyright:
  years: 2024, 2026
lastupdated: "2026-01-02"

keywords: vsi, virtual server, virtual server instances, profile, profiles, balanced, compute, memory, dedicated host, gen 2, intel, amd

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# General purpose instance profiles - Gen 2
{: #general-purpose-vsi-profiles-gen2-x86}

The general-purpose 2nd generation virtual server profiles (balanced, compute, and memory) are available to provision virtual servers on both Intel and AMD processor architectures.
{: shortdesc}

## Intel profiles
{: #general-purpose-gen2-intel}

The general-purpose 2nd generation virtual server profiles (balanced, compute, and memory) are built atop the 2nd Generation Intel® Xeon® Scalable processors. They provide a broad set of capabilities and scale from 2 vCPUs (1 physical core) up to 128 vCPUs (64 physical cores).

The general-purpose profiles can also be paired with a corresponding dedicated host. This solution allows
for single-tenant hypervisors that run workloads from a single {{site.data.keyword.cloud}} account.

### Operating systems
{: #general-purpose-os-gen2-intel}

- Linux
- Windows

### Processor generation
{: #general-purpose-processor-gen2-intel}

- Intel - 2nd Generation Xeon® Scalable processors
   - Intel® Xeon® Gold 6248
   - Intel® Xeon® Platinum 8260

### Availability
{: #general-purpose-availability-gen2-intel}

- Status: Generally available
- Regions: All, except for Montreal and Chennai

### Capabilities
{: #general-purpose-capabilities-gen2-intel}

- Core type: Dedicated
- Dedicated host: Yes
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: Yes
- Instance storage: Select profiles
- [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth): `weighted` by default, `pooled` is not supported.

### VM Configuration
{: #general-purpose-vm-config-gen2-intel}

- Hardware type: i440fx
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based virtual server instances
- Block data volumes: virtio
- Instance storage: virtio

### Instance profiles
{: #general-purpose-vsi-profiles-gen2-intel}

#### Balanced
{: #balanced-profiles-gen2-intel}

| Profile      | vCPUs / Cores | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
| ------------ | ------------- | ------------ | -------------------- | --------------------------- |
| bx2-2x8      | 2 / 1         | 8            | 4                    |                             |
| bx2d-2x8     | 2 / 1         | 8            | 4                    | 1x75                        |
| bx2-4x16     | 4 / 2         | 16           | 8                    |                             |
| bx2d-4x16    | 4 / 2         | 16           | 8                    | 1x150                       |
| bx2-8x32     | 8 / 4         | 32           | 16                   |                             |
| bx2d-8x32    | 8 / 4         | 32           | 16                   | 1x300                       |
| bx2-16x64    | 16 / 8        | 64           | 32                   |                             |
| bx2d-16x64   | 16 / 8        | 64           | 32                   | 1x600                       |
| bx2-32x128   | 32 / 16       | 128          | 64                   |                             |
| bx2d-32x128  | 32 / 16       | 128          | 64                   | 2x600                       |
| bx2-48x192   | 48 / 24       | 192          | 80                   |                             |
| bx2d-48x192  | 48 / 24       | 192          | 80                   | 1x900                       |
| bx2-64x256   | 64 / 32       | 256          | 80                   |                             |
| bx2d-64x256  | 64 / 32       | 256          | 80                   | 2x1200                      |
| bx2-96x384   | 96 / 48       | 384          | 80                   |                             |
| bx2d-96x384  | 96 / 48       | 384          | 80                   | 2x1800                      |
| bx2-128x512  | 128 / 64      | 512          | 80                   |                             |
| bx2d-128x512 | 128 / 64      | 512          | 80                   | 2x2400                      |
{: caption="Balanced instance profile options, Gen 2 Intel" caption-side="bottom"}


#### Compute
{: #compute-profiles-gen2-intel}

| Profile      | vCPUs / Cores | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
| ------------ | ------------- | ------------ | -------------------- | --------------------------- |
| cx2-2x4      | 2 / 1         | 4            | 4                    |                             |
| cx2d-2x4     | 2 / 1         | 4            | 4                    | 1x75                        |
| cx2-4x8      | 4 / 2         | 8            | 8                    |                             |
| cx2d-4x8     | 4 / 2         | 8            | 8                    | 1x150                       |
| cx2-8x16     | 8 / 4         | 16           | 16                   |                             |
| cx2d-8x16    | 8 / 4         | 16           | 16                   | 1x300                       |
| cx2-16x32    | 16 / 8        | 32           | 32                   |                             |
| cx2d-16x32   | 16 / 8        | 32           | 32                   | 1x600                       |
| cx2-32x64    | 32 / 16       | 64           | 64                   |                             |
| cx2d-32x64   | 32 / 16       | 64           | 64                   | 2x600                       |
| cx2-48x96    | 48 / 24       | 96           | 80                   |                             |
| cx2d-48x96   | 48 / 24       | 96           | 80                   | 1x900                       |
| cx2-64x128   | 64 / 32       | 128          | 80                   |                             |
| cx2d-64x128  | 64 / 32       | 128          | 80                   | 2x1200                      |
| cx2-96x192   | 96 / 48       | 192          | 80                   |                             |
| cx2d-96x192  | 96 / 48       | 192          | 80                   | 2x1800                      |
| cx2-128x256  | 128 / 64      | 256          | 80                   |                             |
| cx2d-128x256 | 128 / 64      | 256          | 80                   | 2x2400                      |
{: caption="Compute instance profile options, Gen 2 Intel" caption-side="bottom"}

#### Memory
{: #memory-profiles-gen2-intel}

| Profile      | vCPUs / Cores  | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
| ------------- | ------------- | ------------ | -------------------- | --------------------------- |
| mx2-2x16      | 2 / 1         | 16           | 4                    |                             |
| mx2d-2x16     | 2 / 1         | 16           | 4                    | 1x75                        |
| mx2-4x32      | 4 / 2         | 32           | 8                    |                             |
| mx2d-4x32     | 4 / 2         | 32           | 8                    | 1x150                       |
| mx2-8x64      | 8 / 4         | 64           | 16                   |                             |
| mx2d-8x64     | 8 / 4         | 64           | 16                   | 1x300                       |
| mx2-16x128    | 16 / 8        | 128          | 32                   |                             |
| mx2d-16x128   | 16 / 8        | 128          | 32                   | 1x600                       |
| mx2-32x256    | 32 / 16       | 256          | 64                   |                             |
| mx2d-32x256   | 32 / 16       | 256          | 64                   | 2x600                       |
| mx2-48x384    | 48 / 24       | 384          | 80                   |                             |
| mx2d-48x384   | 48 / 24       | 384          | 80                   | 1x900                       |
| mx2-64x512    | 64 / 32       | 512          | 80                   |                             |
| mx2d-64x512   | 64 / 32       | 512          | 80                   | 2x1200                      |
| mx2-96x768    | 96 / 48       | 768          | 80                   |                             |
| mx2d-96x768   | 96 / 48       | 768          | 80                   | 2x1800                      |
| mx2-128x1024  | 128 / 64      | 1024         | 80                   |                             |
| mx2d-128x1024 | 128 / 64      | 1024         | 80                   | 2x2400                      |
{: caption="Memory instance profile options, Gen 2 Intel" caption-side="bottom"}

### Limits
{: #general-purpose-vsi-limits-gen2-intel}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="General purpose profile family limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

### Dedicated host profiles
{: #general-purpose-dh-profiles-gen2-intel}

Dedicated hosts allow users to provision a single-tenant host server and then provision virtual server instances of the corresponding family on the dedicated host.

| Profile            | vCPUs / Cores | Memory (GiB) | Instance storage | Sockets per host (NUMA) | Supported instance types |
| ------------------ | ------------- | ------------ | ---------------- | ----------------------- | ------------------------ |
| bx2-host-152x608   | 152 / 76      | 608          | N/A              | 4                       | bx2 (all)                |
| bx2d-host-152x608  | 152 / 76      | 608          | 5700 GB          | 4                       | bx2d (all)               |
| cx2-host-152x304   | 152 / 76      | 608          | N/A              | 4                       | cx2 (all)                |
| cx2d-host-152x304  | 152 / 76      | 608          | 5700 GB          | 4                       | cx2d (all)               |
| mx2-host-152x1216  | 152 / 76      | 608          | N/A              | 4                       | mx2 (all)                |
| mx2d-host-152x1216 | 152 / 76      | 608          | 5700 GB          | 4                       | mx2d (all)               |
{: caption="General purpose dedicated host profile options, Gen 2 Intel" caption-side="bottom"}

## AMD profiles
{: #general-purpose-gen2-amd}

The general-purpose 2nd generation virtual server profiles are built atop the AMD’s 3rd generation EPYC processors. They provide a broad set of capabilities and scale from 2 vCPUs (1 physical core) up to 228 vCPUs (114 physical cores).

The general-purpose profiles can also be paired with a corresponding dedicated host. This solution allows
for single-tenant hypervisors that run workloads from a single {{site.data.keyword.cloud}} account.

### Operating systems
{: #general-purpose-os-gen2-amd}

- Linux
- Windows

### Processor generation
{: #general-purpose-processor-gen2-amd}

- AMD EPYC 7763

### Availability
{: #general-purpose-availability-gen2-amd}

- Status: Generally available
- Regions: Toronto (`ca-tor`)

### Capabilities
{: #general-purpose-capabilities-gen2-amd}

- Core type: Dedicated
- Dedicated host: Yes
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: Yes
- Instance storage: No

### VM configuration
{: #general-purpose-vm-config-gen2-amd}

- Hardware type: i440fx
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based virtual server instances
- Block data volumes: virtio

### Instance profiles
{: #general-purpose-vsi-profiles-gen2-amd}

#### Balanced
{: #balanced-profiles-gen2-amd}

| Profile      | vCPUs / Cores | Memory (GiB) | Bandwidth cap (Gbps) |
| ------------ | ------------- | ------------ | -------------------- |
| bx2a-2x8     | 2 / 1         | 8            | 4                    |
| bx2a-4x16    | 4 / 2         | 16           | 8                    |
| bx2a-8x32    | 8 / 4         | 32           | 16                   |
| bx2a-16x64   | 16 / 8        | 64           | 32                   |
| bx2a-32x128  | 32 / 16       | 128          | 64                   |
| bx2a-48x192  | 48 / 24       | 192          | 80                   |
| bx2a-64x256  | 64 / 32       | 256          | 80                   |
| bx2a-96x384  | 96 / 48       | 384          | 80                   |
| bx2a-128x512 | 128 / 64      | 512          | 80                   |
| bx2a-228x912 | 228 / 114     | 912          | 80                   |
{: caption="Balanced instance profile options, Gen 2 AMD" caption-side="bottom"}

### Limits
{: #general-purpose-vsi-limits-gen2-amd}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="General purpose profile family limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

### Dedicated host profiles
{: #general-purpose-dh-profiles-gen2-amd}

Dedicated hosts allow users to provision a single-tenant host server and then provision virtual server instances of
the corresponding family on the dedicated host.

| Profile            | vCPUs / Cores | Memory (GiB) | Instance storage | Sockets per host (NUMA) | Supported instance types |
| ------------------ | ------------- | ------------ | ---------------- | ----------------------- | ------------------------ |
| bx2a-host-228x912 | 228 / 114     | 912          | N/A              | 2                       | bx2a (all)               |
{: caption="General purpose dedicated host profile options, Gen 2 AMD" caption-side="bottom"}
