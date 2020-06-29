---

copyright:
  years: 2020
lastupdated: "2020-06-30"

keywords: load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:term: .term}
{:beta: .beta}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Load balancers overview
{: #nlb-vs-elb}

There are several differences between {{site.data.keyword.cloud}} Application Load Balancer and Network Load Balancer to be aware of when choosing one or the other for your needs.
{: shortdesc}

{{site.data.keyword.cloud_notm}} provides public and private facing application load balancers. An application load balancer provides Layer-7 and Layer-4 load balancing on {{site.data.keyword.cloud_notm}} and supports SSL offloading. The incoming and outgoing packets flow through the load balancer.

In contrast, a network load balancer only provides Layer-4 load balancing on {{site.data.keyword.cloud_notm}}, and does not support SSL offloading. Currently, {{site.data.keyword.cloud_notm}} provides public facing network load balancers only. The client sends public network traffic to the network load balancer, which forwards it to target Virtual Machines (VMs). Then, the target VMs respond directly to the client using Direct Server Return (DSR).

This gives network load balancers an advantage over application load balancers by enhancing performance in the following ways:

* The return traffic from the target VMs bypass the network load balancer and responds directly to the client.
* The network load balancer is only required to process incoming traffic, which allows it to be a very fast distributor of traffic/load.
* The network load balancer has a single highly available VIP that can be used directly (instead of through the assigned fully qualified domain name (FQDN). This helps clients that must use an IP to access the application/service served by the load balancer. It also helps with faster recovery in the event of failures compared to a DNS-based availability that the application load balancer uses.

The following table provides a comparison of the two types of load balancers.

|                             | Network load balancer    | Application load balancer            |
|-----------------------------|------------------|--------------------|
| HA mode                     | Active-standby (with single VIP)   |  Active-active (with multiple VIPs assigned to a DNS name) |
| Monitoring metrics with Sysdig | Not in beta<br />(coming soon) | Yes |
| Multi-zone support | No (see [Multi-zone support](/docs/vpc?topic=vpc-nlb-vs-elb#nlb-mz-support)) | Yes |     
| Source IP address is preserved | Yes | No |
| SSL offloading              | No              | Yes |
| Supported protocols         | TCP | HTTPS, HTTP, TCP  |
| Transport layer             | Layer-4         | Layer-4, Layer-7 |
| Types of load balancers | Public only for beta | Public and private |
| Virtual IP Address (VIP)    | Single    | Multiple |
{: caption="Table 1. Comparison of network and application load balancers" caption-side="top"}

## High Availability (HA) mode
{: #nlb-ha-mode}
The application load balancer is configured in active-active mode. All compute resources of the load balancer are actively involved in forwarding traffic.

High Availability (HA) is achieved using a Domain Name Service (DNS). VIP of each compute resource is registered with DNS. If any of the compute resources go down, the other resources continue to forward traffic.

Network load balancer is configured in active-standby mode. There is a single VIP registered with DNS, and traffic is forwarded through that compute resource. In the case of the active compute resource going down, the standby takes over and the VIP is transferred to the standby.

## Multi-zone support
{: #nlb-mz-support}
The network load balancer is limited to a single zone. All back-end servers should be in the same zone. A zone is identified by the subnet that is selected when a load balancer is created. Cloud Internet Services (CIS) Global Load Balancer can be used with multiple zonal network load balancers for multi-zone availability.

The application load balancer can be configured to span multiple zones. The back-end servers can be in any zone within a region.

## Application load balancer data flow path
{: #alb-data-flow}

![ALB traffic flow](images/alb-datapath.png)

## Network load balancer data flow
{: #nlb-management-access-data-flow}

The load balancer control plane configures the network load balancer through RESTful APIs using the floating IPs in the management port. The NLB belongs to the VPC load balancer service account; however, it is provisioned such that the VPC and subnet belongs to your account. Each network load balancer has a single network interface with a floating IP address for load balancing traffic.

![Network load balancer Architecture](images/nlb-workflow-customer-view.png "Network load balancer architecture"){: caption="n=Network load balancer work flow" caption-side="top"}

See [Data is stored and encrypted](/docs/vpc?topic=vpc-load-balancers#load-balancer-data-stored-encryted) for more details.
