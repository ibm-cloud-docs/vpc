---

copyright:
  years: 2018, 2020
lastupdated: "2020-08-26"

keywords: 

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:note: .note}

# Network interfaces
{: #using-instance-vnics}

A network interface connects a virtual server instance to a network. When you create a virtual server 
instance, you can use network interfaces to assign multiple IP addresses. 
{:shortdesc}

The following list highlights how network interfaces work with your instance.

* You can create and assign up to 5 network interfaces for each virtual server instance. 
* You can attach each network interface to a different subnet in the same zone.
* Each network interface receives a private IP address from the subnet range.
* You can attach one floating IP address to the virtual server instance. Initially, the floating IP address must be attached to the primary network interface. By default the primary interface is `eth0` in {{site.data.keyword.cloud_notm}} console.
* You can associate and unassociate a single floating IP address for the instance to and from each network interface.
* You can assign security groups to each network interface.
* You can change the name of any existing network interface.

Bandwidth is distributed across the network interfaces that are attached to the virtual server instance. For more information, see [Network performance notes for profiles](/docs/vpc?topic=vpc-profiles#network-perf-notes-for-profiles).

If you assign a new network interface to a virtual server instance while it is running, you must take action to configure the network interface for the instance to use. You can either stop and then restart the instance, or you can manually configure the interface in the guest operating system. For example, on a Linux-based operating sytem, you can use the `ip link set dev <interface> up` to retrieve the IP address configuration for the interface. 
{: note}
