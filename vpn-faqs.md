---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-24"

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

No, IKE and IPsec policies can apply to multiple connections.

## What happens to a VPN gateway if I try to delete the subnet that the gateway is on?
{: #faq-vpn-3}
{: faq}
{: support}

The subnet cannot be deleted if any virtual server instances are present, including the VPN gateway.

## Are there default IKE and IPsec policies?
{: #faq-vpn-4}
{: faq}
{: support}

When you create a VPN connection without referencing a policy ID (IKE or IPsec), auto-negotiation is used.

## Why do I need to choose a subnet during VPN gateway provisioning?
{: #faq-vpn-5}
{: faq}
{: support}

The VPN gateway must be deployed in the VPC to provide connectivity. A route-based VPN can be configured to provide connectivity to all zones. A VPN gateway needs four available private IP addresses in the subnet to provide high availability and automatic maintenance. It is best if you use a dedicated subnet for the VPN gateway of size 16, where the length of the subnet prefix is shorter or equal to 28.

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

No. To set up a VPN gateway in your classic environment, you must use an [IPsec VPN](/catalog/infrastructure/ipsec-vpn){: external}.

## What does a rekey collision cause?
{: #faq-vpn-14}
{: faq}

If you use IKEv1, rekey collision deletes the IKE/IPsec security association (SA). To re-create the IKE/IPsec SA, set the connection admin state to `down` and then `up` again. You can use IKEv2 to minimize rekey collisions.

## How can I send all traffic from the VPC side to the on-premises side in a policy-based VPN?
{: #faq-vpn-16}
{: faq}
{: support}

To send all traffic from the VPC side to the on-premises side, set peer CIDRs to `0.0.0.0/0` when you create a connection.

When a connection is created successfully, the VPN service adds a `0.0.0.0/0` via `<VPN gateway private IP>` route into the default routing table of the VPC. However, this new route can cause routing issues, such as virtual servers in different subnets not being able to communicate with each other, and VPN gateways not communicating with on-premises VPN gateways.

   To troubleshoot routing issues, see [Why aren't my VPN gateways or virtual server instances communicating?](/docs/vpc?topic=vpc-troubleshoot-routing-issues).



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

When you use a VPN gateway, you are also charged for all outbound public internet traffic billed at VPC data rates.
{: note}

## When isn't traffic routed through a route-based VPN gateway?
{: #vpn-gateway-route}
{: faq}

If you configured a VPC route with a VPN connection as the next hop, traffic might not be routed as expected due to the following conditions:

* The security groups that are associated with the VPC instance don't permit the traffic, or the network ACLs associated with the instance's subnet or the VPN gateway blocked the traffic. Make sure that your security groups and ACLs allow the intended traffic. For more information, see [Configuring network ACLs for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn).
* If the traffic's source IP isn't in a subnet that is associated with the routing table containing the VPN route, the VPN gateway drops the traffic. For example, suppose that there is a VPC routing table that is associated only with subnet A and includes a route whose next hop is a VPN connection. When the traffic reaches the VPN gateway, the source IP isn't in subnet A or any other subnets that are associated with the routing table. As a result, the VPN gateway doesn't forward the traffic.
