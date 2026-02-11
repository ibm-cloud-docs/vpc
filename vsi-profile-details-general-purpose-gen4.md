---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-11"

keywords: vsi, virtual server, virtual server instances, profile, profiles, balanced, compute, memory, dedicated host, gen 4

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# General purpose instance profiles - Intel Gen 4
{: #general-purpose-vsi-profiles-gen4-intel}

The general-purpose 4th generation virtual server profiles (balanced, compute, and memory) are built atop the 6th Generation Intel® Xeon® Scalable processors. This generation offers an increased networking speed, larger cache sizes, and improved workload performance.
{: shortdesc}

The 4th generation general-purpose profiles are available in the Dallas (us-south) region.
{: preview}

The general-purpose profiles can also be paired with a corresponding dedicated host. This solution allows
for single-tenant hypervisors that run workloads from a single {{site.data.keyword.cloud}} account.

## Operating Systems
{: #general-purpose-os-gen4}

- Linux
- Windows

## Processor Generation
{: #general-purpose-processor-gen4}

- Intel 6952P - 6th Generation Xeon® Scalable processor

## Availability
{: #general-purpose-availability-gen4}

- Status: Select location availability
- Regions: `us-south`

## Capabilities
{: #general-purpose-capabilities-gen4}

- Core type: Dedicated
- Dedicated host: Yes
- Hyperthreading: Yes (SMT-2)
- Secure boot: Yes (optional)
- Confidential computing: No
- Live migration: Yes
- Instance storage: Yes
- NUMA Pinning: Yes
- NIC Capabilities:
   - Max single NIC throughput: up to 100 Gbps VPC traffic and 32 Gbps external traffic
   - Bandwidth Pooling: Yes
- Volume bandwidth:
   - Default: `pooled`
   - Options: `pooled` or `weighted`

For more information about networking bandwidth allocation for profiles, see [Optimizing network bandwidth allocation for profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles#network-perf-notes-for-profiles). For more information about volume bandwidth, see [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth).

## VM Configuration
{: #general-purpose-vm-config-gen4}

- Hardware type: i440fx
   - Utilizes Q35 hardware type when it's running in secure boot mode
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based virtual server instances
- Block data volumes: virtio
- Instance storage: virtio

## Instance profiles
{: #general-purpose-profiles-gen4-intel}

### Balanced
{: #balanced-profiles-gen4-intel}

| Profile | vCPU / Cores / NUMA domains | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
|---------|---------|---------|---------|---------|
| bx4-2x8      | 2 / 1 / 1    | 8   | 4   | - |
| bx4d-2x8     | 2 / 1 / 1    | 8   | 4   | 1x65 |
| bx4-4x16     | 4 / 2 / 1    | 16  | 8   | - |
| bx4d-4x16    | 4 / 2 / 1    | 16  | 8   | 1x130 |
| bx4-8x32     | 8 / 4 / 1    | 32  | 16  | - |
| bx4d-8x32    | 8 / 4 / 1    | 32  | 16  | 1x260 |
| bx4-16x64    | 16 / 8 / 1   | 64  | 32  | - |
| bx4d-16x64   | 16 / 8 / 1   | 64  | 32  | 1x520 |
| bx4-24x96    | 24 / 12 / 1  | 96  | 48  | - |
| bx4d-24x96   | 24 / 12 / 1  | 96  | 48  | 1x780 |
| bx4-32x128   | 32 / 16 / 2  | 128 | 64  | - |
| bx4d-32x128  | 32 / 16 / 2  | 128 | 64  | 2x520 |
| bx4-48x192   | 48 / 24 / 2  | 192 | 96  | - |
| bx4d-48x192  | 48 / 24 / 2  | 192 | 96  | 2x780 |
| bx4-64x256   | 64 / 32 / 2  | 256 | 128 | - |
| bx4d-64x256  | 64 / 32 / 2  | 256 | 128 | 2x780 |
| bx4-96x384   | 96 / 48 / 2  | 384 | 192 | - |
| bx4d-96x384  | 96 / 48 / 2  | 384 | 192 | 2x780 |
| bx4-128x512  | 128 / 64 / 2 | 512 | 200 | - |
| bx4d-128x512 | 128 / 64 / 2 | 512 | 200 | 2x780 |
| bx4-176x704  | 176 / 88 / 2 | 704 | 200 | - |
| bx4d-176x704 | 176 / 88 / 2 | 704 | 200 | 2x780 |
{: caption="Balanced bx4 profile options for Intel x86-64 instances" caption-side="bottom"}

### Compute
{: #compute-profiles-gen4-intel}

| Profile | vCPU / Cores / NUMA domains | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
|---------|---------|---------|---------|---------|
| cx4-2x4      | 2 / 1 / 1 | 4 | 4 | - |
| cx4d-2x4     | 2 / 1 / 1 | 4 | 4 | 1x65 |
| cx4-4x8      | 4 / 2 / 1 | 8 | 8 | - |
| cx4d-4x8     | 4 / 2 / 1 | 8 | 8 | 1x130 |
| cx4-8x16     | 8 / 4 / 1 | 16 | 16 | - |
| cx4d-8x16    | 8 / 4 / 1 | 16 | 16 | 1x260 |
| cx4-16x32    | 16 / 8 / 1 |  32 | 32 | - |
| cx4d-16x32   | 16 / 8 / 1 | 32 | 32 | 1x520 |
| cx4-24x48    | 24 / 12 / 1 |  48 | 48 | - |
| cx4d-24x48   | 24 / 12 / 1 | 48 | 48 | 1x780 |
| cx4-32x64    | 32 / 16 / 2 | 64 | 64 | - |
| cx4d-32x64   | 32 / 16 / 2 | 64 | 64 | 2x520 |
| cx4-48x96    | 48 / 24 / 2 | 96 | 96 | - |
| cx4d-48x96   | 48 / 24 / 2 | 96 | 96 | 2x780 |
| cx4-64x128   | 64 / 32 / 2 | 128 | 128 | - |
| cx4d-64x128  | 64 / 32 / 2 | 128 | 128 | 2x780 |
| cx4-96x192   | 96 / 48 / 2 | 192 | 192 | - |
| cx4d-96x192  | 96 / 48 / 2 | 192 | 192 | 2x780 |
| cx4-128x256  | 128 / 64 / 2 | 256 | 200 | - |
| cx4d-128x256 | 128 / 64 / 2 | 256 | 200 | 2x780 |
| cx4-176x352  | 176 / 88 / 2 | 352 | 200 | - |
| cx4d-176x352 | 176 / 88 / 2 | 352 | 200 | 2x780 |
{: caption="Compute cx4 profile options for x86-64 instances" caption-side="bottom"}

### Memory
{: #memory-profiles-gen4-intel}

| Profile  | vCPU / Cores / NUMA domains | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
|---------------|---------|---------|---------|---------|
| mx4-2x16      | 2 / 1 / 1    | 16   | 4   | - |
| mx4d-2x16     | 2 / 1 / 1    | 16   | 4   | 1x65 |
| mx4-4x32      | 4 / 2 / 1    | 32   | 8   | - |
| mx4d-4x32     | 4 / 2 / 1    | 32   | 8   | 1x130 |
| mx4-8x64      | 8 / 4 / 1    | 64   | 16  | - |
| mx4d-8x64     | 8 / 4 / 1    | 64   | 16  | 1x260 |
| mx4-16x128    | 16 / 8 / 1   | 128  | 32  | - |
| mx4d-16x128   | 16 / 8 / 1   | 128  | 32  | 1x520 |
| mx4-24x192    | 24 / 12 / 1  | 192  | 48  | - |
| mx4d-24x192   | 24 / 12 / 1  | 192  | 48  | 1x780 |
| mx4-32x256    | 32 / 16 / 2  | 256  | 64  | - |
| mx4d-32x256   | 32 / 16 / 2  | 256  | 64  | 2x520 |
| mx4-48x384    | 48 / 24 / 2  | 384  | 96  | - |
| mx4d-48x384   | 48 / 24 / 2  | 384  | 96  | 2x780 |
| mx4-64x512    | 64 / 32 / 2  | 512  | 128 | - |
| mx4d-64x512   | 64 / 32 / 2  | 512  | 128 | 2x780 |
| mx4-96x768    | 96 / 48 / 2  | 768  | 192 | - |
| mx4d-96x768   | 96 / 48 / 2  | 768  | 192 | 2x780 |
| mx4-128x1024  | 128 / 64 / 2 | 1024 | 200 | - |
| mx4d-128x1024 | 128 / 64 / 2 | 1024 | 200 | 2x780 |
| mx4-176x1408  | 176 / 88 / 2 | 1408 | 200 | - |
| mx4d-176x1408 | 176 / 88 / 2 | 1408 | 200 | 2x780 |
{: caption="Memory mx4 profile options for x86-64 instances " caption-side="bottom"}

## Limits
{: #general-purpose-vsi-limits-gen4}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="General purpose profile family limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

## Dedicated host profiles
{: #general-purpose-dh-profiles-gen4}

Dedicated hosts allow users to provision a single-tenant host server and then provision virtual server instances of
the corresponding family on the dedicated host.

| Profile            | vCPUs / Cores | Memory (GiB) | Instance storage | Sockets per host (NUMA) | Supported instance types |
| ------------------ | ------------- | ------------ | ---------------- | ----------------------- | ------------------------ |
| bx4d-host-372x1442  | 372 / 186      | 1442          | 2x 2.7 TB        | 2                       | bx4, bx4d, cx4, cx4d, mx4, and mx4d |
{: caption="General purpose dedicated host profile option, Gen 4 Intel" caption-side="bottom"}

The bx4d dedicated host type is optimized for both bx4 and bx4d instance profile types and workloads. However, you can also choose to mix and match compute, balanced, and memory optimized profiles on the dedicated host.
{: note}
