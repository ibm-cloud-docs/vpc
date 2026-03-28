---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-28"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my VPN tunnel occasionally fail?
{: #troubleshoot-vpn-tunnel-fails}
{: troubleshoot}
{: support}

If your VPN tunnel fails to establish after periods of inactivity, the issue is often caused by idle timeout settings on the peer device, or multiple CIDRs.
{: shortdesc}

My VPN connection fails to establish a tunnel with my peer device.
{: tsSymptoms}

You might observe:

* The tunnel is up during activity but tears down after approximately 30 minutes of no traffic.
* The first API call after idle time fails, while the next call succeeds.
* When multiple VPC CIDRs or subnets are configured on the peer in a policy-based VPN setup, you might experience unstable connections.

An unstable VPN in this scenario is commonly caused by the peer's idle‑timeout policies, or when multiple subnets or CIDRs are configured in a single policy-based connection.
{: tsCauses}

Follow these steps to resolve peer‑side timing and multi‑subnet issues:
{: tsResolve}

1. Increase the idle timeout on your peer device from 30 minutes to several hours. If your policy allows, disable the idle timeout, which avoids first‑request failures after quiet periods.
1. If the peer idle timeout can't be changed, send periodic keepalives every 20–25 minutes so that the peer device sees activity and keeps the security association alive. You can configure keepalives on the peer device by using features such as IKE keepalive, Dead Peer Detection (DPD), or by generating periodic ICMP pings from either side of the tunnel. Consult your peer device documentation for settings that are related to keepalive intervals.
1. Add a simple retry with a script on the client or the application to automatically retry the first failed request after idle periods.
1. If the VPN is configured in policy‑based mode, split large or multiple remote CIDR blocks into separate VPN connections so that each CIDR pair negotiates its own security association. An unstable connection in this case is by negotiation issues when multiple subnets or CIDRs are configured. For more information, see [Why can't I reach network destinations when multiple CIDRs are configured for my VPN connection?](/docs/vpc?topic=vpc-troubleshoot-multi-cidr-oneway-traffic)
