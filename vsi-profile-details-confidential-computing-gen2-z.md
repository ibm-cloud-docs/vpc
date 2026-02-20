---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-20"

keywords: vsi, virtual server, virtual server instances, profile, profiles, balanced, compute, memory, gen 2, confidential computing, Hyper Protect, s390x architecture

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Confidential Computing instance profiles - s390x Gen 2
{: #confidential-computing-profiles-gen2-z}

[Deprecated]{: tag-deprecated} The IBM Z or LinuxONE confidential computing family of 2nd generation virtual server profiles (balanced, compute, and memory) are built atop the s390x processor architecture, with native integration into {{site.data.keyword.cloud}} VPC.
{: shortdesc}

The confidential computing profiles enable [IBM Secure Execution for Linux](https://www.ibm.com/docs/en/linux-on-systems?topic=management-secure-execution){: external}. The profiles support the
[IBM Hyper Protect Container Runtime image](/docs/vpc?topic=vpc-vsabout-images#hyper-protect-runtime).


## Operating systems
{: #cc-os-gen2-z}

- [Deprecated]{: tag-deprecated} Linux (s390x)

## Processor generation
{: #cc-processor-gen2-z}

- IBM Z or LinuxONE

## Availability
{: #cc-availability-gen2-z}

Status: Select availability

Regions:
- Americas
   - Brazil (`br-sao`)
   - Toronto (`ca-tor`)
   - Dallas (`us-south`)
   - Washington DC (`us-east`)
- Europe
   - Frankfurt (`eu-de`)
   - London (`eu-gb`)
   - Madrid (`eu-es`)
- Asia Pacific
   - Tokyo (`jp-tok`)

## Capabilities
{: #cc-capabilities-gen2-z}

- Dedicated host: Yes
- Secure boot: No
- Confidential computing: IBM Secure Execution
- Live migration: No
- Instance storage: No

## VM Configuration
{: #cc-vm-config-gen2-z}

- Hardware type: s390-ccw-virtio
- Cloud networking: virtio
- Block boot volume: virtio
- Block data volumes: virtio

## Instance profiles
{: #cc-vsi-profiles-gen2-z}

### Balanced
{: #cc-balanced-profiles-gen2-z}


| Profile    | vCPUs | Memory (GiB) | Bandwidth cap (Gbps) |
| ---------- | ----- | ------------ | -------------------- |
| bz2e-1x4   | 1     | 4            | 2                    |
| bz2e-2x8   | 2     | 8            | 4                    |
| bz2e-4x16  | 4     | 16           | 8                    |
| bz2e-8x32  | 8     | 32           | 16                   |
| bz2e-16x64 | 16    | 64           | 32                   |
{: caption="Confidential computing balanced instance profile options for s390x architecture, Gen 2" caption-side="bottom"}


### Compute
{: #cc-compute-profiles-gen2-z}


| Profile    | vCPUs | Memory (GiB) | Bandwidth cap (Gbps) |
| ---------- | ----- | ------------ | -------------------- |
| cz2e-2x4   | 2     | 4            | 4                    |
| cz2e-4x8   | 4     | 8            | 8                    |
| cz2e-8x16  | 8     | 16           | 16                   |
| cz2e-16x32 | 16    | 32           | 32                   |
{: caption="Confidential computing compute instance profile options for s390x architecture, Gen 2" caption-side="bottom"}

### Memory
{: #cc-memory-profiles-gen2-z}

| Profile     | vCPUs | Memory (GiB) | Bandwidth cap (Gbps) |
| ----------- | ----- | ------------ | -------------------- |
| mz2e-2x16   | 2     | 16           | 4                    |
| mz2e-4x32   | 4     | 32           | 8                    |
| mz2e-8x64   | 8     | 64           | 16                   |
| mz2e-16x128 | 16    | 128          | 32                   |
{: caption="Confidential computing memory instance profile options for s390x architecture, Gen 2" caption-side="bottom"}

## Limits
{: #cc-vsi-limits-gen2-z}

An instance has a limit for the number of volumes and virtual network interfaces that can be
attached. This limit is based on the size of the instance.

| Number of vCPUs | Max volumes | Max vNICs |
| --------------- | ----------- | --------- |
| 2-16            | 12          | 5         |
{: caption="Confidential computing profile family limits for vCPU, maximum volumes, and maximum network interfaces" caption-side="bottom"}
