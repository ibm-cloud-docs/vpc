---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-27"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't traffic get through my active connection?
{: #troubleshoot-no-traffic-active-connection}
{: troubleshoot}
{: support}

For application traffic to flow through a connection, the right configurations must be in place, including Access Control Lists (ACLs), which define what traffic is allowed or denied.
{: shortdesc}

Your connection is active, but traffic is not getting through.
{: tsSymptoms}

An interoperability issue might exist.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Make sure that NAT-Traversal is enabled on the peer, if it's a configurable option. For more information, see [Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations).
1. Make sure that ACLs are configured correctly on both sides to allow application traffic. For example, ensure that traffic from your local subnet (for example, `10.0.0.0/16`) to the remote subnet (for example, `192.168.0.0/16`) is explicitly permitted using the required protocol and port ranges. Also, verify that no implicit deny rules exist that can block this traffic. For more information, see [About network ACLs](/docs/vpc?topic=vpc-using-acls) and [Configuring network ACLs for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn&interface=ui).
1. If you use a policy-based VPN, follow these steps:
   * When multiple CIDR subnets are present on either side, create multiple connections with one CIDR subnet pair per connection. Alternatively, check if your can configure your peer gateway to allow multiple CIDRs.
   * If you use a custom routing table and the peer subnet is different from the VPN gateway subnet, make sure to allow VPN gateway resources to create routes in the routing table. For more information, see [Creating a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table&interface=ui#cr-using-the-ui)
1. If you use a route-based VPN on either side, follow these steps:
   * Configure the routes on each side so that traffic is routed to the VPN gateway correctly and that they don't point to a private IP address. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
   * If the 2 tunnels are up for your VPN for VPC connection, make sure that the `distribute_traffic` connection property to set to `false` if the on-premises side doesn't support asymmetrical route. Furthermore, you must always establish a connection to the smaller IP address. For more information, see [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn&interface=ui#use-case-4-vpn).
