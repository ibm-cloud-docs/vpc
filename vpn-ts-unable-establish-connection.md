---

copyright:
  years: 2018, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I establish a VPN connection?
{: #troubleshoot-unable-to-establish-vpn-connection}
{: troubleshoot}
{: support}

To establish a VPN connection, the right configurations must be in place.
{: shortdesc}

Cannot establish a VPN connection.
{: tsSymptoms}

One or more of your {{site.data.keyword.vpn_vpc_short}} configurations might be incorrect.
{: tsCauses}

Follow these steps to verify your configurations:
{: tsResolve}

1. Verify that the IKE Phase 1 and Phase 2 configurations match on both sides.

   One thing to watch out for is that {{site.data.keyword.vpn_vpc_short}} by default has Perfect Forward Secrecy (PFS) disabled in Phase 2. If PFS is enabled on its peer, then a custom policy with PFS enabled is necessary.
   {: important}

1. Make sure that ports UDP `4500` and UDP `500` are open on both sides.
1. Make sure that NAT-Traversal is enabled on the peer, if it is a configurable option.
1. Make sure that the peer device uses its public IP address as the IKE ID. This option is not a configurable option.
