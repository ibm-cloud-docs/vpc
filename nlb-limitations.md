---

copyright:
  years: 2018, 2026
lastupdated: "2026-04-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for network load balancers
{: #nlb-limitations}

Known issues are identified bugs or unexpected behaviors that were not fixed before release, but weren’t critical enough to delay it. These issues are communicated to you, often with workarounds, and are prioritized for resolution in the near term by the development team.
{: shortdesc}

The following sections contain known issues for public, private, and Private Path network load balancers (NLBs).
{: shortdesc}

## Known issues for private and public network load balancers
{: #limitations-network-load-balancers}

* An NLB requires each member and port combination to be unique.
* An NLB member virtual server instance’s port can be used for only NLB traffic.
* Each listener maps to a single pool (one-to-one relationship).
* An NLB uses the primary network interfaces of its associated members for data traffic. Non-primary network interfaces aren't supported.
* To improve availability, use a dedicated subnet for NLBs. Place clients and members in separate subnets when possible.
* Two members with the same instance X and same port Y cannot exist at the same time for an NLB. For example, two members using the same server port on the same instance are not supported, and traffic routing might fail. For more information, see [Why can't I add members with the same server port to my NLB?](/docs/vpc?topic=vpc-troubleshoot-lb-nlb-members-same-port).
* For an NLB with routing mode enabled:
   * Only Virtual Network Function (VNF) instances are supported as back-end targets. When using APIs, set `port_min` to `1` and `port_max` to `65535`; leave `port` empty.
   * Only one listener is supported.
   * The NLB and the VNF back-end targets must be in the same subnet.
* For quotas and service limits, see [Quotas and service limits for Network load balancers](/docs/vpc?topic=vpc-quotas#nlb-quotas). To request an increase, [create a support case](/docs/support?topic=support-open-case).
* When you create a listener for an NLB, you can specify a `protocol` of `tcp` or `udp`. However, each listener in the NLB must use a unique `port`.
* [Private NLB]{: tag-blue} The NLB service might add rules to custom routing tables to ensure service availability for some failure conditions. As a result, if the client is outside the zone and/or VPC of the NLB, you must configure an ingress custom routing table in the VPC that hosts the NLB with the appropriate traffic source.
* [Private NLB]{: tag-blue} The required ingress routing table depends on client location:

   | Client location | Routing table type | Traffic source |
   |----|----|----|
   | On-prem | Ingress | Direct Link |
   | Another VPC or classic infrastructure | Ingress | Transit Gateway |
   | Another availability zone of the same VPC | Ingress | VPC zone |
   {: caption="Traffic sources that require ingress custom routing tables." caption-side="bottom"}

   For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
   {: note}

* You can have a maximum of 128 direct server return configurations for each back-end member virtual server instance.
* When a member target instance is deleted, the corresponding NLB pool member is not automatically removed.

## Known issues for Private Path network load balancers
{: #issues-private-path-network-load-balancers}

* When you configure an ALB as a Private Path NLB member, the NLB TCP health check status always shows `OK`, even if ALB pool members are unhealthy.
* Access to a Virtual Private Endpoint (VPE) gateway associated with a Private Path NLB from Direct Link or Transit Gateway is not supported.

   Workaround: Access an ALB that has the VPE configured as a member. For configuration assistance, contact IBM Support.
  
* Private Path NLB pool members must be VPC virtual server instances or reserved IPs in same VPC as the load balancer. To reach members outside the VPC (for example, on-premises members), you can configure an ALB as a member of the Private Path NLB pool and define the remote destinations as ALB members. For more information, see [Connecting an on-premises service to a consumer using an ALB in a Private Path NLB pool](/docs/vpc?topic=vpc-private-path-service-intro&interface=ui#pps-use-case-5).
* Access to a Private Path NLB from a different region is not supported. The consumer VPE gateway and the Private Path NLB instance must be in the same region.

   Workaround: Create a transit gateway to connect the consumer VPC in the remote region to the VPC that hosts the Private Path VPE. Then, access the service through that VPE. For configuration assistance, contact IBM Support.

* Access to Private Path NLBs from Classic Infrastructure (CSE) is not supported.

   Workaround: Create a transit gateway from the classic environment to the VPC that hosts the Private Path VPE. Then, access the service through that VPE.
   
* Access control for the load balancer is managed through a Private Path service. Security groups and network access control lists (NACLs) are not supported.
* UDP traffic is not supported by the load balancer data path.
* Autoscaler integration is not supported.
* The maximal MTU for Private Path NLB traffic is `8500`.
* For quotas and service limits, see [Quotas and service limits for Private Path network load balancers](/docs/vpc?topic=vpc-quotas#ppnlb-quotas). To request an increase, [create a support case](/docs/support?topic=support-open-case).

### Related link
{: #nlb-limitations-related-links}

[Known issues for Private Path services](/docs/vpc?topic=vpc-ppsg-limitations)
