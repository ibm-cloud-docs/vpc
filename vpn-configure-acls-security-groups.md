---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-14"

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

Access control lists (ACLs) can be configured on the VPN gateway's subnet, which is where the VPN gateway is deployed, and other VPC subnets that communicate over the VPN tunnel. 

If you configure ACLs on the VPN gateway's subnet, make sure that the following rules are in place to allow management traffic and VPN tunnel traffic. For more information, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port | 
|--------------|------|------|------|------|------------------|
| Inbound | All | Peer gateway public IP[^IP] | N/A | VPN gateway's subnet | N/A
| Outbound | All  | VPN gateway's subnet | N/A | Peer gateway public IP[^IP2] | N/A
| Inbound | All | On-premises, private subnet | N/A | VPC subnet | N/A
| Outbound | All  | VPC subnet | N/A | On-premises, private subnet | N/A
| Inbound (optional) | ICMP | Any | N/A | Any | N/A
{: caption="Table 1. Inbound and outbound rules on VPN gateway's subnet" caption-side="top"}

If you use ACLs or security groups on the VPC subnets that communicate over the VPN tunnel, make sure that ACL or security group rules are in place to allow traffic between virtual server instances in your VPC and the other network.
{: important}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port | 
|--------------|------|------|------|------|------------------|
| Inbound | All | On-premises, private subnet | N/A | VPC subnet | N/A
| Outbound | All | VPC subnet | N/A | On-premises, private subnet | N/A
{: caption="Table 2. Inbound and outbound rules on VPC subnets" caption-side="top"}

[^IP]:Set the source IP to the peer gateway public IP address. This allows traffic from the VPC and the on-premises subnets. 

[^IP2]:Set the source IP to the peer gateway public IP address. This allows traffic from the VPC and the on-premises subnets. 

