---

copyright:
  years: 2022, 2025
lastupdated: "2025-06-27"

keywords: cisco peer, ASAv

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to a Cisco ASAv peer
{: #cisco-asav-config}

You can use IBM Cloud VPN for VPC to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your Cisco ASAv VPN gateway to connect to VPN for VPC.
{: shortdesc}

Read [Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations) before you continue to connect to your on-premises peer.
{: note}

## Connecting an IBM policy-based VPN to a Cisco ASAv peer
{: #cisco-asav-config-policy-based}

Cisco ASAv uses IKEv2 when you have multiple subnets either on IBM VPC or your on-premises network, and on your on-premises VPN device. You must create one VPN connection per one subnet pair on an IBM VPN gateway because Cisco ASAv creates a new security association (SA) per subnet pair.
{: important}

These instructions are based on Cisco ASAv, Cisco Adaptive Security Appliance Software Version 9.10(1).

The first step in configuring your Cisco ASAv for use with {{site.data.keyword.vpn_vpc_short}} is to ensure that the following prerequisite conditions are met:

* Cisco ASAv is online and functional with a proper license.
* A password for the Cisco ASAv is enabled.
* There's at least one configured and verified functional internal interface.
* There's at least one configured and verified functional external interface.

When a Cisco ASAv VPN receives a connection request from {{site.data.keyword.vpn_vpc_short}}, it uses IKE Phase 1 parameters to establish a secure connection and authenticate to {{site.data.keyword.vpn_vpc_short}}. Then, if the security policy permits the connection, the Cisco ASAv establishes the tunnel by using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the Cisco ASAv VPN:

* Make sure the public IP address for Cisco ASAv is configured directly on the ASAv. Use `crypto isakmp identity address` to ensure the Cisco ASAv uses the public IP address of the interface as its identity.

   This global setting applies to all connections on the Cisco device. So, if you need to maintain multiple connections, set `crypto isakmp identity auto` instead, to ensure that the Cisco device automatically determines the identity by connection type.

* Define the Phase 1 parameters that the Cisco ASAv VPN requires to authenticate {{site.data.keyword.vpn_vpc_short}} and establish a secure connection.
* Define the Phase 2 parameters that the Cisco ASAv VPN requires to create a VPN tunnel with {{site.data.keyword.vpn_vpc_short}}.

The ASAv device supports object groups for the ACLs feature. This feature extends the conventional ACLs to support object-group-based ACLs. You can create the following object group according to your VPC subnets and on-premises subnets:

```sh
# define network object according to your VPC and on-premises subnet
object-group network on-premise-subnets
 network-object 172.16.0.0 255.255.0.0
object-group network ibm-vpc-zone3-subnets
 network-object 10.241.129.0 255.255.255.0
object-group network ibm-vpc-zone2-subnets
 network-object 10.240.64.0 255.255.255.0
```
{: codeblock}

Create an IKE version 2 proposal object. IKEv2 proposal objects contain the parameters that are required for creating IKEv2 proposals when you define remote access and site-to-site VPN policies. IKE is a key management protocol that facilitates the management of IPsec-based communications. It is used to authenticate IPsec peers, negotiate and distribute IPsec encryption keys, and automatically establish IPsec security associations (SAs).

In this block, the following parameters are set as an example. You can choose other parameters according to your company's security policy; however, make sure to use identical parameters on the IBM VPN gateway and ASAv.

* **Encryption algorithm** - Set to `aes-256` for this example.
* **Integrity algorithm** - Set to `sha256` for this example.
* **Diffie-Hellman group** - IPsec uses the Diffie-Hellman algorithm to generate the initial encryption key between the peers. In this example, it is set to group `19`.
* **Pseudo-Random Function (PRF)** - IKEv2 requires a separate method that is used as the algorithm to derive keying material and hashing operations that are required for the IKEv2 tunnel encryption. This is referred to as the pseudo-random function and is set to `sha256`.
* **SA Lifetime** - Set the lifetime of the security associations (after which time a reconnection occurs) to `36000` seconds.

```sh
crypto ikev2 policy 100
encryption aes-256
integrity sha256
group 19
prf sha256
lifetime seconds 36000
crypto ikev2 enable outside
```
{: codeblock}

Create an IPsec policy for the connection. The IKEv2 supports multiple encryptions and authentication types, and multiple integrity algorithms for a single policy. The ASAv orders the settings from the "most secure" to the "least secure" and negotiates with the peer using that order.

```sh
# Create IPsec policy, IKEv2 support multiple proposals
crypto ipsec ikev2 ipsec-proposal ibm-vpc-proposal
 protocol esp encryption aes-256
 protocol esp integrity sha-256
```
{: codeblock}

Create the group policy and tunnel group. The peer address and pre-shared key are configured in this step.

```sh
# Create VPN default group policy
group-policy ibm_vpn internal
group-policy ibm_vpn attributes
 vpn-tunnel-protocol ikev2

# Create the tunnel-group to configure pre-shared keys, 150.239.170.57 is public IP of IBM policy-based VPN gateway
tunnel-group 150.239.170.57 type ipsec-l2l
tunnel-group 150.239.170.57 general-attributes
 default-group-policy ibm_vpn
tunnel-group 150.239.170.57 ipsec-attributes
 ikev2 remote-authentication pre-shared-key <your pre-shared key>
 ikev2 local-authentication pre-shared-key <your pre-shared key>
```
{: codeblock}

Create an ACL to match the traffic from on-premises to VPC. For the traffic from VPC to on-premises, the ASAv uses the SPI to look up the traffic selector. Make sure that both sides are using a matched traffic selector.

```sh
access-list outside_cryptomap_ibm_vpc_zone2 extended permit ip object-group on-premise-subnets object-group ibm-vpc-zone2-subnets
```
{: codeblock}

Create a crypto map to pull together the various elements of the VPN tunnel, and activate it on the outside interface. `150.239.170.57` is the public IP of the IBM policy-based VPN gateway.

```sh
crypto map ibm_vpc 1 match address outside_cryptomap_ibm_vpc_zone2
crypto map ibm_vpc 1 set peer 150.239.170.57
crypto map ibm_vpc 1 set ikev2 ipsec-proposal ibm-vpc-proposal
crypto map ibm_vpc 1 set pfs group19
crypto map ibm_vpc interface outside
```
{: codeblock}

If you have NAT rules on the ASAv devices, you must exempt the traffic on the VPN from the NAT rules.

```sh
nat (inside,outside) source static on-premise-subnets on-premise-subnets destination static ibm-vpc-zone2-subnets ibm-vpc-zone2-subnets
```
{: codeblock}

Configure TCP MSS clamping on ASAv to avoid unnecessary fragmentation:

```sh
sysopt connection tcpmss 1360
```
{: codeblock}

## Connecting an IBM static, route-based VPN to a Cisco ASAv peer
{: #cisco-asav-config-static-route-based}

These instructions are based on Cisco ASAv, Cisco Adaptive Security Appliance Software Version 9.10(1).

The first step in configuring your Cisco ASAv for use with {{site.data.keyword.vpn_vpc_short}} is to ensure that the following prerequisite conditions are met:

* Cisco ASAv is online and functional with a proper license.
* A password for the Cisco ASAv is enabled.
* There's at least one configured and verified functional internal interface.
* There's at least one configured and verified functional external interface.

When a Cisco ASAv VPN receives a connection request from {{site.data.keyword.vpn_vpc_short}}, it uses IKE Phase 1 parameters to establish a secure connection and authenticate to {{site.data.keyword.vpn_vpc_short}}. Then, if the security policy permits the connection, the Cisco ASAv establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the Cisco ASAv VPN:

* Make sure the public IP address for Cisco ASAv is configured directly on the ASAv. Use `crypto isakmp identity address` to ensure the Cisco ASAv uses the public IP address of the interface as its identity.

   This global setting applies to all connections on the Cisco device, so if you need to maintain multiple connections, set `crypto isakmp identity auto` instead, to ensure that the Cisco device automatically determines the identity by connection type.

* Define the Phase 1 parameters that the Cisco ASAv VPN requires to authenticate {{site.data.keyword.vpn_vpc_short}} and establish a secure connection.
* Define the Phase 2 parameters that the Cisco ASAv VPN requires to create a VPN tunnel with {{site.data.keyword.vpn_vpc_short}}.

Create an IKE version 2 proposal object. IKEv2 proposal objects contain the parameters that are required for creating IKEv2 proposals when you define remote access and site-to-site VPN policies. IKE is a key management protocol that facilitates the management of IPsec-based communications. It is used to authenticate IPsec peers, negotiate and distribute IPsec encryption keys, and automatically establish IPsec SAs.

In this block, the following parameters are set as an example. You can choose other parameters according to your company's security policy; however, make sure to use identical parameters on IBM VPN gateway and ASAv.

* **Encryption algorithm** - Set to `aes-256` for this example.
* **Integrity algorithm** - Set to `sha256` for this example.
* **Diffie-Hellman group** - IPsec uses the Diffie-Hellman algorithm to generate the initial encryption key between the peers. In this example, it is set to group `19`.
* **Pseudo-Random Function (PRF)** - IKEv2 requires a separate method that is used as the algorithm to derive keying material and hashing operations that are required for the IKEv2 tunnel encryption. This is referred to as the pseudo-random function and is set to `sha256`.
* **SA Lifetime** - Set the lifetime of the security associations (after which time a reconnection occurs) to `86400` seconds.

```sh
crypto ikev2 policy 100
 encryption aes-256
 integrity sha256
 group 19
 prf sha256
 lifetime seconds 86400
crypto ikev2 enable outside
```
{: codeblock}

Create an IPsec profile for the virtual tunnel interface (VTI). The profile references the IPsec proposal, and the VTI references the profile. Make sure IBM VPN gateway and ASAv use identical IPsec proposal and IPsec profile parameters.

```sh
crypto ipsec ikev2 ipsec-proposal ibm-ipsec-proposal
 protocol esp encryption aes-256
 protocol esp integrity sha-256
crypto ipsec profile ibm-ipsec-profile
 set ikev2 ipsec-proposal ibm-ipsec-proposal
 set pfs group19
 set security-association lifetime kilobytes unlimited
 set security-association lifetime seconds 3600
 responder-only
```
{: codeblock}

Create the tunnel group to IBM primary tunnel. The peer address `169.59.210.199` is the small public IP of the IBM route-based VPN gateway, and the pre-shared key should be same as the IBM route-based VPN gateway. For more information about the small public IP, see this [important notice](/docs/vpc?topic=vpc-using-vpn#important-notice).

```sh
tunnel-group 169.59.210.199 type ipsec-l2l
tunnel-group 169.59.210.199 ipsec-attributes
 ikev2 remote-authentication pre-shared-key <your-pre-shared-key>
 ikev2 local-authentication pre-shared-key <your-pre-shared-key>
```
{: codeblock}

Create the virtual tunnel interface and configure the link-local address (`169.254.0.2/30`) on the interface. Be careful to choose the link-local address and make sure that it is not overlapping with other addresses on the device. There are two available IP addresses (`169.254.0.1` and `169.254.0.2`) in a subnet with a 30-bit netmask. The first IP address `169.254.0.1` is used as the IBM VPN gateway VTI address; the second, `169.254.0.2`, is used as the ASAv VTI address. If you have more than one VTI on the ASAv, you can choose another link-local subnet, such as `169.254.0.4/30`, `169.254.0.8/30`, and so on.

You do not need to configure `169.254.0.1` on the IBM VPN gateway. It is referenced only when you configure the routes on the ASAv.
{: note}

```sh
interface Tunnel1
 nameif ibm-gateway-primary-tunnel
 no shutdown
 ip address 169.254.0.2 255.255.255.252
 tunnel source interface outside
 tunnel destination 169.59.210.199
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile ibm-ipsec-profile
!
```
{: codeblock}

Add a route to the ASAv. The destination `10.240.65.0` is the IBM VPC subnet, and the next hop is the VTI address of the IBM VPN gateway. The route distance is `1`.

```sh
route ibm-gateway-primary-tunnel 10.240.65.0 255.255.255.0 169.254.0.1 1
```
{: codeblock}

Create the tunnel group to the IBM secondary tunnel. The peer address `169.59.210.200` is the large public IP of the IBM route-based VPN gateway, and the pre-shared key should be same as the IBM route-based VPN gateway. For more information about the large public IP, see this [important notice](/docs/vpc?topic=vpc-using-vpn#important-notice).

```sh
tunnel-group 169.59.210.200 type ipsec-l2l
tunnel-group 169.59.210.200 ipsec-attributes
 ikev2 remote-authentication pre-shared-key <your-pre-shared-key>
 ikev2 local-authentication pre-shared-key <your-pre-shared-key>
!
```
{: codeblock}

Create the virtual tunnel interface and configure the link-local address (`169.254.0.6/30`) on the interface. Be careful to choose the link-local address and make sure that it is not overlapping with other addresses on the device. There are two available IP addresses (`169.254.0.5` and `169.254.0.6`) in a subnet with a 30-bit netmask. The first IP address `169.254.0.5` is used as the IBM VPN gateway VTI address; the second, `169.254.0.6` is used as the ASAv VTI address. If you have more than one VTI on the ASAv, you can choose another link-local subnet, such as `169.254.0.0/30`, `169.254.0.8/30`, and so on.

You do not need to configure `169.254.0.5` on the IBM VPN gateway. It is referenced only when you configure the routes on the ASAv.
{: note}

```sh
interface Tunnel2
 nameif ibm-gateway-secondary-tunnel
 no shutdown
 ip address 169.254.0.6 255.255.255.252
 tunnel source interface outside
 tunnel destination 169.59.210.200
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile ibm-ipsec-profile
!
```
{: codeblock}

Add a route to the ASAv. The destination `10.240.65.0` is the IBM VPC subnet, and the next hop is the secondary VTI address of the IBM VPN gateway. The route distance is `10`.

```sh
route ibm-gateway-secondary-tunnel 10.240.65.0 255.255.255.0 169.254.0.5 10
```
{: codeblock}

Configure TCP MSS clamping on ASAv to avoid unnecessary fragmentation:

```sh
sysopt connection tcpmss 1360
```
{: codeblock}
