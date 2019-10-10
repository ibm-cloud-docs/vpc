---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-09-30"

keywords: secure, region, zone, subnet, public gateway, floating IP, NAT
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

# About Networking for VPC
{: #about-networking-for-vpc}


{{site.data.keyword.vpc_full}} (VPC) is a virtual network that's tied to your customer account. It gives you cloud security, with the ability to scale dynamically, by providing fine-grained control over your virtual infrastructure and your network traffic segmentation.
{:shortdesc}

## Overview
{: #networking-overview}

Each VPC is deployed to a single region. Within that region, the VPC can span multiple zones. 

Subnets in your VPC can connect to the public internet through an optional public gateway. You can assign floating IP addresses to any virtual server instance to enable it to be reachable from the internet, independent of whether its subnet is attached to a public gateway. 

Subnets within the VPC offer private connectivity; they can talk to each other over a private link through the implicit router. Setting up routes is not necessary. Figure 1 shows how you can subdivide a Virtual Private Cloud with subnets and each subnet can reach the public internet.

![Figure showing how a VPC can be subdivided with subnets](images/vpc-connectivity-and-security.svg "Figure showing how a VPC can be subdivided with subnets"){: caption="Figure 1. IBM VPC connectivity and security" caption-side="top"}

## Terminology
{: #networking-terminology}

See [VPC concepts](/docs/vpc?topic=vpc-vpc-concepts) for definitions and information about terms that are used in this document for {{site.data.keyword.vpc_short}}. When working with your VPC, you'll need to be familiar with the basic concepts of _region_ and _zone_ as they apply to your deployment.

### Regions
{: #networking-terms-regions}

A region is an abstraction that is related to the geographic area in which a VPC is deployed. Each region contains multiple zones, which represent independent fault domains. A VPC can span multiple zones within its assigned region.

### Zones
{: #networking-terms-zones}

A zone is an abstraction that refers to the physical data center that hosts the compute, network, and storage resources, as well as the related cooling and power, which provides services and applications. Zones are isolated from each other, so to create no shared single point of failure, improved fault tolerance, and reduced latency. Each zone is assigned a default address prefix, which specifies the address range in which subnets can be created. If the default address scheme does not suit your requirements, such as if you want to bring your own public IPv4 address range, you can customize the address prefixes.

## Characteristics of subnets in the VPC
{: #subnets-in-the-vpc}


Each subnet consists of a specified IP address range (CIDR block). Subnets are bound to a single zone, and they cannot span multiple zones or regions. Subnets in the same VPC are connected to each other.

### Reserved IP addresses
{: #reserved-ip-addresses}

Certain IP addresses are reserved for use by IBM when operating the VPC. Here are the reserved addresses (these IP addresses assume that the subnet's CIDR range is 10.10.10.0/24):

  * First address in the CIDR range (10.10.10.0): Network address
  * Second address in the CIDR range (10.10.10.1): Gateway address
  * Third address in the CIDR range (10.10.10.2): reserved by IBM
  * Fourth address in the CIDR range (10.10.10.3): reserved by IBM for future use
  * Last address in the CIDR range (10.10.10.255): Network broadcast address

### Use a Public gateway for external connectivity of a subnet
{: #public-gateway-for-external-connectivity}

A **Public Gateway** enables a subnet and all its attached virtual server instances to connect to the internet. Subnets are private by default. After a subnet is attached to the public gateway, all instances in that subnet can connect to the internet. Although each zone has only one public gateway, the public gateway can be attached to multiple subnets. 

Public gateways use _Many-to-1 NAT_, which means that thousands of instances with private addresses use one public IP address to communicate with the public internet.

Table 1 summarizes the scope of gateway services for source and destination network address translation (SNAT and DNAT):

| SNAT | DNAT |
| ---- | ---- |
| Instances can have outbound-only access to the internet | Allows inbound connectivity from the internet to a private IP | 
| Entire subnets share the same outbound public endpoint | Provides limited access to a single private server |
| Protects instances. Access to instances can't be initiated through the public endpoint | DNAT service can be scaled up or down based on requirements |
{: caption="Table 1. Scope of gateway services" caption-side="top"}

You can create only one public gateway per zone, but that public gateway can be attached to multiple subnets in the zone.
{:tip}

### Use a Floating IP address for external connectivity of a virtual server instance
{: #floating-ip-for-external-connectivity}

Floating IP addresses are IP addresses that are provided by the system and are reachable from the public internet.

You can reserve a floating IP address from the pool of available addresses that are provided by IBM, and you can associate it with a network interface of any instance in the same zone. That interface also will have a private IP address. Each floating IP address can be associated with only one interface. 

**Notes:**
* Associating a floating IP address with an instance removes the instance from the public gateway's Many-to-1 NAT.
* Currently, floating IP supports only IPv4 addresses.
