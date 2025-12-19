---
copyright:
  years: 2020, 2025
lastupdated: "2025-12-19"

keywords: dedicated host profiles, balanced, compute, memory, ultra high memory, generation 2, gen 2

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# x86-64 dedicated host profiles
{: #dh-profiles}

A dedicated host profile is a combination of attributes, such as the number of vCPUs, amount of RAM, and optional instance storage that define the amount of compute capacity that is provided in a host that is provisioned exclusively for your use.
{: shortdesc}

Dedicated hosts are available in various profile sizes to best suit the size and scale of your workloads: Balanced, Compute, Memory, Very High Memory, and Ultra High Memory. All virtual server instances that are provisioned on the dedicated host are provisioned from the same family and class of profiles. For example, if you choose a Memory profile for your dedicated host, all instances that are provisioned on the host must also be created with a Memory virtual server profile. If you choose a Compute with instance storage dedicated host, all instances that are provisioned on the host must also be created with Compute with instance storage virtual server profiles. This setup helps make sure that you can always use the entire dedicated host resource.

For more information about dedicated host profiles for IBM Z (s390x processor architecture), see [s390x dedicated host profiles](/docs/vpc?topic=vpc-s390x-dh-profiles).



For x86-64 dedicated host profiles, only the Madrid region supports dedicated host profiles with instance storage.
{: important}

The following profile families are available:

| Families | Description |
| -------- | ----------- |
| [Balanced](#balanced-dh-pr) | Best for midsize databases and common cloud applications with moderate traffic. Balanced profiles are offered with and without instance storage. |
| [Compute](#compute-dh-pr) | Best for workloads with intensive CPU demands, such as medium to high web traffic workloads, production batch processing, and front-end web servers. Compute profiles are offered with and without instance storage. |
| [Memory](#memory-dh-pr) | Best for memory intensive processes, such as memory caching, intensive database applications, or in-memory analytics workloads. Memory profiles are offered with and without instance storage. |
|[Very High Memory with instance storage](#vhm-is-dh-pr) | Best for OLAP workloads and SAP-related services, such as SAP NetWeaver. The Very High Memory profile offers 1 vCPU to 14 GiB of RAM memory ratio. This profile is hosted exclusively on the Intel® Xeon® Platinum Cascade Lake server, and includes instance storage for temporary swap space or cache. |
|[Ultra High Memory with instance storage](#uhm-is-dh-pr) | Best for in-memory OLTP databases, such as SAP. The Ultra High Memory profile offers the highest vCPU to memory ratios with 1 vCPU to 28 GiB of RAM. This profile is hosted exclusively on the Intel® Xeon® Platinum Cascade Lake server and includes instance storage for temporary swap space or cache|
{: caption="Dedicated host family selections" caption-side="bottom"}

Profiles with AMD-manufactured processors are available in the Toronto region.
{: preview}

## Balanced
{: #balanced-dh-pr}

The Balanced profile provides a good mix of performance and scalability for more common workloads.

The Balanced with instance storage profile provisions your host with the capability for hosting instances that include [instance storage](/docs/vpc?topic=vpc-instance-storage). If you provision a dedicated host with a Balanced instance storage profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with a Balanced profile that includes instance storage. Profiles with instance storage include *d* in the profile name, for example *bx2d* or *bx3d*.

The Balanced profile family of dedicated hosts on Intel&reg; x86-64 systems are deployed on one of the following processors:
- *bx2* and *bx2d* profiles can be deployed on an Intel&reg; Xeon&reg; Platinum 8260 Cascade Lake processor with 96 cores in a 4-socket configuration.
- *bx2* and *bx2d* profiles can be deployed on an Intel&reg; Xeon&reg; Gold 6248 Cascade Lake processor with 80 cores in a 4-socket configuration.
- *bx3d* profiles are deployed on a custom Intel&reg; Xeon&reg; Platinum 8474C Sapphire Rapids processor with 96 cores in a 2-socket configuration.

New 3rd generation profiles are available in the Dallas, London, Frankfurt, Toronto, Montreal, Chennai, and Madrid regions to create dedicated hosts on hardware that uses 4th Generation Intel&reg; Xeon&reg; Scalable processors, the Intel 8474C processor (previously code named Sapphire Rapids).
{: preview}

The following Balanced profiles are available for dedicated hosts.

| Dedicated host profile | vCPU | Cores | GiB RAM | Instance storage |
|---------|---------|---------|---------| ---------|
| bx2-host-152x608 | 152 | 76 | 608 | - |
| bx2d-host-152x608 | 152 | 76 | 608 | 5700 GB |
{: caption="Intel x86-64 balanced bx2 profile options for dedicated hosts" caption-side="bottom"}
{: #dh-balanced-intel-x86-64-bx2d}
{: tab-title="bx2"}
{: tab-group="DH Balanced"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Balanced profiles options for Intel&reg; x86-64 virtual server instances."}

| Dedicated host profile | vCPU | GiB RAM |
|---------|---------|---------|
| bx2a-host-228x912 | 228 | 912 |
{: caption="AMD x86-64 balanced profile for dedicated hosts" caption-side="bottom"}
{: #dh-balanced-amd-x86-64-bx2a}
{: tab-title="bx2a"}
{: tab-group="DH Balanced"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Balanced profiles options for AMD x86-64 virtual server instances."}

| Dedicated host profile | vCPU | Cores | GiB RAM | Instance storage |
|---------|---------|---------|---------| ---------|
| bx3d-host-176x880 | 176 | 88 | 880 | 2x3.2 Tb |
{: caption="Intel Balanced bx3d profile options for dedicated hosts" caption-side="bottom"}
{: #dh-balanced-intel-x86-64-bx3d}
{: tab-title="bx3d"}
{: tab-group="DH Balanced"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Balanced 3rd generation profile option for Intel&reg; x86-64 virtual server instances."}

For supported instance profiles in the Balanced family, see [balanced profiles](/docs/vpc?topic=vpc-profiles#balanced). Instance profiles that are provisioned on a dedicated host in the Balanced family must include a *bx* prefix in the instance profile name. Profiles with instance storage include *d* in the profile name, for example *bx2d* or *bx3d*.

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.
{: note}

## Compute
{: #compute-dh-pr}

The Compute profile is best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers.

The Compute with instance storage profile provisions your host with the capability for hosting instances that include [instance storage](/docs/vpc?topic=vpc-instance-storage). If you provision a dedicated host with a Compute with instance storage profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with a Compute profile that includes instance storage.

The Compute profile family of dedicated hosts on Intel&reg; x86-64 systems are deployed on one of the following processors:
- *cx2* and *cx2d* profiles can be deployed on an Intel&reg; Xeon&reg; Platinum 8260 Cascade Lake processor with 96 cores in a 4-socket configuration.
- *cx2* and *cx2d* profiles can be deployed on an Intel&reg; Xeon&reg; Gold 6248 Cascade Lake processor with 80 cores in a 4-socket configuration.
- *cx3d* profiles are deployed on a custom Intel&reg; Xeon&reg; Platinum 8474C Sapphire Rapids processor with 96 cores in a 2-socket configuration.

New 3rd generation profiles are available in the Dallas, London, Toronto, Montreal, Chennai, and Madrid regions to create dedicated hosts on hardware that uses 4th Generation Intel&reg; Xeon&reg; Scalable processors, the Intel 8474C processor (previously code named Sapphire Rapids).
{: preview}

The following Compute profiles are available for dedicated hosts.

| Dedicated host profile | vCPU | Cores | GiB RAM | Instance storage |
|---------|---------|---------|---------| ---------|
| cx2-host-152x304 | 152 | 76 | 304 | - |
| cx2d-host-152x304 | 152 | 76 | 304 | 5700 GB |
{: caption="Intel x86-64 Compute profiles for dedicated hosts" caption-side="bottom"}
{: #dh-compute-intel-x86-64-cx2d}
{: tab-title="cx2"}
{: tab-group="DH Compute"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Compute profiles options for Intel&reg; x86-64 virtual server instances."}

| Dedicated host profile | vCPU | Cores | GiB RAM | Instance storage |
|---------|---------|---------|---------| ---------|
| cx3d-host-176x440 | 176 | 88 | 440 | 2x3.2 Tb |
{: caption="Intel x86-64 Compute cx3d profile for dedicated hosts" caption-side="bottom"}
{: #dh-compute-intel-x86-64-cx3d}
{: tab-title="cx3d"}
{: tab-group="DH Compute"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Compute 3rd generation profile option for Intel&reg; x86-64 virtual server instances."}

For instance supported profiles in the Compute family, see [compute profiles](/docs/vpc?topic=vpc-profiles#compute). Instance profiles that are provisioned on a dedicated host in the Compute family must include a *cx* prefix in the instance profile name. Profiles with instance storage include *d* in the profile name, for example *cx2d* or *cx3d*.

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.
{: note}

## Memory
{: #memory-dh-pr}

The Memory profile is best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. The Memory with instance storage profile is best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads.

The Memory with instance storage profile provisions your host with the capability for hosting instances that include [instance storage](/docs/vpc?topic=vpc-instance-storage). If you provision a dedicated host with a Memory with instance storage profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with a Memory profile that includes instance storage.

The Memory profile family of dedicated hosts on Intel&reg; x86-64 systems are deployed on one of the following processors:
- *mx2* and *mx2d* profiles can be deployed on an Intel&reg; Xeon&reg; Platinum 8260 Cascade Lake processor with 96 cores in a 4-socket configuration.
- *mx2* and *mx2d* profiles can be deployed on an Intel&reg; Xeon&reg; Gold 6248 Cascade Lake processor with 80 cores in a 4-socket configuration.
- *mx3d* profiles are deployed on a custom Intel&reg; Xeon&reg; Platinum 8474C Sapphire Rapids processor with 96 cores in a 2-socket configuration.

New 3rd generation profiles are available in the Dallas, London, Toronto, Montreal, Chennai, and Madrid regions to create dedicated hosts on hardware that uses 4th Generation Intel&reg; Xeon&reg; Scalable processors, the Intel 8474C processor (previously code named Sapphire Rapids).
{: preview}

The following Memory profiles are available for dedicated hosts.

| Dedicated host profile | vCPU | Cores | GiB RAM | Instance storage |
|---------|---------|---------|---------| ---------|
| mx2-host-152x1216 | 152 | 76 | 1216 | - |
| mx2d-host-152x1216 | 152 | 76 | 1216 | 5700 GB |
{: caption="Intel x86-64 memory mx2 profile options for dedicated hosts" caption-side="bottom"}
{: #dh-memory-intel-x86-64-mx2d}
{: tab-title="mx2"}
{: tab-group="DH Memory"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Memory profiles options for Intel&reg; x86-64 virtual server instances."}

| Dedicated host profile | vCPU | Cores | GiB RAM | Instance storage |
|---------|---------|---------|---------| ---------|
| mx3d-host-176x1760 | 176 | 88 | 1760 | 2x3.2 Tb |
{: caption="Intel x86-64 Memory mx3d profile for dedicated hosts" caption-side="bottom"}
{: #dh-memory-intel-x86-64-mx3d}
{: tab-title="mx3d"}
{: tab-group="DH Memory"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Memory 3rd generation profile option for Intel&reg; x86-64 virtual server instances."}

For supported instance profiles in the Memory family, see [memory profiles](/docs/vpc?topic=vpc-profiles#memory). Instance profiles that are provisioned on a dedicated host in the Memory family must include an *mx* prefix in the instance profile name. Profiles with instance storage include *d* in the profile name, for example *mx2d* or *mx3d*.

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.
{: note}

## Very High Memory with instance storage
{: #vhm-is-dh-pr}

The Very High Memory with instance storage profile with a high memory ratio of 14 GiB of memory to 1 vCPU of compute is best for OLAP workloads and SAP-related services such as SAP NetWeaver. The Very High Memory profile includes temporary SSD-backed [Instance Storage](/docs/vpc?topic=vpc-instance-storage) at no additional charge.

- The vx2d profile hosted exclusively on the Intel® Xeon® Platinum 8260 Cascade Lake with 96 cores of compute that are running at a base speed of 2.4 GHz and an all-core turbo frequency of 3.1 GHz, a dedicated host that is provisioned with a Very High Memory profile delivers maximum speed and performance.
- The vx3d profile is hosted exclusively on the 4th Generation Intel® Xeon® Scalable processors, the Intel 8474C processor (previously code named Sapphire Rapids).

If you provision a dedicated host with a Very High Memory profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with a Very High Memory profile. All Very High Memory profiles include instance storage that is designated by the *d* in the profile name, for example *vx2d*.

The 3rd generation profile with the vx3d prefix is available in theChennai, Toronto (`ca-tor`) and Washington DC (`us-east`) regions to provision virtual server instances on 4th Generation Intel&reg; Xeon&reg; Scalable processors, the Intel 8474C processor (previously code named Sapphire Rapids). For more information about the capabilities of the new profiles, see [Next generation instance profiles](#next-gen-profiles).
{: preview}

The following Very High Memory profile is available for dedicated hosts.

| Dedicated host profile | vCPU | Cores | GiB RAM | Instance storage |
|---------|---------|---------|---------|---------|
| vx2d-host-176x2464 | 176 | 88 | 2464 | 5280 GB |
{: caption="Intel x86-64 very high memory profiles for dedicated hosts" caption-side="bottom"}
{: #dh-vhmemory-intel-x86-64-vx2d}
{: tab-title="vx2d"}
{: tab-group="DH Very High Memory"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Very High Memory profile option for Intel&reg; x86-64 virtual server instances."}

| Dedicated host profile | vCPU | Cores | GiB RAM | Instance storage |
|---------|---------|---------|---------|---------|
| vx3d-host-176x2816 | 176 | 88 | 2816| 2x2860 GB |
{: caption="Intel x86-64 very high memory profiles for dedicated hosts" caption-side="bottom"}
{: #dh-vhmemory-intel-x86-64-vx3d}
{: tab-title="vx3d"}
{: tab-group="DH Very High Memory"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Very High Memory profile option for Intel&reg; x86-64 virtual server instances."}

For more information about instance profiles that include instance storage in the Very High Memory family, see [very high memory profiles](/docs/vpc?topic=vpc-profiles#vhmemory).

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the instances.
{: note}

## Ultra High Memory with instance storage
{: #uhm-is-dh-pr}

The Ultra High Memory with instance storage profile is best for in-memory OLTP databases, such as SAP. They are optimized for running memory-intensive applications and in-memory databases such as SAP HANA, Memcached, or Redis. This profile family offers our highest vCPU to memory ratio with 28 GiB of memory for every 1 vCPU of compute and up to 5.7 TiB of available RAM.

The Ultra High Memory profile is hosted exclusively on the latest generation Intel® Xeon® Platinum 8280L Cascade Lake server with 112 cores that are running a base speed of 2.7 GHz and an all-core turbo frequency of 4.0 GHz. The Ultra High Memory profile is provisioned with temporary SSD-backed [Instance Storage](/docs/vpc?topic=vpc-instance-storage) at no additional charge.

If you provision a dedicated host with an Ultra High Memory profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with an Ultra High Memory profile. All Ultra High Memory profiles include instance storage that is designated by the *d* in the profile name, for example *ux2d*.
{: #callout-note}

The following Ultra High Memory profile is available for dedicated hosts.

| Dedicated host profile | vCPU | Cores | GiB RAM | Instance storage |
|---------|---------|---------|---------|---------|
| ux2d-host-200x5600 | 200 | 100 | 5600 | 6000 GB |
{: caption="Intel x86-64 ultra high memory profile for dedicated hosts" caption-side="bottom"}
{: #dh-uhmemory-intel-x86-64-ux2d}
{: tab-title="Intel x86-64"}
{: tab-group="DH Ultra High Memory"}
{: class="simple-tab-table"}
{: summary="Dedicated Host Ultra High Memory profile option for Intel&reg; x86-64 virtual server instances."}

For more information about instance profiles that include instance storage in the ultra high memory family, see [ultra high memory profiles](/docs/vpc?topic=vpc-profiles#uhmemory).

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.
{: note}

## Viewing profile configurations
{: #dh-popular-profiles}

You can view available profile configurations by using the {{site.data.keyword.cloud_notm}} console or the CLI. In the {{site.data.keyword.cloud_notm}} console, you can select from popular profile configurations that support the most common use cases.

### Understanding profiles
{: #dh-profiles-naming-rule}

The following example describes the individual parts that make up a dedicated host profile name, for this example, `bx2d-host-152x608`. The prefix `bx2d` is composed of the family, architecture, generation, and specialty. Together this information comprises the profile *class*.

| Family | Architecture | Generation | Specialty | Offering | vCPU | RAM |
| ------ | ------------ | ---------- | ----------| -------- | ---- | --- |
| b      | x            | 2          | d-        | host-    | 152x | 608  |
{: caption="Individual parts that comprise the dedicated host profile" caption-side="bottom"}


The first character represents the profile family. Different profile families have different ratios of CPU to memory, which is designed for different workloads.
-	"b": balanced family, 1:4 ratio
-	"c": compute family (higher on the CPUs), 1:2 ratio
-	"m": memory family (higher on the memory), 1:16 ratio
- "v": very high memory family (higher memory with instance storage), 1:14 ratio
- "u": ultra high memory family (higher memory with instance storage), 1:28 ratio

The second character represents the CPU architecture.
- "x": x86_64
- "z": System Z

The third character represents the generation of VPC the profile is for.
-	The generation of the underlying hardware

The fourth character in the profile prefix, if it exists, represents a profile specialty.
-	"d": indicates that this profile creates a dedicated host with the capability to host virtual server instances that include instance storage.
- "a": AMD manufactured

The field after the first hyphen "-" represents the offering, in this example, the dedicated host offering.

The field after the second hyphen "-" represents the number of vCPU and the size of RAM (GB). "152x608" means that this profile has 152 vCPU and a RAM of 608 GB.

For the “bx2d-host-152x608” profile, you can know from the name that it is a Balanced Instance Storage host profile with a 1:4 CPU to memory ratio (152 vCPU and 608 GB RAM). The CPU architecture is x86_64 and this profile is for second-generation hardware.

### Using the IBM Cloud console
{: #dh-profiles-using-console}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to the **Navigation Menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Dedicated hosts**.
2. From the Dedicated host page, click **Create**.
3. You can select from available profile configurations.

### Using the CLI
{: #dh-profiles-using-cli}
{: cli}

To view the list of available profiles by using the CLI, run the following command:
```sh
ibmcloud is dedicated-host-profiles
```
{: codeblock}

## Intel Hyper-Threading Technology
{: #profiles-hyper-threading}

All Intel&reg;-based servers operate with the Intel&reg; Hyper-Threading Technology enabled. Hyper-Threading Technology provies 2 vCPUs for every physical core in the box. Hyper-Threading Technology can also be disabled if needed. For more information, see [Disabling Intel Hyper-Threading Technology](/docs/vpc?topic=vpc-disabling-hyper-threading).

## Querying dedicated host capacity
{: #profiles-query-dedicated-host-capacity}

For licensing purposes, you can retrieve the total physical capacity of the dedicated host that includes the compute that is held back for intensive resource usage. To see the total physical dedicated host compute, use the `GET` API command to query the dedicated host. The `GET` command returns both `available_memory` and `available_vcpu`. A vCPU is 1/2 of a physical core. For more information, see [Retrieve a dedicated host](/apidocs/vpc#get-dedicated-host).

## Next steps
{: #dh-nextsteps-profiles}

After you choose a profile, it's time to create a dedicated host.

* [Creating a dedicated host](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)
