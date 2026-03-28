---

copyright:
  years: 2024, 2026
lastupdated: "2026-03-28"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my existing VPN connection go down when I create a new VPN connection?
{: #troubleshoot-duplicate-ike-sa}
{: troubleshoot}
{: support}

Existing VPN connections on a policy‑based VPN gateway can go down when newly added connections use the same peer IP address with overlapping traffic selectors (CIDRs), which can conflict existing connections. This condition causes the VPN engine to detect a duplicate IKE Security Associations (IKE SA) and delete the existing tunnel while negotiating the new one.
{: shortdesc}

When you create a new VPN connection, existing tunnels might unexpectedly go down, and new connections might fail to establish. You might often see the following messages in logs:
{: tsSymptoms}

   ```sh
   Got a response on a duplicate IKE_SA ... deleting new IKE_SA
   ```

   ```sh
   Received TEMPORARY_FAILURE notify, will retry later
   ```

* This issue occurs when multiple policy‑based connections share a common peer gateway IP and have identical or overlapping `local cidrs` or `peer cidrs`.
* The VPN engine treats the new connection as a duplicate IKE SA and deletes the active one.
* The `TEMPORARY_FAILURE` message indicates that the peer device was unable to create a new security association at that moment, which delays tunnel establishment.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Verify that no two connections have overlapping `local cidrs` or `peer cidrs`.
1. If multiple tunnels use the same peer IP address, make sure that each tunnel has a unique policy on the peer firewall.
1. If multiple connections exist to the same peer from your VPN gateway, make sure to use the same dead peer value for all connections.
1. Increase dead peer detection values to reduce aggressive renegotiation. For more information, see [Why is my VPN connection unstable?](/docs/vpc?topic=vpc-troubleshoot-unstable-connection)
1. Ask the peer administrator to clear stale IPsec security associations if a re-created tunnel does not come up.
1. If a single VPN gateway hosts many connections, create an additional VPN gateway and migrate some connections to improve stability.
