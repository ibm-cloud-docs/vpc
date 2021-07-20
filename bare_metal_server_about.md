---

copyright:
  years: 2021
lastupdated: "2021-07-20"

keywords: bare metal servers, vpc, baremetal, what is bare metal

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

# About Bare Metal Servers for VPC (beta)
{: #about-bare-metal-servers}

{{site.data.keyword.bm_is_short}} provides bare metal servers to host VMware clusters in the {{site.data.keyword.vpc_full}} (VPC). You can set up VMware management applications and create VMware virtual machines (VM) on the bare metal servers. As bare metal servers are integrated with the VPC platform, you can take advantage of the network, storage, and security capacity provided by the VPC.

This is a Beta feature that requires special approval. Contact your IBM Sales representative if you are interested in getting access.
{:beta}

All the bare metal servers created in the Beta period will be reclaimed when Beta ends. All data on the servers will be deleted. Please migrate your workloads and data before the end of Beta to avoid any data loss.
{:important}

Start [creating a bare metal server](/vpc-ext/provision/bm) on VPC now.
{:tip}

The bare metal server that you provisioned by using this service has VMware ESXi 7.x hypervisor installed. You are responsible for setting up the VMware cluster, including installing vCenter Server, vSAN, NSX-T, and so on.
{: note}

## Product benefits
{: #benefit}

* **VMware-specific**: VMware ESXi is installed automatically on the bare metal server during provisioning. Once the provisioning is complete, you can configure vCenter Server, vSAN, NSX-T by yourself following the guide that we provide. 

* **Support for all native VPC functionality**: The bare metal server leverages SmartNIC to support all VPC network capabilities, thereby making it a first-class player in the VPC world. The VMware VMs that are created on the bare metal server can connect with IBM virtual server instances as long as they are in the same VPC. {{site.data.keyword.bm_is_short}} enables additional network capability compared to virtual server instances to support multiple VMware's technologies, for example, vMotion (for live migration of VMs), IP Failover, and VMware's VM Scheduler.

* **Dedicated resources**: With no sharing of resources, bare metal servers provide higher performance compared to virtual server instances. In addition, you can access and control the low-level hardware of the bare metal server to install your own images. 

* **Fast provisioning**: Compared to the classic bare metal servers, {{site.data.keyword.bm_is_short}} significantly reduces the time for provisioning and deploying. Provisioning a bare metal server on VPC takes minutes instead of hours by comparison.

* **Improved security and compliance**: {{site.data.keyword.bm_is_short}} provides dedicated hardware. It also allows you to take full control over your ESXi hypervisor to better meet your specific security and compliance requirements. 

## How are Bare Metal Servers for VPC different from other compute offerings?
{: #comparison}

### Bare Metal Servers for VPC versus bare metal server on classic infrastructure
{: #vs-classic-bare-metal}

With {{site.data.keyword.bm_is_short}}, you can enjoy the security and performance of the private cloud with the flexibility and scalability of the public cloud. Compared to the classic bare metal infrastructures, {{site.data.keyword.bm_is_short}} provides better connectivity and networking throughput that is empowered by VPC concepts. It also takes less time on provisioning and deploying. 

It is important to notice that {{site.data.keyword.bm_is_short}} is less customizable than classic bare metal servers. Most apparently, {{site.data.keyword.bm_is_short}} currently only supports the VMware use cases with ESXi. In its current state, it lacks file storage support compared to classic bare metal servers too.

### Bare metal server versus virtual server instances
{: #vs-virtual-servers}

In general, you should choose bare metal servers over virtual server instances if you need access to the actual hardware to run a hypervisor such as VMware's ESXi, or run real-time workloads. Otherwise, virtual server instances would be a better choice.

{{site.data.keyword.bm_is_short}} sets up a VMware virtualization environment in the VPC. However, you cannot install and run VMware’s vCenter Server and other management apps on a virtual server instance. 

{{site.data.keyword.bm_is_short}} offers improved security compared to a virtual server instance as customers fully manage the physical resources of a bare metal server until it is decommissioned. By contrast, virtual server instances can share resources such as CPU, memory, and processes, across an IBM managed hypervisor.

In addition, note the following differences regarding lifecycle operations: 

* For bare metal servers, after creating a server, you can choose to reboot, or power off/on the server. When powering off a server, the server is powered off physically, but the data on it is preserved, and it continues to be billed.

* For virtual server instances, you can reboot, stop, and start the instance, but this will not directly impact the physical machine's status. Billing is suspended for some types of instances when powered off. However, any persistent storage will continue to be billed.

<!-- To compare all virtual compute options, see [Compare Virtual Compute options](link)-->

## High-level architecture of Bare Metal Servers for VPC
{: #architecture-diagram}

Refer to the diagram below for a high-level overview of the architecture of Bare Metal Servers for VPC. For more information about VPC workload isolation architecture, see [VPC workload isolation architecture](/docs/vpc?topic=vpc-vpc-isolation#vpc_architecture).

  ![Figure showing high-level architecture of Bare Metal Servers for VPC](images/bare_metal_server_archi_diagram.png "Figure showing high-level architecture of Bare Metal Servers for VPC"){: caption="Figure 1. High-level architecture of Bare Metal Servers for VPC" caption-side="top"}

## Next steps
{: #next-step}

Refer to the following topics to start planning and creating your bare metal servers on VPC:

* [Planning for Bare Metal Servers for VPC](/docs/vpc?topic=vpc-planning-for-bare-metal-servers)
* [Creating a bare metal server on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers)
