---

copyright:
  years: 2018, 2025
lastupdated: "2025-11-17"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for network load balancers
{: #nlb-limitations}

Known issues are identified bugs or unexpected behaviors that were not fixed before release, but weren’t critical enough to delay it. These issues are communicated to you, often with workarounds, and are prioritized for resolution in the near term by the development team.
{: shortdesc} 

The following section contain known issues for public and private network load balancers (NLBs). The next section contains known issues for Private Path network load balancers.
{: shortdesc}

## Known issues for Private and Public network load balancers
{: #limitations-network-load-balancers}

* An NLB requires the member and port combination to be unique.
* An NLB member virtual server instance’s port can be used for only NLB traffic.
* There is a one-to-one mapping between listener and pool.
* An NLB uses the primary network interfaces of its associated members for data traffic. Non-primary network interfaces aren't supported.
* To ensure service availability, use a dedicated subnet with your NLBs. Clients and members should reside in an alternate subnet.
* Two members with the same instance X and same port Y cannot exist at the same time for an NLB. This means that adding two members with the same server port to an NLB is not allowed. This case is not supported and your traffic might not be routed correctly. To learn more, see [Why can't I add members with the same server port to my NLB?](/docs/vpc?topic=vpc-troubleshoot-lb-nlb-members-same-port)
* For an NLB with routing mode enabled:
   * The only supported back-end targets are Virtual Network Function (VNF) instances. When APIs are used, the listener `port_min` and `port_max` is to be set to `1` and `65535` respectively; `port` is to be left empty.
   * Only one listener is supported.
   * The NLB and the VNF back-end targets must be in the same subnet.
* For quotas and service limits, see [Quotas and service limits for Network load balancers](/docs/vpc?topic=vpc-quotas#nlb-quotas). To increase the quota for your Private Path network load balancer, you must [create a support case](/docs/account?topic=account-open-case).
* When you create a listener for a network load balancer, you can specify a `protocol` of `tcp` or `udp`. However, each listener in the network load balancer must have a unique `port`.
* [Private NLB]{: tag-blue} The NLB service might add rules to custom routing tables to ensure service availability for some failure conditions. As a result, if the client is outside the zone and/or VPC of the NLB, you must add an ingress custom routing table to the VPC where the NLB resides with the proper traffic source selected.
* [Private NLB]{: tag-blue} Depending on the location of the clients, you must ensure that ingress routing tables exist.

   | Client location | Routing table type | Traffic source |
   |----|----|----|
   | On-prem | Ingress | Direct link |
   | Another VPC or classic infrastructure | Ingress | Transit gateway |
   | Another availability zone of the same VPC | Ingress | VPC zone |
   {: caption="Traffic sources that require ingress custom routing tables." caption-side="bottom"}

   For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
   {: note}

* You can have a maximum of 128 direct server return configurations for each backend member VSI.
* When a member target instance is deleted, a Network load balancer pool member is not automatically deleted.

## Known issues for Private Path network load balancers
{: #issues-private-path-network-load-balancers}

* When you configure an ALB as a Private Path NLB member, the Private Path NLB's TCP health check status will always show OK, even if the ALB pool members are faulty. Health check status of ALB should be considered to determine the system health.
* Access to a Virtual Private Endpoint gateway associated with Private Path Network load balancer from Direct Link or Transit Gateway is not supported.
   * Workaround: Access an ALB that has the VPE as a member. Contact IBM Support for assistance with the details.
* Private Path network load balancer pool members must be VPC virtual server instances or Reserved IPs in same VPC as the load balancer. If there is a need to reach members outside the VPC (such as on-premises members), an ALB can be defined as member of the Private Path NLB pool and the remote destinations can be defined as ALB members. For more information, see [Connecting an on-premises service to a consumer using an ALB in a Private Path NLB pool](/docs/vpc?topic=vpc-private-path-service-intro&interface=ui#pps-use-case-5).
* Access to a Private Path network load balancer from remote regions is not supported. The consumer Virtual Private Endpoint gateway and the Private Path network load balancer instance must reside in same region. 
   * Workaround: Access an ALB in the remote region that has the VPE as member. Contact IBM Support for assistance with the details.
* Access to Private Path network load balancers from CSE (classic) is not supported.
* Granular control of access to the load balancer is done through a Private Path service rather than by security groups and Network Access Control Lists (NACLs), which are not supported.
* UDP traffic isn't supported by the load balancer data path.
* Autoscaler integration is not supported.
* The maximal MTU for Private Path NLB traffic is `8500`.
* For quotas and service limits, see [Quotas and service limits for Private Path network load balancers](/docs/vpc?topic=vpc-quotas#ppnlb-quotas). To increase the quota for your Private Path network load balancer, you must [create a support case](/docs/account?topic=account-open-case).
* When you create a pool for a Private Path network load balancer and set the `failsafe_policy.action` value to `drop`, the request incorrectly fails with `400` (Bad Request).
   * Workaround: Do not specify the drop value, as drop is the default behavior.
* The `failsafe_policy.action` value included in any response from a Private Path load balancer pool shows `fail` instead of `drop`. Likewise, the `failsafe_policy.actions` value included in any response from a Private Path load balancer profile shows `fail` instead of `drop`.

### Related link
{: #nlb-limitations-related-links}

[Known issues for Private Path services](/docs/vpc?topic=vpc-ppsg-limitations)
