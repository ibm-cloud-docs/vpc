---

copyright:
  years: 2020, 2025
lastupdated: "2025-05-05"

keywords: VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring ACLs for use with VPN
{: #configuring-acls-vpn}

A network access control list (NACL) is a built-in, virtual firewall, similar to a security group. In contrast to security groups, ACL rules control traffic to and from the _subnets_, rather than to and from the _instances_.

You can set up ACLs on the VPN gateway subnet and other VPC subnets that communicate over the VPN tunnel.

The VPN gateway and the VPC virtual server instance can share common or different NACLs and can be in common or different subnet CIDRs.
{: note}

## Case 1 : When the VPN gateway and virtual server instance are in a common NACL
{: #case-1-cmn-acl}

### Scenario 1: The VPN gateway and virtual server instance are in a common subnet
{: #vpn-vsi-cmn-subnet}

The following diagram illustrates the packet flow through the common NACL subnet pair.

   ![Packet flow through VPC NACL](images/vpc-traffic-flow-same-subnet.svg){: caption="Packet flow through the common NACL subnet" caption-side="bottom"}

1. Encrypted traffic flows between your on-premises (peer) gateway and the VPN gateway subnet, covering IP ranges from both sides, which are a part of the encrypted domain (On-premises private CIDR, VPC CIDR).
1. After the packet reaches the VPC VPN gateway, it is decrypted and forwarded from the VPN subnet to the VPC virtual server subnet.
1. The response packets to your on-premises network travel back to the VPN subnet.
1. Finally, the packets are encrypted and returned to the on-premises gateway from the VPN subnet.

When the VPN gateway and virtual server instance are in the common subnet and you create a common NACL, you must add the following rules for bidirectional traffic flow between your on-premises gateway and the common subnet NACL pair. For more information, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).
{: important}

1. The first pair of inbound and outbound rules in the table allow management traffic. This traffic uses IKE and IPsec protocols for establishing and maintaining the VPN connection between your on-premises gateway and the VPN gateway.

1. The second pair of inbound and outbound rules allow VPN tunnel traffic, which flows between your on-premises network and the VPC CIDR through the established VPN tunnel.

1. The last inbound rule allows traffic for connectivity tests, such as pinging the VPN gateway or VPC virtual server instance for reachability checks and troubleshooting. (Optional)

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP | N/A | VPN gateway's subnet | N/A |
| Outbound | All  | VPN gateway's subnet | N/A | On-premises gateway public IP | N/A |
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on the common NACL and common subnet" caption-side="bottom"}

### Scenario 1: Example
{: #example-cmn-nacl-cmn-subnet}

The following table shows the source and destination IP addresses for inbound and outbound rules. In this example, both the VPN gateway and the virtual server instance are in the common subnet CIDR 192.168.1.0/24.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP[^IP] | N/A | 192.168.1.0/24 | N/A |
| Outbound | All  | 192.168.1.0/24 | N/A | On-premises gateway public IP[^IP2] | N/A |
| Inbound | All | On-premises, private CIDR | N/A | 192.168.1.0/24 | N/A |
| Outbound | All | 192.168.1.0/24 | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on the common NACL and common subnet example" caption-side="bottom"}

[^IP]: Set the source IP to your on-premises gateway public IP for the inbound rule. This setting allows traffic from the on-premises subnet to the VPC.

[^IP2]: Set the destination IP to your on-premises gateway public IP address for the outbound rule. This setting allows traffic from the VPC to the on-premises subnet.

### Scenario 2: The VPN gateway and virtual server instance are in different subnets
{: #vpn-vsi-diff-subnet}

When the VPN gateway and virtual server instance are in different subnets and you create a common NACL, you must add the following rules for bidirectional traffic flow between your on-premises gateway and the different subnets.
{: important}

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP | N/A | VPN gateway's subnet | N/A |
| Outbound | All  | VPN gateway's subnet | N/A | On-premises gateway public IP | N/A |
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on the common NACL and different subnet" caption-side="bottom"}

### Scenario 2: Example
{: #example-cmn-nacl-diff-subnet}

The following table shows the source and destination IP addresses for inbound and outbound rules. In this example, the VPN gateway is in the subnet CIDR 192.168.1.0/24 and the virtual server instance is in the subnet CIDR 192.168.2.0/24.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP[^IP3] | N/A | 192.168.1.0/24 | N/A |
| Outbound | All  | 192.168.1.0/24 | N/A | On-premises gateway public IP[^IP4] | N/A |
| Inbound | All | On-premises, private CIDR | N/A | 192.168.2.0/24 | N/A |
| Outbound | All | 192.168.2.0/24 | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on the common NACL and different subnet example" caption-side="bottom"}

[^IP3]: Set the source IP to your on-premises gateway public IP for the inbound rule. This setting allows traffic from the on-premises subnet to the VPC.

[^IP4]: Set the destination IP to your on-premises gateway public IP address for the outbound rule. This setting allows traffic from the VPC to the on-premises subnet.

The following diagram illustrates the packet flow through the NACL with different subnets.

   ![Packet flow through VPC NACL](images/vpc-traffic-flow-diff-subnet.svg){: caption="Packet flow through the NACL with different subnets" caption-side="bottom"}

## Case 2 : When the VPN gateway and virtual server instance are in different NACLs
{: #case-2-diff-acl}

### Scenario 1: The VPN gateway and virtual server instance are in different subnets
{: #vpn-vsi-cmn-vpc-diff-subnet}

When the VPN gateway and virtual server instance are in different subnets and you create two NACLs: one attached to the VPN gateway subnet, and the other attached to the virtual server subnet, you must add the following rules for traffic flow between your on-premises gateway and the different subnets.
{: important}

#### NACL configuration for VPN gateway subnet
{: #nacl-vpn-gateway}

This NACL will be attached to the VPN gateway subnet. The traffic rules for the VPN gateway subnet should cover the management traffic used to set up the VPN tunnel and the encrypted VPN tunnel traffic between your on-premises network and the VPC.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP | N/A | VPN gateway's subnet | N/A |
| Outbound | All  | VPN gateway's subnet | N/A | On-premises gateway public IP | N/A |
| Inbound| All  | On-premises private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on VPN gateway's subnet" caption-side="bottom"}

#### NACL configuration for virtual server instance subnet
{: #nacl-virtual-server}

This NACL will be attached to the virtual server subnet. The traffic rules for the virtual server subnet should cover VPN tunnel traffic for communication between your on-premises network and the virtual server instance.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
{: caption="Inbound and outbound rules on virtual server subnets" caption-side="bottom"}

#### Troubleshooting traffic
{: #traffic-troubleshooting}

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Rules for traffic troubleshooting" caption-side="bottom"}

### Scenario 1: Example
{: #example-diff-subnet}

The following table shows the source and destination IP addresses for inbound and outbound rules. In this example, the VPN gateway is in subnet CIDR 192.168.1.0/24, and the virtual server is in subnet CIDR 192.168.2.0/24

#### VPN gateway subnet NACL
{: #example-vpn-gateway-nacl}

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP[^IP5] | N/A | 192.168.1.0/24 | N/A |
| Outbound | All  | 192.168.1.0/24 | N/A | On-premises gateway public IP[^IP6] | N/A |
| Inbound | All | On-premises, private CIDR | N/A | 192.168.2.0/24 | N/A |
| Outbound | All | 192.168.2.0/24| N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on VPN gateway's subnet example" caption-side="bottom"}

[^IP5]: Set the source IP to your on-premises gateway public IP for the inbound rule. This setting allows traffic from the on-premises subnet to the VPC.

[^IP6]: Set the destination IP to your on-premises gateway public IP address for the outbound rule. This setting allows traffic from the VPC to the on-premises subnet.

#### Virtual server subnet NACL
{: #example-virtual-server-nacl}

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | On-premises, private CIDR | N/A | 192.168.2.0/24 | N/A |
| Outbound | All | 192.168.2.0/24 | N/A | On-premises, private CIDR | N/A |
{: caption="Inbound and outbound rules on virtual server subnet example" caption-side="bottom"}

The following diagram illustrates the packet flow for different NACLs with different subnets.

 ![Packet flow through different VPC NACLs](images/vpc-traffic-flow-diff-acls-subnets.svg){: caption="Packet flow through different NACLs with different subnets" caption-side="bottom"}

### Scenario 2: The VPN gateway and virtual server instance are in different VPCs connected through a transit gateway
{: #vpn-vsi-diff-vpc-tgw}

When the VPN gateway and virtual server instance are in different VPCs with different subnets and different NACLs, you must add the following rules for traffic flow between your on-premises gateway and the subnets in different VPCs.
{: important}

#### NACL configuration for VPN gateway subnet
{: #nacl-vpn-gateway}

This NACL will be attached to the VPN gateway subnet. The traffic rules for the VPN gateway subnet should cover the management traffic used to set up the VPN tunnel and the encrypted VPN tunnel traffic between your on-premises network and the VPC.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP | N/A | VPN gateway's subnet | N/A |
| Outbound | All  | VPN gateway's subnet | N/A | On-premises gateway public IP | N/A |
| Inbound| All  | On-premises private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on VPN gateway's subnet" caption-side="bottom"}

#### NACL configuration for virtual server instance subnet
{: #nacl-virtual-server}

This NACL will be attached to the virtual server subnet. The traffic rules for the virtual server subnet should cover VPN tunnel traffic for communication between your on-premises network and the virtual server instance.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
{: caption="Inbound and outbound rules on virtual server subnets" caption-side="bottom"}

#### Troubleshooting traffic
{: #traffic-troubleshooting}

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Rules for traffic troubleshooting" caption-side="bottom"}

### Scenario 2 : Example
{: #example-diff-subnet-diff-vpc}

The following table shows the source and destination IP addresses for inbound and outbound rules. In this example, the VPN gateway in VPC A is in the subnet CIDR 192.168.1.0/24, and the virtual server in VPC B is in the subnet CIDR 192.168.2.0/24

#### VPN gateway subnet NACL
{: #example-vpn-gateway-nacl-tgw}

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP[^IP7] | N/A | 192.168.1.0/24 | N/A |
| Outbound | All  | 192.168.1.0/24 | N/A | On-premises gateway public IP[^IP8] | N/A |
| Inbound | All | On-premises, private CIDR | N/A | 192.168.2.0/24 | N/A |
| Outbound | All | 192.168.2.0/24| N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on VPN gateway's subnet example" caption-side="bottom"}

[^IP7]: Set the source IP to your on-premises gateway public IP for the inbound rule. This setting allows traffic from the on-premises subnet to the VPC.

[^IP8]: Set the destination IP to your on-premises gateway public IP address for the outbound rule. This setting allows traffic from the VPC to the on-premises subnet.

#### Virtual server subnet NACL
{: #example-virtual-server-nacl-tgw}

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | On-premises, private CIDR | N/A | 192.168.2.0/24 | N/A |
| Outbound | All | 192.168.2.0/24 | N/A | On-premises, private CIDR | N/A |
{: caption="Inbound and outbound rules on virtual server subnet example" caption-side="bottom"}

The following diagram illustrates the packet flow through NACLs with different subnets in different VPCs

 ![Packet flow through different VPC NACLs](images/vpc-traffic-flow-diff-vpc-acls-subnets.svg){: caption="Packet flow through different VPCs with different NACLs and subnets" caption-side="bottom"}
