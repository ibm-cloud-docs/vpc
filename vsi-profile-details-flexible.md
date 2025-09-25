---

copyright:
  years: 2025, 2025
lastupdated: "2025-09-25"

keywords: virtual server instances, flex profile, flexible profile, virtual server profile

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# General purpose - Flex (beta)
{: #flexible-profiles-virtual-servers}

[Beta]{: tag-blue}

Flex profiles are a beta feature and are available for evaluation and testing purposes. Contact [IBM support](/docs/account?topic=account-using-avatar#getting-support) if you're interested in getting access.
{: beta}

The general purpose Flex virtual server profiles (nano, balanced, compute, and memory) are built atop the 2nd and 4th Generation Intel® Xeon® Scalable processors and AMD’s 3rd generation EPYC processors. You can place Flex profiles on any available generation of these processors in a specified region.
{: shortdesc}

Flex profiles offer a broad set of capabilities and scale from 2 vCPUs (1 physical core) up to 64 vCPUs (32 physical cores).

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

* Status: Beta
* Regions: All

## Features and capabilities
{: #flexible-profiles-features-capabilities}

See the following features and capabilities for Flex profiles.

### Capabilities and limitations
{: #flexible-profiles-capabilities-limitations}

The following list shows the capabilities of Flex profiles.

* Core type: Flex
* Dedicated host: No
* Hyperthreading: Yes (SMT-2)
* Secure boot: No
* Confidential computing: No
* TDX: No
* Live migration: Yes
* Instance storage: No
* NUMA Pinning: Yes
* SAP certified: No
* LinunONE (s390x): No
* Counts toward compute quota: Yes
* Bandwidth pooling: No

### Features
{: #flexible-profiles-features}

The following list shows the supported features of Flex profiles.

* Resizable to and away from Flex profiles and between Flex profile types
* Flex profiles can be placed on multiple CPU families, including Intel Cascade Lake, Intel Sapphire Rapids, and AMD Milan.
* 2 Gbps network bandwidth per vCPU for compute, balanced, or memory family profiles
* 1 Gbps network bandwidth per vCPU for nano family profiles 
* Reservations (Burstable virtual servers not supported)
* Instance templates
* Autoscale
* Placement groups
* Metadata
* Lost node remediation (LNR). LNR is supported according to the server's host failure availability policy (stop or restart)
* Live migration

## VM configuration
{: #flexible-profiles-vm-configuration}

See the following list for VM configuration.

* Hardware type: i440fx
* Cloud networking: virtio
* Block boot volume: virtio
* Exception: vscsi for Windows-based virtual server instances
* Block data volumes: virtio
* Instance storage: virtio

## Instance profiles
{: #flexible-profiles-instance-profiles}

The following Flex profiles are available when you provision a virtual server instance and are subject to change.

### Nano
{: #nano-profiles-flexible}

| Instance profile | vCPU | Memory (GiB) | Total instance bandwidth (Gbps)| % vCPU share (beta) |
|------------------|------|--------------|-------------------------------|--------------|
| nxf-1x1          | 1    | 1            | 1                             | 10% 25% 50%  |
| nxf-1x2          | 1    | 2            | 1                             | 10% 25% 50%  |
| nxf-1x4          | 1    | 4            | 1                             | 25% 50%      |
| nxf-1x8          | 1    | 8            | 1                             | 50%          |
| nxf-2x1          | 2    | 1            | 2                             | 10% 25% 50%  |
| nxf-2x2          | 2    | 2            | 2                             | 10% 25% 50% |
{: caption="Nano flex profile options for virtual servers" caption-side="bottom"}

### Balanced
{: #balanced-profiles-flexible}

| Instance profile | vCPU | Memory (GiB) | Total instance bandwidth (Gbps)| % vCPU share (beta) |
|------------------|------|--------------|-------------------------------|--------------|
| bxf-2x8          | 2    | 8            | 4                             | 50%         |
| bxf-4x16         | 4    | 16           | 8                             | 50% |
| bxf-8x32         | 8    | 32           | 16                             | 50% |
| bxf-16x64        | 16   | 64           | 32                            | 50% |
| bxf-24x96        | 24   | 96           | 48                            | - |
| bxf-32x128       | 32   | 128          | 64                            | -|
| bxf-48x192       | 48   | 192          | 80                            | - |
| bxf-64x256       | 64   | 256          | 80                            | - |
{: caption="Balanced flex profile options for virtual servers" caption-side="bottom"}

### Compute
{: #compute-profiles-flexible}

| Instance profile | vCPU | Memory (GiB) | Total instance bandwidth (Gbps)| % vCPU share (beta) |
|------------------|------|--------------|-------------------------------|--------------|
| cxf-2x4          | 2    | 4            | 4                             | 25% 50% |
| cxf-4x8          | 4    | 8            | 8                             | 25% 50% |
| cxf-8x16         | 8    | 16           | 16                             | 25% 50% |
| cxf-16x32        | 16   | 32           | 32                            | 25% 50% |
| cxf-24x48        | 24   | 48           | 48                            | - |
| cxf-32x64        | 32   | 64           | 64                            | - |
| cxf-48x96        | 48   | 96           | 80                            | - |
| cxf-64x128       | 64   | 128          | 80                            | - |
{: caption="Compute flex profile options for virtual servers" caption-side="bottom"}

### Memory
{: #memory-profiles-flexible}

| Instance profile | vCPU | Memory (GiB) | Total instance bandwidth (Gbps)| % vCPU share (beta) |
|------------------|------|--------------|-------------------------------|--------------|
| mxf-2x16         | 2    | 16           | 4                             | - |
| mxf-4x32         | 4    | 32           | 8                             | - |
| mxf-8x64         | 8    | 64           | 16                            | - |
| mxf-16x128       | 16   | 128          | 32                            | - |
| mxf-24x192       | 24   | 192          | 48                            | - |
| mxf-48x384       | 48   | 384          | 80                            | - |
| mxf-64x512       | 64   | 512          | 80                            | - |
{: caption="Memory flex profile options for virtual servers" caption-side="bottom"}

## Limits
{: #flexible-profiles-limits}

An instance has a limit for the number of volumes and virtual network interfaces that can be attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 1 - 4 | 12 | 1 |
| 8+ | 12 | 2 |
{: caption="Flex profile family limits for for maximum volumes and maximum network interfaces" caption-side="bottom"}
