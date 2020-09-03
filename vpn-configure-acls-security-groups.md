---

copyright:
  years: 2020
lastupdated: "2020-08-14"

keywords:  VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway, ACLs, security groups

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

If you configure access control lists (ACLs) on the subnet in which the VPN gateway is deployed, make sure that the following rules are in place to allow management traffic and VPN tunnel traffic:

* **Inbound rules**
    - Allow protocol TCP source port 10514
    - Allow protocol TCP source port 443
    - Allow protocol TCP source port 80
    - Allow protocol TCP source port 53
    - Allow protocol UDP source port 53
    - Allow protocol ALL source IP is VPN peer gateway public IP
    - Allow protocol TCP destination port 443
    - Allow protocol TCP destination port 56500
    - Allow traffic between virtual server instances in your VPC and your on-premises private network
    - Allow ICMP traffic

* **Outbound rules**
   - Allow all traffic

If you use ACLs or security groups on the subnets that need to communicate over the VPN tunnel, make sure that ACL rules or security group rules are in place to allow traffic between virtual server instances in your VPC and the other network.
