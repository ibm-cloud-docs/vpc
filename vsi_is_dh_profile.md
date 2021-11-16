---
copyright:
  years: 2020, 2021
lastupdated: "2021-11-16"

keywords: dedicated host profiles, balanced, compute, memory, ultra high memory, generation 2, gen 2

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:download: .download}

# Dedicated host profiles
{: #dh-profiles}

A dedicated host profile is a combination of attributes, such as the number of vCPUs, amount of RAM, and optionally instance storage that define a dedicated host or instance. 
{: shortdesc}

Dedicated hosts are available in a variety of profile sizes to best suit the size and scale of of your workloads: Balanced, Compute, Memory, Very High Memory, and Ultra High Memory. All virtual server instances that are provisioned on the dedicated host are provisioned from the same family and class of profiles. For example, if you choose a Memory profile for your dedicated host, all instances that are provisioned on the host must also be created with a Memory virtual server profile. If you choose a Compute with instance storage dedicated host, all instances that are provisioned on the host must also be created with Compute with instance storage virtual server profiles. This set up ensures you can always use the entire dedicated host resource.

<!-- The s390x note stays on staging until 7/31 when LinuxONE VSI is available in production -->
Dedicated hosts are not supported for LinuxONE (s390x processor architecture).  
{: note}

The following profile families are available:

| Families | Description |
| -------- | ----------- |
| [Balanced](#balanced-dh-pr)  \n[Balanced with instance storage](#balanced-is-dh-pr) | Best for midsize databases and common cloud applications with moderate traffic. Balanced profiles offer vCPU to memory ratios with 1 vCPU to 4 GiB of RAM |
| [Compute](#compute-dh-pr)  \n[Compute with instance storage](#compute-is-dh-pr) | Best for workloads with intensive CPU demands, such as medium to high web traffic workloads, production batch processing, and front-end web servers. Compute profiles offer vCPU to memory ratios with 1 vCPU to 2 GiB of RAM |
| [Memory](#memory-dh-pr)  \n[Memory with instance storage](#memory-is-dh-pr) | Best for memory intensive processes, such as rmemory caching, intensive database applications, or in-memory analytics workloads. Memory profiles offer vCPU to memory ratios with 1 vCPU to 16 GiB of RAM |
|[Very High Memory with instance storage](#vhmemory-is-dh-pr) | Best for OLAP workloads and SAP-related services, such as SAP NetWeaver. Very High Memory profiles offer 1 vCPU to 14 GiB of RAM memory ratio, is hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server, and include instance storage for temporary swap space or cache. |
|[Ultra High Memory with instance storage](#uhmemory-is-dh-pr) | Best for in-memory OLTP databases, such as SAP. Ultra High Memory profiles offer the highest vCPU to memory ratios with 1 vCPU to 28 GiB of RAM, is hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server, and include instance storage for temporary swap space or cache|
{: caption="Table 1. Dedicated host family selections" caption-side="top"}

## Balanced
{: #balanced-dh-pr}

The Balanced profile provides a good mix of performance and scalability for more common workloads.

The following Balanced profile is available for dedicated hosts.

| Dedicated host profile | vCPU | GiB RAM |
|---------|---------|---------|
| bx2-host-152x608 | 152 | 608 |
{: caption="Table 2. x86-64 balanced profile for dedicated hosts" caption-side="top"}

For supported instance profiles in the Balanced family, see [balanced profiles](/docs/vpc?topic=vpc-profiles#balanced). Instance profiles that are provisioned on a dedicated host in the Balanced family must include a *bx2* prefix in the instance profile name.

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.  
{: note}


## Compute
{: #compute-dh-pr}

The Compute profile is best for workloads with intensive CPU demands, such as high web traffic workloads, production batch
processing, and front-end web servers.

The following Compute profile is available for dedicated hosts.

| Dedicated host profile | vCPU | GiB RAM |
|---------|---------|---------|
| cx2-host-152x304 | 152 | 304 |
{: caption="Table 3. x86-64 compute profile for dedicated hosts" caption-side="top"}

For supported instance profiles in the Compute family, see [compute profiles](/docs/vpc?topic=vpc-profiles#compute). Instance profiles that are provisioned on a dedicated host in the Compute family must include a *cx2* prefix in the instance profile name.

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.  
{: note}

## Memory
{: #memory-dh-pr}

The Memory profile is best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads.

The following Memory profile is available for dedicated hosts.

| Dedicated host profile | vCPU | GiB RAM |
|---------|---------|---------|
| mx2-host-152x1216 | 152 | 1216 |
{: caption="Table 4. x86-64 memory profile for dedicated hosts" caption-side="top"}

For supported instance profiles in the Memory family, see [memory profiles](/docs/vpc?topic=vpc-profiles#memory). Instance profiles that are provisioned on a dedicated host in the Memory family must include an *mx2* prefix in the instance profile name.

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.  
{: note}

## Balanced with instance storage
{: #balanced-is-dh-pr}

The Balanced with instance storage profile provides a good mix of performance and scalability for more common workloads.

The Balanced with instance storage profile provisions your host with the capability for hosting instances that include [instance storage](/docs/vpc?topic=vpc-instance-storage). If you provision a dedicated host with a Balanced with instance storage profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with a Balanced profile that includes instance storage. Profiles with instance storage include *d* in the profile name, for example *bx2d*.

The following Balanced with instance storage profile is available for dedicated hosts.

| Dedicated host profile | vCPU | GiB RAM | Instance Storage |
|---------|---------|---------|---|
| bx2d-host-152x608 | 152 | 608 | 5700 GB |
{: caption="Table 7. x86-64 balanced instance storage profile for dedicated hosts" caption-side="top"}

For instance profiles that include instance storage in the Balanced family, see [balanced profiles](/docs/vpc?topic=vpc-profiles#balanced).

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.  
{: note}

## Compute with instance storage
{: #compute-is-dh-pr}

The Compute with instance storage profile is best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers.

The Compute with instance storage profile provisions your host with the capability for hosting instances that include [instance storage](/docs/vpc?topic=vpc-instance-storage). If you provision a dedicated host with a Compute with instance storage profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with a Compute profile that includes instance storage. Profiles with instance storage include *d* in the profile name, for example *cx2d*.

The following Compute with instance storage profile is available for dedicated hosts.

| Dedicated host profile | vCPU | GiB RAM | Instance Storage |
|---------|---------|---------| ---|
| cx2d-host-152x304 | 152 | 304 | 5700 GB |
{: caption="Table 8. x86-64 compute instance storage profile for dedicated hosts" caption-side="top"}

For instance profiles that include instance storage in the Compute family, see [compute profiles](/docs/vpc?topic=vpc-profiles#compute).

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.  
{: note}

## Memory with instance storage
{: #memory-is-dh-pr}

The Memory with instance storage profile is best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads.

The Memory with instance storage profile provisions your host with the capability for hosting instances that include [instance storage](/docs/vpc?topic=vpc-instance-storage). If you provision a dedicated host with a Memory with instance storage profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with a Memory profile that includes instance storage. Profiles with instance storage include *d* in the profile name, for example *mx2d*.

The following Memory with instance storage profile is available for dedicated hosts.

| Dedicated host profile | vCPU | GiB RAM | Instance Storage |
|---------|---------|---------| ---|
| mx2d-host-152x1216 | 152 | 1216 | 5700 GB |
{: caption="Table 9. x86-64 memory profiles for dedicated hosts" caption-side="top"}

For instance profiles that include instance storage in the memory family, see [memory profiles](/docs/vpc?topic=vpc-profiles#memory).

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.  
{: note}

## Very High Memory with instance storage
{: #vhmemory-is-dh-pr}

The Very High Memory with instance storage profile is hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and is best for OLAP workloads and SAP-related services, such as SAP NetWeaver. The Very High Memory profile offers 1 vCPU to 14 GiB of RAM memory ratio and include temporary SSD-backed [Instance Storage](/docs/vpc?topic=vpc-instance-storage) at no additional charge. 

If you provision a dedicated host with a Very High Memory profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with a Very High Memory profile. All Very High Memory profiles include instance storage which is designated by the *d* in the profile name, for example *vx2d*.

The following Very High Memory profile is available for dedicated hosts.

| Dedicated host profile | vCPU | GiB RAM | Instance Storage |
|---------|---------|---------| ---|
| vx2d-host-176x2464 | 176 | 2464 | 5280 GB |
{: caption="Table 10. x86-64 very high memory profiles for dedicated hosts" caption-side="top"}

For more information about instance profiles that include instance storage in the very high memory family, see [very high memory profiles](/docs/vpc?topic=vpc-profiles#vhmemory).

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.  
{: note}

## Ultra High Memory with instance storage
{: #uhmemory-is-dh-pr}

The Ultra High Memory with instance storage profile is best for in-memory OLTP databases, such as SAP. The Ultra High Memory profile is hosted exclusively on the latest generation Intel® Xeon® Platinum Cascade Lake server hosts and are optimized for running memory intensive applications and in-memory database such as SAP HANA, Memcached, or Redis. This profile family offers our highest vCPU to memory ratio with 28 GiB of memory for every 1 vCPU of compute and up to 5.7 TiB of available RAM. All Ultra High Memory profiles are provisioned with temporary SSD-backed [Instance Storage](/docs/vpc?topic=vpc-instance-storage) at no additional charge.

If you provision a dedicated host with an Ultra High Memory profile, any virtual server instances that are provisioned on the dedicated host must also be provisioned with a Ultra High Memory profile. All Ultra High Memory profiles include instance storage which is designated by the *d* in the profile name, for example *ux2d*.

{: #callout-note}

The following Ultra High Memory profile is available for dedicated hosts.

| Dedicated host profile | vCPU | GiB RAM | Instance Storage |
|---------|---------|---------| ---|
| ux2d-host-200x5600 | 200 | 5600 | 6000 GB |
{: caption="Table11. x86-64 ultra high memory profiles for dedicated hosts" caption-side="top"}

For more information about instance profiles that include instance storage in the ultra high memory family, see [ultra high memory profiles](/docs/vpc?topic=vpc-profiles#uhmemory).

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the
instances.  
{: note}

## Viewing profile configurations
{: #popular-profiles}

You can view available profile configurations by using the {{site.data.keyword.cloud_notm}} console or the CLI. In the {{site.data.keyword.cloud_notm}} console, you can select from popular profile configurations that support most common use cases.

### Understanding profiles
{: #profiles-naming-rule}

The following example describes the individual parts that make up a dedicated host profile name, for this example, `bx2d-host-152x608`. The prefix, `bx2d` is composed of the family, architecture, generation, and specialty. Together this information comprises the profile *class*.   

| Family | Architecture | Generation | Specialty | Offering | vCPU | RAM |
| ------ | ------------ | ---------- | ----------| -------- | ---- | --- |
| b      | x            | 2         | d-        | host-    | 152x | 608  |
{: caption="Table 5. Individual parts that comprise the dedicated host profile" caption-side="top"}


The first character represents the profile family. Different profile families have different ratios of CPU to memory, which is designed for different workloads.
-	"b": balanced family, 1:4 ratio
-	"c": compute family (higher on the CPUs), 1:2 ratio
-	"m": memory family (higher on the memory), 1:16 ratio
- "v": very high memory family (higher memory with instance storage), 1:14 ratio 
- "u": ultra high memory family (higher memory with instance storage), 1:28 ratio

The second character represents the CPU architecture.
- "x": x86_64
<!-- * "z": System Z -->

The third character represents the generation of VPC the profile is for.
-	The generation of the underlying hardware

The fourth character in the profile prefix, if it exists, represents a profile specialty.
-	For example, *d*, indicates that this profile creates a dedicated host with the capability to host virtual server instances that include instance storage.

The field after the first hyphen "-" represents the offering, in this example, the dedicated host offering.

The field after the second hyphen "-" represents the number of vCPU and the size of RAM (GB). "152x608" means that this profile has 152 vCPU and a RAM of 608 GB.

For the “bx2d-host-152x608” profile, you can know from the name that it is a Balanced Instance Storage host profile with a 1:4 CPU to memory ratio (152 vCPU and 608 GB RAM). The CPU architecture is x86_64 and this profile is for second-generation hardware.

### Using the IBM Cloud console
{: #profiles-using-console}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Dedicated hosts**.
2. From the Dedicated host page, click **Create**.
3. You can select from available profile configurations.

### Using the CLI
{: #profiles-using-cli}

To view the list of available profiles by using the CLI, run the following command:

```
$ ibmcloud is dedicated-host-profiles
```
{: pre}

## Next steps
{: nextsteps-profiles}

After you choose a profile, it's time to create a dedicated host.

* [Creating a dedicated host](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)
