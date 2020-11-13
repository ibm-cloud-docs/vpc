---

copyright:
  years: 2020
lastupdated: "2020-11-13"

keywords:  VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:beta: .beta}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Configuring ACLs and security groups for use with VPN
{: #acls-security-groups-vpn}

If you configure access control lists (ACLs) on the subnet in which the VPN gateway is deployed, make sure that the following rules are in place to allow management traffic and VPN tunnel traffic. For more information, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).

If you use ACLs or security groups on the subnets that communicate over the VPN tunnel, make sure that ACL or security group rules are in place to allow traffic between virtual server instances in your VPC and the other network.
{: important}

| Inbound/Outbound rules | Protocol | Source IP | Source Port | Destination IP | Destination port | 
|--------------|------|------|------|------|------------------|
| Inbound | All | Peer gateway public IP[^IP] | N/A | Any IP | N/A | 
| Inbound | ICMP | Any IP | N/A | Any IP | N/A |
| Inbound | All | VPC subnet | N/A | On-premises, private subnet | N/A |
| Inbound | All | On-premises, private subnet | N/A | VPC subnet | N/A |
| Outbound<br />Allow all traffic | | | | | |
{: caption="Table 1. Inbound and outbound rules" caption-side="top"}

[^IP]:Set the source IP to the peer gateway public IP address. This allows traffic from the VPC and on-premises subnets. 
