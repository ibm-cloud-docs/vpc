---

copyright:
  years: 2017, 2026
lastupdated: "2026-01-02"

keywords: secure, region, zone, subnet, public gateway, floating IP, NAT
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About networking
{: #about-networking-for-vpc}

{{site.data.keyword.vpc_full}} (VPC) is a virtual network that is linked to your customer account. It gives you cloud security, with the ability to scale dynamically, by providing fine-grained control over your virtual infrastructure and your network traffic segmentation.
{: shortdesc}

## Overview
{: #networking-overview}

Each VPC is deployed to a single region. Within that region, the VPC can span multiple zones.

Subnets in your VPC can connect to the public internet through an optional public gateway. You can assign floating IP addresses to any virtual server instance to enable it to be reachable from the internet, independent of whether its subnet is attached to a public gateway.

Subnets within the VPC offer private connectivity; they can talk to each other over a private link through the implicit router. Setting up routes is not necessary. Figure 1 shows how you can subdivide a virtual private cloud with subnets and each subnet can reach the public internet.

![Figure showing how a VPC can be subdivided with subnets](images/vpc-experience-simple.svg "Figure showing how a VPC can be subdivided with subnets"){: caption="IBM VPC connectivity and security" caption-side="bottom"}


## Terminology
{: #networking-terminology}

To work with your VPC, review the basic concepts of _region_ and _zone_ as they apply to your deployment.

### Regions
{: #networking-terms-regions}

A [region](#x2091391){: term} is an abstraction of the geographic area in which a VPC is deployed. Each region contains multiple [zones](#x2070723){: term}. A VPC can span multiple zones within its assigned region. {{site.data.keyword.cloud_notm}} provides two tiers of regions: [multizone regions](#x9774820){: term} and [single-campus multizone regions](#x10127487){: term}.

### Zones
{: #networking-terms-zones}

A default address prefix is assigned to each zone. The default address prefix specifies the address range in which subnets can be created. If the default address scheme does not suit your requirements, such as if you want to bring your own public IPv4 address range, you can customize the address prefixes. The mapping of logical zone names to the physical zones is relative to the account, so the default address prefix range for a zone may differ across accounts. For more information, see [IBM Cloud locations for resource deployment](/docs/overview?topic=overview-locations#zone-mapping).

## Characteristics of subnets in the VPC
{: #subnets-in-the-vpc}

Each subnet consists of a specified IP address range (CIDR block). Subnets are bound to a single zone, and they cannot span multiple zones or regions. Subnets in the same VPC are connected to each other.

### Addresses reserved by the system
{: #addresses-reserved-by-the-system}

Certain IP addresses are reserved for use by IBM for operating the VPC. The following addresses are the reserved addresses (these IP addresses assume that the subnet's CIDR range is `10.10.10.0/24`):

* First address in the CIDR range (`10.10.10.0`): Network address
* Second address in the CIDR range (`10.10.10.1`): Gateway address
* Third address in the CIDR range (`10.10.10.2`): reserved by IBM
* Fourth address in the CIDR range (`10.10.10.3`): reserved by IBM for future use
* Last address in the CIDR range (`10.10.10.255`): Network broadcast address

## External connectivity
{: #external-connectivity}

External connectivity can be achieved by using a public gateway that is attached to a subnet, or a floating IP address that is attached to a virtual server instance. Use a public gateway for source network address translation (SNAT) and a floating IP for destination network address translation (DNAT).

This table summarizes the differences between the options:

| Public gateway | Floating IP |
| ---- | ---- |
| Instances can initiate connections to the internet, but they can't receive connections from the internet.| Instances can initiate or receive connections to or from the internet |
| Provides connectivity for an entire subnet | Provides connectivity for a single instance |
{: caption="External connectivity options" caption-side="bottom"}

For secure external connectivity, use the VPN service to connect your VPC to another network. For more information about VPNs, see [Using VPN with your VPC](/docs/vpc?topic=vpc-using-vpn).

### Use a public gateway for external connectivity of a subnet
{: #public-gateway-for-external-connectivity}

A **Public Gateway** enables a subnet and all its attached virtual server instances to connect to the internet. Subnets are private by default. After a subnet is attached to the public gateway, all instances in that subnet can connect to the internet. Although each zone has only one public gateway, the public gateway can be attached to multiple subnets.

Public gateways use _Many-to-1 NAT_, which means that thousands of instances with private addresses use one public IP address to communicate with the public internet.

The following figure summarizes the current scope of gateway services.

| SNAT | DNAT | ACL | VPN |
| ---- | ---- | --- | --- |
| Instances can have outbound-only access to the internet. | Allow inbound connectivity from the internet to a Private IP. | Provide restricted inbound access from the internet to instances or subnets. | Site-to-site VPN handles customers of any size, and single or multiple locations. |
| Entire subnets share the outbound public endpoint. | Provides limited access to a single private server. | Restrict access inbound from internet, based on service, protocol, or port. | High throughput (up to 10 Gbps) provides customers the ability to transfer large data files securely and quickly. |
| Protects instances; Cannot initiate access to instances through the public endpoint. | DNAT service can be scaled up or down, based on requirements. | Stateless ACLs allow for granular control of traffic. | Create secure connections with industry-standard encryption. |
{: caption="Current scope of gateway services" caption-side="bottom"}

You can create only one public gateway per zone. However, that public gateway can be attached to multiple subnets in the zone.
{: tip}

### Use a Floating IP address for external connectivity of a virtual server instance
{: #floating-ip-for-external-connectivity}

Floating IP addresses are IP addresses that are provided by the system and are reachable from the public internet.

You can reserve a floating IP address from the pool of available addresses that are provided by IBM, and you can associate it with a network interface of any instance in the same zone. That interface also has a private IP address. Each floating IP address can be associated with only one interface. 

#### Notes
{: #fip-notes}

* Associating a floating IP address with an instance removes the instance from the public gateway's Many-to-1 NAT.
* Currently, floating IP supports only IPv4 addresses.
