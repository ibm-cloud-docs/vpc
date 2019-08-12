---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-30"

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

# Required authorizations for VPC resources
{: #resource-authorizations-required-for-api-and-cli-calls}

The table below lists the authorizations required to interact with {site.data.keyword.vpc_full} Infrastructure objects. This information is particularly helpful to know when you're making API calls.
{:shortdesc}

The terms _attached_ or _unattached_ refer to whether the resource is associated with one or more VPCs (either directly or indirectly through the resource's subnets). An unattached floating IP has no subnets, and therefore it is not associated with any VPC, so authorization by VPC is not applicable.

* **View** and **List** actions correspond to the **Viewer** or **Operator** roles.
* **Operate** actions correspond to the **Operator** role.
* **Create**, **Delete**, and **Update** actions correspond to **Editor** and **Administrator** roles.
* Resources are protected by authorization, resource reference objects are not (ID, CRN, and so forth).


| Resource | Operation | Required Authorization |
|--------|--------|---------|
| VPC | Create | View authorization on the resource group for the VPC<br />Update authorization on VPC resources|
| VPC | Update, Delete |  Update authorization on VPC |
| VPC |  View, List | View authorization on VPC  |
| VPC default security group|  View, List | View authorization on VPC |
| VPC address prefixes |  Create, Update, Delete | Update authorization on VPC |
| VPC address prefixes |  View, List | View authorization on VPC  |
|—————|——————|———————|
| Floating IP (unassociated) | Create| Update authorization for floating IP resources |
| Floating IP (unassociated) | Update, Delete | Update authorization for the floating IP |
| Floating IP (unassociated) | View, List | View authorization for the floating IP |
|——————|———————|————————|
| Public gateway | Create |  Update authorization on public gateway resources<br />Operate authorization for the VPC and floating IP resources |
| Public gateway | Update, Delete |  Update authorization on the public gateway |
| Public gateway | View, List | View authorization on the public gateway |
|—————————|————————|———————————|
| Geography | View, List |  For regions and zones, any account user |
|———————|————————|—————————|
| SSH key | Create| Update authorization for SSH key resources |
| SSH key | Update, Delete | Update authorization for the SSH key |
| SSH key | View, List | View authorization for the SSH key |
|————————|—————————|————————|
| Subnet | Create | Update authorization for subnet resources<br />Operate authorization for the VPC<br />Operate authorization for the public gateway, if it is to be  associated |
| Subnet | Update | Update authorization for the subnet<br />Operate authorization for the public gateway, if it is  associated  |
| Subnet | Delete | Update authorization for the subnet |
| Subnet | View, List | View authorization for the subnet |
| Subnet's public gateway | Attach, Detach | Update authorization for the subnet<br />Operate authorization for the public gateway |
| Subnet's public gateway | View, List | View authorization for the subnet and public gateway|
|——————|—————————|————————|
| Security group | View, List    | View authorization on the security group |
| Security group | Create  | View authorization on the resource group for the security group<br />Update authorization on security group resources <br />View authorization for the VPC|
| Security group | Update, Delete  | Update authorization on the security group|
| Security group rule | View, List | View authorization on the security group|
| Security group rule | Create, Update, Delete | Update authorization on the security group|
| Security group network interface | View, List | View authorization on the security group and the instance.|
|  |  | |
| Security group network interface | Attach, Detach | Operate authorization on the security group.<br />Update authorization on the instance to which the network interface belongs.|
|—————————|—————————|—————————|
| Images | View, List  | View authorization for the image |
|—————————|—————————|—————————|
| Instances | Create| Update authorization for vitual server instance resources<br />Update authorization for the volume<br />Operate authorization for the VPC<br />Operate authorization for the subnet<br />Operate authorization for the security group <br />Operate authorization for floating IP resources, if it is to be  associated|
| Instances | Create| Update authorization for vitual server instance resources<br />View authorization for the volume<br />Operate authorization for the VPC<br />Operate authorization for the subnet<br />Operate authorization for the security group <br />Operate authorization for floating IP resources, if it is to be  associated|
| Instances | Update, Delete | Update authorization for the instance |
| Instances | View, List  | View authorization for the instance |
| Instance actions | Create, Update, Delete | Update authorization for the instance|
| Instance actions | View, List  | View authorization for the instance |
| Interfaces | View, List  | View authorization for the instance |
| Interface's floating IP | View, List | View authorization for the instance and the floating IP |
| Instance's floating IP | Associate | Update authorization for the instance<br />Operate authorization for the floating IP|
| Instance's floating IP | Disassociate | Update authorization for the instance |
| Volume attachments | View, List | View authorization for the instance |
| Volume attachments | Create | Update authorization for the Instance and volume |
| Volume attachments | Update, Delete | Update authorization for the instance |
|————————|——————|————————|
| Volumes | Create| Update authorization for volume resources |
| Volumes | Update, Delete | Update authorization for the volume |
| Volumes | View, List  | View authorization for the volume |
| Volume profiles | View, List  | Any account user |

