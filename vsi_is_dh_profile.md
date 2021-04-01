---
copyright:
  years: 2020, 2021
lastupdated: "2021-03-31"

keywords: dedicated host profiles, balanced, compute, memory, generation 2, gen 2

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

When you provision dedicated hosts, you can select from these families of profiles:  Balanced, Compute, Memory, Balanced Instance Storage, Compute Instance Storage, and Memory Instance Storage. All virtual server instances that are provisioned on the dedicated host are provisioned from the same family and class of profiles. If you choose a Memory profile for your dedicated host, all instances that are provisioned on the host must also be created with a Memory profile.
{: shortdesc}

A profile is a combination of attributes, such as the number of vCPUs, amount of RAM, and optionally instance storage that define a dedicated host or instance. In the {{site.data.keyword.Bluemix_notm}} console, you can select from a list of profiles that best fit your needs.


The following profile families are available:

| Families | Description |
| -------- | ----------- |
| [Balanced](#balanced-dh-pr); [Balanced Instance Storage](#balanced-is-dh-pr) | Best for midsize databases and common cloud applications with moderate traffic. |
| [Compute](#compute-dh-pr); [Compute Instance Storage](#compute-is-dh-pr) | Best for moderate to high web traffic workloads. Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers. |
| [Memory](#memory-dh-pr); [Memory Instance Storage](#memory-is-dh-pr) | Best for memory caching and real-time analytics workloads. Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. |
{: caption="Table 1. Dedicated host family selections" caption-side="top"}

## Balanced 
{: #balanced-dh-pr}

The Balanced profile provides a good mix of performance and scalability for more common workloads.

The following Balanced profile is available for dedicated hosts. 

| Dedicated host profile | vCPU | GB RAM | 
|---------|---------|---------|
| bx2-host-152x608 | 152 | 608 | 
{: caption="Table 2. x86-64 balanced profile for dedicated hosts" caption-side="top"}

For supported instance profiles in the Balanced family, see [balanced profiles](/docs/vpc?topic=vpc-profiles#balanced). Instance profiles provisioned on a dedicated host in the Balanced family must include a *bx2* prefix in the instance profile name. 

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the 
instances.  
{: note}


## Compute
{: #compute-dh-pr}

The Compute profile is best for workloads with intensive CPU demands, such as high web traffic workloads, production batch 
processing, and front-end web servers.

The following Compute profile is available for dedicated hosts. 

| Dedicated host profile | vCPU | GB RAM | 
|---------|---------|---------|
| cx2-host-152x304 | 152 | 304 | 
{: caption="Table 3. x86-64 compute profile for dedicated hosts" caption-side="top"}

For supported instance profiles in the Compute family, see [compute profiles](/docs/vpc?topic=vpc-profiles#compute). Instance profiles provisioned on a dedicated host in the Compute family must include a *cx2* prefix in the instance profile name.

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the 
instances.  
{: note}

## Memory
{: #memory-dh-pr}

The Memory profile is best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads.

The following Memory profile is available for dedicated hosts. 

| Dedicated host profile | vCPU | GB RAM | 
|---------|---------|---------| 
| mx2-host-152x1216 | 152 | 1216 | 
{: caption="Table 4. x86-64 memory profile for dedicated hosts" caption-side="top"}

For supported instance profiles in the Memory family, see [memory profiles](/docs/vpc?topic=vpc-profiles#memory). Instance profiles provisioned on a dedicated host in the Memory family must include an *mx2* prefix in the instance profile name.

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the 
instances.  
{: note}

## Balanced Instance Storage
{: #balanced-is-dh-pr}

The Balanced Instance Storage profile provides a good mix of performance and scalability for more common workloads.

The Balanced Instance Storage profile provisions your host with the capability for hosting instances that include [instance storage](/docs/vpc?topic=vpc-instance-storage). If you provision a dedicated host with a Balanced Instance Storage profile, any virtual server instances provisioned on the dedicated host must also be provisioned with a Balanced profile that includes instance storage. Profiles with instance storage include *d* in the profile name, for example *bx2d*. 

The following Balanced Instance Storage profile is available for dedicated hosts. 

| Dedicated host profile | vCPU | GB RAM | Instance Storage |
|---------|---------|---------|---|
| bx2d-host-152x608 | 152 | 608 | 5700 GB | 
{: caption="Table 5. x86-64 balanced instance storage profile for dedicated hosts" caption-side="top"}

For instance profiles that include instance storage in the Balanced family, see [balanced profiles](/docs/vpc?topic=vpc-profiles#balanced).

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the 
instances.  
{: note}

## Compute Instance Storage
{: #compute-is-dh-pr}

The Compute Instance Storage profile is best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers.

The Compute Instance Storage profile provisions your host with the capability for hosting instances that include [instance storage](/docs/vpc?topic=vpc-instance-storage). If you provision a dedicated host with a Compute Instance Storage profile, any virtual server instances provisioned on the dedicated host must also be provisioned with a Compute profile that includes instance storage. Profiles with instance storage include *d* in the profile name, for example *cx2d*. 

The following Compute Instance Storage profile is available for dedicated hosts. 

| Dedicated host profile | vCPU | GB RAM | Instance Storage |
|---------|---------|---------| ---|
| cx2d-host-152x304 | 152 | 304 | 5700 GB |
{: caption="Table 6. x86-64 compute instance storage profile for dedicated hosts" caption-side="top"}

For instance profiles that include instance storage in the Compute family, see [compute profiles](/docs/vpc?topic=vpc-profiles#compute).

Dedicated hosts have a network performance cap of 80 Gbps. Instances provisioned on the host share bandwidth across the 
instances.  
{: note}

## Memory Instance Storage
{: #memory-is-dh-pr}

The Memory Instance Storage profile is best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads.

The Memory Instance Storage profile provisions your host with the capability for hosting instances that include [instance storage](/docs/vpc?topic=vpc-instance-storage). If you provision a dedicated host with a Memory Instance Storage profile, any virtual server instances provisioned on the dedicated host must also be provisioned with a Memory profile that includes instance storage. Profiles with instance storage include *d* in the profile name, for example *mx2d*. 

The following Memory Instance Storage profile is available for dedicated hosts. 

| Dedicated host profile | vCPU | GB RAM | Instance Storage |
|---------|---------|---------| ---|
| mx2d-host-152x1216 | 152 | 1216 | 5700 GB |
{: caption="Table 7. x86-64 memory profiles for dedicated hosts" caption-side="top"}

For instance profiles that include instance storage in the memory family, see [memory profiles](/docs/vpc?topic=vpc-profiles#memory).

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

The second character represents the CPU architecture.
- "x": x86_64
<!-- * "z": System Z -->

The third character represents the generation of VPC the profile is for.
-	The generation of the underlying hardware

The fourth character in the profile prefix, if it exists, represents a profile specialty.
-	For example, *d*, indicates that this profile creates a dedicated host with the capability to host virtual server instances that include instance storage. 

The field after the first hyphen "-" represents the offering, in this example, the dedicated host offering. 

The field after the second hyphen "-" represents the number of vCPU and the size of RAM (GB). "152x608" means this profile has 152 vCPU and a RAM of 608 GB.

For the “bx2d-host-152x608” profile, you can know from the name that it is a Balanced Instance Storage host profile with a 1:4 CPU to memory ratio (152 vCPU and 608 GB RAM). The CPU architecture is x86_64 and this profile is for second generation hardware.

### Using the IBM Cloud console
{: #profiles-using-console}

1. In the {{site.data.keyword.cloud_notm}} console, navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Dedicated hosts**.
2. From the Dedicated host page, click **Create**.
3. You can select from available profile configurations.

### Using the CLI
{: #profiles-using-cli}

To view the list of available profiles by using the CLI, run the following command:
```
$ ibmcloud is dedicated-host-profiles
```
{:codeblock}

## Next steps
{: nextsteps-profiles}

After you choose a profile, it's time to create a dedicated host. 

* [Creating a dedicated host](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)

