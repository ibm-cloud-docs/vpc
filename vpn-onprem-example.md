---

copyright:
  years: 2019, 2020
lastupdated: "2020-02-14"

keywords: peering, Vyatta, StrongSwan, FortiGate, Cisco, ASAv, Juniper, vSRX, connection, secure, remote, vpc, vpc network, vpn

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


# Connecting to your on-premises network  
{: #vpn-onprem-example}

You can use VPN for VPC to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your on-premises VPN gateway to connect with VPN for VPC.
{:shortdesc}

## Creating the VPN for VPC gateway and connection
{: #create-vpc-vpn-gateway}

[Create a VPN gateway in your VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#vpn) and create a VPN connection between the VPC and the peer gateway of the on-premises network by specifying the following information.
* **Connection name**: Enter a name for the connection, such as `onprem-connection`.
* **Peer gateway address**: Specify the IP address of the VPN gateway for the on-premises network.
* **Preshared key**: Specify the authentication key of the VPN gateway for the on-premises network.
* **Local subnets**: Specify one or more subnets in the VPC you want to connect through the VPN tunnel.
* **Peer subnets**: Specify one or more subnets in the on-premises network you want to connect through the VPN tunnel.

For the Internet Key Exchange (IKE) and IPsec security parameters, select **Auto** so the cloud gateway uses auto-negotiation to automatically establish the connection with the on-premises gateway.

If you create a connection to a Juniper VPN, you must create a custom IPsec policy instead of the default auto-negotiation. For instructions, see [Creating a custom IPsec policy in VPN for VPC](#custom-ipsec-policy).
{: important}

The gateway status appears as **Pending** while the VPN gateway is being created, and the status changes to **Available** after the gateway is created.
{: tip}

## Configuring the on-premises VPN gateway
{: #configuring-onprem-gateway}

The following section provides guidance for configuring your on-premises VPN gateway to connect with the VPN gateway in your VPC.


### Cisco ASAv configuration
{: #cisco-asav-config}

These instructions are based on Cisco ASAv, Cisco Adaptive Security Appliance Software Version 9.10(1).

The first step in configuring your Cisco ASAv for use with VPN for VPC is to ensure that the following prerequisite conditions are met:

* Cisco ASAv is online and functional with a proper license
* A password for the Cisco ASAv is enabled
* There's at least one configured and verified functional internal interface
* There's at least one configured and verified functional external interface

When a Cisco ASAv VPN receives a connection request from VPN for VPC, it uses IKE Phase 1 parameters to establish a secure connection and authenticate to VPN for VPC. Then, if the security policy permits the connection, the Cisco ASAv establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the Cisco ASAv VPN:

* Define the Phase 1 parameters that the Cisco ASAv VPN requires to authenticate VPN for VPC and establish a secure connection.
* Define the Phase 2 parameters that the Cisco ASAv VPN requires to create a VPN tunnel with VPN for VPC.

Create an IKE version 2 proposal object. IKEv2 proposal objects contain the parameters required for creating IKEv2 proposals when defining remote access and site-to-site VPN policies. IKE is a key management protocol that facilitates the management of
IPsec-based communications. It is used to authenticate IPsec peers, negotiate and distribute IPsec encryption keys, and automatically establish IPsec security associations (SAs).

```
group-policy GroupPolicy_161.156.80.10 internal
group-policy GroupPolicy_161.156.80.10 attributes
 vpn-tunnel-protocol ikev1 ikev2
tunnel-group 161.156.80.10 type ipsec-l2l
tunnel-group 161.156.80.10 general-attributes
 default-group-policy GroupPolicy_161.156.80.10
tunnel-group 161.156.80.10 ipsec-attributes
 ikev1 pre-shared-key <key value>
 ikev2 remote-authentication pre-shared-key <key value>
 ikev2 local-authentication pre-shared-key <key value>
```
{: codeblock}

Create an IKEv2 policy configuration for the IPsec connection. The IKEv2 policy block sets the parameters for the IKE exchange. In this block, the following parameters are set:
* Encryption algorithm: Set to AES-256 for this example.
* Integrity algorithm: Set to SHA256 for this example.
* Diffie-Hellman group: IPsec uses the Diffie-Hellman algorithm to generate the initial encryption key between the peers. In this example, it is set to group 14.
* Pseudo-Random Function (PRF): IKEv2 requires a separate method used as the algorithm to derive keying material and hashing operations that are required for the IKEv2 tunnel encryption. This is referred to as the pseudo-random function and is set to SHA.
* SA Lifetime: Set the lifetime of the security associations (after which time a reconnection occurs) to 36,000 seconds.
* Operation type: Keep this as the default value, bidirectional. (It's not explicit in the "show running" display.)

As shown in the following code example, this sample policy uses AES-256 to encrypt the secure channel. The SHA512 hash algorithm is used to validate the identity of the remote peer, and Diffie-Hellman group 14 is used for key generation. Group 14 uses 2048-bit encryption blocks. Finally, a lifetime for the security association is set to 36,000 seconds.

```
crypto ikev2 policy 100
encryption aes-256
integrity sha-1
group 14
prf sha
lifetime seconds 36000
```
{: codeblock}

Define the access list and crypto map for VPN:

```
access-list outside_cryptomap_1 extended permit ip object NETWORK_OBJ_192.168.236.0_24 object vpc
crypto map outside_map 1 match address outside_cryptomap_1
crypto map outside_map 1 set peer 161.156.80.10
crypto map outside_map 1 set ikev1 transform-set ESP-AES-128-SHA ESP-AES-128-MD5 ESP-AES-192-SHA ESP-AES-192-MD5 ESP-AES-256-SHA ESP-AES-256-MD5 ESP-3DES-SHA ESP-3DES-MD5 ESP-DES-SHA ESP-DES-MD5
crypto map outside_map 1 set ikev2 ipsec-proposal AES256 AES192 AES 3DES DES
crypto map outside_map interface outside
nat (any,outside) source static NETWORK_OBJ_192.168.236.0_24 NETWORK_OBJ_192.168.236.0_24 destination static vpc vpc no-proxy-arp route-lookup
```
{: codeblock}

### FortiGate configuration
{: #fortigate-config}

These instructions are based on FortiGate 300C, Firmware Version	v5.2.13, build762 (GA).

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

### Juniper vSRX configuration
{: #juniper-vsrx-config}

These instructions are based on Juniper vSRX, JUNOS Software Release [15.1X49-D123.3].

When the Juniper VPN receives a connection request from VPN for VPC, Juniper uses IPsec Phase 1 parameters to establish a secure connection and authenticate the VPN for VPC gateway. Then, if the security policy permits the connection, the Juniper VPN establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

Because Juniper vSRX requires PFS to be _enabled_ in Phase 2, you must create a custom IPsec policy to replace the default policy for the VPN in your VPC. For instructions, see [Creating a custom IPsec policy in VPN for VPC](#custom-ipsec-policy).
{: important}

To support these functions, the following general configuration steps must be performed on the Juniper vSRX unit:

* Define the Phase 1 parameters that the Juniper vSRX VPN requires to authenticate the remote peer and establish a secure connection.
* Define the Phase 2 parameters that the Juniper vSRX VPN requires to create a VPN tunnel with VPN for VPC.

Use the following configuration::

1. Choose `IKEv1` in phase 1.
2. Set up policy mode, not route mode.
3. Enable `DH-group 2` in the Phase 1 proposal.
4. Set `lifetime = 36000` in the Phase 1 proposal.
5. Enable PFS in the Phase 2 proposal.
6. Set `lifetime = 10800` in the Phase 2 proposal.
7. Input your peer and subnet information in the Phase 2 proposal.
8. Allow UDP 500 traffic on external interface.

Make sure that you specify IKEv1 in Phase 1. Juniper vSRX supports IKEv2 in _route mode_ only, but VPN for VPC currently supports _policy mode_ only. Therefore, if you set up IKEv2 in policy mode, you'll see the error `IKEv2 requires bind-interface configuration as only route-based is supported`.
{: important}

Here's an example of how to set up security:

```
proposal proposal-ike1 {
    authentication-method pre-shared-keys;
    dh-group group2;
    authentication-algorithm sha1;
    encryption-algorithm aes-128-cbc;
    lifetime-seconds 28800;
}
policy ike-policy1 {
    mode main;
    proposals proposal-ike1;
    pre-shared-key ascii-text "your_psk"
}
gateway ibm-vpc-vpn-gw {
    ike-policy ike-policy1;
    address 169.61.227.228;
    local-identity inet 129.146.215.142;
    remote-identity inet 169.61.227.228;
    external-interface ge-0/0/0.0;
}
proposal ipsec-proposal1 {
    protocol esp;
    authentication-algorithm hmac-sha1-96;
    encryption-algorithm aes-128-cbc;
    lifetime-seconds 3600;
}
policy ipsec-policy1 {
    proposals ipsec-proposal1;
}
vpn ibm_vpc {
    bind-interface st0.1;
    df-bit clear;
    ike {
        gateway ibm-vpc-vpn-gw;
        ipsec-policy ipsec-policy1;
    }
    traffic-selector pair1 {
        local-ip 172.29.6.0/23;
        remote-ip 100.64.28.0/25;
    }
    traffic-selector pair2 {
        local-ip 172.29.6.0/23;
        remote-ip 100.64.28.128/26;
    }
    establish-tunnels immediately;
}
interfaces {
    st0 {
        unit 1 {
            description Tunnel-to-IBM-VPC;
            family inet {
            }
        }
}
security {
    flow {
        tcp-mss {
            ipsec-vpn {
                mss 1350;
            }
}

[edit]

```
{: codeblock}

Here's an example of how to set up the firewall:

```
admin@Juniper-vSRX# show firewall filter PROTECT-IN term VPN_IKE
from {
    destination-address {
        169.61.195.195/32;
        10.93.160.12/32;
    }
    protocol udp;
    port 500;
}
then accept;

[edit]
```
{: codeblock}

After the configuration file finishes running, you can check the connection status from the CLI, with the following command:

```
 run show security ipsec security-associations
```
{: codeblock}

#### Creating a custom IPsec policy in VPN for VPC
{: #custom-ipsec-policy}

By default, VPN for VPC disables PFS in Phase 2, and Juniper vSRX requires PFS to be _enabled_ in Phase 2. Therefore, you must create a custom IPsec policy to replace the default policy for the VPN in your VPC.

To use a custom IPsec policy in VPN for VPC:
1. On the VPN for VPC page in the IBM Cloud console, select the **IPsec policies** tab.
2. Click **New IPsec policy** and specify the following values:
  * For the ***Authentication** field, select **MD5**.
  * For the **Encryption** field, select **aes256**.
  * Select the **PFS** option.
  * For the **DH Group** field, select **2**.
  * For the **Key lifetime** field, specify **1200**.
2. When you create the VPN connection in your VPC, select this custom IPsec policy.

### StrongSwan configuration
{: #strongswan-config}

These instructions are based on Linux StrongSwan U5.3.5/K4.4.0-133-generic.

Go to the **/etc** directory and create a new custom tunnel configuration file with a name such as `ipsec.abc.conf`. Edit the `/etc/ipsec.conf` file to include the new `ipsec.abc.conf` file by adding the following line:

```
include /etc/ipsec.abc.conf
```
{: codeblock}

When the StrongSwan VPN receives a connection request from VPN for VPC, StrongSwan uses IPsec Phase 1 parameters to establish a secure connection and authenticate the VPN for VPC gateway. Then, if the security policy permits the connection, the StrongSwan VPN establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the StrongSwan VPN:

* Define the Phase 1 parameters that StrongSwan requires to authenticate VPN for VPC and establish a secure connection.
* Define the Phase 2 parameters that the StrongSwan requires to create a VPN tunnel with VPN for VPC.

Use the following configuration:
1. Choose `IKEv2` in authentication.
2. Enable `DH-group 2` in the Phase 1 proposal.
3. Set `lifetime = 36000` in the Phase 1 proposal.
4. Disable PFS in the Phase 2 proposal.
5. Set `lifetime = 10800` in the Phase 2 proposal.
6. Input your peers and subnets information in the Phase 2 proposal. In the following example, a connection is defined between the on-premises subnet 10.160.26.64/26 whose StrongSwan VPN gateway has the IP address 169.45.74.119 and the VPC subnet 192.168.17.0/28 whose VPN for VPC gateway has the IP address 169.61.181.116.

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

Set the preshared key in `/etc/ipsec.secrets`

```
vim ipsec.secrets
# This file holds shared secrets or RSA private keys for authentication.

169.45.74.119 169.61.181.116 : PSK "******"

```
{: codeblock}

After the configuration file finishes running, restart the StrongSwan VPN.

```
ipsec restart

```
{: codeblock}

### Vyatta configuration
{: #vyatta-config}

These instructions are based on Vyatta version: AT&T vRouter 5600 1801d.

When the Vyatta VPN receives a connection request from VPN for VPC, Vyatta uses IPsec Phase 1 parameters to establish a secure connection and authenticate the VPN for VPC gateway. Then, if the security policy permits the connection, the Vyatta VPN establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the Vyatta VPN:

* Define the Phase 1 parameters that Vyatta requires to authenticate VPN for VPC and establish a secure connection.
* Define the Phase 2 parameters that the Vyatta requires to create a VPN tunnel with VPN for VPC.

Use the following configuration:

1. Choose `IKEv2` in authentication.
2. Enable `DH-group 2` in the Phase 1 proposal.
3. Set `lifetime = 36000` in the Phase 1 proposal.
4. Disable PFS in the Phase 2 proposal.
5. Set `lifetime = 10800` in the Phase 2 proposal.
6. Input your peers and subnets information in the Phase 2 proposal.

The following commands use the following variables where:

* `{{ peer_address }}` is the VPN gateway public IP address.
* `{{ peer_cidr }}` is the VPN gateway subnet.
* `{{ vyatta_address }}` is the Vyatta public IP address.
* `{{ vyatta_cidr }}` is the Vyatta subnet.

```
vim vyatta_temp/create_vpn.vcli
#!/bin/vcli -f
configure

set security vpn ipsec ike-group {{ peer_address }}_{{ ike["name"] }}
set security vpn ipsec ike-group {{ peer_address }}_{{ ike["name"] }} dead-peer-detection timeout 120
set security vpn ipsec ike-group {{ peer_address }}_{{ ike["name"] }} lifetime {{ ike["lifetime"] }}
set security vpn ipsec ike-group {{ peer_address }}_{{ ike["name"] }} ike-version {{ ike["version"] }}

set security vpn ipsec ike-group {{ peer_address }}_{{ ike["name"] }} proposal {{ loop.index }}
set security vpn ipsec ike-group {{ peer_address }}_{{ ike["name"] }} proposal {{ loop.index }} dh-group {{ proposal["dhgroup"] }} 
set security vpn ipsec ike-group {{ peer_address }}_{{ ike["name"] }} proposal {{ loop.index }} encryption {{ proposal["encryption"] }}
set security vpn ipsec ike-group {{ peer_address }}_{{ ike["name"] }} proposal {{ loop.index }} hash {{ proposal["integrity"] }}


set security vpn ipsec esp-group {{ peer_address }}_{{ ipsec["name"] }} compression disable
set security vpn ipsec esp-group {{ peer_address }}_{{ ipsec["name"] }} lifetime {{ ipsec["lifetime"] }}
set security vpn ipsec esp-group {{ peer_address }}_{{ ipsec["name"] }} mode tunnel
set security vpn ipsec esp-group {{ peer_address }}_{{ ipsec["name"] }} pfs {{ ipsec["proposals"][0]["dhgroup"] }}

set security vpn ipsec esp-group {{ peer_address }}_{{ ipsec["name"] }} proposal {{ loop.index }} encryption {{ proposal["encryption"] }}
set security vpn ipsec esp-group {{ peer_address }}_{{ ipsec["name"] }} proposal {{ loop.index }} hash {{ proposal["integrity"] }}

set security vpn ipsec site-to-site peer {{ peer_address }} authentication mode pre-shared-secret
set security vpn ipsec site-to-site peer {{ peer_address }} authentication pre-shared-secret {{ psk }}
set security vpn ipsec site-to-site peer {{ peer_address }} ike-group {{ peer_address }}_{{ ike["name"] }}
set security vpn ipsec site-to-site peer {{ peer_address }} default-esp-group {{ peer_address }}_{{ ipsec["name"] }}
set security vpn ipsec site-to-site peer {{ peer_address }} description "automation test"
set security vpn ipsec site-to-site peer {{ peer_address }} local-address {{ vyatta_address }}
set security vpn ipsec site-to-site peer {{ peer_address }} connection-type {{ connection_type }}
set security vpn ipsec site-to-site peer {{ peer_address }} authentication remote-id {{ peer_address }}

set security vpn ipsec site-to-site peer {{ peer_address }} tunnel {{ ns.tunnel_index }} local prefix {{ vyatta_cidr }}
set security vpn ipsec site-to-site peer {{ peer_address }} tunnel {{ ns.tunnel_index }} remote prefix {{ peer_cidr }}

commit
end_configure
```
{: codeblock}

## Example
For example, you can run the following commands:

```
#!/bin/vcli -f
configure

set security vpn ipsec ike-group 169.61.247.167_test_ike
set security vpn ipsec ike-group 169.61.247.167_test_ike dead-peer-detection timeout 120
set security vpn ipsec ike-group 169.61.247.167_test_ike lifetime 36000
set security vpn ipsec ike-group 169.61.247.167_test_ike ike-version 2

set security vpn ipsec ike-group 169.61.247.167_test_ike proposal 1
set security vpn ipsec ike-group 169.61.247.167_test_ike proposal 1 dh-group 2 
set security vpn ipsec ike-group 169.61.247.167_test_ike proposal 1 encryption aes256
set security vpn ipsec ike-group 169.61.247.167_test_ike proposal 1 hash sha2_256
set security vpn ipsec esp-group 169.61.247.167_test_ipsec compression disable
set security vpn ipsec esp-group 169.61.247.167_test_ipsec lifetime 18000
set security vpn ipsec esp-group 169.61.247.167_test_ipsec mode tunnel
set security vpn ipsec esp-group 169.61.247.167_test_ipsec pfs disable


set security vpn ipsec esp-group 169.61.247.167_test_ipsec proposal 1 encryption aes256
set security vpn ipsec esp-group 169.61.247.167_test_ipsec proposal 1 hash sha2_256
set security vpn ipsec site-to-site peer 169.61.247.167 authentication mode pre-shared-secret
set security vpn ipsec site-to-site peer 169.61.247.167 authentication pre-shared-secret ***YOUR-PSK***
set security vpn ipsec site-to-site peer 169.61.247.167 ike-group 169.61.247.167_test_ike
set security vpn ipsec site-to-site peer 169.61.247.167 default-esp-group 169.61.247.167_test_ipsec
set security vpn ipsec site-to-site peer 169.61.247.167 description "automation test"
set security vpn ipsec site-to-site peer 169.61.247.167 local-address 169.63.66.53
set security vpn ipsec site-to-site peer 169.61.247.167 connection-type initiate
set security vpn ipsec site-to-site peer 169.61.247.167 authentication remote-id 169.61.247.167


set security vpn ipsec site-to-site peer 169.61.247.167 tunnel 1 local prefix 10.65.15.104/29
set security vpn ipsec site-to-site peer 169.61.247.167 tunnel 1 remote prefix 10.240.0.0/24
    
    
commit
end_configure

```
{: screen}

Finally, make note of your `{{ psk }}` value, as you will need it to set up the VPN connection in the next step.

### Troubleshooting and more examples  
{: #troubleshooting-and-more-examples}

Remember to:
* Config your ACL to allow port 500 and 4500

Allow IKE and ESP traffic for IPsec:

```
# set rule 100 action 'accept' 
# set rule 100 destination port '500'
# set rule 100 protocol 'udp'
# set rule 200 action 'accept'
# set rule 200 protocol 'esp'
```

Allow L2TP over IPsec:

```
# set rule 210 action 'accept'
# set rule 210 destination port '1701'
# set rule 210 ipsec 'match-ipsec'
# set rule 210 protocol 'udp'
```

Allow NAT traversal of IPsec:

```
# set rule 250 action 'accept'
# set rule 250 destination port '4500'
# set rule 250 protocol 'udp'
```

#### Additional examples

To filter on source IP:

```
vyatta@R1# show security firewall name FWTEST-1
rule 1 {
	action accept
	source {
		address 172.16.0.26
	}
}
vyatta@R1# show interfaces dataplane dp0p1p1
address 172.16.1.1/24
	firewall FWTEST-1 {
	in {
	}
}
```

To filter on source and destination IP:

```
vyatta@R1# show security firewall name FWTEST-2
rule 1 {
	action accept
	destination {
		address 10.10.40.101
	}
	source {
		address 10.10.30.46
	}
}
vyatta@R1# show interfaces dataplane dp0p1p2
vif 40 {
	firewall {
	out FWTEST-2
	}
}
```

To filter traffic between zones:

```
vyatta@R1# show security zone-policy

zone dmz {
	description DMZ
	interface dp0p1p3
	to private {
		firewall to_private
	}
	to public {
		firewall to_public
	}
}
zone private {
	description PRIVATE
	interface dp0p1p1
	interface dp0p1p2
	to dmz {
		firewall to_dmz
	}
	to public {
		firewall to_public
	}
}
zone public {
	description PUBLIC
	interface dp0p1p4
	to dmz{
		firewall to_dmz
	}
	to private {
		firewall to_private
	}
}
```
{: codeblock}

## Check the status of the secure connection
{: #check-connection-status}

You can check the status of your connection in the {{site.data.keyword.cloud_notm}} console. On the VPN for VPC page, select your VPN gateway and click **Connections** from the navigation pane on the left side of the page.

You can also test the connection by doing a ping from a virtual server instance in your VPC to a server in the on-premises network.
