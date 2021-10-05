---

copyright:
  years: 2021
lastupdated: "2021-09-16"

keywords: bare metal servers, vpc, baremetal, what is bare metal, about bare metal

subcollection: vpc

---

{:beta: .beta}
{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# About Bare Metal Servers for VPC (Beta)
{: #about-bare-metal-servers}

Bare metal servers on VPC is a Beta feature that requires special approval. Contact your IBM Sales representative if you're interested in getting access. 
{: beta}

{{site.data.keyword.bm_is_short}} provides bare metal servers to host VMware&reg; clusters in the {{site.data.keyword.vpc_full}} (VPC). You can set up VMware management applications and create VMware virtual machines (VM) on the bare metal servers. As bare metal servers are integrated with the VPC platform, you can take advantage of the network, storage, and security capacity provided by the VPC. 

The bare metal server that you provisioned by using this service has VMware ESXi 7.x hypervisor installed. You are responsible for setting up the VMware cluster, including installing vCenter Server, vSAN, NSX-T, and so on.

All the bare metal servers that are created in the Beta period are reclaimed when Beta ends. All data that is on the servers is deleted. Make sure that you migrate your workloads and data before the end of Beta to avoid any data loss.
{: important}

## Benefits of Bare Metal Servers for VPC
{: #bare-metal-benefits}

{{site.data.keyword.bm_is_short}} are single-tenant physical servers that provide you performance and control with low-level access to the hardware resources. Your server isn’t shared with “noisy neighbors,” it’s all yours!

* **Advanced CPU microarchitecture**: {{site.data.keyword.bm_is_short}} on {{site.data.keyword.vpc_full}} use 2nd Generation Intel&reg; Xeon&reg; Platinum 8260 Processors. Intel Xeon 8260 processors are optimized for cloud enterprise applications, HPC workloads, IoT workloads, with enhanced networking and security.

* **VMware-specific**:   VMware ESXi is installed automatically during provisioning. After you provision a server, you can configure vCenter Server, vSAN, and NSX-T. 

* **Support for all native VPC functionality**: Your bare metal server uses SmartNIC to support all VPC network capabilities. The VMware instances that are created on the bare metal server can connect with {{site.data.keyword.cloud}} virtual server instances if they are in the same VPC. {{site.data.keyword.bm_is_short}} enables extra network capability compared to virtual server instances to support multiple VMware's technologies. For example, vMotion (for live migration of instances), IP Failover, and VMware's VM Scheduler.

* **Dedicated resources**: With no sharing of resources, bare metal servers provide higher performance compared to virtual server instances. You can access and control the low-level hardware of the bare metal server to install your own images. 

* **Fast provisioning**: Compared to the classic {{site.data.keyword.baremetal_long}}, {{site.data.keyword.bm_is_short}} significantly reduces provisioning and deploying time. Provisioning a bare metal server on VPC takes minutes instead of hours.

* **Improved security and compliance**: {{site.data.keyword.bm_is_short}} provides dedicated hardware. You can also take full control over your ESXi hypervisor to better meet your specific security and compliance requirements.

You are responsible for security on your bare metal server. That means upgrading or patching the operating system as needed to make sure that vulnerabilities are addressed in a timely manner. Bare metal servers with associated floating IPs are internet-facing and you need to take appropriate precautions. For more information, see [Understanding your responsibilities](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance).
{: note}

## Advantages of Bare Metal Servers for VPC 
{: #comparison}

{{site.data.keyword.bm_is_short}} has the following advantages when you compare it to other compute offerings. 

### Bare Metal Servers for VPC versus bare metal server on classic infrastructure
{: #vs-classic-bare-metal}

With {{site.data.keyword.bm_is_short}}, you can enjoy the security and performance of the private cloud with the flexibility and scalability of the public cloud. Compared to the classic bare metal infrastructures, {{site.data.keyword.bm_is_short}} provides better connectivity and networking throughput by using VPC concepts. 

Keep in mind that {{site.data.keyword.bm_is_short}} is less customizable than classic bare metal servers. Most {{site.data.keyword.bm_is_short}} currently supports only VMware with ESXi. Currently, {{site.data.keyword.bm_is_short}} lacks file storage support compared when compared to classic bare metal servers.

### Bare metal servers versus virtual server instances
{: #bare-metal-vs-virtual-servers}

In general, you choose bare metal servers over virtual server instances if you need access to the actual hardware to run a hypervisor such as VMware's ESXi, or run real-time workloads. 

{{site.data.keyword.bm_is_short}} sets up a VMware virtualization environment in a VPC. However, you can't install and run VMware’s vCenter Server and other management apps on a virtual server instance. 

{{site.data.keyword.bm_is_short}} offers improved security compared to a virtual server instance as customers fully manage the physical resources of a bare metal server until it is decommissioned. By contrast, virtual server instances can share resources such as CPU, memory, and processes, across an IBM managed hypervisor.

Keep the following lifecycle operations differences in mind: 

* For bare metal servers, you can reboot, or power the server off and on. When you power off a server, the server is powered off physically, but the data on it is preserved, and you continue to be billed.

* For virtual server instances, you can reboot, stop, and start the instance. But these functions don't impact the physical server status. Billing is suspended for some types of instances when powered off. However, any persistent storage continues to be billed. For more information about suspend billing, see [Suspend billing for VPC](/docs/vpc?topic=vpc-suspend-billing).

## High-level architecture of Bare Metal Servers for VPC
{: #architecture-diagram}

Figure 1 shows an example of how bare metal servers can use the VPC networking functionality. For more information about VPC networking, see [About networking](/docs/vpc?topic=vpc-about-networking-for-vpc).

![Figure showing connectivity and security of Bare Metal Servers for VPC](images/bare_metal_server_network_diagram.png "Figure showing connectivity and security of Bare Metal Servers for VPC"){: caption="Figure 1. Bare Metal Servers for VPC connectivity and security" caption-side="top"}

See Figure 2 for the isolation architecture of Bare Metal Servers for VPC. For more information about VPC workload isolation architecture, see [VPC workload isolation architecture](/docs/vpc?topic=vpc-vpc-isolation#vpc_architecture).

![Figure showing isolation architecture of Bare Metal Servers for VPC](images/bare_metal_server_archi_diagram.png "Figure showing isolation architecture of Bare Metal Servers for VPC"){: caption="Figure 2. Isolation architecture of Bare Metal Servers for VPC" caption-side="top"}

## Next steps
{: #next-step}

See to the following topics to start planning and creating your bare metal servers on VPC:

* [Planning for Bare Metal Servers for VPC](/docs/vpc?topic=vpc-planning-for-bare-metal-servers)
* [Creating a bare metal server on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers)
