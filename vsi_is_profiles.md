---

copyright:
  years: 2019
lastupdated: "2019-09-30"

keywords: vsi, virtural server instances, profiles, balanced, compute, memory

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
{:download: .download}

# Profiles
{: #profiles}

When you provision {{site.data.keyword.vsi_is_full}}, you can select from three families of profiles: Balanced, Compute, and Memory. A profile is a combination of vCPU and RAM that can be instantiated quickly to start a virtual server instance. In the {{site.data.keyword.Bluemix_notm}} console, you can choose from popular profile configurations or select from a list of profiles that best fit your needs.
{: shortdesc}

The following profile families are available:

| Families | Description |
| -------- | ----------- |
| [Balanced](#balanced) | Best for common cloud workloads that require a balance of performance and scalability. The balanced profiles (with network-attached storage) provide higher performance, since resources are not oversubscribed. |
| [Compute](#compute-profiles)  | Best for moderate to high web traffic workloads. Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers. |
| [Memory](#memory) | Best for memory caching and real-time analytics workloads. Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. |
{: caption="Table 1. Virtual server family selections" caption-side="top"}

## Balanced
{: #balanced}

The balanced profiles provide higher performance, since resources are not oversubscribed.

The following balanced profiles are available:

| Profile | vCPU | RAM | Network Performance Cap (Gbps) |
|---------|---------|---------|---------|
| bx2-2x8 | 2 | 8 | 4 |
| bx2-4x16 | 4 | 16 | 8 |
| bx2-8x32 | 8 | 32 | 16 |
| bx2-16x64 | 16 | 64 | 32 |
| bx2-32x128 | 32  | 128 | 64 |
| bx2-48x192 | 48 | 192 | 80 |
{: caption="Table 2. Virtual server balanced profile options" caption-side="top"}

## Compute
{: #compute-profiles}

Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and
front-end web servers.

The following compute profiles are available:

| Profile | vCPU | RAM | Network Performance Cap (Gbps) |
|---------|---------|---------|---------|
| cx2-2x4 | 2 | 4 | 4 |
| cx2-4x8 | 4 | 8 | 8 |
| cx2-8x16 | 8 | 16 | 16 |
| cx2-16x32 | 16 | 32 | 32 |
| cx2-32x64 | 32  | 64 | 64 |
{: caption="Table 3. Virtual server instance compute profile options" caption-side="top"}

## Memory
{: #memory}

Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory
analytics workloads.

The following memory profiles are available:

| Profile | vCPU | RAM | Network Performance Cap (Gbps) |
|---------|---------|---------|---------|
| mx2-2x16 | 2 | 16 | 4 |
| mx2-4x32 | 4 | 32 | 8 |
| mx2-8x64 | 8 | 64 | 16 |
| mx2-16x128 | 16 | 128 | 32 |
| mx2-32x256 | 32 | 256 | 64 |
{: caption="Table 4. Virtual server instance memory profile options" caption-side="top"}

For all profile families, these supported operating systems are available: CentOS, Debian, and Ubuntu. 

For information about storage, see [Storage notes for profiles](#storage-notes-for-profiles). 

The network bandwidth cap applies separately to egress (transmitted) and ingress (received) traffic. That is, even if an instance with a network performance cap of 4 Gbps reaches its transmit cap of 4 Gbps, it can still receive up to its cap of 4 Gbps. For more information about network performance, see [Network performance notes for profiles](#network-perf-notes-for-profiles).

## Viewing profile configurations
{: #popular-profiles}

You can view available profile configurations by using the {{site.data.keyword.cloud_notm}} console or the CLI. In the {{site.data.keyword.cloud_notm}} console, you can select from popular profile configurations that support most common use cases.

### Using the IBM Cloud console
{: #profiles-using-console}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances** and click **New instance**.
3. You can either select a profile configuration from **Popular profiles** or click **All profiles** to view more configurations.

### Using the CLI
{: #profiles-using-cli}

To view the list of available profiles by using the CLI, run the following command:
```
$ ibmcloud is instance-profiles
```
{:codeblock}

When you use the CLI, the following information describes what the output represents. The profile sizes have different ratios of CPU to memory because they are designed for different workloads:

*  "b" is balanced, which is a 1:4 ratio
*  "c" is compute (higher on the CPUs), which is a 1:2 ratio
*  “m” is memory (higher on the memory), which is a 1:8 ratio

## Storage notes for profiles
{: #storage-notes-for-profiles}

When you create secondary data volumes, you select a volume profile that best meets your requirements. Volume profiles are available
as three predefined [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) or as a [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom). These volume profiles relate to virtual server instance profiles:

* A [3 IOPS general-purpose tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provides IOPS/GB performance suitable for a virtual server instance Balanced profile.
* A [5-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance suitable for a virtual server instance Compute profile.
* A [10-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance suitable for a virtual server instance Memory profile.

## Network performance notes for profiles
{: #network-perf-notes-for-profiles}

Every profile has a maximum network bandwidth of 2 Gbps per vCPU, with a cap of 80 Gbps. Network bandwidth is distributed evenly across network interfaces, and each network interface has a cap of 16 Gbps that might limit the overall performance. You might need to attach multiple network interfaces to your virtual server instance to optimize network performance.

For example, if you choose a profile with 16 vCPU, the network cap for the profile is 32 Gbps. If you have just one network interface, the maximum network performance is 16 Gbps due to the network interface cap. You need to attach two network interfaces (16 Gbps each) to reach the profile cap of 32 Gbps.



