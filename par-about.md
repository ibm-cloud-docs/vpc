---

copyright:
  years: 2026
lastupdated: "2026-08-27"

keywords: vpc, public address ranges, getting started

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with public address ranges
{: #about-par}

A public address range is a contiguous set of public IPs that you can reserve and bind to a VPC in an availability zone.
{: shortdesc}

You can create public address ranges by using IBM-managed public IP address pools.

You can route traffic destined for addresses in the range to a target resource in the VPC, such as a virtual server instance, virtual network function (VNF), or other compute resource.

For example, you can configure public ingress routing to send the destination IP range to a VNF appliance next-hop. Response traffic from the target retains the original source IP as it exits the VPC, ensuring that return traffic isn't dropped. For more information about configuring ingress routing, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes&interface=api).

## Key capabilities and benefits
{: #par-feature-capabilities}

Public address ranges help support scalable, secure, and highly available network architectures.

Contiguous IP Range
:   A public address range provides a continuous set of public IPs for scalable and manageable routing and security policies.

Resource Management
:   Public address ranges can simplify access control and security while enabling a single public ingress route to inspect traffic from multiple endpoints and secure enterprise applications on the VPC.

Scalability
:   When additional public IP addresses are required, you can reserve a larger prefix instead of managing individual addresses.

High Availability
:   Public address ranges are regional resources that can be moved between VPCs and availability zones to support high availability and disaster recovery designs.

## Getting started
{: #par-getting-started}

Before you begin, review [Planning considerations for public address ranges](/docs/vpc?topic=vpc-par-planning).

### IPv4
{: #par-getting-started-ipv4}

Follow these steps to get started with IPv4 public address ranges.

1. Ensure that you have [created a VPC](/docs/vpc?topic=vpc-getting-started&interface=ui#create-and-configure-vpc) and all the needed resources (if not already present).
1. [Create your public address range](/docs/vpc?topic=vpc-par-creating&interface=ui).
1. [Bind, unbind, and move public address ranges](/docs/vpc?topic=vpc-par-unbinding-binding&interface=ui) after creating a public address range.
1. [Configure an ingress routing table for your VPC](/docs/vpc?topic=vpc-about-custom-routes&interface=ui) by adding routes that direct traffic to the next-hop IP of the target appliance, using the public IP address range as the destination. These routes direct public internet traffic to a next-hop within the VPC, such as a Network Functions Virtualization (NFV) instance, router, firewall, or load balancer.

   The next-hop IP must be an IP address that is bound to a network interface on a subnet in the route's zone for ingress routing.
   {: note}



## Related links
{: #par-related-links}

- [Use cases for public address ranges](/docs/vpc?topic=vpc-par-use-cases)
- 
- 
- [IAM roles and actions](/docs/iam?topic=iam-iam-service-roles-actions#is.public-address-range-roles)
- [Quotas](/docs/vpc?topic=vpc-quotas#par-quotas) and [service limits](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services)
- [FAQ](/docs/vpc?topic=vpc-faq-public-address-ranges)
- [Known issues](/docs/vpc?topic=vpc-par-known-issues)
- [Troubleshooting](/docs/vpc?group=tbs-par)
