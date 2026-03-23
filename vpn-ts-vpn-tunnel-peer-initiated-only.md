---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-23"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my VPN tunnel work only when the peer initiates traffic?
{: #troubleshoot-vpn-tunnel-peer-initiated-only}
{: troubleshoot}
{: support}

If your VPN tunnel fails to come up, or establishes only when the peer initiates traffic, the issue is related to tunnel settings or a policy‑based VPN configuration.
{: shortdesc}

If your VPN tunnel doesn't establish when IBM Cloud initiates traffic, or establishes only when the peer gateway sends traffic, the issue is typically related to on-demand tunnel behavior or Phase 2 negotiation constraints on the peer gateway.
{: shortdesc}

Your VPN tunnel doesn't establish, or it establishes only when the peer side sends traffic.
{: tsSymptoms}

You might observe:

* The tunnel drops after being idle and does not re-establish until the peer initiates traffic.
* The tunnel remains down when IBM Cloud initiates traffic, but comes up immediately when the peer sends traffic.
* IBM‑initiated pings, SSH, or cURL fail to establish the tunnel.
* Only one subnet works when multiple CIDRs are configured in a single policy‑based VPN connection.
* Phase 2 negotiation or traffic selector mismatch errors appear in logs.

This behavior is commonly seen with peer gateways that use on-demand VPN tunnels. When the tunnel fails to establish, it might also be due to multiple CIDRs configured for a single policy‑based VPN connection.
{: tsCauses}

Follow these steps to resolve the VPN connection issue with your peer:
{: tsResolve}

1. Ensure that the tunnel supports bidirectional traffic initiation. This action allows IBM‑initiated traffic to establish the tunnel after idle or tunnel resets.
1. If you use a policy‑based VPN, configure only one CIDR per VPN connection. Certain peer devices often negotiate only a single traffic selector per policy‑based tunnel, and any additional CIDRs might fail Phase 2 negotiation. For more information, see [Why can't I reach network destinations when multiple CIDRs are configured for my VPN connection?](/docs/vpc?topic=vpc-troubleshoot-multi-cidr-oneway-traffic)
1. Make sure that the CIDRs match exactly on both sides. Avoid mismatches like `/16` on one end and `/32` on the other.
1. Keep NAT‑Traversal (NAT-T) enabled on the peer if configurable. This setting allows IPsec traffic to pass through devices that perform network address translation by encapsulating IPsec packets over UDP. If NAT-T is disabled and a NAT device exists between peers, tunnel establishment or bidirectional traffic initiation can fail.
1. Re-enable the connection after you make the changes.
