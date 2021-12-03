---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-09"

keywords:  VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring ACLs and security groups for use with VPN
{: #acls-security-groups-vpn}

Access control lists (ACLs) and security groups can be configured on the VPN gateway's subnet where the VPN gateway is deployed, and other VPC subnets that communicate over the VPN tunnel.

If you configure ACLs or security groups on the VPN gateway's subnet, make sure that the following rules are in place to allow management traffic and VPN tunnel traffic. For more information, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls) and [About security groups](/docs/vpc?topic=vpc-using-security-groups).

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Peer gateway public IP[^IP] | N/A | VPN gateway's subnet | N/A
| Outbound | All  | VPN gateway's subnet | N/A | Peer gateway public IP[^IP2] | N/A
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A
| Outbound | All  | VPC CIDR | N/A | On-premises, private CIDR | N/A
| Inbound | All  | VPC CIDR | N/A | On-premises, private CIDR | N/A
| Outbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A
| Inbound (optional) | ICMP | Any | N/A | Any | N/A
{: caption="Table 1. Inbound and outbound rules on VPN gateway's subnet" caption-side="bottom"}

If you use ACLs or security groups on the VPC subnets that communicate over the VPN tunnel, make sure that ACL or security group rules are in place to allow traffic between virtual server instances in your VPC and the other network.
{: important}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A
{: caption="Table 2. Inbound and outbound rules on VPC subnets" caption-side="bottom"}

[^IP]:Set the source IP to the peer gateway public IP address. This allows traffic from the VPC and the on-premises subnets.

[^IP2]:Set the source IP to the peer gateway public IP address. This allows traffic from the VPC and the on-premises subnets.
