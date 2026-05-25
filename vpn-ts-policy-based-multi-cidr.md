---

copyright:
  years: 2018, 2026
lastupdated: "2026-05-25"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my site-to-site VPN fail to establish when I use policy-based VPN with multiple tunnels?
{: #troubleshoot-policy-based-multi-cidr}
{: troubleshoot}
{: support}

This issue occurs when multiple policy‑based VPN connections are configured with the same local and remote CIDR ranges (traffic selectors). In this scenario, the VPN gateway can't reliably determine which tunnel must carry the traffic.
{: shortdesc}

A site‑to‑site VPN connection between two networks is established successfully, and traffic initially flows through the VPN tunnel. However, issues can occur when multiple policy-based VPN connections use the same traffic selectors. Typically, only the first matching connection becomes active, while additional connections might remain inactive.
{: tsSymptoms}

This issue occurs when a policy‑based VPN gateway is configured with multiple active VPN connections that use identical traffic selectors. Specifically, more than one VPN connection is configured with the same local CIDR and the same remote CIDR. Policy‑based VPN relies on fixed traffic‑matching rules to decide which tunnel must carry a packet. When multiple tunnels advertise the same local and remote CIDRs, the VPN gateway cannot consistently select the correct tunnel.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Identify the VPN gateway that is experiencing timeouts.
1. Review all VPN connections that are attached to the gateway.
1. Check the `local_cidrs` and `remote_cidrs` configured on each VPN connection.
1. If multiple VPN connections use the same local and remote CIDR, delete the additional connections so that only one active tunnel exists for each CIDR pair.
1. Verify that traffic is stable after it flows through the single active policy‑based VPN connection.
1. If high availability or redundancy is required, consider redesigning the setup to use route‑based VPN instead of policy‑based VPN because route‑based VPN supports multiple tunnels without CIDR conflicts.

## Related links
{: #related-links-policy-based-multi-cidr}

[Why can't I reach network destinations when multiple CIDRs are configured for my VPN connection?](/docs/vpc?topic=vpc-troubleshoot-multi-cidr-oneway-traffic&interface=cli)
