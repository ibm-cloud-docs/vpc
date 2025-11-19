---

copyright:
  years: 2020, 2025
lastupdated: "2025-11-19"

keywords: virtual private network, faq, faqs, frequently asked questions, vpn, vpn gateway

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for site-to-site VPN gateways
{: #faqs-vpn}

You might encounter these frequently asked questions when you use {{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_short}}.
{: #vpn-faq}

## When I create a VPN gateway, can I create VPN connections at the same time?
{: #faq-vpn-0}
{: faq}
{: support}

In the {{site.data.keyword.cloud_notm}} console, you can create the gateway and a connection at the same time. If you use the API or CLI, VPN connections must be created after the VPN gateway is created.

## If I delete a VPN gateway with attached VPN connections, what happens to the connections?
{: #faq-vpn-1}
{: faq}
{: support}

The VPN connections are deleted along with the VPN gateway.

## Are IKE or IPsec policies deleted if I delete a VPN gateway or VPN connection?
{: #faq-vpn-2}
{: faq}
{: support}

No, IKE and IPsec policies aren't deleted because they can apply to multiple connections.

## What happens to a VPN gateway if I try to delete the subnet that the gateway is on?
{: #faq-vpn-3}
{: faq}
{: support}

The subnet can't be deleted if any virtual server instances are present, including the VPN gateway.

## Are there default IKE and IPsec policies?
{: #faq-vpn-4}
{: faq}
{: support}

Yes. When you create a VPN connection without referencing a policy ID (IKE or IPsec), auto-negotiation is used.

## Why do I need to choose a subnet during VPN gateway provisioning?
{: #faq-vpn-5}
{: faq}
{: support}

You must choose a subnet when you provision a VPN gateway in IBM Cloud, because the gateway is deployed within a VPC subnet to establish connectivity. A route-based VPN can support connectivity across all zones, but the gateway itself requires four available private IP addresses in the chosen subnet to provide high availability and automatic maintenance. It is best if you use a size 16 dedicated subnet for the VPN gateway, where the length of the subnet prefix is shorter or equal to 28.

## What should I do if I am using ACLs on the subnet that is used to deploy the VPN gateway?
{: #faq-vpn-6}
{: faq}
{: support}

Make sure that ACL rules are in place to allow management traffic and VPN tunnel traffic. For more information, see [Configuring network ACLs for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn).

## What should I do if I am using ACLs on the subnets that must communicate with an on-premises private network?
{: #faq-vpn-7}
{: faq}
{: support}

Make sure that ACL rules are in place to allow traffic between virtual server instances in your VPC and your on-premises private network. For more information, see [Configuring ACLs for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn).

## Does {{site.data.keyword.vpn_vpc_short}} support high-availability configurations?
{: #faq-vpn-8}
{: faq}

Yes, {{site.data.keyword.vpn_vpc_short}} supports high availability in an Active-Standby configuration for policy-based VPNs, and Active-Active configuration for a static, route-based VPN.

## Are there plans to support SSL VPN?
{: #faq-vpn-9}
{: faq}

No, only IPsec site-to-site is supported.

## Are there any caps on throughput for site-to-site VPNaaS?
{: #faq-vpn-10}
{: faq}

Up to 650 Mbps of throughput is supported.

## Is Pre-Shared Key (PSK) and certificate-based IKE authentication supported for VPNaaS?
{: #faq-vpn-11}
{: faq}

Only PSK authentication is supported.

## Can you use {{site.data.keyword.vpn_vpc_short}} as a VPN gateway for your {{site.data.keyword.cloud_notm}} classic infrastructure?
{: #faq-vpn-12}
{: faq}

Yes. The recommended method to connect your classic network to a VPC is to use an [IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started). See [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure&interface=ui).

## What is a rekey collision between site-to-site VPNs?
{: #faq-vpn-14}
{: faq}

A rekey collision occurs when both VPN peers attempt to initiate a rekey at the same time, which can lead to conflicting negotiations, tunnel instability or dropped connections. This issue is commonly observed in IKEv1, because both sides must use matching key lifetimes, and the protocol lacks collision-handling mechanisms, which makes it unreliable. However, IKEv2 supports asymmetric key lifetimes to gracefully handle simultaneous rekey attempts. If you use IKEv1, rekey collision deletes the IKE/IPsec security association (SA). To re-create the IKE/IPsec SA, set the connection admin state to `down` and then `up` again. To minimize rekey collisions and maintain stable performance, use IKEv2.

## How can I send all traffic from the VPC side to the on-premises side in a policy-based VPN?
{: #faq-vpn-16}
{: faq}
{: support}

To send all traffic from the VPC side to the on-premises side, set peer CIDRs to `0.0.0.0/0` when you create a connection.

When a connection is created successfully, the VPN service adds a CIDR `0.0.0.0/0` through the `<VPN gateway private IP>` route into the default routing table of the VPC. However, this new route can cause routing issues, such as virtual servers in different subnets not being able to communicate with each other, and VPN gateways not communicating with on-premises VPN gateways.

   To troubleshoot routing issues, see [Why aren't my VPN gateways or virtual server instances communicating?](/docs/vpc?topic=vpc-troubleshoot-routing-issues).

## Can I connect my VPN to multiple Transit Gateways in a dynamic route-based VPN connection?
{: #faq-vpn-17}
{: faq}

No, each VPN gateway can be connected to only one Transit Gateway.

## Is dynamic routing supported when creating a route-based VPN gateway?
{: #faq-vpn-18}
{: faq}

Yes, dynamic VPN connection is supported by a route-based VPN gateway. See [Creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway&interface=ui) to create route-based VPN and select **dynamic** for the connection type.

## What is ASN and why do I need it for dynamic routing?
{: #faq-vpn-19}
{: faq}

An Autonomous System Number (ASN) is a unique identifier that is used in Border Gateway Protocol (BGP) to represent an Autonomous System (AS). It functions similarly to a public IP address in an IPsec connection and serves as a key attribute for identifying devices within a network. Each device within the network is assigned to a specific ASN, and without a valid ASN, a VPN gateway cannot successfully establish a BGP session with other devices in the network.

## Do I need to attach a Transit Gateway for the dynamic routing connection to work?
{: #faq-vpn-20}
{: faq}

Yes, you need to attach a Transit Gateway for the dynamic routing connection to function properly. Without a Transit Gateway, communication between the VPN gateway and your on-premises network doesn't work, even if the IPsec connection is established.

## Can I attach an existing Transit Gateway to the route-based VPN for dynamic routing?
{: #faq-vpn-21}
{: faq}

Yes, you can attach an existing Transit Gateway to the route-based VPN for dynamic routing. You don't need to create a new Transit Gateway.

## Why do I need a Transit Gateway for dynamic route-based connection?
{: #faq-vpn-22}
{: faq}

A Transit Gateway is essential for dynamic routing because it acts as a central hub for all connections within your network. The Transit Gateway manages the routing for all spokes, including VPN connections. Without the Transit Gateway the spokes would be unable to communicate with each other, as they rely on the hub Transit Gateway to facilitate and route traffic between them.

## What is the use of advertised CIDRs?
{: #faq-vpn-23}
{: faq}

Advertised CIDRs are static IP address ranges that are reachable from the VPN and advertised to your on-premises network. These CIDRs are useful for private endpoints that can't be connected directly to a Transit Gateway, such Secrets Manager endpoints or Cloud Object Storage endpoints. By advertising these CIDRs from the IBM VPN to your on-premises network, any resource within your on-premises environment can connect to these endpoints.

## Do I need to configure routes when I create a dynamic route-based VPN connection?
{: #faq-vpn-24}
{: faq}

No, you don't need to manually configure routes when you create a dynamic route-based VPN connection. All spokes connected to the Transit Gateway automatically connect to your on-premises network.

## What is the difference between static and dynamic route-based VPN connection types?
{: #faq-vpn-25}
{: faq}

Static routing connection doesn't use BGP for route advertisements, and can't advertise routes to Transit Gateway or on-premises network. For this connection, all routes need to be manually created and managed, whereas for dynamic connection, no manual configuration is required after the initial provisioning and attachment.

## How many routes does IBM VPN support per VPN peer for dynamic route-based connection?
{: #faq-vpn-26}
{: faq}

Each IBM VPN supports a maximum of 120 routes per VPN peer for dynamic connection. If this limit exceeds, the BGP session automatically shuts down for that peer. To restore the BGP connection, your on-premises peer network must reduce the number of advertised routes to less than or equal to 120. Then, you must toggle the connection in the IBM Cloud to reinitiate the session.

## How many routes in total does the IBM VPN appliance support for dynamic route-based connection?
{: #faq-vpn-27}
{: faq}

Each IBM VPN appliance supports up to 120 routes in total, regardless of how many peers are connected. If the appliance receives more than 120 routes across a combination of peers, it propagates only the first 120 routes to the transit gateway. For example, if the VPN appliance receives 70 routes each from two peers, only the first 120 routes are propagated to the transit gateway.

## Can I use more than 120 routes for a VPN in a dynamic route-based connection?
{: #faq-vpn-28}
{: faq}

A single VPN gateway supports up to 120 routes. If you require more than 120 routes per VPN gateway, you can achieve this function by connecting the VPN appliance to different on-premises devices. However, keep in mind that this configuration does not guarantee high availability or disaster recovery for the VPN appliance when it is connected to different on-premises devices.

## Does IBM complete quarterly ASV scans of data-plane VPN appliances?
{: #vpn-asv}
{: faq}

Approved Scanning Vendor (ASV) quarterly scanning is a requirement of the Payment Card Industry (PCI) Security Standards Council. ASV scanning of VPN data-plane appliances is solely a customer responsibility. IBM does not use ASVs to scan data-plane appliances because these scans can negatively impact customer workload functions and performance.

## What metrics am I charged for if I am using VPN gateway for VPC?
{: #vpn-billing}
{: faq}

The following metrics are collected for VPN gateway billing on a monthly basis:

* VPN Gateway Instance Hour: How much time your VPN gateway instance is up and running.
* VPN Connection Hour: How much time each of your VPN connections is established and maintained on the VPN gateway.
* Floating IP: The number of active floating IP addresses used by the VPN gateway instance.

When you use a VPN gateway, you are also charged for all outbound public internet traffic that is billed at VPC data rates.
{: note}

## When isn't traffic routed through a route-based VPN gateway?
{: #vpn-gateway-route}
{: faq}

If you configured a VPC route with a VPN connection as the next hop, traffic might not be routed as expected due to the following conditions:

* The security groups that are associated with the VPC instance don't permit the traffic. In addition, the network ACLs associated with the instance's subnet or the VPN gateway block the traffic. Make sure that your security groups and ACLs allow the intended traffic. For more information, see [Configuring network ACLs for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn).
* If the source IP of the traffic doesn't belong to a subnet that is associated with the routing table containing the VPN route, the VPN gateway drops the traffic. For example, consider a VPC routing table that is associated only with **Subnet A**, and includes a route whose next hop is a VPN connection. If traffic reaches the VPN gateway but originates from an IP address outside of **Subnet A** or any other subnet that is linked to that routing table, the gateway doesn't forward the traffic.
