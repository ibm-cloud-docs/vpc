---

copyright:
  years: 2020
lastupdated: "2020-08-19"

keywords: fortigate, fortigate peer, vpn fortigate

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:important: .important}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}


# Connecting to a FortiGate peer 
{: #fortigate-config}

You can use {{site.data.keyword.cloud}} VPN for VPC to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your FortiGate VPN gateway to connect to VPN for VPC.
{: shortdesc}

These instructions are based on FortiGate 300C, Firmware Version v5.2.13, build762 (GA).

Go to **VPN \> IPsec \> Tunnels** and create a new custom tunnel or edit an existing tunnel.

When a FortiGate VPN receives a connection request from VPN for VPC, FortiGate uses IPsec Phase 1 parameters to establish a secure connection and authenticate VPN for VPC. Then, if the security policy permits the connection, the FortiGate VPN establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed for the FortiGate VPN:

* Define the Phase 1 parameters that the FortiGate VPN requires to authenticate VPN for VPC and establish a secure connection.
* Define the Phase 2 parameters that the FortiGate VPN requires to create a VPN tunnel with VPN for VPC.
* Create security policies to control the permitted services and permitted direction of traffic between the IP source and destination addresses.

Use the following configuration:

1. Choose IKEv2 in authentication.
2. Enable `DH-group 2` in the Phase 1 proposal.
3. Set `lifetime = 36000` in the Phase 1 proposal.
4. Disable PFS in the Phase 2 proposal.
5. Set `lifetime = 10800` in the Phase 2 proposal.
6. Input your subnet's information in Phase 2.
7. The remaining parameters keep their default values.

