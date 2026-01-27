---

copyright:
  years: 2026, 2026
lastupdated: "2026-01-27"

keywords: vsi, virtual server, virtual server instances, profile, profiles, gpu, very high memory, high memory, gen 3

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# High Memory instance profiles - Gen 3
{: #very-high-memory-vsi-profiles-gen3}

The very high memory family of 3rd generation virtual server profiles are built atop the 4th Generation Intel® Xeon® Scalable processors. This generation provides NUMA pinning, increased overall instance throughput up to 200 Gbps, and provides secure boot as an option for your virtual server. The profiles are targeted towards workloads that need a higher amount of memory per core.

The very high memory profiles can also be paired with a corresponding dedicated host. This solution allows for single tenant hypervisors running workloads from a single {{site.data.keyword.cloud}} account.

### Operating systems
{: #vhm-3-os}

- Linux
- Windows

### Processor generation
{: #vhm-3-processor}

- Intel 8474C - 4th Generation Xeon Scalable processor

### Availability
{: #vhm-3-availability}

- Status: Select availability
- Regions:
   - Washington DC (`us-east`)
   - Toronto (`ca-tor`)

### Capabilities
{: #vhm-3-capabilities}

- Core type: Dedicated
- Dedicated host: Yes
- Hyperthreading: Yes (SMT-2)
- Secure boot: Yes (*optional*)
- Confidential computing: No
- Live migration: Yes
- Instance storage: Yes
- NUMA pinning: Yes
- NIC Capabilities:
   - Max Single NIC throughput: up to 32 Gbps
   - Bandwidth Pooling: No
- [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth): `weighted` by default, it can be updated to `pooled`.

### VM configuration
{: #vhm-3-vm-config}

- Hardware type: i440fx
   - Utilizes Q35 hardware type when running in secure boot mode.
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based instances
- Block data volumes: virtio
- Instance storage: virtio

### Instance profiles
{: #vhm-3-vsi-profiles}

| Instance profile | vCPU / Cores | Memory (GiB)  | Bandwidth cap (Gbps) | Instance storage (Quantity x GB) |
|---------|---------|---------|---------|--------- |
| vx3d-2x32 | 2 / 1 | 32 | 4 | 1x65 |
| vx3d-4x64| 4 / 2 | 64 | 8 | 1x130 |
| vx3d-8x128 | 8 / 4 | 128 | 16 | 1x260 |
| vx3d-16x256| 16 / 8 | 256 | 32 | 1x520 |
| vx3d-24x384| 24 / 12 | 384 | 48 | 1x780 |
| vx3d-32x512 | 32 / 16 | 512 | 64 | 2x520 |
| vx3d-48x768 | 48 / 24 | 768 | 96 | 2x780 |
| vx3d-64x1024 | 64 / 32 | 1024 | 128 | 2x1024 |
| vx3d-88x1408 | 88 / 44 | 1408 | 176 | 2x1430 |
| vx3d-96x1536 | 96 / 48 | 1536 | 192 | 2x1560 |
| vx3d-128x2048 | 128 / 64 | 2048 | 200 | 2x2080 |
| vx3d-176x2816 | 176 / 88 | 2816 | 200 | 2x2860 |
{: caption="Very high memory profile options" caption-side="bottom"}

### Limits
{: #vhm-3-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="Very high memory family limits for maximum volumes, and maximum network interfaces" caption-side="bottom"}

### Dedicated host profiles
{: #vhm-3-dh-profiles}

Dedicated hosts allow users to provision a single-tenant host server and then provision virtual server instances of the corresponding family on the dedicated host.

| Profile            | vCPUs / Cores | Memory (GiB) | Instance storage | Sockets per host (NUMA) | Supported instance types |
| ------------------ | ------------- | ------------ | ---------------- | ----------------------- | ------------------------ |
| vx3d-host-176x2816 | 176 / 88      | 2816         | 2x3200 GB          | 2                       | vx3d (all)               |
{: caption="Very high memory profile option for dedicated hosts" caption-side="bottom"}
