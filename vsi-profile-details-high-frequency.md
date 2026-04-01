---

copyright:
  years: 2025, 2026
lastupdated: "2026-04-01"

keywords: vsi, virtual server, virtual server instances, profile, profiles, amd, turin, chiplet, epyc
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# High frequency profiles -  Gen 4
{: #high-frequency-profile-family}

The high frequency profile family provides access to CPUs with elevated clock speeds that are tailored for compute-intensive workloads. These workloads include Electronic Design Automation (EDA), Computational Fluid Dynamics (CFD), Finite Element Analysis (FEA), and weather or climate modeling. These higher frequencies boost per-core performance, by enabling faster execution, and more efficient use of software licensed on a per-core basis.
{: shortdesc}

## AMD hx4da instance profiles
{: #amd-hx4da-profiles}

The AMD&reg; hx4da accelerated virtual server profiles are built atop AMD&reg; 5th Generation Epyc 9575F. This processor offers all-core boost speeds up to 4.5 GHz and a max turbo speed of 5 GHz. AMD&reg; 5th Generation Epyc 9575F processor is a Chiplet-based architecture and uses distributed L3 cache. Distributed L3 cache can ensure more dedicated L3 per CPUs.

Gen 4 High Frequency profiles are available in the Dallas (us-south) and Sydney (au-syd) regions with the AMD 5th Generation Epyc 9575F processor-based to provision virtual server instances.
{: preview}

### Operating systems
{: #amd-hx4da-os}

- Linux
- Windows

### Processor generation
{: #amd-hx4da-processor}

- AMD E9575F - 5th Generation EPYC® processor

### Availability
{: #amd-hx4da-availability}

Status: Allowlist

The following table lists the available regions and zones for AMD hx4da profiles.

| Region | Zone |
| ----------- | ------------- |
| `us-south`  | `us-south-1` |
| `us-south`  | `us-south-2` |
| `au-syd`   | `au-syd-1` |
| `au-syd`   | `au-syd-2` |
{: caption="Supported regions and zones" caption-side="bottom"}

For more information about regions and universal zones, see [Regions](/docs/overview?topic=overview-locations#regions). You can review the assigned zone mapping for an account on the [VPC Infrastructure Overview](/infrastructure/overview#endpoints) page in the Endpoint section. The zone mapping shows how the zone corresponds to the universal zone name that represents the physical location.

### Capabilities
{: #amd-hx4da-capabilities}

- Core type: Dedicated
- Dedicated host: No
- Hyperthreading: Yes (SMT-2)
- Secure boot: Yes*
- Confidential computing: No
- Live migration: Yes
- Instance storage: Yes
- NUMA Pinning: Yes
- NIC Capabilities:
    - Max Single NIC Throughput: up to 100 Gbps VPC traffic and 32 Gbps external traffic
    - Bandwidth Pooling: Yes
- [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth): `pooled` by default; it can be updated to `weighted`.

For more information about networking bandwidth allocation for profiles, see [Optimizing network bandwidth allocation for profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles#network-perf-notes-for-profiles). For more information about volume bandwidth, see [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth).

Secure boot is supported for Linux but not for Windows
{: note}

### VM configuration
{: #amd-hx4da-vm-config}

- Hardware type: i4440FX/Q35
- Cloud networking: virtio
- Block boot volume: virtio
- Block data volumes: virtio
- Instance storage: virtio

### Instance profiles
{: #amd-hx4da-vsi-profiles}

The following table lists available AMD hx4da profiles.

| Instance profile | vCPU / Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) | NUMA Count |
| ---------------- | ---- | ------- | ------- | ------- | ---- |
| hx4da-248x680 | 248 / 124 | 680 | 200 | 1x2600 | 2 |
{: caption="High frequency profiles for compatible virtual server instances" caption-side="bottom"}

### Limits
{: #amd-hx4da-limits}

- If you enable and then disable secure boot, the machine type and the PCI or PCIe alignment changes. For more information, see [Secure boot for Virtual Servers for VPC](/docs/vpc?topic=vpc-confidential-computing-with-secure-boot-vpc).
- Second-generation boot volumes with the `sdp` profile do not support secure boot.
- The `hx4da` profile doesn't support Flex profiles or Spot instances.

#### Boot volume profiles
{: #amd-hx4d-volume-profiles}

For {{site.data.keyword.block_storage_is_short}}, only first-generation volumes from the tiered and custom volume profile families can be used as boot volumes for hx4d instances.
