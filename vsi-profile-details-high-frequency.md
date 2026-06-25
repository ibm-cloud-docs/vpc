---

copyright:
  years: 2025, 2026
lastupdated: "2026-06-25"

keywords: vsi, virtual server, virtual server instances, profile, profiles, amd, turin, chiplet, epyc
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# High Frequency profiles -  Gen 4
{: #high-frequency-profile-family}

The high frequency profile family provides access to CPUs with elevated clock speeds that are tailored for compute-intensive workloads. These workloads include Electronic Design Automation (EDA), Computational Fluid Dynamics (CFD), Finite Element Analysis (FEA), and weather or climate modeling. These higher frequencies boost per-core performance, by enabling faster execution, and more efficient use of software licensed on a per-core basis.
{: shortdesc}

You can customize the threads_per_core value for these profiles. The default value of `2` enables simultaneous multi-threading.. You can change this to `1` which enables the virtual server instance to receive the full core performance without SMT overhead. Only one vCPU from each core pair is allocated and pinned to the virtual server instance. The other vCPU from the core pair remains unused. Because the actual number of cores provided to the virtual server instance remains the same, billing is unchanged. For more information, see [Editing Threads per core](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#edit-threads-per-core-ui-vpc).

## AMD hx4da instance profiles
{: #amd-hx4da-profiles}

The AMD&reg; hx4da accelerated virtual server profiles are built atop AMD&reg; 5th Generation Epyc 9575F. This processor offers all-core boost speeds up to 4.5 GHz and a max turbo speed of 5 GHz. AMD&reg; 5th Generation Epyc 9575F processor is a Chiplet-based architecture and uses distributed L3 cache. Distributed L3 cache can ensure more dedicated L3 per CPUs.

The Gen 4 High Frequency 1.4 TB RAM hx4da profiles are available only in the Dallas (us-south) region. All other hx4a and hx4da profiles are available in both the Dallas (us-south) and Sydney (au-syd) regions. All profiles have the AMD 5th Generation Epyc 9575F processor-base to provision virtual server instances.
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

For more information about regions and zones, see [Regions](/docs/overview?topic=overview-locations#regions). You can review the assigned zone mapping for an account on the [VPC Infrastructure Overview](/infrastructure/overview#endpoints) page in the Endpoint section.

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

The following table lists available AMD hx4da and hx4a profiles.

| Instance profile | vCPU / Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) | NUMA Count |
| ---------------- | ---- | ------- | ------- | ------- | ---- |
| hx4da-16x32 |	16 / 8 | 32 |	32 | 520 | - |
| hx4da-16x64 |	16 / 8 | 64 |	32 |	520 | - |
| hx4da-16x128 | 16 / 8 |	128 |	32 | 520 | - |
| hx4da-64x680 | 32/64 | 680 | 200 | 2600 | 2 |
| hx4da-128x680 | 64/128 | 680 | 200 | 2600 |	2 |
| hx4da-192x680 |	96/192 | 680 | 200 | 2600 | 2 |
| hx4da-248x680 | 248 / 124 | 680 | 200 | 1x2600 | 2 |
| hx4da-64x1408	| 32/64	| 1408 | 200 | 2600 | 2 |
| hx4da-128x1408 | 64/128 |	1408 | 200 | 2600 | 2 |
| hx4da-192x1408 | 96/192 |	1408 | 200 | 2600 | 2 |
| hx4da-248x1408 | 124/248 | 1408 | 200 | 2600 | 2 |
{: class="simple-tab-table"}
{: caption="High Frequency profiles (hx4da) for compatible virtual server instances" caption-side="bottom"}
{: tab-group="high-frequency-profiles"}
{: #hx4da-profiles}
{: tab-title="hx4da"}


| Instance profile | vCPU / Cores | GiB RAM | Bandwidth cap (Gbps) | Instance storage (GB) | NUMA Count |
| ---------------- | ---- | ------- | ------- | ------- | ---- |
| hx4a-8x16 | 8 / 4 | 16 | 16 |	 - | - |
| hx4a-8x32 |	8 / 4 |	32 |	16 |  - | - |
| hx4a-8x64 |	8 / 4 |	64 |	16 | - | - |
| hx4a-8x128 | 8 / 4 | 128 |	16 | - | - |
| hx4a-16x32 | 16 / 8 |	32 |	32 | - | - |
| hx4a-16x64 | 16 / 8 |	64 | 32 | - | - |
| hx4a-16x128	| 16 / 8 | 128 |	32 | - | - |
{: class="simple-tab-table"}
{: caption="High Frequency profiles (hx4a) for compatible virtual server instances" caption-side="bottom"}
{: tab-group="high-frequency-profiles"}
{: #hx4a-profiles}
{: tab-title="hx4a"}

### Limits
{: #amd-hx4da-limits}

- If you enable and then disable secure boot, the machine type and the PCI or PCIe alignment changes. For more information, see [Secure boot for Virtual Servers for VPC](/docs/vpc?topic=vpc-confidential-computing-with-secure-boot-vpc).
- Second-generation boot volumes with the `sdp` profile do not support secure boot.

#### Boot volume profiles
{: #amd-hx4d-volume-profiles}

For {{site.data.keyword.block_storage_is_short}}, only first-generation volumes from the tiered and custom volume profile families can be used as boot volumes for High Frequnecy profile virtual server instances.
