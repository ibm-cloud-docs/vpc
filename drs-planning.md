---

copyright:
  years: 2026
lastupdated: "2026-10-28"

keywords: dynamic route server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations
{: #planning-drs}

[Dynamic Route Server for VPC](/docs/vpc?topic=vpc-release-notes#vpc-oct2126) is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

Before you create a dynamic route server, review applicable planning considerations, configuration requirements, and other guidelines.
{: shortdesc}

## General considerations
{: #drs-general-considerations}

Review the following general considerations before you create a dynamic route server:

* A dynamic route server is a routing service and does not forward data traffic. Instead, it learns routes from peers by using BGP and adds routes to VPC routing tables based on defined policies.
* A dynamic route server instance is created within a single VPC and manages routes only for that VPC.
* You must provide at least one and no more than three members (one per zone) when you create a dynamic route server instance. For high availability, use one member per availability zone.
* All members of a dynamic route server instance must use the same local ASN.
* After a dynamic route server instance is created, it must reach a stable lifecycle state before you can configure peers, peer groups, or routing policies.

## Virtual network interface considerations
{: #drs-vni-considerations}

Review the following considerations when you select a virtual network interface (VNI) for a dynamic route server instance:

* Each dynamic route server member requires one VNI that you must create in advance.
* All VNIs must belong to the same VPC as the dynamic route server instance.
* Each VNI must have a reserved private IP address and one or more security groups that allow BGP traffic.
* An existing VNI that you use for a member must meet the following requirements:
  * It is in the same account as the dynamic route server and isn't bound to another target.
  * IP spoofing isn't allowed, infrastructure NAT is enabled, and protocol state filtering isn't disabled.
  * It has no floating IP, no secondary reserved IPs, and no IP other than its primary IP.
* For high availability, select VNIs that are located in different availability zones.
* A VNI that is attached to a dynamic route server instance can't be deleted until it is detached from the dynamic route server.
* Subnets that are associated with VNIs must have sufficient IP capacity for the reserved IP addresses and ongoing operations.

## BGP, BFD and ASN considerations
{: #drs-bgp-asn-considerations}

Review the following BGP-related considerations before you create a dynamic route server instance:

* A dynamic route server uses eBGP to exchange routes with peers, such as routers, firewalls, or a transit gateway.
* You must specify a local ASN when you create a dynamic route server instance.
* Each peer must be configured with a peer ASN that is different from the local ASN.
* The ASN values `0`, `13884`, `36351`, `64512`, `64513`, `65100`, `65200-65234`, `65402-65433`, `65500`, `65516`, `65519`, `65521`, `65531`, and `4201065000-4201065999` are reserved and can't be used as local or peer ASNs.
* If an ASN conflict exists, BGP sessions fail to establish until the ASN configuration is corrected.
* Bidirectional Forwarding Detection (BFD) can be enabled on peer groups for faster failure detection and route withdrawal.
* Dynamic route server appliances establish BGP sessions by using multihop. To use Bidirectional Forwarding Detection (BFD) with BGP sessions, configure BGP multihop on both the dynamic route server appliance and the peer device. Ensure that the peer device's BGP configuration supports multihop to enable BFD failure detection.

## Transit Gateway integration considerations
{: #drs-bgp-tgw-considerations}

Review the following considerations when you integrate a dynamic route server with {{site.data.keyword.tg_full}}:

* You can create a transit gateway connection to a dynamic route server either when you create a transit gateway or by adding a network connection to an existing transit gateway.
* A dynamic route server instance can be connected to only one transit gateway.
* A transit gateway connection uses redundant Generic Routing Encapsulation (GRE) tunnels with eBGP between a transit gateway and dynamic route server members.
* If you specify a CIDR for GRE tunnel IP addresses, it must be at least `/27` and not overlap with any VPC subnets or existing transit gateway tunnel CIDRs.
* If no CIDR is specified, a default CIDR range is used.
* When a dynamic route server is connected to a transit gateway, routes that are learned from the transit gateway can be advertised to other peers only if allowed in the dynamic route server policies.
To establish connectivity between Transit Gateway and dynamic route server appliances, configure network ACLs (NACLs) to allow GRE traffic (IP protocol 47). Also, configure security group (SG) rules to allow BGP traffic (TCP port 179) and GRE traffic (IP protocol 47) between Transit Gateway and the dynamic route server appliances.


## Routing table and policy considerations
{: #drs-routing-table-considerations}

Review the following routing considerations before you configure dynamic route server policies:

* You can use peer policies to reflect learned routes between peers, advertise VPC address prefixes to peers, add learned routes to VPC routing tables, and advertise custom static routes.
* A dynamic route server updates VPC custom routing tables based on routing policies that you define.
* Routes are added only to the routing tables that you explicitly reference in the VPC routing table policies.
* If you exceed the routing table limits, route addition might fail. For more information, see quotas for [Routing tables and routes](/docs/vpc?topic=vpc-quotas&interface=ui#routing-tables-routes-quotas). In this case, you might need to request a quota increase.
* When a VPC routing table policy is configured, configure the peer groups for a dynamic route server so that peers in different peer groups do not advertise the same destination. Advertising the same destination across peer groups can cause inconsistencies in learned routes.

## High availability and resiliency considerations
{: #drs-hra-considerations}

Review the following availability considerations:

* For zone fault tolerance, deploy one dynamic route server member per availability zone.
* Each member establishes independent BGP sessions with peers.
* If a member becomes unhealthy, the dynamic route server automatically withdraws affected routes and re-adds routes through healthy members.
* Health monitoring is based on BGP session state and BFD status (if enabled).

## Security considerations
{: #drs-security-considerations}

Review the following security considerations:

* Dynamic route server appliances are managed by {{site.data.keyword.cloud_notm}} and run in isolated service-managed VPCs.
* Communication between dynamic route server components and IBM Cloud services uses private networking and virtual private endpoints.
* Security groups attached to VNIs must allow BGP traffic (TCP port `179`) and any additional peer-specific traffic that is required for routing control.
* All dynamic route server configuration changes and lifecycle events are logged by using {{site.data.keyword.at_full}}. For more information, see [Dynamic route server status and descriptions](/docs/vpc?topic=vpc-drs-status), [Dynamic route server events](/docs/vpc?topic=vpc-at_events#events-dynamic-route-server), and [Dynamic route server logging](/docs/vpc?topic=vpc-logging#logging-dynamic-route-server).

## Operational considerations
{: #drs-operational-considerations}

Review the following operational considerations:

* You cannot delete a dynamic route server instance if peer groups or policies still exist and are dependent on the instance.
* To delete a dynamic route server instance, you must first remove all policies, delete all peer groups, and detach the dynamic route server from any transit gateways.
* Configuration changes, such as adding peers, peer groups, or policies, do not interrupt existing traffic flows.
* Route changes are propagated to VPC routing tables immediately, depending on policy and health status.

## Related links
{: #drs-related-links}

In addition to the planning considerations, review the known issues before you configure a dynamic route server. For more information, see [Known issues for dynamic route servers](/docs/vpc?topic=vpc-known-issues).
