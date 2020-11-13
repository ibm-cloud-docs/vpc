---

copyright:
  years: 2020
lastupdated: "2020-11-13"

keywords: juniper, juniper peer, vpn juniper, vsrx, juniper vsrx, vsrx peer

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


# Connecting an IBM policy-based VPN to a Juniper vSRX peer
{: #juniper-vsrx-config}

You can use {{site.data.keyword.cloud}} VPN for VPC to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance on how to configure your Juniper VPN gateway to connect to VPN for VPC.
{: shortdesc}

Because Juniper vSRX requires Perfect Forward Secrecy (PFS) to be enabled in Phase 2, you must create a custom IPsec policy to replace the default policy for the VPN in your VPC. For more information, see [Creating a custom IPsec policy for Juniper vSRX](#custom-ipsec-policy-with-vsrx).
{: important}

These instructions are based on Juniper vSRX, JUNOS Software Release [15.1X49-D123.3].

When the Juniper VPN receives a connection request from VPN for VPC, Juniper uses IPsec Phase 1 parameters to establish a secure connection and authenticate the VPN for VPC gateway. Then, if the security policy permits the connection, the Juniper VPN establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, you must do the following on the Juniper vSRX unit:

* Define the Phase 1 parameters that the Juniper vSRX VPN requires to authenticate the remote peer and establish a secure connection.
* Define the Phase 2 parameters that the Juniper vSRX VPN requires to create a VPN tunnel with VPN for VPC.

General configuration steps are as follows.

1. Choose `IKEv1` in Phase 1. 
2. Set up policy-based mode.
3. Enable `DH-group 2` in the Phase 1 proposal.
4. Set `lifetime = 36000` in the Phase 1 proposal.
5. Enable PFS in the Phase 2 proposal.
6. Set `lifetime = 10800` in the Phase 2 proposal.
7. Input your peer and subnet information in the Phase 2 proposal.
8. Allow UDP 500 traffic on the external interface.

Make sure that you specify `IKEv1` in Phase 1. Juniper vSRX supports IKEv2 in route-based mode only. If you set up IKEv2 in policy-based mode, you'll see the error `IKEv2 requires bind-interface configuration as only route-based is supported`. For a route-based setup, see the section below on [route-based setup](#route-based-setup-vsrx).
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

After the configuration file finishes running, you can check the connection status from the CLI using the following command:

```
 run show security ipsec security-associations
```
{: codeblock}

## Creating a custom IPsec policy for Juniper vSRX
{: #custom-ipsec-policy-with-vsrx}

By default, VPN for VPC disables PFS in Phase 2, and Juniper vSRX requires PFS to be enabled in Phase 2. Therefore, you must create a custom IPsec policy to replace the default policy for the VPN in your VPC.

To use a custom IPsec policy in VPN for VPC, follow these steps:

1. On the VPN for VPC page in the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, select the **IPsec policies** tab.
1. Click **New IPsec policy** and specify the following values:
  * For the **Authentication** field, select **MD5**.
  * For the **Encryption** field, select **aes256**.
  * Select the **PFS** option.
  * For the **DH Group** field, select **2**.
  * For the **Key lifetime** field, specify **1200**. 
1. When you create the VPN connection in your VPC, select this custom IPsec policy.

## Route-based configuration for Juniper vSRX
{: #route-based-setup-vsrx}

The following configuration shows how to set up two route-based tunnels between the Juniper vSRX VPN and VPN for VPC using a weighted preference for two tunnels.

The VPN for VPC gateway should have a connection where the peer address is the vSRX public IP.
{: note}

Set the vSRX configuration as follows:

```
ike {
    proposal ike-proposal-vpnaas {
        authentication-method pre-shared-keys;
        dh-group group5;
        authentication-algorithm sha1;
        encryption-algorithm aes-128-cbc;
        lifetime-seconds 28800;
    } 
    policy ike-policy-vpnaas {
        mode main;
        proposals ike-proposal-vpnaas;
        pre-shared-key ascii-text "<text>"; ## SECRET-DATA
    }
    gateway ike-gate-vpnaas {
        ike-policy ike-policy-vpnaas;
        address <VPN for VPC Gateway Public IP #1>;
        dead-peer-detection {
            inactive: probe-idle-tunnel;
            interval 10;
            threshold 2;
        }
        local-identity inet <vSRX Public IP>;
        external-interface reth1.0;
        general-ikeid;
        version v2-only;
    }
    gateway ike-gate-vpnaas2 {
        ike-policy ike-policy-vpnaas;   
        address <VPN for VPC Gateway Public IP #2>;          
        dead-peer-detection {           
            inactive: probe-idle-tunnel;
            interval 10;                
            threshold 2;                
        }                               
        local-identity inet <vSRX Public IP>;
        external-interface reth1.0;     
        inactive: general-ikeid;        
        version v2-only;                
    }
```
{: codeblock}

```
ipsec {                                 
    proposal ipsec-proposal-vpnaas {    
        protocol esp;                   
        authentication-algorithm hmac-sha1-96;
        encryption-algorithm aes-128-cbc;
        lifetime-seconds 3600;          
    }                                   
    policy ipsec-policy-vpnaas {        
        proposals ipsec-proposal-vpnaas;
    }                                   
    vpn ipsec-vpn-vpnaas {              
        bind-interface st0.1;           
        ike {                           
            gateway ike-gate-vpnaas;    
            ipsec-policy ipsec-policy-vpnaas;
        }                               
        establish-tunnels immediately;  
    }                                   
    vpn ipsec-vpn-vpnaas2 {             
        bind-interface st0.0;           
        ike {                           
            gateway ike-gate-vpnaas2;   
            ipsec-policy ipsec-policy-vpnaas;
        }                               
        establish-tunnels immediately;  
    }
```
{: codeblock}

```
st0 {                                   
    unit 0 {                            
        description IBM_VPC1;           
        family inet;                    
    }                                   
    unit 1 {                            
        description IBM_VPC2;           
        family inet;                    
    }                                   
} 
```
{: codeblock}

```
routing-options {
    static {
        route <static route> {
            qualified-next-hop st0.0 {
                preference 15;
            }
            qualified-next-hop st0.1 {
                preference 10;
            }
        }
    }
}
```
{: codeblock}

## Verifying the configuration
{: #verifying-configuration-vpn}

Follow these steps to verify the configuration:

1. Verify IKE Phase 1 is working for both tunnels:
`run show security ike sa`
2. Verify IKE Phase 2 is working for both tunnels:
`run show security ipsec sa`
3. Show the route: 
`run show route <static route>`
