---

copyright:
  years: 2019
lastupdated: "2019-09-30"

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


# Connecting to your on-premises network (Beta)
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

For the IKE and IPsec security parameters, select **Auto** so the cloud gateway uses auto-negotiation to automatically establish the connection with the on-premises gateway.

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

When a Cisco ASAv VPN receives a connection request from VPN for VPC, it uses IPsec Phase 1 parameters to establish a secure connection and authenticate to VPN for VPC. Then, if the security policy permits the connection, the Cisco ASAv establishes the tunnel using IPsec Phase 1 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the Cisco ASAv VPN:

* Define the Phase 1 parameters that the Cisco ASAv VPN requires to authenticate VPN for VPC and establish a secure connection.
* Define the Phase 2 parameters that the Cisco ASAv VPN requires to create a VPN tunnel with VPN for VPC.

Create an Internet Key Exchange (IKE) version 2 proposal object. IKEv2 proposal objects contain the parameters required for creating IKEv2 proposals when defining remote access and site-to-site VPN policies. IKE is a key management protocol that facilitates the management of
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

When a FortiGate VPN receives a connection request from VPN for VPC, FortiGate uses IPsec Phase 1 parameters to establish a secure connection and authenticate VPN for VPC. Then, if the security policy permits the connection, the FortiGate VPN establishes the tunnel using IPsec Phase parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

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

admin@Juniper-vSRX# show security    

