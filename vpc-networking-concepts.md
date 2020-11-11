---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-11"

keywords: vpc, vpc network, VRF, router, hypervisor, address prefixes, classic access, implicit router, packet flows, NAT, data flows, Cloud Service Endpoint source addresses, source addresses

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# VPC behind the curtain
{: #vpc-behind-the-curtain}

This page presents a more detailed conceptual picture of what's happening "behind the curtain" with respect to VPC networking. Learn about network isolation, address prefixes, Cloud Service Endpoint source addresses, data packet flows, external IP address life cycle, and Classic infrastructure access. Readers are expected to have some networking background.
{: shortdesc}

## Network isolation
{: #network-isolation}

VPC network isolation takes place at three levels:

* **Hypervisor**: The VSIs (virtual server instances) are isolated by the hypervisor itself. A VSI can not directly reach other VSIs hosted by the same hypervisor if they are not in the same VPC.

* **Network**: Isolation occurs at the network level through the use of **virtual network identifiers** (VNIs). These identifiers are assigned to each subnet and scoped to a single zone. A VNI is added to all data packets entering any zone of the VPC: entering either from the hypervisor, when sent by a VSI, or entering the zone from the cloud, when sent by the implicit routing function.

A packet leaving a zone has the VNI stripped off. When the packet reaches its destination zone, entering through the implicit routing function, the implicit router always adds the proper VNI for that zone.
{: note}

* **Router**: The _implicit router function_ provides isolation to each VPC by providing a **virtual routing function** (VRF) and a VPN with MPLS (multi-protocol label switching) in the cloud backbone. Each VPC's VRF has a unique identifier, and this isolation allows each VPC to have access to its own copy of the IPv4 address space. The MPLS VPN allows for federating all edges of the cloud: Classic Infrastructure, Direct Link, and VPC.

## Address prefixes
{: #address-prefixes}

Address prefixes are the summary information used by a VPC's implicit routing function to locate a _destination VSI_, regardless of the availability zone in which the destination VSI is located. The primary function of address prefixes is to optimize routing over the MPLS VPN, while avoiding pathological routing cases. All subnets created in a VPC must be contained in an address prefix, so that all VSIs in a VPC are reachable from all other VSIs in the VPC.

## Cloud Service Endpoint source addresses 
{: #cse-source-addresses}

Cloud Service Endpoint source addresses are the IP addresses that identify a VPC and zone combination outside of the VPC. For example, a source address is used when a service outside of the VPC is called through a cloud service endpoint. The IP address of the virtual server instance is replaced with an IPv4 address, the source address, that identifies the VPC to the cloud service endpoint. 

## Data packet flows and the implicit router
{: #data-packet-flows-and-the-implicit-router}

Six different types of VSI data packet flows occur in a VPC. In ascending order of complexity, these flows are:

* Intra-subnet, intra-host (same hypervisor)
* Intra-subnet, inter-host
* Inter-subnet, intra-zone
* Inter-subnet, inter-zone
* Extra-VPC service (for IaaS or CSE access)
* Extra-VPC internet (for internet access)

**Intra-subnet, intra-host** data flows: These are the simplest. Packets flow between the VSIs on the hypervisor, and no packets are leaving the hypervisor.

**Intra-subnet, inter-host** data flows: These flows involve packets leaving the hypervisor. Each packet is tagged with the proper VNI (virtualized network identifier) to ensure data isolation, and then it's sent to the destination hypervisor, which hosts the destination VSI. The destination hypervisor strips off the VNI and forwards the data packet to the destination VSI.

**Inter-subnet, intra-zone** data flows: These flows involve packets that utilize the VPC's implicit router function, which connects all subnets created in the VPC. It routes the data packet to the correct destination hypervisor. If the destination hypervisor is different from the source hypervisor, the data packet is tagged with the proper VNI and sent to the destination hypervisor. There, the VNI is stripped off and the data packet is forwarded to the destination VSI. (These last steps are the same as described in the previous type of data flow.)

**Inter-subnet, inter-zone** data flows: For these flows, the implicit router function removes the VNI and forwards the packet in the VPC's MPLS VPN for transit across the cloud backbone. At the destination zone, the implicit router function tags the data packet with the appropriate VNI. Then the packet is forwarded to the destination hypervisor, where the VNI is (as previously described) stripped off again so that the data packet can be forwarded to the destination VSI.

**Extra-vpc service** data flows: Packets destined for IaaS or IBM Cloud Service Endpoint (CSE) services utilize the VPC's implicit router function, and they also utilize a network address translation (NAT) function. The translation function replaces the VSI address with an IPv4 address, which identifies the VPC to the IaaS or CSE service that's being requested.

**Extra-vpc internet** data flows: Packets destined for the internet are the most complex. Besides utilizing the VPC's implicit router function, each of these flows also relies on one of the implicit router's two network address translation (NAT) functions:

  * an explicit one-to-many NAT through a public gateway function that serves all subnets connected to it.
  * one-to-one NAT assigned to individual VSIs.

After NAT translation, the implicit router forwards these internet-destined packets to the internet, using the cloud backbone.

## Life cycle of external IPs associated with public gateway functions
 {: #pgw-external-IP-lifecycle}
 As both external IPs and PGWs are bound to an availability zone, a public gateway function can only have a single external IP. This external IP has the following lifecycle:
 
   * The external IP is allocated when the public gateway is created.
   * The external IP is released when the public gateway is deleted.

## Classic access
{: #classic-access}

The [**classic access**](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure) feature for VPC is accomplished by re-using the VRF identifier from the {{site.data.keyword.cloud}} classic infrastructure account as the VRF identifier for VPC. This implementation allows the VPC's implicit router function to join the same MPLS VPN that is used by the classic infrastructure account. Thus, the VPC has access to classic resources and to anything else that's reachable by means of existing Direct Link connections.
