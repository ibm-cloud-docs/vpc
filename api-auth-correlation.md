---

copyright:
  years: 2018, 2023
lastupdated: "2023-09-14"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Required permissions
{: #resource-authorizations-required-for-api-and-cli-calls}

Table 1 lists the minimum Identity and Access Management (IAM) roles that are required to interact with {{site.data.keyword.vpc_full}} (VPC) infrastructure objects.
{: shortdesc}

For more information about IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).

| Resource | Action | Minimum IAM role |
|--------|--------|---------|
| VPC | Create | Viewer for the resource group of the VPC \n \n Editor for Virtual Private Cloud resources \n \n  Operator for the ACL, if the user selects a specific ACL to be the default ACL|
| VPC | Update, Delete |  Editor for the VPC |
| VPC |  View, List | Viewer for the VPC  |
| VPC default ACL and security group|  View, List | Viewer for the VPC |
| VPC address prefixes |  Create, Update, Delete | Editor for the VPC |
| VPC address prefixes |  View, List | Viewer for the VPC  |
|——————|———————|————————|
| ACL | Create | Editor for Network ACL and VPC resources |
| ACL | Update, Delete | Editor for the ACL |
| ACL| View, List | Viewer for the ACL |
| ACL rule | Create, Update, Delete | Editor for the ACL |
| ACL rule | View, List | Viewer for the ACL |
|————————|—————————|————————|
| Backup | Create, Update, Delete | Editor to create, update, or delete backup policy |
| Backup | View, List | Viewer to view policy details, list policies |
| Backup | View, List | Viewer to view backup plan details, list plans |
| Backup | Create, Update, Delete  | Editor to create, update, or delete backup plan |
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
| Instances | Create| Editor for Virtual Server for VPC and Block Storage for VPC resources \n \n Editor for Floating IP for VPC resources, if a floating IP is to be associated \n \n Operator for the VPC, subnet, and the security group |
| Instances | Update, Delete | Editor for the instance |
| Instances | View, List  | Viewer for the instance |
| Instances | Post | IP Spoofing Operator for instance |
| Instances | Create console | Operator and Console Administrator for the instance |
| Instance actions | Create, Update, Delete | Operator for the instance|
| Instance actions | View, List  | Viewer for the instance |
| Instance storage | View, List  | Viewer for the instance |
| Instance storage | Update name  | Editor for the instance |
| Interfaces | View, List  | Viewer for the instance |
| Interface's floating IP | View, List | Viewer for the instance and the floating IP |
| Instance's floating IP | Associate | Editor for the instance \n \n Operator for the floating IP|
| Instance's floating IP | Disassociate | Editor for the instance |
| Volume attachments | View, List | Viewer for the instance |
| Volume attachments | Create | Editor for the Instance and volume |
| Volume attachments | Update, Delete | Editor for the instance |
|————————|—————————|————————|
| Dedicated host group | Create, Update, Delete | Editor for the dedicated host group |
| Dedicated host group | View, List | Viewer for the dedicated host group |
| Dedicated host group | Create an instance in | Operator for the dedicated host group |
| Dedicated host | Create | Editor for the dedicated host \n \n Editor for the dedicated host group |
| Dedicated host | Update, Delete | Editor for the dedicated host |
| Dedicated host | View, List | Viewer for the dedicated host |
| Dedicated host | Create an instance on | Operator for the dedicated host |
|————————|—————————|————————|
| Bare metal server | View, List | Viewer for the bare metal server |
| Bare metal server | Update, Delete | Editor for the bare metal server |
| Bare metal server | Create | Editor for the bare metal server \n \n Advanced Network Operator for the bare metal server \n \n Subnet Editor \n \n Operator for the security group \n \n Operator for VPC \n \n Operator for Key \n \n Operator for Image |
| Bare metal server | IP spoofing, Infrastructure NAT | Advanced Network Operator for the bare metal server |
| Bare metal server | Operate (Restart, Start, Stop, Retrieve initialization data) | Operator for the bare metal server |
| Bare metal server | Create console access token | Console Admin for the bare metal server \n \n Operator for the bare metal server |
| Bare metal server disk | View, List | Viewer for the bare metal server |
| Bare metal server disk | Update | Operator for the bare metal server |
| Bare metal server network interface | Create | Editor for the bare metal server \n \n Editor for subnet \n \n Operator for the security group \n \n Advanced Network Operator for the bare metal server |
| Bare metal server network interface | Update | Editor for the bare metal server \n \n Advanced Network Operator for the bare metal server |
| Bare metal server network interface | Delete | Editor for the bare metal server |
| Bare metal server network interface | View, List | Viewer for the bare metal server |
| Bare metal server floating IP | View, List | Viewer for the bare metal server \n \n Viewer for Floating IP |
| Bare metal server floating IP | Operate (Associate, Detach) | Editor for the bare metal server \n \n Operator for Floating IP |
|————————|—————————|————————|
| Instance group | Create | Editor for Virtual Server for VPC and Block Storage for VPC resources \n \n Operator for the VPC and subnet \n \n Viewer for the instance template \n \n Editor for the load balancer, if a load balancer is to be associated |
| Instance group | Update | Editor for the instance group \n \n Operator for the subnet \n \n Viewer for the instance template \n \n Editor for the load balancer, if a load balancer is specified |
| Instance group | Delete | Editor for the instance group \n \n  Editor for the associated instances \n \n Editor for the load balancer, if a load balancer is specified |
| Instance group | View, List  | Viewer for the instance group |
| Instance group membership | Update | Editor for the instance group |
| Instance group membership | Delete | Editor for the instance group \n \n Editor for the associated instance \n \n Editor for the load balancer, if a load balancer is specified |
| Instance group manager | Create, Update, Delete  | Editor for the instance group |
| Instance group manager | View | Viewer for the instance group |
| Instance group manager policy | Create, Update, Delete  | Editor for the instance group |
| Instance group manager policy | View  | Viewer for the instance group |
| Instance network interface | Create, Update | IP Spoofing Operator for instance |
| Instance template | Create, Update, Delete  | Editor for instance |
| Instance template | View  | Viewer for instance |
|————————|—————————|————————|
| Load balancer | Create | Editor for load balancer for VPC resources \n \n Operator for security groups \n \n Viewer for VPC |
| Load balancer | Update | Editor for the load balancer |
| Load balancer | Delete | Editor for the load balancer \n \n Operator for security groups |
| Load balancer | View, List  | Viewer for the load balancer |
| Load balancer pools and listeners | Create, Update, Delete | Editor for the load balancer |
| Load balancer pools and listeners | View, List  | Viewer for the load balancer |
|————————|—————————|————————|
| Placement group | View | Viewer for placement groups |
| Placement group | Create, Delete, Update | Editor for placement groups |
|————————|—————————|————————|
| Public gateway | Create |  Editor for Public Gateway resources \n \n Operator for the VPC and Floating IP resources |
| Public gateway | Update, Delete | Editor for the public gateway |
| Public gateway | View, List | Viewer for the public gateway |
|——————|———————|————————|
| Routing tables | List | Viewer to list routing tables |
| Routing tables | Read | Viewer of a routing table |
| Routing tables | Create | Editor of a routing table |
| Routing tables | Update | Editor of a routing table and routes |
| Routing tables | Delete | Editor to delete a routing table |
| Routing tables | Operate | Operator to configure a subnet attachment to a routing table |
| Routing tables | Advertise | Editor to configure route advertisement |
|——————|———————|————————|
| Shares | View, List | Viewer for file shares and mount targets |
| Shares | Create | Editor for creating file shares |
| Shares | Create | Operator role for creating mount targets |
| Shares | Update, Delete | Editor for updating and deleting file shares |
| Shares | Update | Editor role for updating mount targets |
| Shares | Delete | Operator role for deleting mount targets |
| Share profiles | View, List | Any account user |
|——————|———————|————————|
| Security group | View, List | Viewer for the security group |
| Security group | Create  | Viewer for the VPC and the resource group of the security group \n \n Editor for security group for VPC resources |
| Security group | Update, Delete | Editor for the security group|
| Security group rule | View, List | Viewer for the security group|
| Security group rule | Create, Update, Delete | Editor for the security group|
| Security group target | View, List | Viewer for the security group |
| Security group target | Attach, Detach | Operator for the security group \n \n Editor for instance if the target is a network interface \n \n Editor for load balancer if the target is a load balancer |
|————————|—————————|————————|
| Snapshots | View, List | Viewer for snapshots|
| Snapshots | Create | Editor for creating snapshots |
| Snapshots | Update, Delete | Editor for updating and deleting snapshots |
|————————|—————————|————————|
| SSH key | Create| Editor for SSH Key for VPC resources |
| SSH key | Update, Delete | Editor for the SSH key |
| SSH key | View, List | Viewer for the SSH key |
|————————|—————————|————————|
| Subnet | Create | Editor for Subnet resources \n \n Operator for the VPC and the public gateway, if it is to be associated \n \n Viewer for the ACL |
| Subnet | Update | Editor for the subnet \n \n Operator for the public gateway, if it is associated \n \n Viewer for the ACL  |
| Subnet | Delete | Editor for the subnet |
| Subnet | View, List | Viewer for the subnet |
| Subnet's ACL | Attach, Detach | Editor for the subnet \n \n Viewer for the ACL |
| Subnet's ACL | View, List | Viewer for the subnet and ACL|
| Subnet's public gateway | Attach, Detach | Editor for the subnet \n \n Operator for the public gateway |
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
| VPN gateway connections | Create, Update, Delete | Editor for the VPN gateway |
| VPN gateway connections | View, List  | Viewer for the VPN gateway |
| VPN gateway ike_policies, ipsec_policies and connections | Create, Update, Delete | Editor for the VPN gateway |
| VPN gateway ike_policies, ipsec_policies and connections|View, List  | Viewer for the VPN gateway |
{: caption="Table 1. Minimum IAM roles for VPC actions" caption-side="top"}

