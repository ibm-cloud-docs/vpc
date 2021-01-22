---

copyright:
  years: 2020
lastupdated: "2020-11-13"

keywords: strongswan peer

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


# Connecting to a strongSwan peer
{: #strongswan-config}

You can use {{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_short}} to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your strongSwan VPN gateway to connect to {{site.data.keyword.vpn_vpc_short}}.
{: shortdesc}

These instructions are based on Linux strongSwan U5.3.5/K4.4.0-133-generic.

Go to the **/etc** directory and create a new custom tunnel configuration file with a name such as `ipsec.abc.conf`. Edit the `/etc/ipsec.conf` file to include the new `ipsec.abc.conf` file by adding the following line:

```
include /etc/ipsec.abc.conf
```
{: codeblock}

When the strongSwan VPN receives a connection request from {{site.data.keyword.vpn_vpc_short}}, strongSwan uses IPsec Phase 1 parameters to establish a secure connection and authenticate the {{site.data.keyword.vpn_vpc_short}} gateway. Then, if the security policy permits the connection, the strongSwan VPN establishes the tunnel by using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the strongSwan VPN:

* Define the Phase 1 parameters that the strongSwan requires to authenticate {{site.data.keyword.vpn_vpc_short}} and establish a secure connection.
* Define the Phase 2 parameters that the strongSwan requires to create a VPN tunnel with {{site.data.keyword.vpn_vpc_short}}.


## Connecting an IBM policy-based VPN to a strongSwan peer
{: #strongswan-config-policy-based}

Use the following configuration:
1. Choose `IKEv2` in authentication.
1. Enable `DH-group 2` in the Phase 1 proposal.
1. Set `lifetime = 36000` in the Phase 1 proposal.
1. Disable PFS in the Phase 2 proposal.
1. Set `lifetime = 10800` in the Phase 2 proposal.
1. Input your peers and subnets information in the Phase 2 proposal. In the following example, a connection is defined between the on-premises subnet `10.160.26.64/26` whose strongSwan VPN gateway has the IP address `169.45.74.119` and the VPC subnet `192.168.17.0/28` whose {{site.data.keyword.vpn_vpc_short}} gateway has the IP address `169.61.181.116`.

    ```
    vim /etc/ipsec.abc.conf
    conn all
           type=tunnel
           auto=route
           #aggressive=no
           esp=aes256-sha256!
           ike=aes128-sha1-modp1024!
           left=169.45.74.119
           leftsubnet=10.160.26.64/26
           rightsubnet=192.168.17.0/28
           right=169.61.181.116
           leftauth=psk
           rightauth=psk
           leftid="169.45.74.119"
           keyexchange=ikev2
           rightid="169.61.181.116"
           lifetime=10800s
           ikelifetime=36000s
           dpddelay=30s
           dpdaction=restart
           dpdtimeout=120s
    ```
    {: codeblock}

Set the preshared key in `/etc/ipsec.secrets`:

```
vim ipsec.secrets
# This file holds shared secrets or RSA private keys for authentication.

169.45.74.119 169.61.181.116 : PSK "******"

```
{: codeblock}

After the configuration file finishes running, restart the strongSwan VPN.

```
ipsec restart

```
{: pre}
