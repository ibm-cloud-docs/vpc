---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-25"

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
1. Make sure that ACLs are configured correctly on both sides to allow application traffic. For example, ensure that traffic from your local subnet (for example, `10.0.0.0/16`) to the remote subnet (for example, `192.168.0.0/16`) is explicitly permitted using the required protocol and port ranges. Also, verify that there are no implicit deny rules blocking this traffic. For more information, see [About network ACLs](/docs/vpc?topic=vpc-using-acls) and [Configuring network ACLs for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn&interface=ui).
1. If you're using a policy-based VPN with multiple CIDR subnets on either side, create multiple connections with one CIDR subnet pair per connection. Alternatively, check if your can configure your peer gateway to allow multiple CIDRs.
1. If you're using a route-based VPN on either side, configure the routes on each side so that traffic is routed to the VPN gateway correctly. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
1. Your peer device might end the session too early due to its own timeout behavior. In this case, increase the idle‑timeout on the peer or configure it to send periodic keepalive traffic so that the tunnel stays active without requiring long timers. You can validate this by checking logs on the peer device for entries that indicate expiration or inactivity timeouts. For more information, see [Why does my VPN tunnel occasionally fail?](/docs/vpc?topic=vpc-troubleshoot-vpn-tunnel-fails).
1. If you're using a route-based VPN and the 2 tunnels are up for your VPN for VPC connection, make sure that the `distribute_traffic` connection property to set to `false` if the on-premises side doesn't support asymmetrical route. For more information, see the [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn&interface=ui#use-case-4-vpn) use case.
