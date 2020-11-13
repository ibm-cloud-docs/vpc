---

copyright:
  years: 2018, 2020
lastupdated: "2020-11-13"

keywords: virtual private network, VPN, vpn gateway, troubleshooting

subcollection: vpc

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Why can't traffic get through my active connection?
{: #troubleshoot-no-traffic-active-connection}
{: troubleshoot}
{: support}

For application traffic to flow through a connection, the right configurations must be in place, including ACLs configured on both sides.
{: shortdesc}

{: tsSymptoms}
Your connection is active, but traffic is not getting through.

{: tsCauses}
There might be an interoperability issue.

{: tsResolve}
Follow these steps to resolve this issue:

1. Make sure NAT-Traversal is enabled on the peer, if it is a configurable option.
1. Make sure ACLs are configured properly on both sides to allow application traffic.
1. If using a policy-based mode VPN with a static, route-based VPN peer and multiple CIDR/subnets on either side, make sure to create multiple connections with one CIDR/subnet pair per connection.
1. If using a Cisco Adaptive Security Appliance (ASA) as the peer of VPN for VPC with multiple CIDRs/subnets configured on the Cisco side, try moving different subnets to separate connections.
1. If using a route-based VPN on either side, make sure that routes on each side are configured properly so that traffic is routed to the VPN gateway appropriately. 
