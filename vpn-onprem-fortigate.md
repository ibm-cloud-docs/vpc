---

copyright:
  years: 2020
lastupdated: "2020-11-13"

keywords: fortigate, fortigate peer

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to a FortiGate peer
{: #fortigate-config}

You can use IBM Cloud VPN Gateway for VPC  to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your FortiGate VPN gateway to connect to VPN Gateway for VPC.
{: shortdesc}

These instructions are based on FortiGate 300C, Firmware Version v5.2.13, build762 (GA).

Read [VPN gateway limitations](/docs/vpc?topic=vpc-vpn-limitations) before continuing to connect to your on-premises peer. 
{: note}

Go to **VPN \> IPsec \> Tunnels** and create a new custom tunnel or edit an existing tunnel.

When a FortiGate VPN receives a connection request from {{site.data.keyword.vpn_vpc_short}}, FortiGate uses IPsec Phase 1 parameters to establish a secure connection and authenticate {{site.data.keyword.vpn_vpc_short}}. Then, if the security policy permits the connection, the FortiGate VPN establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed for the FortiGate VPN:

* Define the Phase 1 parameters that the FortiGate VPN requires to authenticate {{site.data.keyword.vpn_vpc_short}} and establish a secure connection.
* Define the Phase 2 parameters that the FortiGate VPN requires to create a VPN tunnel with {{site.data.keyword.vpn_vpc_short}}.
* Create security policies to control the permitted services and permitted direction of traffic between the IP source and destination addresses.

Use the following configuration:

1. Choose IKEv2 in authentication.
1. Enable `DH-group 2` in the Phase 1 proposal.
1. Set `lifetime = 36000` in the Phase 1 proposal.
1. Disable PFS in the Phase 2 proposal.
1. Set `lifetime = 10800` in the Phase 2 proposal.
1. Input your subnet's information in Phase 2.
1. The remaining parameters keep their default values.
