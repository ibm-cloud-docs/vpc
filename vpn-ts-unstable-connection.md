---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-16"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my VPN connection unstable?
{: #troubleshoot-unstable-connection}
{: troubleshoot}
{: support}

If your VPN connection is intermittent, it is often related to IPsec rekey behavior, encryption mismatches, or Dead Peer Detection (DPD) events between the IBM Cloud VPN gateway and the peer device.
{: shortdesc}

My VPN connection is unstable and keeps going down.
{: tsSymptoms}

You might observe one or more of the following symptoms:

* The VPN tunnel establishes successfully but disconnects after a fixed interval.
* The VPN tunnel repeatedly disconnects and reconnects.
* Traffic stops even though the VPN connection still appears to be up.
* DPD failure messages on the peer device.
* Intermittent packet loss during tunnel rekey events.
* Logs showing repeated IKE or IPsec negotiation attempts from both sides.

An unstable VPN connection is commonly caused by IPsec rekey failures between the IBM Cloud VPN gateway and the peer device.
{: tsCauses}

Follow these steps to resolve VPN instability that is caused by rekey or negotiation issues:
{: tsResolve}

1. Verify that NAT-Traversal is enabled on the peer, if it is a configurable option. Allow UDP ports `500` and `4500` on firewalls and ACLs.
1. To avoid simultaneous rekeying, configure rekey timers so that only one side consistently initiates the rekey.

   If you're using IKEv2, configure your peer device's Phase 1 (IKE) lifetime shorter than IBM Cloud lifetime, which ensures that rekeying is always initiated by the peer and not both sides.
   {: tip}

1. Verify that the IKE and IPsec encryption settings match exactly on the IBM Cloud VPN gateway and the peer.
1. Review VPN logs on both sides and compare the configured and received proposals during rekey.
1. Reset the VPN and monitor logs to confirm successful rekeying without errors.
