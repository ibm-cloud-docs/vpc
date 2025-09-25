---

copyright:
  years: 2025
lastupdated: "2025-09-25"

keywords: vpc, public address ranges, about

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About public address ranges
{: #about-par}

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

* Make sure that you have the appropriate [IAM permissions](/docs/account?topic=account-iam-service-roles-actions#is.public-address-range-roles).
* You can bind multiple public address ranges to a VPC.
* You can't divide public address ranges into subranges and bind it to multiple VPCs or zones.
* You can't change the size of the address range after it is reserved. Make sure to reserve a public address range size large enough to meet your needs.

   IPs in different public address ranges aren't guaranteed to be contiguous.
   {: note}

* You can limit the use of a public address range to specific resources within the VPC by configuring network ACLs, security groups, or a combination of both.
* You can reserve a public address range with the following prefix sizes. For more information, review public address range [quotas](/docs/vpc?topic=vpc-quotas&interface=ui#par-quotas) and [service limits](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services). 
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

   When a VPC is created, the default security group and network ACL allow inbound and outbound traffic for the supported protocols. To ensure secure and intentional use of these public address range IPs, it is highly recommended to review and customize your security group rules, network ACLs, and egress routes to ensure an adequate security posture. This practice helps prevent unintended access to traffic patterns, particularly when multiple users have permission to deploy virtual server instances and compute resources in your account.
   {: attention} 

* Before you create a public address range, review the following limitations: 

   * You can't assign IP addresses from a public address range to resources in a VPC. You can only use these destination IP addresses in ingress custom route tables with "Public internet" enabled as the Traffic source to direct traffic to a next-hop target resource, such as a virtual server instance, network appliance, or other compute resource.
   * This service only supports IBM-provided public IP ranges. Bringing your own public IP or subnet is not supported. 
   *  You can't divide public address ranges into subranges or bind one to multiple VPCs or zones.
   * Using public address ranges with a shared virtual IP for ingress routing (for example, NSX-T Tier 0 HA VIP or similar appliances with Bare Metal Server VNI VLAN attachments), can cause an asymmetric routing issue that disrupts stateful firewall handling in security groups. 

      To mitigate this, there are two workarounds:

      * Option A: Treat the security group rules as stateless for the affected public address range prefixes.
      * Option B: Allow all inbound and outbound traffic in the security group rules, and rely on the appliance’s built-in firewall capabilities.

      These appliances are typically NFVs with integrated firewall capabilities, and the purpose of the public address range and the VPC ingress route is to direct traffic to that appliance.

## Getting started with public address ranges
{: #par-getting-started}

To get started with using public address ranges, follow these steps:

1. Ensure that you have [created a VPC](/docs/vpc?topic=vpc-getting-started&interface=ui#create-and-configure-vpc) and all the needed resources (if not already present).
1. [Create your public address range](/docs/vpc?topic=vpc-par-creating&interface=ui).
1. [Bind, unbind,and move public address ranges](/docs/vpc?topic=vpc-par-unbinding-binding&interface=ui) after creating a public address range.
1. [Configure an ingress routing table for your VPC](/docs/vpc?topic=vpc-about-custom-routes&interface=ui) by adding routes that direct traffic to the next-hop IP of the target appliance, using the public IP address range as the destination. These routes direct public internet traffic to a next-hop within the VPC, such as a Network Functions Virtualization (NFV) instance, router, firewall, or load balancer.
 
   The next-hop IP must be an IP address that is bound to a network interface on a subnet in the route's zone for ingress routing.
   {: note}

## Common use cases
{: #par-use-cases}

Public address ranges help customers simplify the integration of network and security appliances into their network topology to run their workloads on a VPC. This provides more control over routing, security, and traffic management, enabling solutions that enhance network performance and security.

### Securing your workloads in VPC
{: #secure-workloads-vpc}

To secure workloads in a VPC, you can assign public address ranges to provide secure access to selected services or applications, enabling controlled ingress into the environment. Next, configure routing rules for those public endpoints to redirect incoming traffic to third-party appliances for inspection before reaching the final destination. Firewalls then enforce defined security policies, inspecting traffic and mitigating threats during the process. This approach simplifies the deployment of production-grade applications with the networking and security services required in a VPC.

The following diagram illustrates how to secure your workloads in a VPC using public address ranges. First, traffic from the internet enters the VPC through a reserved public address range bound to the VPC. The traffic is then routed by the ingress routing table to a security appliance (for example, a firewall or third-party appliance), where it is inspected. After inspection, the traffic is forwarded to the protected applications.

![Secure your workloads in VPC](images/par_use_case_1.svg "Secure your workloads in VPC"){: caption="Secure your workloads in VPC" caption-side="bottom"} 

### Deploying highly-available and resilient workloads in VPC
{: #deploy-ha-resilient-workloads-vpc}

To ensure workload resilience, you can use public address ranges to maintain consistent access to services during zonal failures. In the event of a zonal failure, internet traffic is rerouted to a virtual network function (VNF) appliance deployed in another zone for monitoring and inspection, maintaining high availability within the VPC. 

The following diagram illustrates how routes and firewalls can be configured using public address ranges to enable cross-zone failover and support highly available, resilient workloads in your VPC. Set up two routes: Route 1 for the internet traffic reaching the Active Firewall in Zone 1 (`us-south-1`), and Route 2 for the Passive Firewall in Zone 2 (`us-south-2`). The routes use the public address range as the destination, with the next hop set to the firewall in each zone. 

The public address range is attached to the zone with the Active Firewall, `us-south-1`. Internet traffic incoming through the public address range is routed to the Active Firewall per Route 1 in `us-south-1` for inspection and filtering. When the Active Firewall is down, the customer must manually (or through automation) update the zone attachment of the public address range to `us-south-2`, so that incoming traffic is routed through Route 2 to the Passive Firewall. This setup makes the firewall highly available for internet traffic inspection, securing workloads in the VPC even during zonal failures.

![Deploying highly-available and resilient workloads in VPC](images/par_use_case_2.svg "Deploy highly-available and resilient workloads in VPC"){: caption="Deploy highly-available and resilient workloads in VPC" caption-side="bottom"} 
    
## Related links
{: #par-related-links} 
 
- [IAM roles and actions](/docs/account?topic=account-iam-service-roles-actions#is.public-address-range-roles)
- [Quotas](/docs/vpc?topic=vpc-quotas#par-quotas) and [service limits](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services)
- [FAQ](/docs/vpc?topic=vpc-faq-public-address-ranges)
- [Known issues](/docs/vpc?topic=vpc-par-known-issues)
- [Troubleshooting](/docs/vpc?group=tbs-par)
