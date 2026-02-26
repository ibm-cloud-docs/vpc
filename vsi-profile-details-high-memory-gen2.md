---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-26"

keywords: vsi, virtual server, virtual server instances, profile, profiles, gpu, very high memory, ultra high memory, high memory

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# High Memory instance profiles - Gen 2
{: #high-memory-vsi-profiles}

The High Memory profiles include Very High Memory profiles and Ultra High Memory profiles. The Very High Memory family is optimized for running small to medium in-memory databases and OLAP workloads, such as SAP BW/4 HANA. The Ultra High Memory family is optimized to run large in-memory databases and OLTP workloads, such as SAP S/4 HANA.
{: shortdesc}

## Very High Memory instance profiles
{: #very-high-memory-profiles}

The very high memory family of 2nd generation virtual server profiles are built atop the 2nd Generation Intel® Xeon® Scalable processors. These profiles provide a broad set of capabilities and scale from 2 vCPUs (1 physical core) up to 176 vCPUs (88 physical cores). The profiles are targeted towards workloads that need a higher amount of memory per core.

The very high memory profiles can also be paired with a corresponding dedicated host. This solution allows for
single tenant hypervisors running workloads from a single {{site.data.keyword.cloud}} account.

### Operating systems
{: #vhm-2-os}

- Linux
- Windows

### Processor generation
{: #vhm-2-processor}

- Intel 2nd Generation Xeon Scalable processors
   - Intel Xeon Gold 6248
   - Intel Xeon Platinum 8260

### Availability
{: #vhm-2-availability}

- Status: Generally Available
- Regions: All, except for Montreal, Mumbai - Airtel, and Chennai - Airtel

### Capabilities
{: #vhm-2-capabilities}

- Core type: Dedicated
- Dedicated host: Yes
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: Yes
- Instance storage: Yes
- [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth): `weighted` by default, `pooled` is not supported.

### VM configuration
{: #vhm-2-vm-config}

- Hardware type: i440fx
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based instances
- Block data volumes: virtio
- Instance storage: virtio

### Instance profiles
{: #vhm-2-vsi-profiles}

| Instance profile | vCPU / Cores | Memory (GiB)  | Bandwidth cap (Gbps) | Instance storage (Quantity x GB) |
|---------|---------|---------|---------|--------- |
| vx2d-2x28     | 2 / 1         | 28           | 4                    | 1x60                        |
| vx2d-4x56     | 4 / 2         | 56           | 8                    | 1x120                       |
| vx2d-8x112    | 8 / 2         | 112          | 16                   | 1x240                       |
| vx2d-16x224   | 16 / 8        | 224          | 32                   | 1x480                       |
| vx2d-44x616   | 44 / 22       | 616          | 80                   | 1x1320                      |
| vx2d-88x1232  | 88 / 44       | 1232         | 80                   | 2x1320                      |
| vx2d-144x2016 | 144 / 72      | 2016         | 80                   | 2x2160                      |
| vx2d-176x2464 | 176 / 88      | 2464         | 80                   | 2x2640                      |
{: caption="Very high memory profile options" caption-side="bottom"}

### Limits
{: #vhm-2-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="Very high memory family limits for maximum volumes, and maximum network interfaces" caption-side="bottom"}

### Dedicated host profiles
{: #vhm-dh-profiles}

Dedicated hosts allow users to provision a single-tenant host server and then provision virtual server instances of the corresponding family on the dedicated host.

| Profile            | vCPUs / Cores | Memory (GiB) | Instance storage | Sockets per host (NUMA) | Supported instance types |
| ------------------ | ------------- | ------------ | ---------------- | ----------------------- | ------------------------ |
| vx2d-host-176x2464 | 176 / 88      | 2464         | 5280 GB          | 4                       | vx2d (all)               |
{: caption="Very high memory profile option for dedicated hosts" caption-side="bottom"}

## Ultra High Memory instance profiles
{: #ultra-high-memory-profiles}

The ultra high memory family of 2nd generation virtual server profiles are built atop the 2nd Generation Intel® Xeon® Scalable processors. These profiles provide a broad set of capabilities and scale from 2 vCPUs (1 physical core) up to 200 vCPUs (100 physical cores). The profiles are meant for high-end, in-memory database workloads such as SAP HANA.

The ultra high memory profiles can also be paired with a corresponding dedicated host. This solution allows for
single tenant hypervisors running workloads from a single {{site.data.keyword.cloud_notm}} account.

### Operating systems
{: #uhm-2-os}

- Linux
- Windows

### Processor generation
{: #uhm-2-processor}

- Intel 8280L - 2nd Generation Xeon Scalable processors

### Availability
{: #uhm-2-availability}

- Status: Generally Available
- Regions: All

### Capabilities
{: #uhm-2-capabilities}

- Core type: Dedicated
- Dedicated host: Yes
- Hyperthreading: Yes (SMT-2)
- Secure boot: No
- Confidential computing: No
- Live migration: Yes
- Instance storage: Yes

### VM configuration
{: #uhm-2-vm-config}

- Hardware type: i440fx
- Cloud networking: virtio
- Block boot volume: virtio
   - Exception: vscsi for Windows-based instances
- Block data volumes: virtio
- Instance storage: virtio

### Instance profiles
{: #uhm-2-vsi-profiles}

| Instance profile | vCPU / Cores | Memory (GiB)  | Bandwidth cap (Gbps) | Instance storage (Quantity x GB) |
|---------|---------|---------|---------|---------|
| ux2d-2x56     | 2 / 1         | 28           | 4                    | 1x60                        |
| ux2d-4x112    | 4 / 2         | 56           | 8                    | 1x120                       |
| ux2d-8x224    | 8 / 2         | 224          | 16                   | 1x240                       |
| ux2d-16x448   | 16 / 8        | 448          | 32                   | 1x480                       |
| ux2d-36x1008  | 36 / 18       | 1008         | 72                   | 1x1080                      |
| ux2d-48x1344  | 48 / 24       | 1344         | 80                   | 2x720                       |
| ux2d-72x2016  | 72 / 36       | 2016         | 80                   | 2x1080                      |
| ux2d-100x2800 | 100 / 50      | 2800         | 80                   | 2x1500                      |
| ux2d-200x5600 | 200 / 100     | 2016         | 80                   | 2x3000                      |
{: caption="Ultra high memory profile options" caption-side="bottom"}

### Limits
{: #uhm-2-vsi-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
| 17-48           | 12          | 10        |
| 49+             | 12          | 15        |
{: caption="Ultra high memory family limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}

### Dedicated host profiles
{: #uhm-dh-profiles}

Dedicated hosts allow users to provision a single-tenant host server and then provision virtual server instances of the corresponding family on the dedicated host.

| Profile            | vCPUs / Cores | Memory (GiB) | Instance Storage | Sockets per host (NUMA) | Supported instance types |
| ------------------ | ------------- | ------------ | ---------------- | ----------------------- | ------------------------ |
| ux2d-host-200x5600 | 200 / 100     | 5600         | 6000 GB          | 4                       | ux2d (all)               |
{: caption="Ultra high memory profile option for dedicated hosts" caption-side="bottom"}