log {
    mode stream;
    report;
}
ike {
    traceoptions {
        file ike_log_20 size 10240000;
        flag all;
    }
    proposal ike-proposal-1 {
        authentication-method pre-shared-keys;
        dh-group group2;
        authentication-algorithm sha-256;
        encryption-algorithm aes-256-cbc;
    }
    policy ike1 {
        mode main;
        proposals ike-proposal-1;
        pre-shared-key ascii-text "$9$sO2JGjHqfQFiH0BRhrl"; ## SECRET-DATA
    }
    gateway gw1 {
        ike-policy ike1;
        address 169.45.74.119;
        dead-peer-detection always-send;
        no-nat-traversal;
        local-identity inet 169.61.195.195;
        external-interface ge-0/0/1.0;
        local-address 169.61.195.195;
        version v1-only;
    }
}
ipsec {
    proposal AES128cbc-SHA256-esp {
        protocol esp;
        authentication-algorithm hmac-sha-256-128;
        encryption-algorithm aes-128-cbc;
    }
    policy ipsec1 {
        perfect-forward-secrecy {
            keys group2;
        }
        proposals AES128cbc-SHA256-esp;
    }
    vpn to-strongswan {
        ike {
            gateway gw1;
            proxy-identity {
                local 10.93.152.152/29;
                remote 10.160.26.64/26;
                service any;
            }
            ipsec-policy ipsec1;
        }
        establish-tunnels immediately;
    }
}
address-book {
    global {
        address SL_PRIV_MGMT 10.93.160.12/32;
        address SL_PUB_MGMT 169.61.195.195/32;
        }
    }
    local_cidr {
        address local_cidr 10.93.160.0/26;
        attach {
            zone SL-PRIVATE;
        }
    }
    remote_cidr {
        address remote 10.160.26.64/26;
        attach {
            zone SL-PUBLIC;
        }
    }
}
screen {
    ids-option untrust-screen {
        icmp {
            ping-death;
        }
        ip {
            source-route-option;
            tear-drop;                  
        }
        tcp {
            syn-flood {
                alarm-threshold 1024;
                attack-threshold 200;
                source-threshold 1024;
                destination-threshold 2048;
                queue-size 2000; ## Warning: 'queue-size' is deprecated
                timeout 20;
            }
            land;
        }
    }
}
policies {
    from-zone SL-PRIVATE to-zone SL-PRIVATE {
        policy Allow_Management {
            match {
                source-address any;
                destination-address any;
                application any;
            }
            then {
                permit;
            }
        }
    }
    from-zone SL-PUBLIC to-zone SL-PUBLIC {
        policy pub2pub {
            match {
                source-address any;
                destination-address SL_PUB_MGMT;
                application any;
            }
            then {
                permit;
            }
        }
        policy Allow_Management {
            match {
                source-address any;     
                destination-address SL_PUB_MGMT;
                application [ junos-ssh junos-https junos-http junos-icmp-ping ];
            }
            then {
                permit;
            }
        }
    }
    from-zone SL-PRIVATE to-zone SL-PUBLIC {
        policy out {
            match {
                source-address any;
                destination-address any;
                application any;
            }
            then {
                permit {
                    tunnel {
                        ipsec-vpn to-strongswan;
                    }
                }
            }
        }
    }
    from-zone SL-PUBLIC to-zone SL-PRIVATE {
        policy in {
            match {
                source-address any;
                destination-address any;
                application any;
            }
            then {
                permit {
                    tunnel {
                        ipsec-vpn to-strongswan;
                    }
                }
            }
        }
    }
}                                       
zones {
    security-zone SL-PRIVATE {
        interfaces {
            ge-0/0/0.0 {
                host-inbound-traffic {
                    system-services {
                        all;
                    }
                }
            }
            st0.1;
            ge-0/0/0.986 {
                host-inbound-traffic {
                    system-services {
                        all;
                    }
                }
            }
        }
    }
    security-zone SL-PUBLIC {
        interfaces {
            ge-0/0/1.0 {
                host-inbound-traffic {
                    system-services {
                        all;
                    }
                }
            }
        }
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

You can use the Vyatta command line to configure the IPsec tunnel, or you can upload a `*.vcli` file to load your configuration.

```
vim vyatta_temp/create_vpn.vcli
#!/bin/vcli -f
configure

set security vpn ipsec ike-group 169.61.161.151_test_ike
set security vpn ipsec ike-group 169.61.161.151_test_ike dead-peer-detection timeout 120
set security vpn ipsec ike-group 169.61.161.151_test_ike lifetime 36000
set security vpn ipsec ike-group 169.61.161.151_test_ike ike-version 2

set security vpn ipsec ike-group 169.61.161.151_test_ike proposal 1
set security vpn ipsec ike-group 169.61.161.151_test_ike proposal 1 dh-group 2
set security vpn ipsec ike-group 169.61.161.151_test_ike proposal 1 encryption aes256
set security vpn ipsec ike-group 169.61.161.151_test_ike proposal 1 hash sha2_256
set security vpn ipsec esp-group 169.61.161.151_test_ipsec compression disable
set security vpn ipsec esp-group 169.61.161.151_test_ipsec lifetime 10800
set security vpn ipsec esp-group 169.61.161.151_test_ipsec mode tunnel
set security vpn ipsec esp-group 169.61.161.151_test_ipsec pfs disable


set security vpn ipsec esp-group 169.61.161.151_test_ipsec proposal 1 encryption aes256
set security vpn ipsec esp-group 169.61.161.151_test_ipsec proposal 1 hash sha2_256
set security vpn ipsec site-to-site peer 169.61.161.151 authentication mode pre-shared-secret
set security vpn ipsec site-to-site peer 169.61.161.151 authentication pre-shared-secret ******
set security vpn ipsec site-to-site peer 169.61.161.151 ike-group 169.61.161.151_test_ike
set security vpn ipsec site-to-site peer 169.61.161.151 default-esp-group 169.61.161.151_test_ipsec
set security vpn ipsec site-to-site peer 169.61.161.151 description "automation test"
set security vpn ipsec site-to-site peer 169.61.161.151 local-address 169.45.74.119
set security vpn ipsec site-to-site peer 169.61.161.151 connection-type initiate


set security vpn ipsec site-to-site peer 169.61.161.151 tunnel 1 local prefix 192.168.200.0/24
set security vpn ipsec site-to-site peer 169.61.161.151 tunnel 1 remote prefix 192.168.17.0/28

commit
```
{: codeblock}

Upload this `*.vcli` file to the Vyatta using SCP to apply the configuration.

```
scp example.vcli <vyatta_username>@<vyatta_ip>`
```
{: codeblock}

## Check the status of the secure connection
{: #check-connection-status}

You can check the status of your connection in the {{site.data.keyword.cloud_notm}} console. On the VPN for VPC page, select your VPN gateway. Click **Connections** from the navigation pane on the left side of the page.

You can also test the connection by doing a ping from a virtual server instance in your VPC to a server in the on-premises network.