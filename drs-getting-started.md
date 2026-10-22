---

copyright:
  years: 2026
lastupdated: "2026-10-22"

keywords: dynamic route server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with dynamic route server (beta)
{: #about-drs}

Dynamic Route Server for VPC is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

Dynamic Route Server for VPC is designed for complex network topologies that require scalable, resilient, and automated routing, reducing the need for manual route configuration.
{: shortdesc}

You can use a dynamic route server to dynamically manage routing between your Virtual Private Cloud (VPC) and connected networks, such as transit gateways, firewalls, SD-WAN devices, and overlay networks. A dynamic route server uses Border Gateway Protocol (BGP) to learn and advertise routes, and applies policy-driven routing logic to automatically update VPC routing tables and peer connections as network conditions change.

## Before you begin
{: #getting-started-drs-prerequisites}

Before you provision a dynamic route server:

- Make sure that you have the required level of access to create VPC services. For more information, see [Getting started with Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started) and [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).
- Review [Planning considerations for dynamic route servers](/docs/vpc?topic=vpc-planning-drs) to understand BGP, ASN, and routing table requirements.
- [Create a VPC](/docs/vpc?topic=vpc-getting-started) and provision one or more [virtual network interfaces](/docs/vpc?topic=vpc-vni-about#vni-getting-started) (VNIs). The dynamic route server uses the VNIs to deploy routing appliances and establish BGP sessions with peers.

## Setting up a dynamic route server
{: #drs-configure}

To set up a dynamic route server, follow these general steps:

1. [Create a dynamic route server instance](/docs/vpc?topic=vpc-drs-create) and add at least one member.
1. [Create a peer group](/docs/vpc?topic=vpc-drs-create-peer-group) for the dynamic route server instance.
1. [Attach peers](/docs/vpc?topic=vpc-drs-attach-peer-to-group), such as firewalls, routers, or transit gateways, to the peer groups.
1. [Create peer group policies](/docs/vpc?topic=vpc-drs-create-peer-group-policy) to control how routes are learned, advertised, or added to VPC routing tables.
1. Optionally, [create static routes](/docs/vpc?topic=vpc-drs-create-static-routes) to define fixed network paths that are managed by the dynamic route server. Static routes are health-aware and are automatically added to or removed from VPC routing tables based on the health of the associated next-hop peer group.

## Next steps
{: #getting-started-drs-next-steps}

After your dynamic route server is set up, consider these next steps:

- Explore [Dynamic route server use cases](/docs/vpc?topic=vpc-drs-use-cases) for guidance on common topologies such as routing through a firewall, multi-zone deployments, and active and standby failover.
- Read [Dynamic route server architecture](/docs/vpc?topic=vpc-drs-architecture) to understand how the service components, routing appliances, and Transit Gateway integration work together.
