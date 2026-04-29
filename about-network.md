---

copyright:
  years: 2017, 2026
lastupdated: "2026-04-29"

keywords: secure, region, zone, subnet, acl, vpn, direct link, public gateway, floating IP, NAT
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About VPC networking
{: #about-networking-for-vpc}

{{site.data.keyword.vpc_full}} (VPC) networking provides a private, isolated, and software-defined network environment for deploying cloud resources. It helps you control how resources communicate within a VPC, across private networks, and with the public internet, while maintaining strong isolation and security by default.

The following sections explain VPC networking concepts and how different networking components work together to help you design secure and scalable network architectures.

## VPC, regions, zones, and subnets
{: #vpc-regions-zones-subnets}

Before you work with your VPC, review the basic concepts of _regions_, _zones_, and _subnets_ as they apply to your deployment.

### Regions
{: #networking-terms-regions}

A [region](#x2091391){: term} is an abstraction of the geographic area in which a VPC is deployed. Each region contains multiple [zones](#x2070723){: term}. A VPC can span multiple zones within its assigned region.

{{site.data.keyword.cloud_notm}} provides two tiers of regions:

* [Multizone regions](#x9774820){: term} - Regions with three or more zones for high availability.
* [Single-campus multizone regions](#x10127487){: term} - Regions with multiple zones within a single campus.

For more information about regions, see [Locations for resource deployment](/docs/overview?topic=overview-locations).

### Zones
{: #networking-terms-zones}

A zone is a fault-isolated data center within a region. Each zone in a VPC is assigned a default address prefix that specifies the address range where subnets can be created. If the default address scheme doesn't meet your requirements, you can customize the address prefixes. For example, you might want to bring your own public IPv4 address range. For more information, see [Bring your own subnet](/docs/vpc?topic=vpc-configuring-address-prefixes).

The mapping of logical zone names to physical zones is relative to each account. As a result, the default address prefix range for a zone might differ across accounts. For more information, see [Zone mapping](/docs/overview?topic=overview-locations#zone-mapping).

### Subnets
{: #networking-terms-subnets}

A subnet is a range of IP addresses (CIDR block) within a VPC where you deploy resources. Subnets provide network segmentation and isolation for your workloads. For more information, see [About subnets](/docs/vpc?topic=vpc-about-subnets-vpc&interface=ui).

#### Subnet characteristics
{: #subnet-characteristics}

* Each subnet consists of a specified IP address range (CIDR block)
* Subnets are bound to a single zone and can't span multiple zones or regions
* Subnets within the same VPC are automatically connected to each other through an implicit router
* Each subnet reserves specific IP addresses for system use

#### Addressing and system‑reserved IP addresses
{: #system-reserved-addresses}

When you create a subnet, certain IP addresses within the CIDR range are reserved by IBM for operating the VPC. These addresses can't be assigned to your workloads. For example, if your subnet's CIDR range is `10.10.10.0/24`, the following addresses are reserved:

* First address in the CIDR range (`10.10.10.0`): Network address
* Second address in the CIDR range (`10.10.10.1`): Gateway address
* Third address in the CIDR range (`10.10.10.2`): Reserved by IBM
* Fourth address in the CIDR range (`10.10.10.3`): Reserved by IBM for future use
* Last address in the CIDR range (`10.10.10.255`): Reserved by the platform (traditionally the broadcast address)

   Plan your subnet sizing so that sufficient available IP addresses are available for your workloads.
   {: tip}

## The VPC networking model
{: #vpc-networking-model}

VPC networking is built around three connectivity scopes:

* **Internal connectivity** - Communication between resources within a single VPC
* **Private connectivity** - Communication of resources with other private networks and without using the public internet
* **External connectivity** - Communication of resources with or from the public internet

These scopes help you choose the appropriate networking services for your use case and design architectures that meet your security and performance requirements.

## Internal VPC connectivity
{: #internal-vpc-connectivity}

Internal connectivity enables communication between resources within a single VPC. This connectivity is automatic, private, and requires no additional configuration for basic subnet-to-subnet communication. All subnets within the same VPC are connected through an **implicit router**, which enables private subnet‑to‑subnet and instance‑to‑instance communication. You don’t need to configure routing tables or static routes for basic connectivity within a VPC.

This internal connectivity supports common architectures such as multitier applications (web, application, and database subnets), east–west traffic between instances, and cross‑zone communication for high availability.

### Virtual network interfaces
{: #virtual-network-interfaces}

All VPC resources that participate in networking attach to the network by using a **virtual network interface** (VNI). A VNI is a logical representation of a network interface that connects a resource to a subnet.

Key characteristics of VNIs:

* Created within a specific subnet
* Assigned a private IP address from the subnet's CIDR range
* Serve as attachment points for security groups
* Can be associated with floating IP addresses for external connectivity
* Form the foundation for traffic flow, security enforcement, and connectivity

VNIs help you to control network access at a granular level and provide flexibility in managing network configurations. For more information, see [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about).

### Security controls for internal connectivity
{: #internal-security-controls}

Traffic within a VPC is governed by multiple layers of security controls that work together to protect your resources.

#### Security groups
{: #security-groups}

Security groups provide stateful, instance-level security by controlling inbound and outbound traffic to individual resources. Applied at the **VNI level**, security groups maintain connection state information, which automatically permits the return traffic for an allowed connection without requiring an explicit rule. For more information, see [About security groups](/docs/vpc?topic=vpc-using-security-groups).

#### Network ACLs
{: #network-acls}

Network access control lists (ACLs) provide stateless, subnet-level security by filtering traffic that enters or leaves a subnet. Unlike security groups, network ACLs are stateless, which means that return traffic must be explicitly allowed through separate rules. Applied at the **subnet level**, network ACLs affect all resources within the subnet and provide coarse-grained filtering that serves as a baseline security policy. For more information, see [About network ACLs](/docs/vpc?topic=vpc-using-acls).

### Routing tables and routes
{: #routing-tables-routes}

VPC uses routing tables to control how traffic is directed within and beyond your VPC. While the implicit router automatically handles basic subnet-to-subnet connectivity, you can create custom routes to implement advanced routing scenarios and control traffic flow more precisely.

Use custom routing tables in the following cases:

* To override the default routing behavior for specific subnets or traffic sources.
* To route traffic through a virtual network appliance for inspection or processing.
* To implement hub-and-spoke network topologies with centralized services.
* To control how ingress traffic from the internet or VPN connections is routed.
* To implement advanced network architectures with multiple routing domains.

For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

## Private connectivity beyond a single VPC
{: #private-connectivity}

Private connectivity enables your VPC to communicate with other networks without routing traffic over the public internet. All traffic remains on the IBM Cloud Private backbone, providing enhanced security, lower latency, and predictable performance.

### Connecting multiple VPCs
{: #connecting-multiple-vpcs}

Use **{{site.data.keyword.tg_full}}** to enable scalable routing between multiple VPCs. Transit Gateway provides:

* **Cross-region connectivity** - Connect VPCs across different IBM Cloud regions
* **Cross-account connectivity** - Connect VPCs across different IBM Cloud accounts
* **Hub-and-spoke topology** - Centralize connectivity through a single transit gateway
* **Transitive routing** - Enable communication between connected networks

Transit Gateway is ideal for enterprise architectures that require centralized network management and connectivity across multiple VPCs. For more information, see [About {{site.data.keyword.tg_full_notm}}](/docs/transit-gateway?topic=transit-gateway-about).

### Connecting to on-premises networks
{: #connecting-on-premises}

Connect your VPC to on-premises data centers or other cloud environments by using the following options:

#### VPN for VPC
{: #vpn-for-vpc}

VPN for VPC provides encrypted connectivity over the internet between your VPC and remote networks. This solution is ideal for hybrid cloud architectures where you need to connect your VPC to on-premises data centers, enable remote access for users, or establish site-to-site connectivity between different locations.

VPN for VPC uses industry-standard encryption protocols to secure data in transit and supports high throughput, which makes it suitable for bandwidth-intensive workloads. You can configure VPN connections by using either policy-based or route-based approaches, depending on your network requirements and the capabilities of your on-premises VPN devices.

   * Policy-based VPNs use access control lists to determine which traffic is encrypted and sent through the tunnel.
   * Route-based VPNs use routing tables to direct traffic through the VPN connection, offering more flexibility for complex network topologies. For more information, see [About VPN for VPC](/docs/vpc?topic=vpc-using-vpn).

#### Direct Link
{: #direct-link}

{{site.data.keyword.dl_full}} provides dedicated, private physical connectivity between your on-premises infrastructure and IBM Cloud. This solution is designed for high-bandwidth workloads that require consistent performance, latency-sensitive applications and scenarios where regulatory compliance requirements restrict data from traversing the public internet.

Direct Link offers dedicated bandwidth with predictable performance characteristics, which can ensure that your network traffic is not affected by internet congestion or routing changes. You can choose between the following options:

   * Direct Link Dedicated, which provides a single-tenant connection with dedicated bandwidth exclusively for your use.
   * Direct Link Connect, which offers a multi-tenant connection through a network service provider.

Both options provide private connectivity that bypasses the public internet entirely, giving you greater control over your network path and improved security for sensitive data transfers. For more information, see [About {{site.data.keyword.dl_full_notm}}](/docs/dl?topic=dl-dl-about).

### Accessing IBM Cloud services privately
{: #private-service-access}

Access IBM Cloud services from your VPC without exposing traffic to the public internet by using the following options:

#### Virtual Private Endpoints (VPE)
{: #virtual-private-endpoints}

Virtual Private Endpoints allow resources in your VPC to access supported IBM Cloud services by using private IP addresses from your VPC address space. This capability is useful when you need to access services such as {{site.data.keyword.cos_full_notm}} for object storage, {{site.data.keyword.keymanagementserviceshort}} for encryption key management, or managed database services without routing traffic through the public internet. For more information, see [About Virtual Private Endpoints](/docs/vpc?topic=vpc-about-vpe).

#### Private Path
{: #private-path}

{{site.data.keyword.cloud_notm}} Private Path enables private consumer-to-provider connectivity for IBM, partner, or customer-hosted services over the IBM Cloud Private network. This solution is valuable when you need to access third-party services, partner solutions, or custom applications without exposing traffic to the public internet. For more information, see [About {{site.data.keyword.cloud_notm}} Private Path](/docs/vpc?topic=vpc-private-path-service-intro).

## External connectivity
{: #external-connectivity}

External connectivity enables communication between your VPC resources and the public internet. By default, all VPC resources are private and can't be accessed from the internet. You must explicitly enable external connectivity by using network address translation (NAT).

### Network address translation (NAT)
{: #understanding-nat}

Network address translation (NAT) is a method of mapping private IP addresses that are used within your VPC to public IP addresses that can communicate with the internet. NAT is essential for external connectivity because VPC resources use private IP addresses from RFC 1918 address ranges (such as `10.0.0.0/8`, `172.16.0.0/12`, and `192.168.0.0/16`) that are not routable on the public internet. NAT enables these private resources to communicate with internet services by translating their private IP addresses to public IP addresses.

IBM Cloud VPC supports two types of NAT:

Source NAT (SNAT)
:   Translates the source IP address of outbound traffic from a private IP to a public IP. This behavior allows virtual server instances to initiate connections to the internet while hiding their private IP addresses. SNAT is implemented through **public gateways** and uses a _many-to-one_ mapping where multiple instances share a single public IP address.

Destination NAT (DNAT)
:   Translates the destination IP address of inbound traffic from a public IP to a private IP. This behavior allows external clients to initiate connections to instances in your VPC. DNAT is implemented through **floating IP addresses** and uses a _one-to-one_ mapping where each public IP address corresponds to a single private IP address.

### External connectivity options
{: #external-connectivity-options}

IBM Cloud VPC provides the following options for external connectivity:

#### Public gateway
{: #public-gateway}

A public gateway enables all instances in a subnet to access the internet for outbound connections. Instances can't receive inbound connections from the internet.

Use a public gateway in the following cases:

* Instances need to download software updates or packages from the internet
* Applications need to make outbound API calls to external services
* Multiple instances require outbound internet access without exposing them to inbound traffic

For more information, see [About public gateways](/docs/vpc?topic=vpc-about-public-gateways).

#### Floating IP address
{: #floating-ip}

A floating IP address is a public IP address that you can associate with a virtual network interface to enable bidirectional internet connectivity for a single instance.

Use a floating IP address in the following cases:

* To expose a web server, application, or API endpoint to the internet
* To provide SSH or RDP access to an instance from the internet
* To facilitate bidirectional connectivity for a specific instance
* To implement a bastion host or jump server

   When you associate a floating IP with an instance, the instance is exposed to inbound internet traffic. Use security groups and network ACLs to control access and protect your instance.
   {: important}

For more information, see [About floating IP addresses](/docs/vpc?topic=vpc-fip-about).

The following table highlights the difference between a public gateway and floating IP:

| Feature | Public gateway | Floating IP |
| ------- | -------------- | ----------- |
| **Direction** | Outbound only. Instances can initiate connections to the internet but cannot receive inbound connections | Bidirectional. Instances can initiate and receive connections |
| **Scope** | Entire subnet | Single instance |
| **NAT type** | Source NAT (SNAT) - Many-to-1 NAT | Destination NAT (DNAT) - 1-to-1 NAT |
| **Use case** | Software updates, package downloads, outbound API calls | Web servers, application endpoints, SSH access |
| **Security** | Instances are protected from inbound internet traffic | Instances are exposed to inbound internet traffic (use security groups to control access) |
{: caption="Comparison of external connectivity options" caption-side="bottom"}

#### Public address ranges
{: #public-address-ranges}

A public address range is a contiguous set of IBM-provided public IP addresses that you can reserve and bind to a VPC in an availability zone. Unlike floating IP addresses that provide individual public IP addresses, public address ranges give you a block of consecutive public IP addresses that you can use for advanced routing and security scenarios.

Use a public address range in the following cases:

* To route incoming internet traffic through a centralized security appliance or firewall for inspection.
* To avoid managing multiple floating IP addresses individually and instead handle traffic through a single, scalable IP range.
* To allocate contiguous blocks of public IP addresses for scalable routing and security policies.
* To implement advanced network architectures with VNF appliances or third-party security solutions.

   Public address ranges can't be assigned directly to resources. They are used only in ingress custom route tables to direct traffic to next-hop target resources such as firewalls. Review and customize your security group rules, network ACLs, and egress routes to control traffic flow for public address range IP.
   {: important}

For more information, see [About public address ranges](/docs/vpc?topic=vpc-about-par).

### Gateway services comparison
{: #gateway-services-comparison}

The following table summarizes the capabilities of VPC gateway services:

| Capability | Public gateway (SNAT) | Floating IP (DNAT) | Network ACL | VPN for VPC |
| ---------- | ------------------- | ----------------- | ----------- | ----------- |
| **Outbound internet access** | Yes - Entire subnet | Yes - Single instance | N/A | N/A |
| **Inbound internet access** | No | Yes - Limited to a single instance | Yes - Restricted by rules | No |
| **Access control** | Instances protected from inbound traffic | Requires security groups for protection | Stateless filtering by service, protocol, or port | Encrypted site-to-site connectivity |
| **Scalability** | Supports thousands of instances per subnet | One floating IP per instance | Applied to the entire subnet | Supports multiple VPN connections |
| **Use case** | Outbound-only internet access | Public-facing services | Baseline subnet security | Hybrid cloud connectivity |
{: caption="Gateway services capabilities" caption-side="bottom"}

![Figure showing how a VPC can be subdivided with subnets](images/vpc-experience-simple.svg "Figure showing how a VPC can be subdivided with subnets"){: caption="IBM VPC connectivity and security" caption-side="bottom"}

## Load balancing and DNS services
{: #load-balancing-dns}

VPC provides load balancing and DNS services to distribute traffic, improve availability, and enable service discovery across your network architecture.

### Load balancers for VPC
{: #load-balancers-for-vpc}

{{site.data.keyword.cloud_notm}} Load Balancer for VPC distributes traffic across multiple instances to improve application availability and performance.

#### Load balancer types
{: #load-balancer-types}

* **Application Load Balancer** - Layer 7 load balancing with advanced routing capabilities based on HTTP/HTTPS traffic
* **Network Load Balancer** - Layer 4 load balancing for TCP and UDP traffic with high throughput and low latency

#### Load balancer deployment options
{: #load-balancer-deployment}

* **Public load balancers** - Distribute traffic from the internet to instances in your VPC
* **Private load balancers** - Distribute traffic between internal resources or from private networks

Load balancers integrate seamlessly with VPC subnets, VNIs, and security controls to provide a complete traffic distribution solution. For more information, see [About {{site.data.keyword.cloud_notm}} Load Balancer for VPC](/docs/vpc?topic=vpc-nlb-vs-elb).

### DNS Services
{: #dns-services}

{{site.data.keyword.dns_full}} provides authoritative DNS name resolution for your VPC resources and enables service discovery across your network architecture. DNS Services plays a critical role in all three connectivity scopes (internal, private, and external) by providing name resolution for resources and services. For more information, see [About {{site.data.keyword.dns_short}}](/docs/dns-svcs?topic=dns-svcs-about-dns-services).

## Next steps
{: #networking-next-steps}

Now that you understand VPC networking concepts, you can:

* [Create a VPC](/docs/vpc?topic=vpc-getting-started&interface=ui#create-and-configure-vpc)
* [Configure security groups](/docs/vpc?topic=vpc-configuring-the-security-group&interface=ui)
* [Set up network ACLs](/docs/vpc?topic=vpc-acl-create-ui&interface=ui)
* [Enable external connectivity](/docs/vpc?topic=vpc-create-public-gateways&interface=ui)
* [Connect to other networks](/docs/vpc?topic=vpc-interconnectivity)
