---

copyright:
  years: 2025
lastupdated: "2025-04-18"

keywords: vpc, public address ranges, about

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About public address ranges
{: #about-par}

Public Address Ranges for VPC is only available for evaluation and testing purposes for users with special access.
{: beta}

A public address range is a contiguous set of public IPs that you can reserve and bind to a VPC in an availability zone. 
{: shortdesc}

The IPs in the range can be routed to a Virtual Network Function (VNF) appliance in the VPC by configuring public ingress routing to route the destination IP range to a VNF appliance next-hop. Response traffic from the VNF appliance retains the source IP as it egresses, ensuring traffic isn't dropped. 
 
For more information about configuring ingress routing, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes&interface=api).
{: note}

## Key capabilities and benefits
{: #par-feature-capabilities}

The following key benefits highlight how the Public Address Ranges for VPC service supports scalable, secure, and highly available networks.

Contiguous IP Range
:   A public address range provides a continuous set of IBM-provided public IPs for scalable and manageable routing and security policies.

Resource Management
:   Public address ranges can simplify access control and security while enabling a single public ingress route to inspect traffic from multiple endpoints and secure enterprise applications on the VPC.

Scalability
:   When you need more public IPs, you can reserve a larger prefix without needing to manually manage individual addresses.

High Availability
:   In regions with availability zones, you can create zone-redundant public IP ranges to distribute IP addresses for high availability. These public address ranges are regional and can be moved between VPCs in the same or different availability zones.
   
## Planning considerations
{: #par-planning}

Review the following considerations before creating a public address range:

* Make sure that you have the appropriate [IAM permissions](/docs/vpc?topic=vpc-about-par#par-access-management).
* Review public address range [limitations](/docs/vpc?topic=vpc-par-limitations).
* You can bind multiple public address ranges to a VPC.
* You can't divide public address ranges into subranges and bind it to multiple VPCs or zones.
* You can't change the size of the address range after it is reserved. Make sure to reserve a public address range size large enough to meet your needs.

   IPs in different public address ranges aren't guaranteed to be contiguous.
   {: note}

* You can limit a public address range to specific resources within the VPC by configuring network ACLs, security groups, or a combination of both.

* You can reserve a public address range with the following prefix sizes. Review public address range [quotas and service limits](/docs/vpc?topic=vpc-quotas&interface=ui#par-quotas). 
   * `/28` = 16 addresses
   * `/29`  = 8 addresses
   * `/30` = 4 addresses
   * `/31` = 2 addresses
   * `/32` = 1 address
* If there is an overlap between a public address range and the private address prefix in a VPC, the private address prefix takes precedence.  

## Getting started with public address ranges
{: #par-getting-started}

To get started with using public address ranges, follow these steps:

1. Ensure that you have [created a VPC](/docs/vpc?topic=vpc-getting-started&interface=ui#create-and-configure-vpc) and all the needed resources (if not already present).
1. [Create your public address range](/docs/vpc?topic=vpc-par-creating&interface=ui).
1. [Bind, unbind,and move public address ranges](/docs/vpc?topic=vpc-par-unbinding-binding&interface=ui) after creating a public address range.
1. [Configure an ingress routing table for your VPC](/docs/vpc?topic=vpc-about-custom-routes&interface=ui) by adding routes that direct traffic to the next-hop IP of the target appliance, using the public IP address range as the destination. These routes direct internet traffic to a next-hop within the VPC, such as a Network Functions Virtualization (NFV) instance, router, firewall, or load balancer.
 
   The next-hop IP must be an IP address that is bound to a network interface on a subnet in the route's zone for ingress routing.
   {: note}

## IAM roles and actions
{: #par-access-management}

IBM Cloud Identity and Access Management (IAM) controls access to public address ranges for users in your account. Every user that accesses this service in your account must be assigned an access policy with an IAM role. Review the following available platform and service roles and the actions mapped to each to help you assign access.

If you're using the CLI or API to assign access, use **`is.public-address-range`** for the service name.
{: note}

| Role | Description |
| ----- | :----- |
| Administrator | As an administrator, you can perform all platform actions based on the resource this role is being assigned, including assigning access policies to other users. |
| Editor | As an editor, you can perform all platform actions except for managing the account and assigning access policies. |
| Operator | As an operator, you can perform platform actions required to configure and operate service instances, such as viewing a service's dashboard. |
| Viewer | As a viewer, you can view service instances, but you can't modify them. |
{: row-headers}
{: caption="Platform roles - Public Address Ranges for VPC" caption-side="top"}
{: tab-title="Platform roles"}
{: tab-group="is.public-address-range"}
{: class="simple-tab-table"}
{: summary="Use the tab buttons to change the context of the table. This table has row and column headers. The row headers provide the platform role name and the column headers identify the specific information available about each role."}
{: #platform-roles-table-pr}

| Action | Description | Roles |
| ----- | :----- | :----- |
| `is.public-address-range.public-address-range.read` | Read a public address range | Administrator, Editor, Operator, Viewer |
| `is.public-address-range.public-address-range.list` | List a public address range | Administrator, Editor, Operator, Viewer |
| `is.public-address-range.public-address-range.create` \n \n Requires `is.vpc.vpc.operate` if the public address range is to be bound at the time of creation. | Create a public address range | Administrator, Editor|
| `is.public-address-range.public-address-range.update` \n \n | Modify a public address range | Administrator, Editor|
| `is.public-address-range.public-address-range.operate` \n \n and `is.public-address-range.public-address-range.update` | Bind, unbind, or move a public address range | Administrator, Editor |
| `is.public-address-range.public-address-range.delete` \n \n Requires `is.vpc.vpc.operate` if a public address range is bound at the time of deletion. | Delete a public address range | Administrator, Editor |
{: caption="Service actions - Public Address Ranges for VPC" caption-side="top"}
{: tab-title="Actions"}
{: tab-group="is.public-address-range"}
{: class="simple-tab-table"}
{: summary="Use the tab buttons to change the context of the table. This table provides the available actions for the service, descriptions of each, and the roles that each action are mapped to."}
{: #actions-table-pr}

## Common use cases
{: #par-use-cases}

Public address ranges help customers simplify the integration of network and security appliances into their network topology to run their workloads on a VPC. This provides more control over routing, security, and traffic management, enabling solutions that enhance network performance and security.

### Secure your workloads in VPC
{: #secure-workloads-vpc}

To manage access to sensitive data effectively, firewalls automatically detect and handle threats using policy-based routing for enhanced security. Define routing rules for public endpoints to redirect ingress traffic to third-party appliances before it reaches the final destination. This makes it easier for customers to deploy production-grade applications with the networking and security services they require in their VPC.

### Deploy highly-available and resilient workloads in VPC
{: #deploy-ha-resilient-workloads-vpc}

In the event of a zonal failure, customers can reroute internet traffic for monitoring and inspection to a VNF appliance deployed in another zone, providing highly available workloads in their VPC.
    
## Related links
{: #par-related-links}

* [Quotas and service limits](/docs/vpc?topic=vpc-quotas)
