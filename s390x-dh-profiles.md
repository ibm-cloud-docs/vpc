---
copyright:
  years: 2023, 2026
lastupdated: "2026-02-20"

keywords: dedicated host profiles, s390x

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# s390x dedicated host profiles
{: #s390x-dh-profiles}

The IBM dedicated host for VPC (s390x architecture) is deprecated. As of 28 February 2026, you can't create new instances. Existing instances are supported until 20 February 2027. Any instances that still exist on that date will be deleted. You can migrate to an on-premises deployment model. For more information, see [Linux on IBM Z and LinuxONE](https://www.ibm.com/docs/en/linux-on-systems?topic=linux-z-linuxone). For more information, see the [Service deprecation announcement](/docs/vpc?topic=vpc-ichpcs_deprecated_anmt).
{: deprecated}

A dedicated host profile is a combination of attributes, such as the number of vCPUs, amount of RAM, and optionally instance storage that define the amount of compute capacity that is provided in a host that is provisioned exclusively for your use.
{: shortdesc}

For more information about dedicated host profiles for x86 processor architecture, see [x86-64 dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles).

For Wazi as a Service, dedicated hosts are currently available in the Spain (Madrid) and US South (Dallas) region only.
{: note}

The following profile families are available:

| Families | Description |
| -------- | ----------- |
| [Memory](#s390x-dh-memory) | Best for memory intensive processes, such as memory caching, intensive database applications, or in-memory analytics workloads. Memory profiles offer vCPU to memory ratios with 1 vCPU to 8 GiB of RAM. Memory profiles are offered with and without instance storage. |
{: caption="Dedicated host family selections" caption-side="bottom"}
{: #dh-memory-s390x}


## Memory
{: #s390x-dh-memory}

The Memory profile is best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. The Memory with instance storage profile is best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads.

The following Memory profile is available for dedicated hosts.

| Dedicated host profile | vCPU | GiB RAM | Instance storage |
|---------|---------|---------| ---------|
| mz2-host-40x320 | 40 | 320 | - |
{: caption="s390x memory profile options for dedicated hosts" caption-side="bottom"}

Dedicated hosts have a network performance cap of 80 Gbps. Instances that are created on the host share bandwidth across the instances.
{: note}

## Viewing profile configurations
{: #s390x-dh-popular-profiles}

You can view available profile configurations by using the {{site.data.keyword.cloud_notm}} console or the CLI. In the {{site.data.keyword.cloud_notm}} console, you can select from popular profile configurations that support most common use cases.

### Understanding profiles
{: #s390x-dh-profiles-naming-rule}

The following example describes the individual parts that make up a dedicated host profile name.

| Family | Architecture | Generation | Specialty | Offering | vCPU | RAM |
| ------ | ------------ | ---------- | ----------| -------- | ---- | --- |
| m      | z            | 2         | -        | host-    | 40x | 320  |
{: caption="Individual parts that comprise the dedicated host profile" caption-side="bottom"}

The first character represents the profile family. Different profile families have different ratios of CPU to memory, which is designed for different workloads.
- "b": balanced family, 1:4 ratio
- "c": compute family (higher on the CPUs), 1:2 ratio
- "m": memory family (higher on the memory), 1:16 ratio
- "v": very high memory family (higher memory with instance storage), 1:14 ratio
- "u": ultra high memory family (higher memory with instance storage), 1:28 ratio

The second character represents the CPU architecture.
- "x": x86_64
- "z": System Z

The third character represents the generation of VPC the profile is for.
- The generation of the underlying hardware

The fourth character in the profile prefix, if it exists, represents a profile specialty.
- "d": indicates that this profile creates a dedicated host with the capability to host virtual server instances that include instance storage.
- "a": AMD manufactured

The field after the first hyphen "-" represents the offering, in this example, the dedicated host offering.

The field after the second hyphen "-" represents the number of vCPU and the size of RAM (GB). "40x320" means that this profile has 40 vCPU and a RAM of 320 GB.

For the “mz2-host-40x320” profile, you can know from the name that it is a Memory Instance Storage host profile with a 1:16 CPU to memory ratio (40 vCPU and 320 GB RAM). The CPU architecture is System Z and this profile is for second-generation hardware.

### Using the IBM Cloud console
{: #s390x-dh-profiles-using-console}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Dedicated hosts**.
2. From the Dedicated host page, click **Create**.
3. You can select from available profile configurations.

### Using the CLI
{: #s390x-dh-profiles-using-cli}
{: cli}

To view the list of available profiles by using the CLI, run the following command:
```sh
ibmcloud is dedicated-host-profiles
```
{: codeblock}


## Next steps
{: #s390x-dh-nextsteps}

After you choose a profile, it's time to create a dedicated host.

- [Creating a dedicated host](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)
