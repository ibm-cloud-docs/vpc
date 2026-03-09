---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-09"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my VPN tunnel fail to establish with a Cisco peer?
{: #troubleshoot-cisco-connection-issues}
{: troubleshoot}
{: support}

If your VPN tunnel with Cisco ASAv peer fails to establish or disconnects after periods of inactivity, the issue is often caused by idle timeout settings on the peer device, or configuration of multiple CIDRs.
{: shortdesc}

My VPN connection fails to establish a tunnel when my peer is a Cisco ASAv device.
{: tsSymptoms}

You might observe:

1. With a Cisco peer, the tunnel is up during activity but tears down after ~30 minutes of no traffic.
1. The first API call after idle time fails, while the next call succeeds.
1. When multiple VPC CIDRs or subnets are configured on Cisco ASA in one policy-based VPN entry, you might experience unstable connections.

An unstable VPN in this scenario is commonly caused by Cisco idle‑timeout policies, or by combining many subnets or CIDRs in a single connection, which can stress policy-based selectors.
{: tsCauses}

Follow these steps to resolve peer‑side timing and multi‑subnet issues on Cisco devices:
{: tsResolve}

1. Increase the idle timeout on the Cisco peer device from 30 minutes to several hours. If your policy allows, disable idle timeout, which avoids first‑request failures after quiet periods.
1. If Cisco idle timeout can't be changed, send periodic keepalives every 20–25 minutes so that the Cisco device sees activity and keeps the security association alive.
1. Add a simple retry after idle periods, which can often resolve transient failures.
1. If you use multiple VPC CIDRs and subnets, avoid putting them all in a single policy-based definition. Move different subnets into separate IBM Cloud VPN connections to reduce selector overlap and improve stability.
