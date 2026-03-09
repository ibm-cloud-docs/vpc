---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-09"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my VPN tunnel fail to establish with a Check Point Security Gateway peer?
{: #troubleshoot-check-point-connection-issues}
{: troubleshoot}
{: support}

If your VPN tunnel fails to come up consistently, or establishes only when the Check Point peer initiates traffic, the issue is commonly related to Check Point tunnel settings or its policy‑based VPN configuration.
{: shortdesc}

My VPN tunnel doesn't establish, or it establishes only when the Check Point side sends traffic.
{: tsSymptoms}

You might observe:

1. The tunnel stays down when IBM Cloud initiates traffic but comes up immediately when the Check Point peer sends traffic.
1. IBM‑initiated pings, SSH, or cURL don't bring up the tunnel.
1. Only one subnet works when multiple CIDRs are configured in a single policy‑based VPN connection.
1. Phase 2 negotiation or traffic selector mismatch errors appear in logs.

Tunnel bring‑up issues with Check Point are commonly caused when the _Permanent Tunnel_ settings are disabled. When the tunnel fails to establish, it might also be due to multiple CIDRs configured for a single policy‑based VPN connection.
{: tsCauses}

Follow these steps to resolve the VPN connection issue with your Check Point peer:
{: tsResolve}

1. Enable Permanent Tunnels (On‑Demand VPN tunnels) on the Check Point peer. This option allows IBM‑initiated traffic to bring up the tunnel after idle or tunnel resets.
1. If you use a policy‑based VPN, configure only one CIDR per VPN connection. Check Point devices often negotiate only a single traffic selector per policy‑based tunnel and any additional CIDRs fail Phase 2 negotiation.
1. Make sure that the CIDRs match exactly on both sides. Avoid mismatches like `/16` on one end and `/32` on the other.
1. Keep NAT‑Traversal enabled on the Check Point peer (if configurable).
1. Re-enable the connection after you make the changes.
