---

copyright:
  years: 2020, 2025
lastupdated: "2025-11-11"

keywords: VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring network ACLs for use with VPN
{: #configuring-acls-vpn}

You can set up network access control lists (NACLs) on the VPN gateway subnet and other VPC subnets that communicate over the VPN tunnel.
{: shortdesc}

A NACL is a stateless set of rules that controls incoming and outgoing traffic at the subnet level. Unlike security groups, which filter traffic to and from individual virtual server instances, NACLs manage traffic that flows to and from entire subnets. You can apply network ACL rules to restrict traffic to VPN gateways and virtual server instances placed in specifc subnets. These rules help you control which network entities can establish IPsec tunnel with your on-premises network.

A VPN gateway and a VPC virtual server instance can share the same or different NACLs, and can reside in the same or different subnet CIDR blocks.
{: note}

## Use case 1: VPN gateway and virtual server instance share NACL
{: #case-1-same-nacl}

This use case demonstrates scenarios where the IBM Cloud VPN gateway and the VPC virtual server instance are governed by a shared NACL, enabling consistent traffic control policies. In both scenarios, the VPN gateway and virtual server are part of the same VPC.

### Scenario 1: VPN gateway and virtual server instance are in the same subnet
{: #vpn-vsi-same-subnet}

In this scenario, both the VPN gateway and the virtual server instance reside within the same subnet in the VPC and are protected by a shared NACL. This setup simplifies network control by applying a consistent set of rules to both resources.

These steps describe the packet flow through the shared NACL subnet pair, as illustrated in the following diagram.

1. Encrypted traffic flows between your on-premises (peer) gateway and the shared subnet, covering IP ranges from both sides, which are a part of the encrypted domain (On-premises private CIDR, VPC CIDR).
1. After the packet reaches the VPC VPN gateway, it is decrypted and forwarded to the virtual server instance in the same subnet.
1. The response packets to your on-premises network travel back to the VPN gateway.
1. Finally, the packets are encrypted and returned to the on-premises gateway from the shared subnet.

![Packet flow through VPC NACL](images/vpc-traffic-flow-same-subnet.svg){: caption="Packet flow through the shared NACL subnet" caption-side="bottom"}

When the VPN gateway and virtual server instance are in the shared subnet and you create a shared NACL, you must add the following rules for bidirectional traffic flow between your on-premises gateway and the shared subnet NACL pair. For more information, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).
{: important}

1. The first pair of inbound and outbound rules in the table allow management traffic. This traffic uses IKE and IPsec protocols for establishing and maintaining the VPN connection between your on-premises gateway and the VPN gateway.
1. The second pair of inbound and outbound rules allow VPN tunnel traffic, which flows between your on-premises network and the VPC CIDR through the established VPN tunnel.
1. Optional: The last inbound rule allows traffic for connectivity tests, such as pinging the VPN gateway or VPC virtual server instance for reachability checks and troubleshooting.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP | N/A | VPN gateway's subnet | N/A |
| Outbound | All  | VPN gateway's subnet | N/A | On-premises gateway public IP | N/A |
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on the shared NACL and shared subnet" caption-side="bottom"}

For example, the following table shows the source and destination IP addresses for inbound and outbound rules. In this example, both the VPN gateway and the virtual server instance are in the shared subnet CIDR `192.168.1.0/24`.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | `1.2.3.4`[^IP] | N/A | `192.168.1.0/24`| N/A |
| Outbound | All | `192.168.1.0/24` | N/A | `1.2.3.4`[^IP2] | N/A |
| Inbound | All | `10.0.0.0/8`[^IP3]| N/A | `192.168.1.0/24` | N/A |
| Outbound | All | `192.168.1.0/24` | N/A | `10.0.0.0/8`[^IP4] | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on the shared NACL and shared subnet example" caption-side="bottom"}

[^IP]: This address is your on-premises gateway public IP for the inbound rule. This setting allows traffic from the on-premises subnet to the VPC.

[^IP2]: Set the destination IP to your on-premises gateway public IP address for the outbound rule. This setting allows traffic from the VPC to the on-premises subnet.

[^IP3]: This address is your on-premises, private CIDR.

[^IP4]: This address is your on-premises, private CIDR.

### Scenario 2: VPN gateway and virtual server instance are in different subnets in the same VPC
{: #vpn-vsi-diff-subnet}

In this scenario, the VPN gateway and the virtual server instance reside in different subnets within the same VPC, and a shared NACL is applied to manage traffic between them. This configuration requires addiotnal considerations for traffic routing between the subnets.

These steps describe the packet flow through the shared NACL and different subnets, as illustrated in the following diagram.

1. Encrypted traffic flows between your on-premises (peer) gateway and the VPN gateway subnet, covering IP ranges from both sides, which are a part of the encrypted domain (On-premises private CIDR, VPC CIDR).
1. After the packet reaches the VPC VPN gateway, it is decrypted and forwarded from the VPN subnet to the VPC virtual server subnet.
1. The response packets to your on-premises network travel back to the VPN subnet.
1. Finally, the packets are encrypted and returned to the on-premises gateway from the VPN subnet.

![Packet flow through VPC NACL](images/vpc-traffic-flow-diff-subnet.svg){: caption="Packet flow through the NACL with different subnets" caption-side="bottom"}

When the VPN gateway and virtual server instance are in different subnets and you create a shared NACL, you must add the following rules for bidirectional traffic flow between your on-premises gateway and the different subnets.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP | N/A | VPN gateway's subnet | N/A |
| Outbound | All  | VPN gateway's subnet | N/A | On-premises gateway public IP | N/A |
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on the shared NACL and different subnet" caption-side="bottom"}

For example, the following table shows the source and destination IP addresses for inbound and outbound rules. In this example, the VPN gateway is in the subnet CIDR `192.168.1.0/24` and the virtual server instance is in the subnet CIDR `192.168.2.0/24`.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | `1.2.3.4`[^IP5] | N/A | `192.168.1.0/24` | N/A |
| Outbound | All | `192.168.1.0/24` | N/A | `1.2.3.4`[^IP6] | N/A |
| Inbound | All | `10.0.0.0/8`[^IP7] | N/A | `192.168.2.0/24` | N/A |
| Outbound | All | `192.168.2.0/24` | N/A | `10.0.0.0/8`[^IP8] | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on the shared NACL and different subnet example" caption-side="bottom"}

[^IP5]: This address is your on-premises gateway public IP for the inbound rule. This setting allows traffic from the on-premises subnet to the VPC.

[^IP6]: Set the destination IP to your on-premises gateway public IP address for the outbound rule. This setting allows traffic from the VPC to the on-premises subnet.

[^IP7]: This address is your on-premises, private CIDR.

[^IP8]: This address is your on-premises, private CIDR.

## Use case 2: VPN gateway and virtual server instance use different NACLs
{: #case-2-diff-acl}

This use case demonstrates scenarios where the IBM Cloud VPN gateway and the VPC virtual server instance are governed by different NACL, enabling consistent traffic control policies. In the first scenario, the VPN gateway and virtual server are part of the same VPC, whereas in the second scenario they are in different VPCs connected by a transit gateway.

### Scenario 1: VPN gateway and virtual server instance in different subnets with separate NACLs
{: #vpn-vsi-shared-vpc-diff-subnet}

When the VPN gateway and virtual server instance are in different subnets and you create two NACLs: one attached to the VPN gateway subnet, and the other attached to the virtual server subnet, you must add the following rules for traffic flow between your on-premises gateway and the different subnets.

Encrypted traffic flows between your on-premises gateway and the VPN gateway subnet, covering both on-premises and VPC IP ranges within the encrypted domain. After the packet reaches the VPC VPN gateway, it is decrypted and forwarded to the VPC virtual server subnet. The response packets are then sent back through the VPN subnet, where they are encrypted again and returned to the on-premises gateway.

![Packet flow through different VPC NACLs](images/vpc-traffic-flow-diff-acls-subnets.svg){: caption="Packet flow through different NACLs with different subnets" caption-side="bottom"}

#### Configuring NACL for VPN gateway subnet
{: #nacl-vpn-gateway}

This NACL is attached to the VPN gateway subnet. The traffic rules for the VPN gateway subnet must cover the management traffic that is used to set up the VPN tunnel and the encrypted VPN tunnel traffic between your on-premises network and the VPC.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP | N/A | VPN gateway's subnet | N/A |
| Outbound | All  | VPN gateway's subnet | N/A | On-premises gateway public IP | N/A |
| Inbound| All  | On-premises private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on VPN gateway's subnet" caption-side="bottom"}

#### Configuring NACL for virtual server instance subnet
{: #nacl-virtual-server}

This NACL is attached to the virtual server subnet. The traffic rules for the virtual server subnet must cover VPN tunnel traffic for communication between your on-premises network and the virtual server instance.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
{: caption="Inbound and outbound rules on virtual server subnets" caption-side="bottom"}

#### Troubleshooting traffic
{: #traffic-troubleshooting}

Optional: This rule allows traffic for connectivity tests, such as pinging the VPN gateway or VPC virtual server instance for reachability checks and troubleshooting.

| Inbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Rules for traffic troubleshooting" caption-side="bottom"}

#### Examples: Configuring VPN gateway and virtual server subnets in a shared VPC
{: #configure-vpn-gateway-virtual-server-subnets}

The following examples illustrate the specific NACL rules that are applied to both the VPN gateway and virtual server instance subnets. These examples help you to set up your NACLs correctly according to your specific subnet CIDRs and traffic requirements.

This table shows the source and destination IP addresses for inbound and outbound rules. In this example, the VPN gateway is in subnet CIDR `192.168.1.0/24`, and the virtual server is in subnet CIDR `192.168.2.0/24`.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | `1.2.3.4`[^IP9] | N/A | `192.168.1.0/24` | N/A |
| Outbound | All | `192.168.1.0/24` | N/A | `1.2.3.4`[^IP10] | N/A |
| Inbound | All | `10.0.0.0/8`[^IP11] | N/A | `192.168.2.0/24` | N/A |
| Outbound | All | `192.168.2.0/24`| N/A | `10.0.0.0/8`[^IP12] | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on VPN gateway's subnet example" caption-side="bottom"}

[^IP9]: This address is your on-premises gateway public IP for the inbound rule. This setting allows traffic from the on-premises subnet to the VPC.

[^IP10]: Set the destination IP to your on-premises gateway public IP address for the outbound rule. This setting allows traffic from the VPC to the on-premises subnet.

[^IP11]: This address is your on-premises, private CIDR.

[^IP12]: This address is your on-premises, private CIDR.

This table illustrates the NACL rules for the virtual server subnet, showing the same type of inbound and outbound traffic flow as described for the VPN gateway subnet.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | `10.0.0.0/8` | N/A | `192.168.2.0/24` | N/A |
| Outbound | All | `192.168.2.0/24` | N/A | `10.0.0.0/8` | N/A |
{: caption="Inbound and outbound rules on virtual server subnet example" caption-side="bottom"}

### Scenario 2: VPN gateway and virtual server instance in different VPCs connected through a transit gateway
{: #vpn-vsi-diff-vpc-tgw}

In this scenario, the VPN gateway and the virtual server instance reside in different subnets within different VPCs connected by a transit gateway. This configuration uses the same procedure as the preceding scenarios to forward packets through the subnets in different VPCs.

1. Encrypted traffic flows between your on-premises gateway and the VPN gateway subnet.
1. After the packet reaches the VPC VPN gateway, it is decrypted and forwarded to the VPC virtual server subnet.
1. The response packets are then sent back through the VPN subnet, where they are encrypted again and returned to the on-premises gateway.

![Packet flow through different VPC NACLs](images/vpc-traffic-flow-diff-vpc-acls-subnets.svg){: caption="Packet flow through different VPCs with different NACLs and subnets" caption-side="bottom"}

When the VPN gateway and virtual server instance are in different VPCs with different subnets and different NACLs, you must add the following rules for traffic flow between your on-premises gateway and the subnets in different VPCs.

#### Configuring NACL for VPN gateway subnet
{: #nacl-vpn-gateway2}

This NACL is attached to the VPN gateway subnet. The traffic rules for the VPN gateway subnet must cover the management traffic that is used to set up the VPN tunnel and the encrypted VPN tunnel traffic between your on-premises network and the VPC.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | Your on-premises gateway public IP | N/A | VPN gateway's subnet | N/A |
| Outbound | All  | VPN gateway's subnet | N/A | On-premises gateway public IP | N/A |
| Inbound| All  | On-premises private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on VPN gateway's subnet" caption-side="bottom"}

#### Configuring NACL for virtual server instance subnet
{: #nacl-virtual-server2}

This NACL is attached to the virtual server subnet. The traffic rules for the virtual server subnet must cover VPN tunnel traffic for communication between your on-premises network and the virtual server instance.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | On-premises, private CIDR | N/A | VPC CIDR | N/A |
| Outbound | All | VPC CIDR | N/A | On-premises, private CIDR | N/A |
{: caption="Inbound and outbound rules on virtual server subnets" caption-side="bottom"}

#### Troubleshooting traffic
{: #traffic-troubleshooting2}

Optional: This rule allows traffic for connectivity tests, such as pinging the VPN gateway or VPC virtual server instance for reachability checks and troubleshooting.

| Inbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Rules for traffic troubleshooting" caption-side="bottom"}

#### Examples: Configuring VPN gateway and virtual server subnets in different VPC
{: #configure-vpn-gateway-virtual-server-subnets-diff-vpc}

The following examples illustrate the specific NACL rules that are applied to both the VPN gateway and virtual server instance subnets in different VPC. These examples help you to set up your NACLs correctly according to your specific subnet CIDRs and traffic requirements.

The following table shows the source and destination IP addresses for inbound and outbound rules. In this example, the VPN gateway in VPC A is in subnet CIDR `192.168.1.0/24`, and the virtual server in VPC B is in subnet CIDR `192.168.2.0/24`.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | `1.2.3.4`[^IP13] | N/A | `192.168.1.0/24` | N/A |
| Outbound | All  | `192.168.1.0/24` | N/A | `1.2.3.4`[^IP14] | N/A |
| Inbound | All | `10.0.0.0/8`[^IP15] | N/A | `192.168.2.0/24` | N/A |
| Outbound | All | `192.168.2.0/24`| N/A | `10.0.0.0/8`[^IP16] | N/A |
| Inbound (optional) | ICMP | Any | N/A | Any | N/A |
{: caption="Inbound and outbound rules on VPN gateway's subnet example" caption-side="bottom"}

[^IP13]: This address is your on-premises gateway public IP for the inbound rule. This setting allows traffic from the on-premises subnet to the VPC.

[^IP14]: Set the destination IP to your on-premises gateway public IP address for the outbound rule. This setting allows traffic from the VPC to the on-premises subnet.

[^IP15]: This address is your on-premises, private CIDR.

[^IP16]: This address is your on-premises, private CIDR.

This table illustrates the NACL rules for the virtual server subnet in VPC B, showing the same type of inbound and outbound traffic flow as described for the VPN gateway subnet in VPC A.

| Inbound and Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound | All | `10.0.0.0/8` | N/A | `192.168.2.0/24` | N/A |
| Outbound | All | `192.168.2.0/24` | N/A |`10.0.0.0/8`| N/A |
{: caption="Inbound and outbound rules on virtual server subnet example" caption-side="bottom"}
