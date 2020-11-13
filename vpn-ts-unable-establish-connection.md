---

copyright:
  years: 2018, 2020
lastupdated: "2020-11-13"

keywords: virtual private network, VPN, vpn gateway, troubleshooting

subcollection: vpc

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:important: .important}
{:screen: .screen}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Why can't I establish a VPN connection?
{: #troubleshoot-unable-to-establish-vpn-connection}
{: troubleshoot}
{: support}

To establish a VPN connection, the right configurations must be in place.
{: shortdesc}

{: tsSymptoms}
Cannot establish a VPN connection.

{: tsCauses}
One or more of your VPN for VPC configurations might be incorrect.

{: tsResolve}
Follow these steps to verify your configurations:

1. Verify that the IKE Phase 1 and Phase 2 configurations match on both sides.

   One thing to watch out for is that VPN for VPC by default has Perfect Forward Secrecy (PFS) disabled in Phase 2. If its peer has PFS enabled, then custom policy with PFS enabled is necessary.
   {: important}
1. Make sure ports UDP 4500 and UDP 500 are open on both sides.
1. Make sure NAT-Traversal is enabled on the peer, if it is a configurable option.  
1. Make sure that the peer device uses its public IP address as the IKE ID. This is not a configurable option.
