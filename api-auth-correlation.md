---

copyright:
  years: 2018, 2020
lastupdated: "2020-10-30"

keywords: resource, resource authorizations, policies, authorization, resource type, resource groups, roles, API, CLI, editor, viewer, administrator, operator

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Required permissions
{: #resource-authorizations-required-for-api-and-cli-calls}

Table 1 lists the minimum Identity and Access Management (IAM) roles that are required to interact with {{site.data.keyword.vpc_full}} (VPC) infrastructure objects.
{:shortdesc}

For more information about IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).

| Resource | Action | Minimum IAM role |
|--------|--------|---------|
| VPC | Create | Viewer for the resource group of the VPC<br />Editor for Virtual Private Cloud resources<br />Operator for the ACL, if the user selects a specific ACL to be the default ACL|
| VPC | Update, Delete |  Editor for the VPC |
| VPC |  View, List | Viewer for the VPC  |
| VPC default ACL and security group|  View, List | Viewer for the VPC |
| VPC address prefixes |  Create, Update, Delete | Editor for the VPC |
| VPC address prefixes |  View, List | Viewer for the VPC  |
| VPC routing table | Create | Editor of a routing table for VPC resources |
| VPC routing table | Update, Delete | Editor of a VPC routing table |
| VPC routing table | View, List | Viewer of a VPC routing table |
|——————|———————|————————|
| ACL | Create | Editor for Network ACL and VPC resources |
| ACL | Update, Delete | Editor for the ACL |
| ACL| View, List | Viewer for the ACL |
| ACL rule | Create, Update, Delete | Editor for the ACL |
| ACL rule | View, List | Viewer for the ACL |
|————————|—————————|————————|
| Floating IP (unassociated) | Create| Editor for Floating IP for VPC resources |
| Floating IP (unassociated) | Update, Delete | Editor for the floating IP |
| Floating IP (unassociated) | View, List | Viewer for the floating IP |
|————————|—————————|————————|
| Flow logs | Create, Delete, List, Operate, Read, Update | See [Managing access for flow logs](/docs/vpc?topic=vpc-fl-iam) for details. |
|————————|—————————|————————|
| Geography | View, List |  For regions and zones, any account user |
|————————|—————————|————————|
| Images | Create  | Editor for Image Service for VPC resources |
| Images | Update, Delete  | Editor for the image |
| Images | View, List  | Viewer for the image |
|————————|—————————|————————|
| Instances | Create| Editor for Virtual Server for VPC and Block Storage for VPC resources<br />Editor for Floating IP for VPC resources, if a floating IP is to be associated<br />Operator for the VPC, subnet, and the security group |
| Instances | Update, Delete | Editor for the instance |
| Instances | View, List  | Viewer for the instance |
| Instance actions | Create, Update, Delete | Operator for the instance|
| Instance actions | View, List  | Viewer for the instance |
| Interfaces | View, List  | Viewer for the instance |
| Interface's floating IP | View, List | Viewer for the instance and the floating IP |
| Instance's floating IP | Associate | Editor for the instance<br />Operator for the floating IP|
| Instance's floating IP | Disassociate | Editor for the instance |
| Volume attachments | View, List | Viewer for the instance |
| Volume attachments | Create | Editor for the Instance and volume |
| Volume attachments | Update, Delete | Editor for the instance |
|————————|—————————|————————|
| Instance group | Create | Editor for Virtual Server for VPC and Block Storage for VPC resources<br />Operator for the VPC and subnet<br />Viewer for the instance template<br />Editor for the load balancer, if a load balancer is to be associated |
| Instance group | Update | Editor for the instance group<br />Operator for the subnet<br />Viewer for the instance template<br />Editor for the load balancer, if a load balancer is specified |
| Instance group | Delete | Editor for the instance group<br />Editor for the associated instances<br />Editor for the load balancer, if a load balancer is specified |
| Instance group | View, List  | Viewer for the instance group |
| Instance group membership | Update | Editor for the instance group |
| Instance group membership | Delete | Editor for the instance group<br />Editor for the associated instance<br />Editor for the load balancer, if a load balancer is specified |
| Instance group manager | Create, Update, Delete  | Editor for the instance group |
| Instance group manager | View | Viewer for the instance group |
| Instance group manager policy | Create, Update, Delete  | Editor for the instance group |
| Instance group manager policy | View  | Viewer for the instance group |
| Instance template | Create, Update, Delete  | Editor for instance |
| Instance template | View  | Viewer for instance |
|————————|—————————|————————|
| Load balancer | Create| Editor for the Load Balancer for VPC resources |
| Load balancer | Update, Delete | Editor for the load balancer |
| Load balancer | View, List  | Viewer for the load balancer |
| Load balancer pools and listeners | Create, Update, Delete | Editor for the load balancer |
| Load balancer pools and listeners | View, List  | Viewer for the load balancer |
|————————|—————————|————————|
| Public gateway | Create |  Editor for Public Gateway resources<br />Operator for the VPC and Floating IP resources |
| Public gateway | Update, Delete |  Editor for the public gateway |
| Public gateway | View, List | Viewer for the public gateway |
|——————|———————|————————|
| Security group | View, List    | Viewer for the security group |
| Security group | Create  | Viewer for the VPC and the resource group of the security group<br />Editor for Security Group for VPC resources|
| Security group | Update, Delete  | Editor for the security group|
| Security group rule | View, List | Viewer for the security group|
| Security group rule | Create, Update, Delete | Editor for the security group|
| Security group network interface | View, List | Viewer for the security group and the instance.|
| Security group network interface | Attach, Detach | Operator for the security group.<br />Editor for the instance to which the network interface belongs.|
|————————|—————————|————————|
| SSH key | Create| Editor for SSH Key for VPC resources |
| SSH key | Update, Delete | Editor for the SSH key |
| SSH key | View, List | Viewer for the SSH key |
|————————|—————————|————————|
| Subnet | Create | Editor for Subnet resources<br />Operator for the VPC and the public gateway, if it is to be associated<br />Viewer for the ACL |
| Subnet | Update | Editor for the subnet<br />Operator for the public gateway, if it is associated<br />Viewer for the ACL  |
| Subnet | Delete | Editor for the subnet |
| Subnet | View, List | Viewer for the subnet |
| Subnet's ACL | Attach, Detach | Editor for the subnet<br />Viewer for the ACL |
| Subnet's ACL | View, List | Viewer for the subnet and ACL|
| Subnet's public gateway | Attach, Detach | Editor for the subnet<br />Operator for the public gateway |
| Subnet's public gateway | View, List | Viewer for the subnet and public gateway|
| Subnet's route | Create, Update, Delete | Editor for VPC routes |
| Subnet's route | View, List | Viewer of VPC routes |
|————————|—————————|————————|
| Volumes | Create| Editor for Block Storage for VPC resources |
| Volumes | Update, Delete | Editor for the volume |
| Volumes | View, List  | Viewer for the volume |
| Volume profiles | View, List  | Any account user |
|————————|—————————|————————|
| VPN gateway | Create | Editor for VPN for VPC resources|
| VPN gateway | Update, Delete | Editor for the VPN gateway|
| VPN gateway | View, List  | Viewer for the VPN gateway |
| VPN gateway (advertised CIDRs) | Create | Editor for VPN for VPC resources|
| VPN gateway (advertised CIDRs) | Update, Delete | Editor for the VPN gateway|
| VPN gateway (advertised CIDRs) | View, List  | Viewer for the VPN gateway |
| VPN gateway connections | Create, Update, Delete | Editor for the VPN gateway |
| VPN gateway connections | View, List  | Viewer for the VPN gateway |
| VPN gateway ike_policies, ipsec_policies and connections | Create, Update, Delete | Editor for the VPN gateway |
| VPN gateway ike_policies, ipsec_policies and connections|View, List  | Viewer for the VPN gateway |
{: caption="Table 1. Minimum IAM roles for VPC actions" caption-side="top"}
