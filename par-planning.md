---

copyright:
  years: 2026
lastupdated: "2026-08-25"

keywords: vpc, public address ranges, planning, considerations, limitations

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations for public address ranges
{: #par-planning}

Review the following considerations before creating a public address range.

## General considerations
{: #par-planning-general}

* Make sure that you have the appropriate [IAM permissions](/docs/iam?topic=iam-iam-service-roles-actions#is.public-address-range-roles).
* You can bind multiple public address ranges to a VPC.
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
* If an overlap exists between a public address range and the private address prefix in a VPC, the private address prefix takes precedence.
* When a public address range is bound to a zone in a VPC, traffic originating from any virtual server or bare metal server in that zone can use a source IP from the public address range. Such traffic can communicate with public and private destinations. To limit this behavior, you can:

   * [Configure security groups](/docs/vpc?topic=vpc-configuring-the-security-group&interface=ui) to limit which resources can send or receive traffic with IP addresses from the public address range.
   * [Configure network ACLs](/docs/vpc?topic=vpc-acl-create-ui&interface=ui) to allow or deny traffic at the subnet level.
   * [Configure VPC egress routes](/docs/vpc?topic=vpc-create-vpc-route&interface=ui) to explicitly direct outbound traffic and avoid unintended paths.

   When setting up these network traffic controls, keep in mind that some users might lack the necessary IAM permissions to secure resources properly.

   When a VPC is created, the default security group and network ACL allow inbound and outbound traffic for the supported protocols. To ensure secure and intentional use of these public address range IPs, review and customize your security group rules, network ACLs, and egress routes, which helps ensure an adequate security posture. It also helps prevent unintended access to traffic patterns, particularly when multiple users have permission to deploy virtual server instances and compute resources in your account.
   {: attention}

## Limitations
{: #par-planning-limitations}

* You can't assign IP addresses from a public address range to resources in a VPC. You can use these destination IP addresses only in ingress custom route tables with "Public internet" enabled as the Traffic source to direct traffic to a next-hop target resource, such as a virtual server instance, network appliance, or other compute resource.
* This service supports only IBM-provided public IP ranges. Bringing your own public IP or subnet is not supported.
* You can't divide public address ranges into subranges or bind one to multiple VPCs or zones.
* Using public address ranges with a shared virtual IP for ingress routing (for example, NSX-T Tier 0 HA VIP or similar appliances with Bare Metal Server VNI VLAN attachments), can cause an asymmetric routing issue that disrupts stateful firewall handling in security groups.

   To mitigate this issue, consider the following workarounds:

   * Option A: Treat the security group rules as stateless for the affected public address range prefixes.
   * Option B: Allow all inbound and outbound traffic in the security group rules, and rely on the appliance's built-in firewall capabilities.

   These appliances are typically NFVs with integrated firewall capabilities, and the purpose of the public address range and the VPC ingress route is to direct traffic to that appliance.







## Related links
{: #par-planning-related-links}

- [Getting started with public address ranges](/docs/vpc?topic=vpc-about-par)
- [Use cases for public address ranges](/docs/vpc?topic=vpc-par-use-cases)
- 
- 
- [IAM roles and actions](/docs/iam?topic=iam-iam-service-roles-actions#is.public-address-range-roles)
- [Quotas](/docs/vpc?topic=vpc-quotas#par-quotas) and [service limits](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services)
