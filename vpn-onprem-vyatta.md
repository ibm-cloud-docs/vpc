---

copyright:
  years: 2020
lastupdated: "2020-11-13"

keywords: vyatta peer, vra peer, vpn vyatta

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

# Connecting to a Vyatta peer
{: #vyatta-config}

You can use {{site.data.keyword.cloud}} VPN for VPC to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your Vyatta VPN gateway to connect to VPN for VPC.
{: shortdesc}

These instructions are based on Vyatta version: AT&T vRouter 5600 1801d.

When the Vyatta VPN receives a connection request from VPN for VPC, Vyatta uses IPsec Phase 1 parameters to establish a secure connection and authenticate the VPN for VPC gateway. Then, if the security policy permits the connection, the Vyatta VPN establishes the tunnel by using IPsec Phase 2 parameters and applies the IPsec security policy. Key management, authentication, and security services are negotiated dynamically through the IKE protocol.

To support these functions, the following general configuration steps must be performed on the Vyatta VPN:

* Define the Phase 1 parameters that the Vyatta requires to authenticate VPN for VPC and establish a secure connection.
* Define the Phase 2 parameters that the Vyatta requires to create a VPN tunnel with VPN for VPC.

## Connecting an IBM policy-based VPN to a Vyatta peer
{: #vyatta-config-policy-based}

Use the following configuration:

1. Choose `IKEv2` in authentication.
1. Enable `DH-group 2` in the Phase 1 proposal.
1. Set `lifetime = 36000` in the Phase 1 proposal.
1. Disable PFS in the Phase 2 proposal.
1. Set `lifetime = 10800` in the Phase 2 proposal.
1. Input your peers and subnets information in the Phase 2 proposal.

The following commands use the following variables where:

* `{{ peer_address }}` is the VPN gateway public IP address.
* `{{ peer_cidr }}` is the VPN gateway subnet.
* `{{ vyatta_address }}` is the Vyatta public IP address.
* `{{ vyatta_cidr }}` is the Vyatta subnet.

### Before you begin
{: #vyatta-preparation}

To set up your remote Vyatta peer, make sure that the following prerequisites are created.

* A VPC
* A subnet in the VPC
* A VPN gateway in the VPC without connections

   After the VPN gateway gets provisioned, note its public IP address.
   {: tip}
* The Vyatta public IP address
* The Vyatta subnet that you want to connect using a VPN

### Configuring the Vyatta
{: #configure-policy-based-vyatta-peer}

There are two ways you can run the configuration on your Vyatta:

1. Log in to the Vyatta and run the file `create_vpn.vcli` using the commands that follow.
1. Run the following commands in the Vyatta console.

Remember to:
* Choose `IKEv2` in authentication
* Enable `DH-group 2`
* Set `lifetime = 36000`
* Disable PFS
* Set `lifetime = 10800`  

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

Finally, make note of your `{{ psk }}` value, as you will need it to set up the VPN connection in the next step.

{: codeblock}


## Troubleshooting
{: #vyatta-troubleshooting}

Remember that:

* If you enable CPP firewall on Vyatta, you must configure the rules to allow traffic from IBM gateway. For example, if your CPP firewall name is `GATEWAY_CPP`, add these rules to the firewall:

    ```
    # set security firewall name GATEWAY_CPP rule 250 source address 169.61.247.167
    # set security firewall name GATEWAY_CPP rule 250 action accept
    ```
    {: codeblock}

* If you are applying the firewall to the interface, you must permit the traffic from IBM VPC. For example:

    ```
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
