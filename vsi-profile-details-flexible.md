---

copyright:
  years: 2025, 2026
lastupdated: "2026-11-03"

keywords: virtual server instances, flex profile, flexible profile, virtual server profile

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# General purpose - Flex
{: #flexible-profiles-virtual-servers}

The general purpose Flex virtual server profiles (nano, balanced, compute, and memory) are built atop the 2nd and 4th Generation Intel® Xeon® Scalable processors and AMD’s 3rd generation EPYC processors. You can place Flex profiles on any available generation of these processors in a specified region.
{: shortdesc}

Flex profiles offer a broad set of capabilities and scale from 1 vCPU up to 128 vCPUs.

Virtual servers with a Flex profile are configured with a baseline CPU family regardless of hypervisor host CPU family.

With a Flex profile, you can't select hardware or CPU family. The hardware that is selected is based on hardware availability.
{: note}

## Operating systems
{: #flexible-profiles-operating-systems}

Flex profiles support the following operating systems.

* Linux
* Windows

## Processor generation
{: #flexible-profiles-processor-generation}

* Intel 2nd Generation Xeon® Gold 6248 and Platinum 8260 Scalable processors
* Intel 4th Generation Xeon® 8474C Scalable processor
* AMD 3rd generation EPYC 7763 processor

Keep in mind that you don't have control over which hardware that a Flex profile provisions.

## Availability
{: #flexible-profiles-availability}

* Status: GA
* Regions: All

## Capabilities
{: #flexible-profiles-features-capabilities}

See the following capabilities for Flex profiles.

* Core type: Flex

* Burstable: Yes (with select profiles)
* Dedicated host: No
* Hyperthreading: Yes (SMT-2)
* Secure boot: Yes
* Confidential computing: No
* Live migration: Yes
* Instance storage: No
* NUMA Pinning: Yes (with select profiles)
- NIC capabilities:
    - Max single NIC throughput: up to 25 Gbps
    - Bandwidth pooling: No
* [Volume bandwidth allocation method](/docs/vpc?topic=vpc-block-storage-bandwidth#attached-block-vol-bandwidth): `pooled` by default; it can be updated to `weighted`.

## VM configuration
{: #flexible-profiles-vm-configuration}

See the following list for VM configuration.

* Hardware type: i440fx
* Cloud networking: virtio
* Block boot volume: virtio
   - Exception: vscsi for most Windows-based instances. However, Windows instances that run on second-generation storage volumes, and Windows instances with secure boot enabled that run Windows Server 2022 or newer, use virtio.
* Block data volumes: virtio
* Instance storage: virtio

## Instance profiles
{: #flexible-profiles-instance-profiles}

The following Flex profiles are available when you provision a virtual server instance and are subject to change.

### Nano
{: #nano-profiles-flexible}

| Instance profile | vCPU | Memory (GiB) | Total instance bandwidth (Gbps)| % vCPU share options (default 100%) |
|------------------|------|--------------|-------------------------------|--------------|
| nxf-1x1          | 1    | 1            | 1                             | 10% 25% 50%  |
| nxf-1x2          | 1    | 2            | 1                             | 10% 25% 50%  |
| nxf-1x4          | 1    | 4            | 1                             | 25% 50%      |
| nxf-1x8          | 1    | 8            | 1                             | 50%          |
| nxf-2x1          | 2    | 1            | 2                             | 10% 25% 50%  |
| nxf-2x2          | 2    | 2            | 2                             | 10% 25% 50%  |
{: caption="Nano flex profile options for virtual servers" caption-side="bottom"}

### Balanced
{: #balanced-profiles-flexible}

| Instance profile | vCPU | Memory (GiB) | Total instance bandwidth (Gbps)| % vCPU share options (default 100%) |
|------------------|------|--------------|-------------------------------|--------------|
| bxf-2x8          | 2    | 8            | 4                             | 50% |
| bxf-4x16         | 4    | 16           | 8                             | 50% |
| bxf-8x32         | 8    | 32           | 16                            | 50% |
| bxf-16x64        | 16   | 64           | 32                            | 50% |
| bxf-24x96        | 24   | 96           | 48                            | N/A |
| bxf-32x128       | 32   | 128          | 64                            | N/A |
| bxf-48x192       | 48   | 192          | 80                            | N/A |
| bxf-64x256       | 64   | 256          | 80                            | N/A |
| bxf-96x384       | 96   | 384          | 80                            | N/A |
| bxf-128x512      | 128  | 512          | 80                            | N/A |
{: caption="Balanced flex profile options for virtual servers" caption-side="bottom"}

### Compute
{: #compute-profiles-flexible}

| Instance profile | vCPU | Memory (GiB) | Total instance bandwidth (Gbps)| % vCPU share options (default 100%) |
|------------------|------|--------------|-------------------------------|--------------|
| cxf-2x4          | 2    | 4            | 4                             | 25% 50% |
| cxf-4x8          | 4    | 8            | 8                             | 25% 50% |
| cxf-8x16         | 8    | 16           | 16                            | 25% 50% |
| cxf-16x32        | 16   | 32           | 32                            | 25% 50% |
| cxf-24x48        | 24   | 48           | 48                            | N/A |
| cxf-32x64        | 32   | 64           | 64                            | N/A |
| cxf-48x96        | 48   | 96           | 80                            | N/A |
| cxf-64x128       | 64   | 128          | 80                            | N/A |
| cxf-96x192       | 96   | 192          | 80                            | N/A |
| cxf-128x256      | 128  | 256          | 80                            | N/A |
{: caption="Compute flex profile options for virtual servers" caption-side="bottom"}

### Memory
{: #memory-profiles-flexible}

| Instance profile | vCPU | Memory (GiB) | Total instance bandwidth (Gbps)| % vCPU share options (default 100%) |
|------------------|------|--------------|-------------------------------|--------------|
| mxf-2x16         | 2    | 16           | 4                             | N/A |
| mxf-4x32         | 4    | 32           | 8                             | N/A |
| mxf-8x64         | 8    | 64           | 16                            | N/A |
| mxf-16x128       | 16   | 128          | 32                            | N/A |
| mxf-24x192       | 24   | 192          | 48                            | N/A |
| mxf-32x256       | 32   | 256          | 64                            | N/A |
| mxf-48x384       | 48   | 384          | 80                            | N/A |
| mxf-64x512       | 64   | 512          | 80                            | N/A |
| mxf-96x768       | 96   | 768          | 80                            | N/A |
| mxf-128x1024     | 128  | 1024         | 80                            | N/A |
{: caption="Memory flex profile options for virtual servers" caption-side="bottom"}

The following profiles don't support NUMA pinning but do support secure boot:
{: note}

- `bxf-96x384`
- `bxf-128x512`
- `cxf-96x192`
- `cxf-128x256`
- `mxf-96x768`
- `mxf-128x1024`

## Limits
{: #flexible-profiles-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 1 | 12 | 2 |
| 2–16 | 12 | 5 |
| 17–48 | 12 | 10 |
| 49+ | 12 | 15 |
{: caption="Flex profile family limits for maximum volumes and maximum network interfaces" caption-side="bottom"}

The new vNIC limits apply to any new virtual server instance created with a Flex profile. To take advantage of the new limits on an existing virtual server instance, you can resize the virtual server instance to update with the new vNIC limits.

### Resize an existing virtual server instance to update vNIC limits by using the console
{: #flexible-profiles-vnic-limit-update-ui}
{: ui}

You can use the following process to resize the virtual server instance.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
1. On the **Virtual server instances** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance you want to update.
1. Click **Stop** to stop the virtual server instance.
1. Click **Resize**. You first resize to an interim profile. Select any compatible Flex profile different from your current one, then confirm.
1. Click **Resize** again. You now resize back to your original profile. Select your original profile, and confirm. The virtual server instance starts and is refreshed with the updated maximum vNIC value.
1. On the **Networking** tab, to verify that the maximum allowed count reflects the new value. If it still shows the old value, wait a few minutes for the cache to clear and refresh the page.

### Resize an existing virtual server instance to update vNIC limits by using the CLI
{: #flexible-profiles-vnic-limit-update-cli}
{: cli}

You can update the vNIC limits for an existing virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI). The virtual server instance must be stopped first.

To update the vNIC limits for an existing virtual server instance, use the `ibmcloud is instance-update` command. Specify your virtual server instance ID for the `INSTANCE` and the Flex profile that the virtual server instance is already using (for example, `bxf-8x32`) for the `PROFILE_NAME`.

```sh
ibmcloud is instance-update INSTANCE_ID --profile PROFILE_NAME
```
{: pre}

### Resize an existing virtual server instance to update vNIC limits by using the API
{: #flexible-profiles-vnic-limit-update-api}
{: api}

You can update the vNIC limits for an existing virtual server instance in your {{site.data.keyword.vpc_short}} by using the API. The virtual server instances must be stopped first.

To update the vNIC limits for an existing virtual server instance, make a `PATCH` request, specify the `$instance_id`, and the Flex profile that the virtual server instance is already using (for example, `bxf-8x32`) for `my_profile`.

```sh
curl -X PATCH \
  "https://us-south.iaas.cloud.ibm.com/v1/instances/$instance_id?version=2026-09-14&generation=2" \
  -H "Authorization: Bearer $IAM_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "profile": {
      "name": "my_profile"
    }
  }'
```
{: codeblock}

### Enabling secure boot on Flex profiles
{: #flexible-profiles-secure-boot}

Secure boot on Flex profiles is opt-in. You can enable secure boot independently of any vNIC change. After you enable secure boot, you must fully stop and start the virtual server instance for the change to take effect.

Secure boot is only compatible with UEFI boot mode and is not supported on virtual server instances that use legacy BIOS.
{: important}

For more information, see [Enabling and disabling secure boot](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#disable-secure-boot-ui).
