---

copyright:
  years: 2018, 2021
lastupdated: "2021-06-07"

keywords:  

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.nlb_full}} limitations
{: #nlb-limitations}

Known limitations for {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) are as follows:

* The NLB requires the member and port combination to be unique. In other words, a member with instance X and port Y cannot be added to a pool if a member with instance X and port Y exists in another pool.
* There is a one-to-one mapping between listener and pool.
* All members that are associated with a network load balancer must be in the same zone as the load balancer.
* The NLB uses the primary network interfaces of its associated members for data traffic. Non-primary network interfaces are not supported.
* To ensure service availability, use a dedicated subnet with your NLBs. Clients and members should reside in an alternate subnet.
* Two members with the same instance X and same port Y cannot exist at the same time for an NLB. This case is not supported and your traffic might not be routed correctly. 
* For an NLB with routing mode enabled:
   * The only supported back-end targets are VNF instances. When using APIs, the listener `port_min` and `port_max` should be set to `1` and `65535` respectively; `port` should be left empty.
   * Only one listener is supported.
   * The NLB and the VNF back-end targets must be in the same subnet.
* The default load balancer quota is 50 per region. To increase the number, you must [create a support case](/docs/get-support?topic=get-support-open-case).
* For Private NLB, the NLB service might add rules to custom routing tables to ensure service availability for some failure conditions. As a result, if the client is outside the zone and/or VPC of the NLB, you must add an ingress custom routing table to the VPC where the NLB resides with the proper traffic source selected.

   For Private NLB, depending on the location of the clients, you must ensure that ingress routing tables exist (as described in Table 1).

| Client Location | Routing Table Type	| Traffic Source |
|----|----|----|
| On-premises | Ingress | Direct Link |
| Another VPC or classic infrastructure | Ingress | Transit Gateway |
| Another Availability Zone of the same VPC |	Ingress	| VPC zone |
{: caption="Table 1: Traffic sources that require ingress custom routing tables" caption-side="bottom"}

   For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
   {: note}
