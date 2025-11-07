---

copyright:
  years: 2023, 2025
lastupdated: "2025-11-07"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my IPsec connection status showing up but BGP state isn't established?
{: #troubleshoot-ipsec-up-bgp-not-established}
{: troubleshoot}
{: support}

An IPsec VPN connection in IBM Cloud may show as "up" while the BGP session isn't established. This issue indicates that a secure tunnel between the public IPs of the VPN devices is established, but the BGP session is not yet successfully established.
{: shortdesc}

The IPsec connection status is "up", whereas the BGP session isn't established and no routes are exchanged between IBM VPN Gateway and the peer device.
{: tsSymptoms}

BGP relies on correct tunnel IP setup and mutual compatibility between devices. This issue occurs due to unsupported routing protocols on your peer device, misconfiguration of tunnel IPs, or tunnel IP conflicts.
{: tsCauses}

Follow these steps to troubleshoot and resolve the BGP session issue:
{: tsResolve}

1. **Verify peer device routing support**: Ensure that your peer device supports dynamic routing with BGP. If it does not, you must configure a [static routing connection](/docs/vpc?topic=vpc-vpn-adding-connections&interface=ui) instead.
1. **Check tunnel IP configuration**: Confirm that the tunnel IPs are correctly configured on both the IBM VPN Gateway and your peer device. The `tunnel_interface_ip` on IBM should match the remote tunnel IP on your peer device. The peer’s tunnel IP should be configured as the remote IP on IBM.
1. **Check tunnel IPs for VPN gateway members**: Each connection on IBM VPN is configured on both gateway members. Ensure that the correct tunnel IPs are assigned to each member and that the peer device is configured to connect to the intended member.
1. **Avoid tunnel IP conflicts**: Make sure the tunnel IPs do not overlap with VPC address ranges, routes received from other VPN connections or transit gateway routes. Use IPs from the `169.254.0.0/16`range for tunnel interfaces.
