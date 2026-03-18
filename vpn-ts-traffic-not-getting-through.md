---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-18"

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

1. Make sure that NAT-Traversal is enabled on the peer, if it's a configurable option. For more information, see [Known issues for VPN gateways](/docs/vpc/build/vpc-review-output?topic=vpc-vpn-limitations).
1. Make sure that ACLs are configured properly on both sides to allow application traffic.
1. If you're using a policy-based VPN with multiple CIDR subnets on either side, create multiple connections with one CIDR subnet pair per connection. Alternatively, check if your can configure your peer gateway to allow multiple CIDRs. 
1. If you're using a route-based VPN on either side, configure the routes on each side properly so that traffic is routed to the VPN gateway correctly. 
1. Your peer device might end the session too early due to its own timeout behavior. In this case, increase the idle‑timeout on the peer or configure it to send periodic keepalive traffic so that the tunnel stays active without requiring long timers.
1. If you're using a route-based VPN and the 2 tunnels are up for your VPN for VPC connection, make sure that the `distribute_traffic` connection property to set to `false` if the on-premises side doesn't support asymmetrical route. For more information, see the [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn&interface=ui#use-case-4-vpn) use case.
