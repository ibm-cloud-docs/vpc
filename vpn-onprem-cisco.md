---

copyright:
  years: 2020
lastupdated: "2020-11-13"

keywords: cisco peer, vpn cisco

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


# Connecting to a Cisco ASAv peer
{: #cisco-asav-config}

You can use {{site.data.keyword.cloud}} VPN for VPC to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your Cisco ASAv VPN gateway to connect to VPN for VPC.
{: shortdesc}


## Connecting an IBM policy-based VPN to a Cisco ASAv peer
{: #cisco-asav-config-policy-based}

Cisco ASAv uses IKEv2 when you have multiple subnets either on IBM VPC or your on-premises network, and on your on-premises VPN device. You must create one VPN connection per one subnet pair on a IBM VPN gateway because Cisco ASAv creates a new SA per subnet pair.
{: important}

These instructions are based on Cisco ASAv, Cisco Adaptive Security Appliance Software Version 9.10(1).

The first step in configuring your Cisco ASAv for use with VPN for VPC is to ensure that the following prerequisite conditions are met:

* Cisco ASAv is online and functional with a proper license.
* A password for the Cisco ASAv is enabled.
* There's at least one configured and verified functional internal interface.
* There's at least one configured and verified functional external interface.

When a Cisco ASAv VPN receives a connection request from VPN for VPC, it uses IKE Phase 1 parameters to establish a secure connection and authenticate to VPN for VPC. Then, if the security policy permits the connection, the Cisco ASAv establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the Cisco ASAv VPN:

* Define the Phase 1 parameters that the Cisco ASAv VPN requires to authenticate VPN for VPC and establish a secure connection.
* Define the Phase 2 parameters that the Cisco ASAv VPN requires to create a VPN tunnel with VPN for VPC.

Create an IKE version 2 proposal object. IKEv2 proposal objects contain the parameters that are required for creating IKEv2 proposals when defining remote access and site-to-site VPN policies. IKE is a key management protocol that facilitates the management of
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
* **Encryption algorithm** - Set to AES-256 for this example.
* **Integrity algorithm** - Set to SHA256 for this example.
* **Diffie-Hellman group** - IPsec uses the Diffie-Hellman algorithm to generate the initial encryption key between the peers. In this example, it is set to group 14.
* **Pseudo-Random Function (PRF)** - IKEv2 requires a separate method that is used as the algorithm to derive keying material and hashing operations that are required for the IKEv2 tunnel encryption. This is referred to as the pseudo-random function and is set to SHA.
* **SA Lifetime** - Set the lifetime of the security associations (after which time a reconnection occurs) to 36,000 seconds.
* **Operation type** - Keep this as the default value, bidirectional. (It's not explicit in the "show running" display.)

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

## Connecting an IBM static, route-based VPN to a Cisco ASAv peer
{: #cisco-asav-config-static-route-based}

Currently, a VPN for VPC static, route-based VPN does not support a Cisco ASAv peer. 
{: important}


