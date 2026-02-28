---

copyright:
  years: 2021, 2026
lastupdated: "2026-02-28"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can’t I access a VPE when using a client-to-site VPN in split‑tunnel mode?
{: #troubleshoot-cannot-connect-to-vpe-split-tunnel}
{: troubleshoot}

In split-tunnel mode, your VPN might not send the right traffic through the tunnel. If DNS or routing isn’t configured correctly, your VPN client can't reach the private virtual private endpoint (VPE).
{: shortdesc}

You can’t connect to a private resource through a VPE when using client-to-site VPN in split-tunnel mode. The same setup works in full-tunnel mode.
{: tsSymptoms}

This issue usually occurs because one or both of the following settings are missing:
{: tsCauses}

* DNS is blocked by the security group: Your VPN client must contact IBM Cloud DNS to look up the private VPE address. If port `53` (DNS) isn’t allowed in the security group, the request is blocked, and the VPE IP address is never returned.
* Missing VPN server route: In split-tunnel mode, the VPN server specifies routes for the traffic that should go through the tunnel. If no VPN route exists for the VPC subnet where the VPE is located, your client sends the traffic to the public internet, where private IP addresses aren’t accessible.

Follow these steps to fix this issue:
{: tsResolve}

1. Allow DNS in the security group

   | Protocol | Port | Purpose              |
   | -------- | ---- | -------------------- |
   | UDP/TCP  | `53` | Allow DNS resolution |
   {: caption="DNS rule for security group" caption-side="bottom"}

1. Add a VPN server route for the VPE

   In the VPN server routes section, add one of the following IP addresses:
   * VPC subnet CIDR: `10.x.y.0/24` (recommended)
   * VPE private IP as a /32 route: `10.x.y.z/32`

1. Reconnect the VPN: Route changes are applied only after you disconnect and reconnect the VPN.
