---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my IPsec connection status appear to be up but the BGP state isn't established?
{: #troubleshoot-ipsec-up-bgp-not-established}
{: troubleshoot}
{: support}

An IPsec VPN connection in {{site.data.keyword.cloud_notm}} can appear to be "up" while the BGP session isn't established. This issue indicates that a secure tunnel between the public IP addresses of the VPN devices is established, but the BGP session is not yet successfully established.
{: shortdesc}

The IPsec connection status is "up", whereas the BGP session isn't established and no routes are exchanged between IBM VPN Gateway and the peer device.
{: tsSymptoms}

BGP relies on correct tunnel IP setup and mutual compatibility between devices. This issue occurs due to unsupported routing protocols on your peer device, misconfiguration of tunnel IP addresses, or tunnel IP conflicts.
{: tsCauses}

Follow these steps to troubleshoot and resolve the BGP session issue:
{: tsResolve}

1. **Verify peer device routing support**: Make sure that your peer device supports dynamic routing with BGP. If it does not, you must configure a [static routing connection](/docs/vpc?topic=vpc-vpn-adding-connections&interface=ui) instead.
1. **Check tunnel IP configuration**: Confirm that the tunnel IP addresses are correctly configured on both the IBM VPN Gateway and your peer device. The `tunnel_interface_ip` on IBM VPN must match the remote tunnel IP on your peer device. The peer’s tunnel IP must be configured as the remote IP on IBM VPN.
1. **Check tunnel IP addresses for VPN gateway members**: Each connection on IBM VPN is configured on both gateway members. Make sure that the correct tunnel IP addresses are assigned to each member and that the peer device is configured to connect to the intended member.
1. **Avoid tunnel IP conflicts**: Make sure that the tunnel IP addresses do not overlap with VPC address ranges, routes that are received from other VPN connections, or transit gateway routes. Use IP addresses from the `169.254.0.0/16`range for tunnel interfaces.
