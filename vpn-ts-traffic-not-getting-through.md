---

copyright:
  years: 2018, 2023
lastupdated: "2023-01-31"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't traffic get through my active connection?
{: #troubleshoot-no-traffic-active-connection}
{: troubleshoot}
{: support}

For application traffic to flow through a connection, the right configurations must be in place, including ACLs configured on both sides.
{: shortdesc}

Your connection is active, but traffic is not getting through.
{: tsSymptoms}

An interoperability issue might exist.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Make sure that NAT-Traversal is enabled on the peer, if it's a configurable option.
1. Make sure that ACLs are configured properly on both sides to allow application traffic.
1. If you're using a policy-based mode VPN with a static, route-based VPN peer and multiple CIDR subnets on either side, create multiple connections with one CIDR subnet pair per connection.
1. If you're using a Cisco Adaptive Security Appliance (ASA) as the peer of {{site.data.keyword.vpn_vpc_short}} with multiple CIDRs and subnets that are configured on the Cisco side, try moving different subnets to separate connections.
1. If you're using a route-based VPN on either side, configure the routes on each side properly so that traffic is routed to the VPN gateway correctly. 
