---
copyright:
  years: 2022, 2026
lastupdated: "2026-02-20"


keywords: vsi, virtual server instances, profile, profiles, balanced, compute, memory, ultra high memory, storage optimized

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# s390x instance profiles
{: #vs-profiles}

The IBM Cloud LinuxONE Virtual Server for VPC (s390x architecture) is deprecated. As of 28 February 2026, you can't create new instances. Existing instances are supported until 20 February 2027. Any instances that still exist on that date will be deleted. You can migrate to an on-premises deployment model. For more information, see [Linux on IBM Z and LinuxONE](https://www.ibm.com/docs/en/linux-on-systems?topic=linux-z-linuxone). For more information, see the [Service deprecation announcement](/docs/vpc?topic=vpc-ichpcs_deprecated_anmt).
{: deprecated}

You can use s390x instance profiles to provision virtual server instances and you can select from three families of profiles: Balanced, Compute, and Memory. IBM Z or LinuxONE (s390x processor architecture) is a uniquely secure and scalable architecture that provides dedicated CPU core, memory, and I/O channel to better manage your high-performance workloads.
{: shortdesc}

A profile is a combination of instance attributes, such as the number of vCPUs, amount of RAM, network bandwidth, and default bandwidth allocation. The attributes define the size and capabilities of the virtual server instance that is provisioned. In the {{site.data.keyword.Bluemix_notm}} console, you can select the most recently used profile or click **View All Profiles** to choose the profile that best fits your needs.

The secure execution profiles that become available when you enable confidential computing and select IBM Hyper Protect as the operating system for your virtual instance, can be identified by the fourth character of the profile name, which is an "e", such as bz2e.

For more information about profiles for x86 processor architecture, see [x86 instance profiles](/docs/vpc?topic=vpc-profiles).
For more information about confidential computing, see [Confidential computing with LinuxONE](/docs/vpc?topic=vpc-about-se).

The following profile families are available:

| Families | Description |
| -------- | ----------- |
| [Balanced](#vs-balanced) | Balanced profiles offer a core to RAM ratio 1 vCPU to 4 GiB of RAM ratio and are best for midsize databases and common cloud applications with moderate traffic. |
| [Compute](#vs-compute)  | Compute profiles offer a core to RAM ratio 1 vCPU to 2 GiB of RAM ratio and are best for moderate to high web traffic workloads. Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers. |
| [Memory](#vs-memory) | Memory profiles offer a core to RAM ratio 1 vCPU to 8 GiB of RAM ratio and are best for memory caching and real-time analytics workloads. Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. |
{: caption="Virtual server family selections" caption-side="bottom"}

s390x processor architecture profiles can be used to provision {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}}, LinuxONE virtual server instances, and z/OS virtual server instances.

For the LinuxONE and z/OS virtual server instances, the profiles are available in the US South (Dallas), Japan (Tokyo), Brazil (São Paulo), Spain (Madrid), Canada (Toronto), United Kingdom (London), Germany (Frankfurt), and US East (Washington DC) regions.
{: preview}


## Balanced
{: #vs-balanced}

Balanced profiles provide a mix of performance and scalability for more common workloads with a ratio of 4 GiB of memory for every 1 vCPU of compute. The following table shows all Balanced profiles available for the IBM Z or LinuxONE (s390x architecture) processors.

Ensure that you select a secure execution enabled profile (for example, cz2e-2x4) when you enable the **Run your workload with an OS and a profile protected by Secure Execution** toggle (to activate support for secure execution images). Selecting any profile that is not secure execution enabled will cause the deployment of the virtual instance to fail.


| Instance profile | vCPU | Cores | GiB RAM | Bandwidth Cap (Gbps) |
|---------|---------|---------|---------|---------|
| bz2-1x4 | 1 | - | 4 | 2 |
| bz2-2x8 | 2 | 1 | 8 | 4 |
| bz2-4x16 | 4 | 2 | 16 | 8 |
| bz2-8x32 | 8 | 4 | 32 | 16 |
| bz2-16x64 | 16 | 8 | 64 | 32 |
{: caption="Balanced profiles options for IBM Z or LinuxONE s390x instances" caption-side="bottom"}
{: #balanced-s390x}
{: tab-title="s390x"}
{: tab-group="Balanced"}
{: class="simple-tab-table"}
{: summary="Balanced profiles options for s390x virtual server instances."}

| Instance profile | vCPU | Cores | GiB RAM | Bandwidth Cap (Gbps) |
|---------|---------|---------|---------|---------|
| bz2e-1x4 | 1 | - | 4 | 2 |
| bz2e-2x8 | 2 | 1 | 8 | 4 |
| bz2e-4x16 | 4 | 2 | 16 | 8  |
| bz2e-8x32 | 8 | 4 | 32 | 16 |
| bz2e-16x64 | 16 | 8 | 64 | 32 |
{: caption="Balanced secure execution profiles options for IBM Z or LinuxONE s390x instances" caption-side="bottom"}
{: #balanced-se-s390x}
{: tab-title="s390x with secure execution "}
{: tab-group="Balanced"}
{: class="simple-tab-table"}
{: summary="Balanced secure execution profiles options for s390x virtual server instances."}


## Compute
{: #vs-compute}

Compute profiles are best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers that can benefit from 2 GiB of memory for every 1 vCPU of compute. The following table shows all Compute profiles available for the IBM Z or LinuxONE (s390x architecture) processors.

Ensure that you select a secure execution enabled profile (for example, cz2e-2x4) when you enable the **Run your workload with an OS and a profile protected by Secure Execution** toggle (to activate support for secure execution images). Selecting any profile that is not secure execution enabled will cause the deployment of the virtual instance to fail.


| Instance profile | vCPU | Cores | GiB RAM | Bandwidth Cap (Gbps) |
|---------|---------|---------|---------|---------|
| cz2-2x4 | 2 | 1 | 4 | 2 |
| cz2-4x8 | 4 | 2 | 8 | 8 |
| cz2-8x16 | 8 | 4 | 16 | 16 |
| cz2-16x32 | 16 | 8 | 32 | 32 |
{: caption="Compute profiles options for s390x instances" caption-side="bottom"}
{: #compute-s390x}
{: tab-title="s390x"}
{: tab-group="Compute"}
{: class="simple-tab-table"}
{: summary="Compute profiles options for IBM Z or LinuxONE s390x virtual server instances."}

| Instance profile | vCPU | Cores | GiB RAM | Bandwidth Cap (Gbps) |
|---------|---------|---------|---------|---------|
| cz2e-2x4 | 2 | 1 | 4 | 2 |
| cz2e-4x8 | 4 | 2 | 8 | 8 |
| cz2e-8x16 | 8 | 4 | 16 | 16 |
| cz2e-16x32 | 16 | 8 | 32 | 32 |
{: caption="Compute secure execution profiles options for IBM Z or LinuxONE s390x instances" caption-side="bottom"}
{: #compute-se-s390x}
{: tab-title="s390x with secure execution"}
{: tab-group="Compute"}
{: class="simple-tab-table"}
{: summary="Compute secure execution profiles options for s390x virtual server instances."}


## Memory
{: #vs-memory}

Memory profiles are best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads with 8 GiB of memory for every 1 vCPU of compute. The following table shows all Memory profiles available for the IBM Z or LinuxONE (s390x architecture) processors.

Ensure that you select a secure execution enabled profile (for example, mz2e-2x16) when you enable the **Run your workload with an OS and a profile protected by Secure Execution** toggle (to activate support for secure execution images). Selecting any profile that is not secure execution enabled will cause the deployment of the virtual instance to fail.


| Instance profile | vCPU | Cores | GiB RAM | Bandwidth Cap (Gbps) | I
|---------|---------|---------|---------|---------|---------|
| mz2-2x16 | 2 | 1 | 16 | 4 |
| mz2-4x32 | 4 | 2 | 32 | 8 |
| mz2-8x64 | 8 | 4 | 64 | 16 |
| mz2-16x128 | 16 | 8 | 128 | 32 |
{: caption="Memory profiles options for IBM Z or LinuxONE s390x instances" caption-side="bottom"}
{: #memory-s390x}
{: tab-title="s390x"}
{: tab-group="Memory"}
{: class="simple-tab-table"}
{: summary="Memory profiles options for s390x virtual server instances."}

| Instance profile | vCPU | Cores | GiB RAM | Bandwidth Cap (Gbps) | I
|---------|---------|---------|---------|---------|---------|
| mz2e-2x16 | 2 | 1 | 16 | 4 |
| mz2e-4x32 | 4 | 2 | 32 | 8 |
| mz2e-8x64 | 8 | 4 | 64 | 16 |
| mz2e-16x128 | 16 | 8 | 128 | 32 |
{: caption="Memory secure execution profiles options for IBM Z or LinuxONE s390x instances" caption-side="bottom"}
{: #memory-se-s390x}
{: tab-title="s390x with secure execution"}
{: tab-group="Memory"}
{: class="simple-tab-table"}
{: summary="Memory secure execution profiles options for s390x virtual server instances."}


## How the bandwidth is allocated using the UI
{: #vs-bandwidth-allocation-ui}
{: ui}

Instance bandwidth is allocated between volume bandwidth and networking bandwidth. The bandwidth capacity (Bandwidth Cap) is determined by the virtual server profile that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a bandwidth cap of 4 Gbps. The initial volume and network bandwidth allocation depends on the bandwidth set by the instance profile you selected. You can also see the bandwidth allocations in the profile information during instance creation in the console. The bandwidth allocation can be changed on the instance details page after provisioning an instance.

For example, for the bz2-2x8 profile you might have:

* Storage: 1 Gbps
* Network: 3 Gbps

For a cz2-8x16 profile:

* Storage: 4 Gbps
* Network: 12 Gbps

The amount of overall bandwidth provided to volume bandwidth can be adjusted within the overall instance limits. A default amount of volume bandwidth is set on each instance profile.

For more information, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles) and [Adjusting bandwidth allocation using the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).

## How the bandwidth is allocated using the API
{: #vs-bandwidth-allocation-api}
{: api}

Instance bandwidth is allocated between volume bandwidth and networking bandwidth. The bandwidth capacity (Bandwidth Cap) is determined by the virtual server profile that you select during instance provisioning. For example, a bz2-2x8 balanced server profile allows a bandwidth cap of 4 Gbps. The initial volume and network bandwidth allocation depends on the bandwidth you set by using the API or by the instance profile you selected. You can see bandwidth allocations with the `/instance/profiles` endpoint in the API. The bandwidth can be changed during or after provisioning an instance.

For example, for the bz2-2x8 profile you might have:

* Storage: 1 Gbps
* Network: 3 Gbps

For a cz2-8x16 profile:

* Storage: 4 Gbps
* Network: 12 Gbps

The amount of overall bandwidth provided to volume bandwidth can be adjusted within the overall instance limits. A default amount of volume bandwidth is set on each instance profile.

For more information, see [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api) and [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

## Viewing profile configurations
{: #vs-popular-profiles}

You can view available profile configurations by using the {{site.data.keyword.cloud_notm}} console or the CLI. In the {{site.data.keyword.cloud_notm}} console, you can select from popular profile configurations that support most common use cases.

### Understanding the naming rule of the profiles
{: #vs-profiles-naming-rule}

The following information describes the naming rule of the profiles.

The first character represents the profile families. Different profile families have different ratios of vCPU to memory and other characteristics that are designed for different workloads.

- "b": balanced family of profiles, 1 vCPU to 4 GiB of memory ratio
- "c": compute family of profiles (higher on the CPUs), 1 vCPU to 2 GiB of memory ratio
- "m": memory family of profiles (higher on the memory), 1 vCPU to 8 GiB of memory ratio

The second character represents the CPU architecture.

- "z": s390x

The third character represents the generation of the IBM Cloud infrastructure where the profile is provisioned.

- "1": IBM Cloud Virtual Servers for Classic
- "2": IBM Cloud Virtual Servers for VPC


The fourth character is an "e", such as bz2e, which identifies the profile as a secure execution profile. These profiles are available when you select IBM Hyper Protect Container Runtime image as the image for provisioning the virtual server instance. For more information, see [s390x virtual server images](/docs/vpc?topic=vpc-vsabout-images). The characters after "-" represents the number of vCPUs and the size of RAM (GiB). For example, "2x8" means that this profile has 2 vCPU and 8 GiB of RAM.

Using “bz2-4x16” as an example, you can know from the name that it is a balanced profile that provides 4 vCPUs of compute and 16 GiB of memory. The profile is deployed on an s390x host and is for the second-generation VPC.

### Viewing instance profiles in the console
{: #vs-profiles-using-console}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
2. From the Virtual server instances page, click **New instance**.
3. You can either select a profile configuration from **Popular profiles** or click **All profiles** to view more configurations.

### Viewing instance profiles from the CLI
{: #vs-profiles-using-cli}
{: cli}

To view the list of available instance profiles by using the CLI, run the following command:
```sh
ibmcloud is instance-profiles
```
{: pre}

### Viewing instance profiles with the API
{: #vs-profiles-using-api}
{: api}

To view the list of available instance profiles by using the API, you can call the [List all instance profiles API](/apidocs/vpc/latest#list-instance-profiles).

The following request example lists the available instance profiles. When you call the API, replace the API endpoint and IAM token with the values from your enterprise. For more information about the `$vpc_api_endpoint` and `$iam_token` variables, see the Authentication and Endpoint URLs sections in [Virtual Private Cloud API Introduction](/apidocs/vpc/latest#about-vpc-api).

```curl
curl -X GET \
"$vpc_api_endpoint/v1/instance/profiles?version=2021-02-23&generation=2" \
-H "Authorization: $iam_token"
```
{: codeblock}

## Block Storage volume notes for profiles
{: #vs-block-storage-notes-for-profiles}

When you create secondary data volumes, you select a volume profile that best meets your requirements. Volume profiles are available as three predefined [tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) or as a [custom volume profile](/docs/vpc?topic=vpc-block-storage-profiles#custom). These volume profiles relate to virtual server instance profiles:

* A [3 IOPS general-purpose tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers) provides IOPS/GB performance suitable for a virtual server instance Balanced profile.
* A [5-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance suitable for a virtual server instance Compute profile.
* A [10-IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profile provides IOPS/GB performance suitable for a virtual server instance Memory profile.

## Next steps
{: #vs-nextsteps-profiles}

After you choose a profile, it's time to create an instance.

* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli)
* [Creating an instance by using the API](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api#create-instance-api)
