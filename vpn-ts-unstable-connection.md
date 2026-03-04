---

copyright:
  years: 2018, 2026
lastupdated: "2026-03-04"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my VPN connection unstable?
{: #troubleshoot-unstable-connection}
{: troubleshoot}
{: support}

If you notice that your VPN connection is intermittent and goes up and down occasionally, the issue is often related to IPsec rekey timing or Dead Peer Detection (DPD) failures.
{: shortdesc}

My VPN connection is unstable and keeps going down.
{: tsSymptoms}

You might observe:

1. The VPN tunnel repeatedly disconnects and reconnects.
1. Dead Peer Detection (DPD) failure messages on your device.
1. Intermittent packet loss during tunnel rekey events.
1. Logs showing simultaneous IKE or IPsec negotiation attempts from both sides.

An unstable VPN connection is commonly caused by rekey timing conflicts between the IBM Cloud VPN gateway and the peer device.
{: tsCauses}

Follow the steps to resolve any timing issues:
{: tsResolve}

1. Make sure that NAT-Traversal is enabled on the peer, if it is a configurable option.
1. To avoid simultaneous rekeying, make sure that only one side consistently initiates the rekey.
   If you're using IKEv2, configure your peer device's Phase 1 (IKE) lifetime shorter than IBM Cloud lifetime, which ensures that rekeying is always initiated by the peer and not both sides.
   {: tip}

1. Try disabling and reenabling the connection.
