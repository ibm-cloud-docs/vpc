---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-12"

keywords:  network, encryption, client VPN, server VPN

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring security groups and NACLs for use with a VPN server
{: #vpn-client-to-site-security-groups}

Security groups and network access control lists (NACLs) can be configured on the VPN server's subnet where the VPN server is deployed. They can be configured on other VPC subnets that communicate over the VPN tunnel also.
{: shortdesc}

If you configure security groups and NACLs on the VPN server's subnet, make sure that the following rules are in place to allow VPN tunnel traffic. For more information, see [About security groups](/docs/vpc?topic=vpc-using-security-groups) and [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).

If you don't specify a security group when you provision the VPN server, the default security group of the VPC is attached to the VPN server. You can attach other security groups to the VPN server after your server is provisioned.
{: note}

## Rules for VPN protocol traffic
{: #rules-vpn-protocol-traffic}

VPN servers can run on TCP or UDP protocols. You can specify the protocol and port when you provision the VPN server. You must open the corresponding protocol and port with the security group rules and NACL.

|Inbound/Outbound rules| Protocol | Source/Destination type | Source | Value |
|-----------|-----------|------|------|-------|
|`inbound`| `VPN server protocol`| CIDR block | `0.0.0.0/0` | `VPN server port` |
|`outbound`| `VPN server protocol`| CIDR block | `0.0.0.0/0` | `VPN server port` |
{: caption="Security group rules for a VPN server" caption-side="bottom"}

| Inbound/Outbound rules | Protocol | Source IP | Source port | Destination IP | Destination port |
|--------------|------|------|------|------|------------------|
| `inbound` | `VPN server protocol` | Any | Any | `VPN server subnet` | `VPN server port`|
| `outbound` | `VPN server protocol`  | `VPN server subnet` | `VPN server port` | Any | Any |
{: caption="NACL rules for a VPN server" caption-side="bottom"}

For example, by default, the VPN server runs on UDP port `443`. You can configure the security group and NACL rules as follows:

|Inbound/Outbound rules| Protocol | Source/Destination type | Source | Value |
|-----------|-----------|------|------|-------|
|`inbound`| UDP| CIDR block | `0.0.0.0/0` | `443` |
|`outbound`| UDP| CIDR block | `0.0.0.0/0` | `443` |
{: caption="Configuration information for security group rules of a VPN server with default protocol and port" caption-side="bottom"}

| Inbound/Outbound rules | Protocol | Source IP | Source port | Destination IP | Destination port |
|--------------|------|------|------|------|------------------|
| `inbound` | UDP | Any | Any | `VPN server subnet` | `443` |
| `outbound` | UDP  | `VPN server subnet` | `443` | Any | Any |
{: caption="Configuration information for a NACL of a VPN server with default protocol and port" caption-side="bottom"}

## Rules for VPN traffic with `deliver` route action
{: #rules-vpn-traffic-with-deliver-route}

When the client is connected to the VPN server, the VPN server drops all traffic from the client by default. You must configure the VPN route on the VPN server to allow this traffic. The action of the route can be either `deliver` or `translate`.

* When the action is `deliver`, after the packet is decrypted from the tunnel, the packet is forwarded directly, so the source IP of the packet is the VPN client IP, which is from the client IP pool.
* When the action is `translate`, after the packet is decrypted from the tunnel, the source IP of the packet is translated to the private IP of the VPN server, then the packet is sent.

Note which CIDR should be specified when you create the security group and NACL rules.

|Inbound/Outbound rules| Protocol | Source/Destination type | Source | Value |
|-----------|-----------|------|------|-------|
|`inbound`| ALL| CIDR block | `VPN route destination CIDR` | Any |
|`outbound`| ALL| CIDR block | `VPN route destination CIDR` | Any |
{: caption="Security group rules for a VPN deliver route on the VPN server" caption-side="bottom"}

| Inbound/Outbound rules | Protocol | Source IP | Source port | Destination IP | Destination port |
|--------------|------|------|------|------|------------------|
| `inbound` | ALL | `VPN route destination CIDR` | Any | `VPN server client IP pool` | Any|
| `outbound` | ALL  | `VPN server client IP pool` | Any | `VPN route destination CIDR` | Any |
{: caption="NACL rules for a VPN deliver route on the VPN server subnet" caption-side="bottom"}

At the same time, you need to configure the security group and NACL on the VPC virtual server to unblock traffic from the VPN client:

|Inbound/Outbound rules| Protocol | Source/Destination type | Source | Value |
|-----------|-----------|------|------|-------|
| `inbound` | ALL | CIDR block | `VPN server client IP pool` | Any |
| `outbound` | ALL | CIDR block | `VPN server client IP pool` | Any |
{: caption="Security group rules for a VPN deliver route on the VPC virtual server interface" caption-side="bottom"}

| Inbound/Outbound rules | Protocol | Source IP | Source port | Destination IP | Destination port |
|--------------|------|------|------|------|------------------|
| `inbound` | ALL | `VPN server client IP pool` | Any | `VPN route destination CIDR` | Any|
| `outbound` | ALL  | `VPN route destination CIDR` | Any | `VPN server client IP pool` | Any |
{: caption="NACL rules for a VPN deliver route on the VPC virtual server subnet" caption-side="bottom"}

For example, if you use `172.16.0.0/20` as the client IP pool when you provision the VPN server, and you want to access the resources of subnet `10.240.128.0/24` from the VPN client, you need to create a VPN route with destination:`10.240.128.0/24`, and action: `deliver`. You must also configure security group and NACL rules as follows:

|Inbound/Outbound rules| Protocol | Source/Destination type | Source | Value |
|-----------|-----------|------|------|-------|
|`inbound`| ALL| CIDR block | `10.240.128.0/24` | Any |
|`outbound`| ALL| CIDR block | `10.240.128.0/24` | Any |
{: caption="Security group rules for a VPN deliver route on the VPN server" caption-side="bottom"}

| Inbound/Outbound rules | Protocol | Source IP | Source port | Destination IP | Destination port |
|--------------|------|------|------|------|------------------|
| `inbound` | ALL | `10.240.128.0/24` | Any | `172.16.0.0/20` | Any|
| `outbound` | ALL | `172.16.0.0/20` | Any | `10.240.128.0/24` | Any |
{: caption="NACL rules for a VPN deliver route on the VPN server subnet" caption-side="bottom"}

|Inbound/Outbound rules| Protocol | Source/Destination type | Source | Value |
|-----------|-----------|------|------|-------|
|`inbound`| ALL| CIDR block | `172.16.0.0/20` | Any |
|`outbound`| ALL| CIDR block | `172.16.0.0/20` | Any |
{: caption="Security group rules for a VPN deliver route on the VPC virtual server" caption-side="bottom"}

| Inbound/Outbound rules | Protocol | Source IP | Source port | Destination IP | Destination port |
|--------------|------|------|------|------|------------------|
| `inbound` | ALL | `172.16.0.0/20` | Any | `10.240.128.0/24` | Any|
| `outbound` | ALL  | `10.240.128.0/24` | Any | `172.16.0.0/20` | Any |
{: caption="NACL rules for a VPN deliver route on the VPC virtual server subnet" caption-side="bottom"}

## Rules for VPN traffic with `translate` route action
{: #rules-vpn-traffic-with-translate-route}

The rules for the VPN translate route are almost the same as the deliver route. The only difference is that the source IP of the packet is translated to the VPN server's private IP. Therefore, you must use the VPN server's subnet instead of the VPN server's client IP pool.

If you select multiple subnets when you provision the VPN server, you must include all subnets in the security group and NACL rules.
{: important}

|Inbound/Outbound rules| Protocol | Source/Destination type | Source | Value |
|-----------|-----------|------|------|-------|
|`inbound`| ALL| CIDR block | `VPN route destination CIDR` | Any |
|`outbound`| ALL| CIDR block | `VPN route destination CIDR` | Any |
{: caption="Security group rules for a VPN translate route on the VPN server" caption-side="bottom"}

| Inbound/Outbound rules | Protocol | Source IP | Source port | Destination IP | Destination port |
|--------------|------|------|------|------|------------------|
| `inbound` | ALL | `VPN route destination CIDR` | Any | `VPN server subnet CIDR` | Any|
| `outbound` | ALL  | `VPN server subnet CIDR` | Any | `VPN route destination CIDR` | Any |
{: caption="NACL rules for a VPN translate route on the VPN server subnet" caption-side="bottom"}

Normally, the destination of the VPN translate route is out of the VPC. For example, you might want to use a VPN server to access the IBM classic infrastructure virtual server instance that is in the subnet `10.187.190.0/26`. At the same time, you must configure firewall rules on the destination to unblock traffic from the VPN client.

In the example, the subnets `10.240.64.0/24` and `10.240.129.0/24` are used when the VPN server is provisioned. The classic virtual server instance that you want to access from the VPN client is in the subnet `10.187.190.0/26` in the IBM classic infrastructure. You must create a VPN route with destination: `10.187.190.0/26`, action: `translate`, and configure security group and NACL rules as follows:

|Inbound/Outbound rules| Protocol | Source/Destination type | Source | Value |
|-----------|-----------|------|------|-------|
| `inbound` | ALL| CIDR block | `10.187.190.0/26` | Any |
| `outbound` | ALL| CIDR block | `10.187.190.0/26` | Any |
{: caption="Security group rules for a VPN translate route on the VPN server" caption-side="bottom"}

| Inbound/Outbound rules | Protocol | Source IP | Source port | Destination IP | Destination port |
|--------------|------|------|------|------|------------------|
| `inbound` | ALL | `10.187.190.0/26` | Any | `10.240.64.0/24` | Any|
| `inbound` | ALL | `10.187.190.0/26` | Any | `10.240.129.0/24` | Any|
| `outbound` | ALL  | `10.240.64.0/24` | Any | `10.187.190.0/26` | Any |
| `outbound` | ALL  | `10.240.129.0/24` | Any | `10.187.190.0/26` | Any |
{: caption="NACL rules for a VPN translate route on the VPN server subnet" caption-side="bottom"}

| Inbound/Outbound rules | Protocol | Source IP | Source port | Destination IP | Destination port |
|--------------|------|------|------|------|------------------|
| `inbound` | ALL | `10.240.64.0/24` | Any | `10.187.190.0/26` | Any|
| `inbound` | ALL | `10.240.129.0/24` | Any | `10.187.190.0/26` | Any|
| `outbound` | ALL  | `10.187.190.0/26` | Any | `10.240.64.0/24` | Any |
| `outbound` | ALL  | `10.187.190.0/26` | Any | `10.240.129.0/24` | Any |
{: caption="Firewall rules for a VPN translate route on the target firewall device (optional)" caption-side="bottom"}
