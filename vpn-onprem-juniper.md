---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-07"

keywords: juniper, juniper peer, vSRX peer

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting a Juniper vSRX peer
{: #juniper-vsrx-config}

You can use IBM Cloud VPN for VPC to securely connect your VPC to an on-prem network through a VPN tunnel. This topic provides guidance on how to configure your Juniper VPN gateway to connect to VPN for VPC.
{: shortdesc}

If Juniper vSRX requires Perfect Forward Secrecy (PFS) to be enabled in Phase 2, you need to create a custom IPsec policy to replace the default policy for the VPN in your VPC. For more information, see [Creating a custom IPsec policy for Juniper vSRX](#custom-ipsec-policy-with-vsrx).
{: important}

These instructions are based on Juniper vSRX, JUNOS Software Release [23.2R1-S1 Standard 23.2.1.1].
{: note}

Read [VPN gateway limitations](/docs/vpc?topic=vpc-vpn-limitations) before you continue to connect to your on-premises peer.
{: tip}

When the Juniper VPN receives a connection request from VPN for VPC, Juniper uses IPsec Phase 1 parameters to establish a secure connection and authenticate the VPN for VPC gateway. Then, if the security policy permits the connection, the Juniper VPN establishes the tunnel using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, you must do the following on the Juniper vSRX unit:

* Define the Phase 1 parameters that the Juniper vSRX VPN requires to authenticate the remote peer and establish a secure connection.
* Define the Phase 2 parameters that the Juniper vSRX VPN requires to create a VPN tunnel with {{site.data.keyword.vpn_vpc_short}}.

General configuration steps are as follows.

1. Choose `IKEv2` in Phase 1.
1. Set up policy-based mode.
1. Enable `DH-group 19` in the Phase 1 proposal.
1. Set `lifetime = 36000` in the Phase 1 proposal.
1. Enable PFS in the Phase 2 proposal.
1. Set `lifetime = 10800` in the Phase 2 proposal.
1. Input your peer and subnet information in the Phase 2 proposal.
1. Allow UDP 500 traffic on the external interface.

## Policy-based configuration for Juniper vSRX
{: #policy-based-setup-vsrx}

Here's an example of how to set up security.

1. Configure an IKE proposal for a policy-based VPN.

   ```sh
   set security ike proposal ibm-vpc-ike-proposal authentication-method pre-shared-keys
   set security ike proposal ibm-vpc-ike-proposal dh-group group19
   set security ike proposal ibm-vpc-ike-proposal authentication-algorithm sha-256
   set security ike proposal ibm-vpc-ike-proposal encryption-algorithm aes-256-cbc
   set security ike proposal ibm-vpc-ike-proposal lifetime-seconds 86400
   set security ike policy ibm-vpc-ike-policy mode main
   set security ike policy ibm-vpc-ike-policy proposals ibm-vpc-ike-proposal
   set security ike policy ibm-vpc-ike-policy pre-shared-key ascii-text <your-psk>
   ```
   {: codeblock}

1. Configure an IKE gateway to a policy-based VPN gateway.

   ```sh
   set security ike gateway ibm-vpc-policy-vpn-gateway ike-policy ibm-vpc-ike-policy
   set security ike gateway ibm-vpc-policy-vpn-gateway address <VPN for VPC Gateway Public IP>
   set security ike gateway ibm-vpc-policy-vpn-gateway dead-peer-detection interval 2
   set security ike gateway ibm-vpc-policy-vpn-gateway dead-peer-detection threshold 3
   set security ike gateway ibm-vpc-policy-vpn-gateway local-identity inet <vSRX Public IP>
   set security ike gateway ibm-vpc-policy-vpn-gateway external-interface ae1.0
   set security ike gateway ibm-vpc-policy-vpn-gateway version v2-only
   ```
   {: codeblock}

1. Configure an IPsec proposal for a policy-based VPN.

   ```sh
   set security ipsec proposal ibm-vpc-ipsec-proposal protocol esp
   set security ipsec proposal ibm-vpc-ipsec-proposal authentication-algorithm hmac-sha-256-128
   set security ipsec proposal ibm-vpc-ipsec-proposal encryption-algorithm aes-256-cbc
   set security ipsec proposal ibm-vpc-ipsec-proposal lifetime-seconds 3600
   set security ipsec policy ibm-vpc-ipsec-policy perfect-forward-secrecy keys group19
   set security ipsec policy ibm-vpc-ipsec-policy proposals ibm-vpc-ipsec-proposal
   ```
   {: codeblock}

1. Configure a VTI and VPN connection to a policy-based VPN gateway.

   ```sh
   set interfaces st0 unit 2 description Tunnel-to-IBM-VPC-POLICY-VPN-GATEWAY
   set interfaces st0 unit 2 family inet

   set security ipsec vpn ibm-vpc-policy-vpn-gateway-vpn bind-interface st0.2
   set security ipsec vpn ibm-vpc-policy-vpn-gateway-vpn ike gateway ibm-vpc-policy-vpn-gateway
   set security ipsec vpn ibm-vpc-policy-vpn-gateway-vpn ike ipsec-policy ibm-vpc-ipsec-policy
   set security ipsec vpn ibm-vpc-policy-vpn-gateway-vpn traffic-selector pair1 local-ip <on-premise-subnet>
   set security ipsec vpn ibm-vpc-policy-vpn-gateway-vpn traffic-selector pair1 remote-ip <vpc-subnet>
   set security ipsec vpn ibm-vpc-policy-vpn-gateway-vpn establish-tunnels immediately
   ```
   {: codeblock}

1. Configure a control plane firewall to permit IKE/IPsec protocol traffic.

   ```sh
   set firewall filter PROTECT-IN term IPSec-IKE from source-address <VPN for VPC Gateway Public IP>/32
   set firewall filter PROTECT-IN term IPSec-IKE from protocol udp
   set firewall filter PROTECT-IN term IPSec-IKE from port 500
   set firewall filter PROTECT-IN term IPSec-IKE then accept
   set firewall filter PROTECT-IN term IPSec-ESP from source-address <VPN for VPC Gateway Public IP>/32
   set firewall filter PROTECT-IN term IPSec-ESP from protocol esp
   set firewall filter PROTECT-IN term IPSec-ESP then accept
   set firewall filter PROTECT-IN term IPSec-4500 from source-address <VPN for VPC Gateway Public IP>/32
   set firewall filter PROTECT-IN term IPSec-4500 from protocol udp
   set firewall filter PROTECT-IN term IPSec-4500 from port 4500
   set firewall filter PROTECT-IN term IPSec-4500 then accept
   ```
   {: codeblock}

1. Configure a data plane firewall to allow traffic between on-premises and IBM VPC.

   ```sh
   set security zones security-zone vpn-zone interfaces st0.2
   set security policies from-zone SL-PRIVATE to-zone vpn-zone policy private_to_vpn match source-address any
   set security policies from-zone SL-PRIVATE to-zone vpn-zone policy private_to_vpn match destination-address any
   set security policies from-zone SL-PRIVATE to-zone vpn-zone policy private_to_vpn match application any
   set security policies from-zone SL-PRIVATE to-zone vpn-zone policy private_to_vpn then permit
   set security policies from-zone vpn-zone to-zone SL-PRIVATE policy vpn_to_private match source-address any
   set security policies from-zone vpn-zone to-zone SL-PRIVATE policy vpn_to_private match destination-address any
   set security policies from-zone vpn-zone to-zone SL-PRIVATE policy vpn_to_private match application any
   set security policies from-zone vpn-zone to-zone SL-PRIVATE policy vpn_to_private then permit
   ```
   {: codeblock}

1. Configure TCP MSS clamping on vSRX to avoid unnecessary fragmentation.

   ```sh
   set security flow tcp-mss ipsec-vpn mss 1360
   ```
   {: codeblock}

1. After the configuration file finishes running, you can check the connection status from the CLI using the following command:

   ```sh
   run show security ipsec security-associations
   ```
   {: pre}

## Creating a custom IPsec policy for Juniper vSRX
{: #custom-ipsec-policy-with-vsrx}

By default, {{site.data.keyword.vpn_vpc_short}} disables PFS in Phase 2. If Juniper vSRX requires PFS to be enabled in Phase 2, you need to create a custom IPsec policy to replace the default policy for the VPN in your VPC.

To use a custom IPsec policy in {{site.data.keyword.vpn_vpc_short}}, follow these steps:

1. On the {{site.data.keyword.vpn_vpc_short}} page in the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **IPsec policies** tab.

1. Click **New IPsec policy** and specify the following values:

   * For the **Authentication** field, select **sha256**.
   * For the **Encryption** field, select **aes256**.
   * Select the **PFS** option.
   * For the **DH Group** field, select **19**.
   * For the **Key lifetime** field, specify **3600**.

1. When you create the VPN connection in your VPC, select this custom IPsec policy.

## Setting up a route-based configuration for Juniper vSRX
{: #route-based-setup-vsrx}

The following configuration shows how to set up two route-based tunnels between the Juniper vSRX VPN and VPN for VPC using a weighted preference for two tunnels.

The VPN for VPC gateway should have a connection where the peer address is the vSRX public IP.
{: note}

Here's an example to set the vSRX configuration.

1. Configure an IKE proposal for a route-based VPN:

   ```sh
   set security ike proposal ibm-vpc-ike-proposal authentication-method pre-shared-keys
   set security ike proposal ibm-vpc-ike-proposal dh-group group19
   set security ike proposal ibm-vpc-ike-proposal authentication-algorithm sha-256
   set security ike proposal ibm-vpc-ike-proposal encryption-algorithm aes-256-cbc
   set security ike proposal ibm-vpc-ike-proposal lifetime-seconds 86400
   set security ike policy ibm-vpc-ike-policy mode main
   set security ike policy ibm-vpc-ike-policy proposals ibm-vpc-ike-proposal
   set security ike policy ibm-vpc-ike-policy pre-shared-key ascii-text <your-psk>
   ```
   {: codeblock}

1. Configure an IKE gateway to the primary tunnel:

   ```sh
   set security ike gateway ibm-vpc-gateway-primary ike-policy ibm-vpc-ike-policy
   set security ike gateway ibm-vpc-gateway-primary address <VPN for VPC Gateway Small Public IP>
   set security ike gateway ibm-vpc-gateway-primary dead-peer-detection interval 2
   set security ike gateway ibm-vpc-gateway-primary dead-peer-detection threshold 3
   set security ike gateway ibm-vpc-gateway-primary local-identity inet <vSRX Public IP>
   set security ike gateway ibm-vpc-gateway-primary external-interface ae1.0
   set security ike gateway ibm-vpc-gateway-primary version v2-only
   ```
   {: codeblock}

1. Configure an IPsec proposal for a route-based VPN:

   ```sh
   set security ipsec proposal ibm-vpc-ipsec-proposal protocol esp
   set security ipsec proposal ibm-vpc-ipsec-proposal authentication-algorithm hmac-sha-256-128
   set security ipsec proposal ibm-vpc-ipsec-proposal encryption-algorithm aes-256-cbc
   set security ipsec proposal ibm-vpc-ipsec-proposal lifetime-seconds 3600
   set security ipsec policy ibm-vpc-ipsec-policy perfect-forward-secrecy keys group19
   set security ipsec policy ibm-vpc-ipsec-policy proposals ibm-vpc-ipsec-proposal
   ```
   {: codeblock}

1. Configure a VTI and VPN connection to the primary VPN tunnel:

   Create the virtual tunnel interface and configure the link-local address (`169.254.0.2/30`) on the interface. Be careful to choose the link-local address and make sure that it is not overlapping with other addresses on the device. There are two available IP addresses (`169.254.0.1` and `169.254.0.2`) in a subnet with a 30-bit netmask. The first IP address `169.254.0.1` is used as the IBM VPN gateway VTI address; the second, `169.254.0.2`, is used as the vSRX VTI address. If you have more than one VTI on the vSRX, you can choose another link-local subnet, such as `169.254.0.4/30`, `169.254.0.8/30`, and so on.

   You do not need to configure `169.254.0.1` on the IBM VPN gateway. It is referenced only when you configure the routes on the vSRX.
   {: note}
   
   ```sh
   set interfaces st0 unit 2 multipoint
   set interfaces st0 unit 2 family inet next-hop-tunnel 169.254.0.1 ipsec-vpn ibm-vpc-gateway-primary-vpn
   set interfaces st0 unit 2 family inet address 169.254.0.2/30

   set security ipsec vpn ibm-vpc-gateway-primary-vpn bind-interface st0.2
   set security ipsec vpn ibm-vpc-gateway-primary-vpn ike gateway ibm-vpc-gateway-primary
   set security ipsec vpn ibm-vpc-gateway-primary-vpn ike ipsec-policy ibm-vpc-ipsec-policy
   set security ipsec vpn ibm-vpc-gateway-primary-vpn establish-tunnels immediately
   ```
   {: codeblock}

1. Configure a route to the primary VPN tunnel:

   ```sh
   set routing-options static route <your-VPC-subnet> next-hop 169.254.0.1
   ```
   {: pre}

1. Configure a control plane firewall to permit IKE/IPsec protocol traffic for a route-based VPN:

   ```sh
   set firewall filter PROTECT-IN term IPSec-IKE from source-address <VPN for VPC Gateway Small Public IP>/32
   set firewall filter PROTECT-IN term IPSec-IKE from protocol udp
   set firewall filter PROTECT-IN term IPSec-IKE from port 500
   set firewall filter PROTECT-IN term IPSec-IKE then accept
   set firewall filter PROTECT-IN term IPSec-ESP from source-address <VPN for VPC Gateway Small Public IP>/32
   set firewall filter PROTECT-IN term IPSec-ESP from protocol esp
   set firewall filter PROTECT-IN term IPSec-ESP then accept
   set firewall filter PROTECT-IN term IPSec-4500 from source-address <VPN for VPC Gateway Small Public IP>/32
   set firewall filter PROTECT-IN term IPSec-4500 from protocol udp
   set firewall filter PROTECT-IN term IPSec-4500 from port 4500
   set firewall filter PROTECT-IN term IPSec-4500 then accept
   ```
   {: codeblock}

1. Configure a data plane firewall to allow traffic between on-prem and IBM VPC for a route-based VPN:

   ```sh
   set security zones security-zone vpn-zone interfaces st0.2
   set security policies from-zone SL-PRIVATE to-zone vpn-zone policy private_to_vpn match source-address any
   set security policies from-zone SL-PRIVATE to-zone vpn-zone policy private_to_vpn match destination-address any
   set security policies from-zone SL-PRIVATE to-zone vpn-zone policy private_to_vpn match application any
   set security policies from-zone SL-PRIVATE to-zone vpn-zone policy private_to_vpn then permit
   set security policies from-zone vpn-zone to-zone SL-PRIVATE policy vpn_to_private match source-address any
   set security policies from-zone vpn-zone to-zone SL-PRIVATE policy vpn_to_private match destination-address any
   set security policies from-zone vpn-zone to-zone SL-PRIVATE policy vpn_to_private match application any
   set security policies from-zone vpn-zone to-zone SL-PRIVATE policy vpn_to_private then permit
   ```
   {: codeblock}

1. Configure TCP MSS clamping on vSRX to avoid unnecessary fragmentation for a route-based VPN:

   ```sh
   set security flow tcp-mss ipsec-vpn mss 1360
   ```
   {: pre}

1. Configure an IKE gateway to the secondary tunnel:

   ```sh
   set security ike gateway ibm-vpc-gateway-secondary ike-policy ibm-vpc-ike-policy
   set security ike gateway ibm-vpc-gateway-secondary address <VPN for VPC Gateway Big Public IP>
   set security ike gateway ibm-vpc-gateway-secondary dead-peer-detection interval 2
   set security ike gateway ibm-vpc-gateway-secondary dead-peer-detection threshold 3
   set security ike gateway ibm-vpc-gateway-secondary local-identity inet <vSRX Public IP>
   set security ike gateway ibm-vpc-gateway-secondary external-interface ae1.0
   set security ike gateway ibm-vpc-gateway-secondary version v2-only
   ```
   {: codeblock}

1. Configure a VTI and VPN connection to the secondary VPN tunnel:

   ```sh
   set interfaces st0 unit 3 multipoint
   set interfaces st0 unit 3 family inet next-hop-tunnel 169.254.0.5 ipsec-vpn ibm-vpc-gateway-secondary-vpn
   set interfaces st0 unit 3 family inet address 169.254.0.6/30

   set security ipsec vpn ibm-vpc-gateway-secondary-vpn bind-interface st0.3

   set security ipsec vpn ibm-vpc-gateway-secondary-vpn ike gateway ibm-vpc-gateway-secondary
   set security ipsec vpn ibm-vpc-gateway-secondary-vpn ike ipsec-policy ibm-vpc-ipsec-policy
   set security ipsec vpn ibm-vpc-gateway-secondary-vpn establish-tunnels immediately
   ```
   {: codeblock}

1. Configure a control plane firewall to permit IKE/IPsec protocol traffic from the secondary tunnel:

   ```sh
   set firewall filter PROTECT-IN term IPSec-IKE from source-address <VPN for VPC Gateway Big Public IP>/32
   set firewall filter PROTECT-IN term IPSec-ESP from source-address <VPN for VPC Gateway Big Public IP>/32
   set firewall filter PROTECT-IN term IPSec-4500 from source-address <VPN for VPC Gateway Big Public IP>/32
   ```
   {: codeblock}

1. Add the VTI into a security zone:

   ```sh
   set security zones security-zone vpn-zone interfaces st0.3
   ```
   {: pre}

1. Add a route to the secondary tunnel:

   ```sh
   set routing-options static route <your-VPC-subnet> qualified-next-hop 169.254.0.5 preference 30
   ```
   {: codeblock}

## Verifying the configuration
{: #verifying-configuration-vpn}

Follow these steps to verify the configuration:

1. Verify IKE Phase 1 is working for both tunnels: `run show security ike sa`

1. Verify IKE Phase 2 is working for both tunnels: `run show security ipsec sa`

1. Show the route: `run show route <static route>`
