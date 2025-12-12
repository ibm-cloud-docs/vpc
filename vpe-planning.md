---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-12"

keywords: VPE, virtual private endpoint, endpoint gateway, planning

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning for virtual private endpoint gateways
{: #vpe-planning-considerations}

Before you create a virtual private endpoint gateway, review the following considerations.

## VPE creation and configuration limits
{: #vpe-creation-configuration-limits}

* You can create only one VPE per service within a single VPC.
* You can bind only one IP address per VPC zone to a VPE gateway.
* A reserved IP address that is bound to a VPE gateway can receive traffic from other zones within the same VPC, as long as your Network ACL (NACL) rules allow it.

## Security groups
{: #vpe-planning-considerations-security-groups} 

* You can attach up to five security groups when you create a VPE. If you do not specify a security group during VPE creation, the default security group for your VPC is used.

   If you do not select at least one security group, it is recommended that you update your default security group rules to minimize disruption in traffic on a newly created VPE.
   {: important}

* You can create a security group before or during provisioning (in the console).
   * The security group must be created in the same VPC as your VPE.
   * Configure inbound rules that define what type of traffic is allowed to the VPE. For each rule, complete the following information:
      * Specify a CIDR block or IP address for the permitted inbound traffic. Alternatively, you can specify a security group in the same VPC to allow traffic to all sources that are attached to the selected security group.
      * Select the protocols and ports that the rule applies to.

## Cross-account VPE considerations
{: #cross-account-vpe-considerations} 

* Cross-account VPEs require specifying the target resource CRN, as resources from other accounts are not discoverable through the UI or CLI.
* Both accounts must be within the same IBM Cloud region for connectivity; cross-region connections are supported only between regions participating in a multizone region (MZR).
* Cross-account VPEs connecting to IBM services are subject to CBR enforcement and require appropriate policy configuration on both accounts.
* The target service instance must have cross-account access enabled by the service team, as not all IBM Cloud services support delegated VPE connectivity. Verify service documentation for specific support and access models.

## Accessing IBM Cloud services
{: #vpe-accessing-cloud-services}

* You can access {{site.data.keyword.cloud}} services by using either VPEs or the service endpoint directly. However, if you want your VPC to enforce a certain behavior or discipline, it is recommended to block direct access to the service endpoint IP addresses that use NACLs. For more information, see [Configuring ACLs and security groups for use with endpoint gateways](/docs/vpc?topic=vpc-configure-acls-sgs-endpoint-gateways).

   {{site.data.keyword.cloud_notm}} services do not support accessing a service endpoint and VPE simultaneously from the same virtual instance.
   {: note} 

## Unsupported features
{: #vpe-unsupported-features}

* The following items are not supported:

   * Services that are deployed in zones and regions that are not part of [IBM Cloud Multi-Zone Regions (MZRs)](/docs/overview?topic=overview-locations#table-mzr)
   * {{site.data.keyword.cloud_notm}} Flow Logs for VPC 
   * Egress custom routes 
   * UDP traffic to a VPE is not supported over {{site.data.keyword.cloud_notm}} Direct Link or Transit Gateway (for example, when connecting from a virtual server instance or bare metal server outside the VPE’s VPC). This includes UDP-based services, such as Network Time Protocol (NTP).

## Architectural restrictions
{: #vpe-architectural-restrictions}

* Virtual private endpoints support only IPv4 addressing.
* Each endpoint gateway is bound to a single VPC.
* When a cross-account VPE is created, the account that owns the target resource receives notifications about VPE creation and deletion through Activity Tracker events. This is important because the resource owner must have Activity Tracker enabled to be aware of these actions, and it is the only way to see whether anyone has connected to the resource.
* The way an endpoint gateway maps to a service depends on the specific {{site.data.keyword.cloud_notm}} service that you are enabling. For best practices, refer to the documentation for the specific {{site.data.keyword.cloud_notm}} service.
* The total number of active connections through a VPE gateway is limited. For more information, see [Is there a limit to the total number of concurrent active connections to an endpoint gateway?](/docs/vpc?topic=vpc-faqs-vpe#faq-reserved-ips-and-number-ports).

## Related links
{: #vpe-faq-related-links}

* [DNS sharing planning considerations](/docs/vpc?topic=vpc-vpe-dns-sharing-planning-considerations)
* [FAQ for virtual private endpoint gateways](/docs/vpc?topic=vpc-faqs-vpe)
