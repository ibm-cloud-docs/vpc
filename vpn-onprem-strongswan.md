---

copyright:
  years: 2020, 2025
lastupdated: "2025-11-05"

keywords: strongswan peer

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to a strongSwan peer
{: #strongswan-config}

You can use IBM Cloud VPN for VPC to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your strongSwan VPN gateway to connect to VPN for VPC.
{: shortdesc}

These instructions are based on Linux strongSwan U5.3.5/K4.4.0-133-generic.
{: note}

Read [Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations) before continuing to connect to your on-premises peer.
{: important}

Go to the **/etc** directory and create a new custom tunnel configuration file with a name (such as `ipsec.abc.conf`). Edit the `/etc/ipsec.conf` file to include the new `ipsec.abc.conf` file by adding the following line:

```sh
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
1. Input your peers and subnets information in the Phase 2 proposal. 

   The following example defines a connection between the on-premises subnet `10.160.26.64/26` (whose strongSwan VPN gateway has the IP address `169.45.74.119`) and the VPC subnet `192.168.17.0/28` (whose {{site.data.keyword.vpn_vpc_short}} gateway has the IP address `169.61.181.116`).

    ```sh
    vim /etc/ipsec.abc.conf
    conn all
           type=tunnel
           auto=start
           #aggressive=no
           esp=aes256-sha256!
           ike=aes256-sha256-modp2048!
           left=%any
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

1. Set the preshared key in `/etc/ipsec.secrets`:

   ```sh
   vim ipsec.secrets
   # This file holds shared secrets or RSA private keys for authentication.

   169.45.74.119 169.61.181.116 : PSK "******"

   ```
   {: codeblock}

1. After the configuration file finishes running, restart the strongSwan VPN.

   ```sh
   ipsec restart

   ```
   {: pre}

## Connecting an IBM route-based VPN to a strongSwan peer
{: #route-based-configuration-strongswan}

The following example configuration shows how to set up two route-based tunnels between the strongSwan and VPN for VPC.

1. To enable IP forwarding, enter the following command:

   ```sh
   sudo sysctl -w net.ipv4.conf.all.forwarding=1
   ```
   {: pre}

1. Create a file named `/etc/strongswan.d/charon-no-route-install.conf` and add the following content:

   ```sh
   charon {
         install_routes = no
   }
   ```
   {: pre}

1. To configure the VPN connection, update the `/etc/ipsec.abc.conf` file. In the following example, the VPN gateway has two public IPs (`135.90.134.86`, `135.90.134.87`). The strongSwan server IP is `169.59.212.125`. This configuration creates two connections. The `leftid` is the public IP of the strongSwan server, while the `right` and `rightid` represent the VPN gateway's public IP.

   ```sh
   conn peer_135.90.134.86
     keyexchange=ikev2
     left=%any
     leftid=169.59.212.125
     leftsubnet=0.0.0.0/0
     rightsubnet=0.0.0.0/0
     right=135.90.134.86
     rightid=135.90.134.86
     auto=start
     ike=aes256-aes192-aes128-sha512-sha384-sha256-modp2048s256-modp2048s224-modp1024s160-ecp521-ecp384-ecp256-modp8192-modp6144-modp4096-modp3072-modp2048-x25519!
     ikelifetime=36000s
     esp=aes256gcm16-aes192gcm16-aes128gcm16,aes256-aes192-aes128-sha512-sha384-sha256!
     lifetime=10800s
     type=tunnel
     leftauth=psk
     rightauth=psk
     dpdaction = restart
     dpddelay = 10s
     mark = 1
   conn peer_135.90.134.87
     keyexchange=ikev2
     left=%any
     leftid=169.59.212.125
     leftsubnet=0.0.0.0/0
     rightsubnet=0.0.0.0/0
     right=135.90.134.87
     rightid=135.90.134.87
     auto=start
     ike=aes256-aes192-aes128-sha512-sha384-sha256-modp2048s256-modp2048s224-modp1024s160-ecp521-ecp384-ecp256-modp8192-modp6144-modp4096-modp3072-modp2048-x25519!
     ikelifetime=36000s
     esp=aes256gcm16-aes192gcm16-aes128gcm16,aes256-aes192-aes128-sha512-sha384-sha256!
     lifetime=10800s
     type=tunnel
     leftauth=psk
     rightauth=psk
     dpdaction = restart
     dpddelay = 10s
     mark = 2
   ```
   {: codeblock}

1. Set the preshared key in the `/etc/ipsec.secrets` file and replace `******` for the real preshared key value:

   ```sh
   169.59.212.125 135.90.134.86 : PSK "******"
   169.59.212.125 135.90.134.86 : PSK "******"
   ```
   {: pre}

1. Create virtual interfaces on the server and set the VTI interface status to `up`. Replace `mark_num` with the value of `mark` (as defined in step 3). The `local_ip` is the **private IP** of the strongSwan server, and the `remote_ip` is the **public IP** of the VPN gateway's public IP addresses.

   ```sh
   # sample: sudo ip tunnel add vti<mark_num> local <local_ip> remote <remote_ip> mode vti key <mark_num>
   sudo ip tunnel add vti1 local 10.240.2.11 remote 135.90.134.86 mode vti key 1
   sudo ip tunnel add vti1 local 10.240.2.11 remote 135.90.134.87 mode vti key 2
   sudo ip link set vti1 up
   sudo ip link set vti2 up
   ```
   {: pre}

1. Add a route on the strongSwan server. In this example, `10.240.0.0/24` is the subnet of the VPN gateway to connect to and the VTI names are the VTI names from step 5.

   ```sh
   sudo ip route add 10.240.0.0/24 proto static next hop dev vti1 next hop dev vti2
   ```
   {: pre}

1. After the configuration file finishes running, restart the strongSwan VPN.

   ```sh
   ipsec restart
   ```
   {: pre}
