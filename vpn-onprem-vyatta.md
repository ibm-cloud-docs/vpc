---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-27"

keywords: vyatta peer

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to a Vyatta peer
{: #vyatta-config}

You can use IBM Cloud VPN for VPC to securely connect your VPC to an on-prem network through a VPN tunnel. This topic provides guidance about how to configure your Vyatta VPN gateway to connect to VPN for VPC.
{: shortdesc}

These instructions are based on Vyatta version: AT&T vRouter 5600 1801d.
{: note}

Read [Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations) before you continue to connect to your on-premises peer.
{: important}

When the Vyatta VPN receives a connection request from VPN for VPC, Vyatta uses IPsec Phase 1 parameters to establish a secure connection and authenticate the {{site.data.keyword.vpn_vpc_short}} gateway. Then, if the security policy permits the connection, the Vyatta VPN establishes the tunnel by using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the Vyatta VPN:

* Define the Phase 1 parameters that the Vyatta requires to authenticate {{site.data.keyword.vpn_vpc_short}} and establish a secure connection.
* Define the Phase 2 parameters that the Vyatta requires to create a VPN tunnel with {{site.data.keyword.vpn_vpc_short}}.

## Connecting an IBM policy-based VPN to a Vyatta peer
{: #vyatta-config-policy-based}

Use the following configuration:

1. Choose `IKEv2` in authentication.
1. Enable `DH-group 19` in the Phase 1 proposal.
1. Set `lifetime = 36000` in the Phase 1 proposal.
1. Disable PFS in the Phase 2 proposal.
1. Set `lifetime = 10800` in the Phase 2 proposal.
1. Input your peers and subnets information in the Phase 2 proposal.

The following commands use the following variables where:

* `{{ peer_address }}` is the VPN gateway public IP address.
* `{{ ibm_vpc_cidr }}` is IBM VPC subnet.
* `{{ vyatta_address }}` is the Vyatta public IP address.
* `{{ vyatta_cidr }}` is the Vyatta subnet.

### Before you begin
{: #vyatta-preparation}

To set up your remote Vyatta peer, make sure that you create the following prerequisites.

* A VPC
* A subnet in the VPC
* A VPN gateway in the VPC without connections

   After the VPN gateway gets provisioned, note its public IP address.
   {: tip}

* The Vyatta public IP address
* The Vyatta subnet that you want to connect using a VPN

### Configuring the Vyatta
{: #configure-policy-based-vyatta-peer}

There are two ways that you can run the configuration on your Vyatta:

1. Log in to the Vyatta and run the file `create_vpn.vcli` using the commands that follow.
1. Run the following commands in the Vyatta console.

Remember to:

* Choose `IKEv2` in authentication.
* Enable `DH-group 19`.
* Set `lifetime = 36000`.
* Disable PFS.
* Set `lifetime = 10800`.

The following commands use the following variables, where:

* `{{ peer_address }}` is the VPN gateway public IP address.
* `{{ ibm_vpc_cidr }}` is the IBM VPC subnet.
* `{{ vyatta_address }}` is the Vyatta public IP address.
* `{{ vyatta_cidr }}` is the Vyatta subnet.

```sh
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
set security vpn ipsec site-to-site peer {{ peer_address }} tunnel {{ ns.tunnel_index }} remote prefix {{ ibm_vpc_cidr }}

commit
end_configure
```
{: codeblock}

For example, you can run the following commands:

```sh
#!/bin/vcli -f
configure

set security vpn ipsec ike-group 169.61.247.167_test_ike
set security vpn ipsec ike-group 169.61.247.167_test_ike dead-peer-detection timeout 120
set security vpn ipsec ike-group 169.61.247.167_test_ike lifetime 36000
set security vpn ipsec ike-group 169.61.247.167_test_ike ike-version 2

set security vpn ipsec ike-group 169.61.247.167_test_ike proposal 1
set security vpn ipsec ike-group 169.61.247.167_test_ike proposal 1 dh-group 19
set security vpn ipsec ike-group 169.61.247.167_test_ike proposal 1 encryption aes256
set security vpn ipsec ike-group 169.61.247.167_test_ike proposal 1 hash sha2_256
set security vpn ipsec esp-group 169.61.247.167_test_ipsec compression disable
set security vpn ipsec esp-group 169.61.247.167_test_ipsec lifetime 10800
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

Finally, make note of your `{{ psk }}` value. You need it to set up the VPN connection in the next step.

## Connecting an IBM route-based VPN to a Vyatta peer
{: #vyatta-config-route-based}

Use the following IKE and IPsec policy to create the VPN connection to Vyatta:

1. Choose `IKEv2` in authentication.
1. Enable `DH-group 19`, `aes256`, `sha256` in the Phase 1 proposal.
1. Set `lifetime = 86400` in the Phase 1 proposal.
1. Enable PFS, `aes256`, and `sha256` in the Phase 2 proposal.
1. Set `lifetime = 10800` in the Phase 2 proposal.

This policy is an example only. You can use any other values that are matched with the Vyatta proposal.
{: note}

### Before you begin
{: #vyatta-route-based-vpn-preparation}

To set up your remote Vyatta peer, make sure that you create the following prerequisites:

* A VPC
* A subnet in the VPC
* A VPN gateway in the VPC without connections

   After the VPN gateway gets provisioned, note its public IP address. The small IP address is the primary IP and the large IP address is the secondary IP.
   {: tip}

* The Vyatta public IP address
* The Vyatta subnet that you want to connect using a VPN

### Configuring the Vyatta
{: #configuring-the-vyatta}

The following commands use the following variables where:

* `{{ primary_peer_address }}` is the route-based VPN gateway small public IP address.
* `{{ ibm_vpc_cidr }}` is the IBM VPC subnet.
* `{{ secondary_peer_address }}` is the route-based VPN gateway large public IP address.
* `{{ vyatta_address }}` is the Vyatta public IP address.

Here's an example of configuring the Vyatta.

1. Define the matched IKE proposal:

   ```sh
   set security vpn ipsec ike-group ibm-vpc-ike-group
   set security vpn ipsec ike-group ibm-vpc-ike-group dead-peer-detection interval 2
   set security vpn ipsec ike-group ibm-vpc-ike-group dead-peer-detection action clear
   set security vpn ipsec ike-group ibm-vpc-ike-group lifetime 86400
   set security vpn ipsec ike-group ibm-vpc-ike-group ike-version 2
   set security vpn ipsec ike-group ibm-vpc-ike-group proposal 1
   set security vpn ipsec ike-group ibm-vpc-ike-group proposal 1 dh-group 19
   set security vpn ipsec ike-group ibm-vpc-ike-group proposal 1 encryption aes256
   set security vpn ipsec ike-group ibm-vpc-ike-group proposal 1 hash sha2_256
   ```
   {: codeblock}

1. Define the matched IPsec proposal:

   ```sh
   set security vpn ipsec esp-group ibm-vpc-ipsec-group compression disable
   set security vpn ipsec esp-group ibm-vpc-ipsec-group lifetime 10800
   set security vpn ipsec esp-group ibm-vpc-ipsec-group mode tunnel
   set security vpn ipsec esp-group ibm-vpc-ipsec-group pfs dh-group19
   set security vpn ipsec esp-group ibm-vpc-ipsec-group proposal 1 encryption aes256
   set security vpn ipsec esp-group ibm-vpc-ipsec-group proposal 1 hash sha2_256
   ```
   {: codeblock}

1. Create the VTI and VPN connection to the IBM primary tunnel:

   Create the virtual tunnel interface and configure the link-local address (`169.254.0.2/30`) on the interface. Be careful to choose the link-local address and make sure that it is not overlapping with other addresses on the device. There are two available IP addresses (`169.254.0.1` and `169.254.0.2`) in a subnet with a 30-bit netmask. The first IP address `169.254.0.1` is used as the IBM VPN gateway VTI address; the second, `169.254.0.2`, is used as the Vyatta VTI address. If you have more than one VTI on the Vyatta, you can choose another link-local subnet, such as `169.254.0.4/30`, `169.254.0.8/30`, and so on.

   You do not need to configure `169.254.0.1` on the IBM VPN gateway. It is referenced only when you configure the routes on the Vyatta.
   {: note}

   ```sh
   set interfaces vti vti1 description "to-IBM-VPN-primary"
   set interfaces vti vti1 address 169.254.0.2/30
   set interfaces vti vti1 ip tcp-mss limit 1360
   set security vpn ipsec site-to-site peer {{ primary_peer_address }} authentication mode pre-shared-secret
   set security vpn ipsec site-to-site peer {{ primary_peer_address }} authentication pre-shared-secret {{your_pre_shared_key}}
   set security vpn ipsec site-to-site peer {{ primary_peer_address }} ike-group ibm-vpc-ike-group
   set security vpn ipsec site-to-site peer {{ primary_peer_address }} default-esp-group ibm-vpc-ipsec-group
   set security vpn ipsec site-to-site peer {{ primary_peer_address }} description "to-IBM-VPN-primary"
   set security vpn ipsec site-to-site peer {{ primary_peer_address }} local-address {{ vyatta_address }}
   set security vpn ipsec site-to-site peer {{ primary_peer_address }} connection-type initiate
   set security vpn ipsec site-to-site peer {{ primary_peer_address }} authentication remote-id {{ primary_peer_address }}
   set security vpn ipsec site-to-site peer {{ primary_peer_address }} vti bind vti1
   ```
   {: codeblock}

1. Create the primary route:

   ```sh
   set protocols static route {{ ibm_vpc_cidr }} next-hop 169.254.0.1 distance 10
   set protocols static route {{ ibm_vpc_cidr }} next-hop 169.254.0.1 interface vti1
   ```
   {: codeblock}

1. Create the VTI and VPN connection to the IBM secondary tunnel:

   ```sh
   set interfaces vti vti2 description "to-IBM-VPN-secondary"
   set interfaces vti vti2 address 169.254.0.6/30
   set interfaces vti vti2 ip tcp-mss limit 1360
   set security vpn ipsec site-to-site peer {{ secondary_peer_address }} authentication mode pre-shared-secret
   set security vpn ipsec site-to-site peer {{ secondary_peer_address }} authentication pre-shared-secret {{your_pre_shared_key}}
   set security vpn ipsec site-to-site peer {{ secondary_peer_address }} ike-group ibm-vpc-ike-group
   set security vpn ipsec site-to-site peer {{ secondary_peer_address }} default-esp-group ibm-vpc-ipsec-group
   set security vpn ipsec site-to-site peer {{ secondary_peer_address }} description "to-IBM-VPN-secondary"
   set security vpn ipsec site-to-site peer {{ secondary_peer_address }} local-address {{ vyatta_address }}
   set security vpn ipsec site-to-site peer {{ secondary_peer_address }} connection-type initiate
   set security vpn ipsec site-to-site peer {{ secondary_peer_address }} authentication remote-id {{ secondary_peer_address }}
   set security vpn ipsec site-to-site peer {{ secondary_peer_address }} vti bind vti2
   ```
   {: codeblock}

   The VTI IP address `169.254.0.6` is an example. You can use any other unused IP address.
   {: tip}

1. Create the primary route:

   ```sh
   set protocols static route {{ ibm_vpc_cidr }} next-hop 169.254.0.5 distance 20
   set protocols static route {{ ibm_vpc_cidr }} next-hop 169.254.0.5 interface vti2
   ```
   {: codeblock}

## Troubleshooting
{: #vyatta-troubleshooting}

* If you enable CPP firewall on Vyatta, you must configure the rules to allow traffic from IBM gateway. For example, if your CPP firewall name is `GATEWAY_CPP`, add these rules to the firewall:

    ```sh
    # set security firewall name GATEWAY_CPP rule 250 source address 169.61.247.167
    # set security firewall name GATEWAY_CPP rule 250 action accept
    ```
    {: codeblock}

* If you are applying the firewall to the interface, you must permit the traffic from IBM VPC. For example:

    ```sh
    # set security firewall name to-vpc rule 20 destination address 10.240.0.0/24
    # set security firewall name to-vpc rule 20 action accept
    # set security firewall name from-vpc rule 20 source address 10.240.0.0/24
    # set security firewall name from-vpc rule 20 action accept
    # set interfaces bonding dp0bond0 vif 862 firewall out to-vpc
    # set interfaces bonding dp0bond0 vif 862 firewall in from-vpc
    ```
    {: codeblock}

    You might need to add other rules according to your network requirement to allow other traffic.

* If you are using a zone firewall with IPsec, see [Setting up an IPsec tunnel that works with zone firewalls](/docs/virtual-router-appliance?topic=virtual-router-appliance-setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls).
