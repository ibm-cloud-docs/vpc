---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-20"

keywords: vsi, virtual server, virtual server instances, profile, profiles, balanced, compute, memory, dedicated host, gen 2, s390x architecture, LinuxONE, IBM z, mainframe

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# General purpose instance profiles - s390x Gen 2
{: #general-purpose-profiles-gen2-s390x}

[Deprecated]{: tag-deprecated} The IBM Z or LinuxONE general-purpose instance profiles (balanced, compute, and memory) are built atop the s390x processor architecture, with native integration into {{site.data.keyword.cloud}} VPC. The profiles fit a wide variety of purposes for workloads that require IBM Z resilience, performance, and platform.
{: shortdesc}

The general-purpose profiles can also be paired with a corresponding dedicated host. This solution allows
for single-tenant hypervisors that run workloads from a single {{site.data.keyword.cloud_notm}} account. For s390x, a dedicated host offering is a dedicated logical partition that's running the {{site.data.keyword.cloud_notm}} hypervisor.

## Operating systems
{: #general-purpose-os-gen2-s390x}

- Linux (s390x)

## Processor generation
{: #general-purpose-processor-gen2-s390x}

- IBM Z or LinuxONE

## Availability
{: #general-purpose-availability-gen2-s390x}

- Status: Select geographic availability
- Regions: All

## Capabilities
{: #general-purpose-capabilities-gen2-s390x}

- Dedicated host: Yes
- Secure boot: No
- Confidential computing: No
- Live migration: Yes
- Instance storage: No

## VM configuration
{: #general-purpose-vm-config-gen2-s390x}

- Hardware type: s390-ccw-virtio
- Cloud networking: virtio
- Block boot volume: virtio
- Block data volumes: virtio

## Instance profiles
{: #general-purpose-vsi-profiles-gen2-s390x}

### Balanced
{: #general-purpose-balanced-profiles-gen2-s390x}

| Profile      | vCPUs / Cores | Memory (GiB) | Bandwidth cap (Gbps) |
| ------------ | ------------- | ------------ | -------------------- |
| bz2-1x4   | 1     | 4            | 2                    |
| bz2-2x8   | 2     | 8            | 4                    |
| bz2-4x16  | 4     | 16           | 8                    |
| bz2-8x32  | 8     | 32           | 16                   |
| bz2-16x64 | 16    | 64           | 32                   |
{: caption="Balanced instance profile options, Gen 2 s390x" caption-side="bottom"}

### Compute
{: #general-purpose-compute-profiles-gen2-s390x}

| Profile      | vCPUs / Cores | Memory (GiB) | Bandwidth cap (Gbps) |
| ------------ | ------------- | ------------ | -------------------- |
| cz2-2x4   | 2     | 4            | 4                    |
| cz2-4x8   | 4     | 8            | 8                    |
| cz2-8x16  | 8     | 16           | 16                   |
| cz2-16x32 | 16    | 32           | 32                   |
{: caption="Compute instance profile options, Gen 2 s390x" caption-side="bottom"}

### Memory
{: #general-purpose-memory-profiles-gen2-s390x}

| Profile      | vCPUs / Cores | Memory (GiB) | Bandwidth cap (Gbps) |
| ------------ | ------------- | ------------ | -------------------- |
| mz2-2x16   | 2     | 16           | 4                    |
| mz2-4x32   | 4     | 32           | 8                    |
| mz2-8x64   | 8     | 64           | 16                   |
| mz2-16x128 | 16    | 128          | 32                   |
{: caption="Memory instance profile options, Gen 2 s390x" caption-side="bottom"}

## Limits
{: #general-purpose-vsi-limits-gen2-s390x}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
{: caption="General purpose profile family limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

## Dedicated host profiles
{: #general-purpose-dh-profiles-gen2-s390x}

Dedicated hosts allow users to provision a single-tenant host server and then provision virtual server instances of
the corresponding family on the dedicated host.

| Profile            | vCPUs / Cores | Memory (GiB) | Supported instance types |
| ------------------ | ------------- | ------------ | ---------------- |
| mz2-host-40x320 | 40    | 320          | mz2 (all)                |
{: caption="Dedicated host profile options, Gen 2 s390x" caption-side="bottom"}