## Client-to-site VPN server tasks
{: #client-to-site-vpn-server-tasks}

The following table lists tasks that are associated with the client-to-site VPN server service, the minimum IAM role required to complete the task, and the associated API method.

| Description | Resource | Minimum IAM role | Action | API |
|--------|--------|---------|---------|---------|
| List all VPN servers | VPN server | Viewer | is.vpn-server.vpn-server.read | `GET /vpn_servers/<vpn-server-id>` and `GET /vpn_servers/<vpn-server-id>/configuration` |
| Create VPN server | VPN server | Editor | is.vpn-server.vpn-server.create | `POST /vpn_servers` |
| Delete VPN server | VPN server | Editor | is.vpn-server.vpn-server.delete | `DELETE /vpn_servers/<vpn-server-id>` |
| Update VPN server | VPN server | Editor | is.vpn-server.vpn-server.update | `PATCH /vpn_servers/<vpn-server-id>` |
| Create security group | VPN server | Security Group Operator | is.security-group.security-group.operate | `POST /vpn_servers` |
| Create subnet | VPN server | Subnet Operator | is.subnet.subnet.update | `POST /vpn_servers` |
| Update subnet | VPN server | Subnet Operator | is.subnet.subnet.update | `PATCH /vpn_servers/<vpn-server-id>` |
| List VPN server  | VPN server | Operator | is.vpn-server.vpn-server.read | `GET /vpn_servers/<vpn-server-id>/clients/<vpn-server-client-id>` |
| Delete a VPN client (This request disconnects and deletes the VPN client from the VPN server immediately) | VPN server | Operator | is.vpn-server.vpn-server.operate | `DELETE /vpn_servers/<vpn-server-id>/clients/<vpn-server-client-id>` |
| Disconnect a VPN client (This request disconnects the specified VPN client, and deletes the client according to the VPN server's auto-deletion policy) | VPN server | Operator | is.vpn-server.vpn-server.operate | `POST /vpn_servers/<vpn-server-id>/clients/<vpn-server-client-id>/disconnect` |
| List all routes for a VPN server | VPN server | Operator | is.vpn-server.vpn-server.read | `GET /vpn_servers/<vpn-server-id>/routes/<vpn-server-route-id>` |
| Create a route for a VPN server | VPN server | Editor | is.vpn-server.vpn-server.update | `POST /vpn_servers/routes` |
| Connect to VPN server | VPN server | VPNClient | is.vpn-server.vpn-server.connect | N/A |
{: caption="Table 2. Minimum IAM roles for VPN gateway API and CLI calls" caption-side="top"}
