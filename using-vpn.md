---



copyright:
  years: 2019, 2020
lastupdated: "2020-09-03"

keywords: VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway, auto-negotiation

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:important: .important}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# About VPN gateways
{: #using-vpn}
[comment]: # (linked help topic)

You can use the {{site.data.keyword.cloud}} VPN for VPC service to securely connect your virtual private cloud to another private network. You can use VPN to set up an IPsec site-to-site tunnel between your VPC and your on-premises private network or another VPC.
{:shortdesc}

Currently, only policy-based routing is supported.

## Features
{: #vpn-features}

* IKEv1 and IKEv2
* Authentication algorithms: `md5`, `sha1`, `sha256`
* Encryption algorithms: `3des`, `aes128`, `aes256`
* Diffie-Hellman (DH) groups: 2, 5, 14
* IKE negotiation mode: main
* IPsec transform protocol: ESP
* IPsec encapsulation mode: tunnel
* Perfect Forward Secrecy (PFS)
* Dead Peer Detection
* Routing: Policy-based
* Authentication mode: Pre-shared key
* High availability support in active/standby mode only

## APIs available
{: #apis-available}

You can use the following APIs to create and manage VPNs in your VPC. For more information, see the [VPC REST API reference](https://{DomainName}/apidocs/vpc#list-all-ike-policies){: external}.

### VPN gateways and VPN connections
{: #vpn-gateways-and-vpn-connections}

Table 1 describes API methods for VPN gateways and connections.

| Description | API |
|----------------------------|-------------|
| Create a VPN gateway | `POST /vpn_gateways` |
| Retrieve VPN gateways | `GET /vpn_gateways` |
| Retrieve a VPN gateway | `GET /vpn_gateways/ID` |
| Delete a VPN gateway | `DELETE /vpn_gateways/ID` |
| Update a VPN gateway | `PATCH /vpn_gateways/ID` |
| Create a VPN connection | `POST /vpn_gateways/VPN_GATEWAY_ID/connections` |
| Retrieve VPN connections | `GET /vpn_gateways/VPN_GATEWAY_ID/connections` |
| Retrieve a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID` |
| Delete a VPN connection | `DELETE /vpn_gateways/VPN_GATEWAY_ID/connections/ID` |
| Update a VPN connection | `PATCH /vpn_gateways/VPN_GATEWAY_ID/connections/ID` |
| Retrieve all local CIDRs for a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID/local_cidrs` |
| Delete a local CIDR from a VPN connection | `DELETE /vpn_gateways/VPN_GATEWAY_ID/connections/ID/local_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Check if a specific local CIDR exists on a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID/local_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Sets a local CIDR on a VPN connection | `PUT /vpn_gateways/VPN_GATEWAY_ID/connections/ID/local_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Retrieve all peer CIDRs for a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID/peer_cidrs` |
| Delete a peer CIDR from a VPN connection | `DELETE /vpn_gateways/VPN_GATEWAY_ID/connections/ID/peer_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Checks if a specific peer CIDR exists on a VPN connection | `GET /vpn_gateways/VPN_GATEWAY_ID/connections/ID/peer_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
| Sets a peer CIDR on a VPN connection | `PUT /vpn_gateways/VPN_GATEWAY_ID/connections/ID/peer_cidrs/PREFIX_ADDRESS/PREFIX_LENGTH` |
{: caption="Table 1. Description of APIs for VPN gateways and connections" caption-side="top"}

### IKE policies
{: #ike-policies}

| Description | API |
|-----------------------------|--------------|
| Retrieve all IKE policies | `GET /ike_policies` |
| Create an IKE policy | `POST /ike_policies` |
| Delete an IKE policy | `DELETE /ike_policies/ID` |
| Retrieve an IKE policy | `GET /ike_policies/ID` |
| Update an IKE policy | `PATCH /ike_policies/ID` |
| Retrieve all the connections that use the specified IKE policy | `GET /ike_policies/ID/connections` |
{: caption="Table 2. Description of APIs for IKE policies" caption-side="top"}

### IPsec policies
{: #ipsec-policies}

| Description | API |
|---------------------|-------------|
| Retrieve all IPsec policies | `GET /ipsec_policies` |
| Create an IPsec policy | `POST /ipsec_policies` |
| Delete an IPsec policy | `DELETE /ipsec_policies/ID` |
| Retrieve an IPsec policy | `GET /ipsec_policies/ID` |
| Update an IPsec policy | `PATCH /ipsec_policies/ID` |
| Retrieve all the connections that use the specified IPsec policy | `GET /ipsec_policies/ID/connections` |
| Retrieve all the connections that use the specified IKE policy | `GET /ike_policies/ID/connections` |
{: caption="Table 3. Description of APIs for IPsec policies" caption-side="top"}

## Policy auto-negotiation
{: #policy-auto-negotiation}

The use of IKE and IPsec policies to configure a VPN connection is optional. When no policy is selected, default proposals are chosen automatically through a process known as _auto-negotiation_.

Because {{site.data.keyword.cloud_notm}} auto-negotiation uses **IKEv2**, the on-premises device must also use **IKEv2**. Use a customized IKE policy if your on-premises device does not support **IKEv2**.
{: tip}

### IKE auto-negotiation (Phase 1)
{: #ike-auto-negotiation-phase-1}

The following encryption, authentication, and Diffie-Hellman Group options detailed in Table 4 might be used in any combination:

|    | Encryption | Authentication | DH Group |
|----|------------|----------------|----------|
| 1  | aes128 | sha1   | 2  |
| 2  | aes256 | sha256 | 5  |
| 3  | 3des   | md5    | 14 |
{: caption="Table 4. Encryption, authentication, and DH Group options for IPsec auto-negotiation phase 1" caption-side="top"}

### IPsec auto-negotiation (Phase 2)
{: #ipsec-auto-negotiation-phase-2}

The following encryption and authentication options detailed in Table 5 might be used in any combination:

|    | Encryption | Authentication | DH Group |
|----|------------|----------------|----------|
| 1  | aes128 | sha1   | Disabled  |
| 2  | aes256 | sha256 | Disabled  |
| 3  | 3des   | md5    | Disabled  |
{: caption="Table 5. Encryption and authentication options for IPsec auto-negoation phase 2" caption-side="top"}

## Architecture
{: #vpn-architecture-diagram}

The architecture shown in Figure 2 displays the control and data flow of the VPN for VPC service.

![Figure showing VPN for VPC architecture](images/vpn-architecture-diagram.png){: caption="Figure 2: VPN for VPC Architecture " caption-side="top"}

## Related links
{: #vpn-related-links}

These links provide additional information about {{site.data.keyword.cloud_notm}} VPN for VPC.

* [VPN gateways CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpn-clis)
* [VPN gateways API reference](https://{DomainName}/apidocs/vpc#list-ike-policies)
* [Activity Tracker with LogDNA events](/docs/vpc?topic=vpc-at-events#events-vpns)
* [VPN quotas](/docs/vpc?topic=vpc-quotas)
* [FAQs for VPN gateways](/docs/vpc?topic=vpc-faqs-vpn)
