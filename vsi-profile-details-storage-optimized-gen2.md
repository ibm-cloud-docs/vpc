---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-05"

keywords: vsi, virtual server, virtual server instances, profile, profiles, storage optimized, gen 2

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Storage Optimized instance profiles - Gen 2
{: #storage-optimized-vsi-profiles-gen2}

The storage optimized family of 2nd generation virtual server profiles are built atop the 2nd Generation Intel® Xeon® Scalable processors. These profiles provide a broad set of capabilities and scale from 2 vCPUs (1 physical core) up to 128 vCPUs (64 physical cores).
{: shortdesc}

## Operating systems
{: #storage-optimized-os-gen2}

- Linux
- Windows

## Processor generation
{: #storage-optimize-processor-gen2}

- Intel 2nd Generation Xeon Scalable processors
   - Intel Xeon Gold 6248
   - Intel Xeon Platinum 8260

## Availability
{: #storage-optimized-availability-gen2}

Status: Generally available

Regions:
- Americas
   - Toronto (`ca-tor`)
   - Dallas (`us-south`)
   - Washington DC (`us-east`)
- Europe
   - Frankfurt (`eu-de`)
   - London (`eu-gb`)
   - Madrid (`eu-es`)
- Asia Pacific
   - Sydney (`au-syd`)
   - Osaka (`jp-osa`)
   - Tokyo (`jp-tok`)

## Capabilities
{: #storage-optimized-capabilities-gen2}

- Core type: Dedicated
- Dedicated host: Yes
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: Yes
- Instance storage: Yes

## VM configuration
{: #storage-optimized-vm-config-gen2}

- Hardware type: i440fx
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based virtual server instances
- Block data volumes: virtio
- Instance storage: virtio

## Instance profiles
{: #storage-optimized-vsi-profiles-gen2-intel}


| Profile      | vCPUs / Cores  | Memory (GiB) | Bandwidth cap (Gbps) | Instance storage (Qty x GB) |
| ------------ | ---------------------------- | ------------ | -------------------- | --------------------------- |
| ox2-2x16     | 2 / 1         | 16           | 4                    | 1 x 600 GB                  |
| ox2-4x32     | 4 / 2         | 32           | 8                    | 1 x 1.2 TB                  |
| ox2-8x64     | 8 / 4         | 64           | 16                   | 2 x 1.2 TB                  |
| ox2-16x128   | 16 / 8        | 128          | 32                   | 2 x 2.4 TB                  |
| ox2-32x256   | 32 / 16       | 256          | 64                   | 3 x 3.2 TB                  |
| ox2-64x512   | 64 / 32       | 512          | 80                   | 6 x 3.2 TB                  |
| ox2-96x768   | 96 / 48       | 768          | 80                   | 9 x 3.2 TB                  |
| ox2-128x1024 | 128 / 64      | 1024         | 80                   | 12 x 3.2 TB                 |
{: caption="Storage optimized instance profile options, Gen 2 Intel" caption-side="bottom"}

## Limits
{: #storage-optimized-vsi-limits-gen2}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12         | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 12        |
{: caption="Storage optimized profile family limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}
