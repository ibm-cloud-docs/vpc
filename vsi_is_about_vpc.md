---

copyright:
  years: 2018, 2020
lastupdated: "2020-03-02"

keywords: virtual server instances, VSI, planning, best practices

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}

# About virtual server instances for VPC
{: #about-advanced-virtual-servers}

Virtual server instances for VPC give you access to all of the benefits of {{site.data.keyword.vpc_short}}, including network isolation, security, and flexibility. 
{:shortdesc}

## What are virtual server instances for VPC?
{: #what-are-vsis}
With virtual server instances for VPC, you can quickly provision instances with high network performance. When you provision an instance, you select a profile that matches the amount of memory and compute power that you need for the application that you plan to run on the instance. After you provision an instance, you control and manage those infrastructure resources. 

## How are virtual server instances for VPC different from other IBM virtual server offerings?
{: #how-are-vsis-different}

In today's IBM Cloud Virtual Server offering, instances use native subnet and VLAN networking to communicate to each other within a data center (and single pod). Using subnet and VLAN networking in one pod works well until you must scale up or have large virtual resource demands that require resources to be created between pods. (Adding appliances for VLAN spanning can get expensive and complicated!) 

{{site.data.keyword.vpc_short}} adds a network orchestration layer that eliminates the pod boundary, creating increased capacity for scaling instances. The network orchestration layer handles all of the networking for all virtual server instances that are within a VPC across regions and zones. With the software defined networking capabilities that VPC provides, you have more options for multi-vNIC instances and larger subnet sizes. 

## Virtual Servers for VPC on POWER
{: #about-power-vsi}
Virtual server instances with an IBM POWER9 CPU run the Power Instruction Set Architecture (ISA) and Linux workloads. These systems provide up to 3 Gbps per vCPU, provide NVLink2 CPU-to-GPU connectivity for virtual server profiles with NVIDIA Tesla v100 GPUs (with 32 GB RAM per GPU), and natively run within your standard VPC. POWER-based instances that run within your VPC, share the same security groups, subnets, and storage as your x86-based instances.


POWER-based instances run many Linux distributions. However, they run a different ISA than x86-based instances. So, any compiled code (for example, C, C++, or golang) must be compiled for ppc64le. Many build tools support compiling for ppc64le (such as Jenkins, Travis CI, and other tools) and have flows to support building for the ppc64le ISA. If you are building a LAMP or MEAN stack application, these technologies usually use interpreted code. Often times these applications don't require any changes to run on the instances.


### Maximizing the performance of POWER-based instances
{: #power-performance}
The CPUs that are used in this solution are SMT-4 based cores. SMT-4 based cores offer up to four threads per physical core and are configurable from one to four threads per core. The lower the threads per core, the faster a single thread can run on that core. The more threads that are enabled, the overall throughput of the core is higher (potentially at the expense of per thread latency). Different workloads have different characteristics.

By default, the POWER-based instances start in SMT-2 mode. For more information see, [Changing SMT settings for POWER-based instances](/docs/vpc?topic=vpc-vsi_is_power_perf).

### How do the POWER-based instances in a VPC differ from all the Power Systems offerings in IBM Cloud?
{: #power-differences}

The Power Virtual Server offering provides AIX and IBM i instances hosted as a separate service.  This offers its own network and can use IBM Cloud Direct Link to secure connections to other services in the IBM Cloud.

POWER-based instances in VPC provide Linux on Power access (with or without GPUs).  These are located natively on the same network, have the VPC feature set, and use the same API as the rest of the service. For instance, security groups can be shared across x86 and POWER-based instances.

### What applications can I run on Linux on Power?
{: #power-apps}

There are thousands of applications available to run on Linux on POWER.  Many open source applications have been compiled for the Power Architecture and are available in public repositories.  For a complete inventory of those applications, see the [Open Source Power Availability Tool](https://www.ibm.com/developerworks/library/l-ospat-trs/index.html){: external}. Additionally, many applications that are written for x86 architectures can run (without any or few code changes) on Power.  For more information on running x86 applications on Power, see [Porting x86 Linux applications to IBM POWER](https://developer.ibm.com/linuxonpower/porting-guide/){: external}. 

For more informatiom about the IBM Cloud Virtual Servers for VPC on POWER or to dive into deep learning and AI with Power systems, see the following links:

* [IBM Cloud Virtual Private Cloud on POWER](https://developer.ibm.com/linuxonpower/power-virtual-private-cloud/){: external}  
* [Deep learning and AI on IBM Power Systems](https://developer.ibm.com/linuxonpower/deep-learning-powerai/){: external}

