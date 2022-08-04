---

copyright:
  years: 2018, 2021
lastupdated: "2022-07-01"

keywords:  

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.nlb_full}} limitations
{: #nlb-limitations}

Known limitations for {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) are as follows:

* The NLB requires the member and port combination to be unique. In other words:
   * A member with instance X and port Y cannot be added to a pool of NLB A, if a member with instance X and port Y exists in a pool of NLB B.
* An NLB member VSI’s port can only be used for NLB traffic.
* There is a one-to-one mapping between listener and pool.
* The NLB uses the primary network interfaces of its associated members for data traffic. Non-primary network interfaces are not supported.
* To ensure service availability, use a dedicated subnet with your NLBs. Clients and members should reside in an alternate subnet.
* Two members with the same instance X and port Y cannot exist at the same time for an NLB. In this case, your traffic will not route correctly.
* For an NLB with routing mode enabled:
   * The only supported back-end targets are VNF instances. When using APIs, the listener `port_min` and `port_max` should be set to `1` and `65535` respectively; `port` should be left empty.
   * Only one listener is supported.
   * The NLB and the VNF back-end targets must be in the same subnet.
* The default load balancer quota is 50 per region. To increase the number, you must [create a support case](/docs/get-support?topic=get-support-open-case).
* The port range feature for public network load balancers supports a single port range listener with scaling up to 400 configurations. This means that the number of listener ports, multiplied by the number of listener pool members, must be less than or equal to 400.
* For Private NLB, the NLB service might add rules to custom routing tables to ensure service availability for some failure conditions. As a result, if the client is outside the zone and/or VPC of the NLB, you must add an ingress custom routing table to the VPC where the NLB resides with the proper traffic source selected.

   For Private NLB, depending on the location of the clients, you must ensure that ingress routing tables exist (as described in Table 1).

| Client location | Routing table type | Traffic source |
|----|----|----|
| On-prem | Ingress | Direct link |
| Another VPC or classic infrastructure | Ingress | Transit gateway |
| Another availability zone of the same VPC | Ingress | VPC zone |
{: caption="Table 1: Traffic sources that require ingress custom routing tables" caption-side="bottom"}

   For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
   {: note}
