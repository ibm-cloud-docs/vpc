---

copyright:
  years: 2025
lastupdated: "2025-07-06"

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

You can route the IPs in the range to a target resource in the VPC, such as a virtual server instance, VNF appliance, or other compute resource. For example, you can configure public ingress routing to send the destination IP range to a VNF appliance next-hop. Response traffic from the target retains the original source IP as it exits the VPC, ensuring that return traffic isn't dropped. 
 
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
* Review public address range [known issues](/docs/vpc?topic=vpc-par-limitations).
* You can bind multiple public address ranges to a VPC.
* You can't divide public address ranges into subranges and bind it to multiple VPCs or zones.
* You can't change the size of the address range after it is reserved. Make sure to reserve a public address range size large enough to meet your needs.

   IPs in different public address ranges aren't guaranteed to be contiguous.
   {: note}

* You can limit the use of a public address range to specific resources within the VPC by configuring network ACLs, security groups, or a combination of both.
* You can reserve a public address range with the following prefix sizes. For more information, review public address range [quotas and service limits](/docs/vpc?topic=vpc-quotas&interface=ui#par-quotas). 
   * `/28` = 16 addresses
   * `/29`  = 8 addresses
   * `/30` = 4 addresses
   * `/31` = 2 addresses
   * `/32` = 1 address
* If there is an overlap between a public address range and the private address prefix in a VPC, the private address prefix takes precedence.  
* When a public address range is bound to a zone in a VPC, traffic originating from any virtual server or bare metal server in that zone in the VPC, and using a source IP from the public address range, can communicate with the public and private destinations. To limit this, you can:

   * [Configure security groups](/docs/vpc?topic=vpc-configuring-the-security-group&interface=ui) to limit which resources can send or receive traffic with IP addresses from the public address range. 
   * [Configure network ACLs](/docs/vpc?topic=vpc-acl-create-ui&interface=ui) to allow or deny traffic at the subnet level.
   * [Configure VPC egress routes](/docs/vpc?topic=vpc-create-vpc-route&interface=ui) to explicitly direct outbound traffic and avoid unintended paths.

   When setting up these network traffic controls, keep in mind that some users might lack the necessary IAM permissions to secure resources properly.

   When a VPC is created, the default security groups and network ACLs allows inbound and outbound traffic for the supported protocols. To ensure secure and intentional use of these public address range IPs, it is highly recommended to review and customize your security group rules, network ACLs, and egress routes to maintain a strong security posture. This practice helps prevent unintended access to traffic patterns, particularly when multiple users have permission to deploy compute resources in your account.
   {: attention} 

## Getting started with public address ranges
{: #par-getting-started}

To get started with using public address ranges, follow these steps:

1. Ensure that you have [created a VPC](/docs/vpc?topic=vpc-getting-started&interface=ui#create-and-configure-vpc) and all the needed resources (if not already present).
1. [Create your public address range](/docs/vpc?topic=vpc-par-creating&interface=ui).
1. [Bind, unbind,and move public address ranges](/docs/vpc?topic=vpc-par-unbinding-binding&interface=ui) after creating a public address range.
1. [Configure an ingress routing table for your VPC](/docs/vpc?topic=vpc-about-custom-routes&interface=ui) by adding routes that direct traffic to the next-hop IP of the target appliance, using the public IP address range as the destination. These routes direct public internet traffic to a next-hop within the VPC, such as a Network Functions Virtualization (NFV) instance, router, firewall, or load balancer.
 
   The next-hop IP must be an IP address that is bound to a network interface on a subnet in the route's zone for ingress routing.
   {: note}

## IAM roles and actions
{: #par-access-management}

{{site.data.keyword.iamlong}} (IAM) controls access to public address ranges for users in your account. Every user that accesses this service in your account must be assigned an access policy with an IAM role. Review the following available platform and service roles and the actions mapped to each to help you assign access.

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
| `is.public-address-range.public-address-range.create`| Create a public address range | Administrator, Editor|
| `is.public-address-range.public-address-range.update` \n \n | Modify a public address range | Administrator, Editor|
| `is.public-address-range.public-address-range.operate` \n \n and `is.public-address-range.public-address-range.update` | Bind, unbind, or move a public address range | Administrator, Editor |
| `is.public-address-range.public-address-range.delete` | Delete a public address range | Administrator, Editor |
{: caption="Service actions - Public Address Ranges for VPC" caption-side="top"}
{: tab-title="Actions"}
{: tab-group="is.public-address-range"}
{: class="simple-tab-table"}
{: summary="Use the tab buttons to change the context of the table. This table provides the available actions for the service, descriptions of each, and the roles that each action are mapped to."}
{: #actions-table-pr}

## Common use cases
{: #par-use-cases}

Public address ranges help customers simplify the integration of network and security appliances into their network topology to run their workloads on a VPC. This provides more control over routing, security, and traffic management, enabling solutions that enhance network performance and security.

### Securing your workloads in VPC
{: #secure-workloads-vpc}

To manage access to sensitive data effectively, firewalls automatically detect and handle threats using policy-based routing for enhanced security. You can define public address ranges to expose specific services or applications, enabling controlled ingress into the VPC. Define routing rules for public endpoints to redirect ingress traffic to third-party appliances before it reaches the final destination. This approach simplifies the deployment of production-grade applications with the networking and security services required in a VPC.

### Deploying highly-available and resilient workloads in VPC
{: #deploy-ha-resilient-workloads-vpc}

To ensure workload resilience, you can use public address ranges to maintain consistent access to services during zonal failures. In the event of a zonal failure, internet traffic is rerouted to a virtual network function (VNF) appliance deployed in another zone for monitoring and inspection, maintaining high availability within the VPC.
    
## Related links
{: #par-related-links}

* [Quotas and service limits](/docs/vpc?topic=vpc-quotas#par-quotas)
* [FAQ for public address ranges](/docs/vpc?topic=vpc-faq-public-address-ranges)
