---

copyright:
  years: 2018, 2024
lastupdated: "2024-05-02"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Network load balancer limitations
{: #nlb-limitations}

The following lists contain known limitations for {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) and {{site.data.keyword.cloud}} Private Path Network Load Balancer.
{: shortdesc}

## Known limitations for {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB)
{: #limitations-network-load-balancers}

* The NLB requires the member and port combination to be unique.
* An NLB member virtual server instance’s port can be used for only NLB traffic.
* There is a one-to-one mapping between listener and pool.
* The NLB uses the primary network interfaces of its associated members for data traffic. Non-primary network interfaces are not supported.
* To ensure service availability, use a dedicated subnet with your NLBs. Clients and members should reside in an alternate subnet.
* Two members with the same instance X and port Y cannot exist at the same time for an NLB. In this case, your traffic does not route correctly.
* For an NLB with routing mode enabled:
   * The only supported back-end targets are Virtual Network Function (VNF) instances. When APIs are used, the listener `port_min` and `port_max` is to be set to `1` and `65535` respectively; `port` is to be left empty.
   * Only one listener is supported.
   * The NLB and the VNF back-end targets must be in the same subnet.
* The default load balancer quota is 50 per region. To increase the number, you must [create a support case](/docs/get-support?topic=get-support-open-case).
* When you create a listener for a network load balancer, you can specify a `protocol` of `tcp` or `udp`. However, each listener in the network load balancer must have a unique `port`. 
* [Private NLB]{: tag-blue} The NLB service might add rules to custom routing tables to ensure service availability for some failure conditions. As a result, if the client is outside the zone and/or VPC of the NLB, you must add an ingress custom routing table to the VPC where the NLB resides with the proper traffic source selected.
* [Private NLB]{: tag-blue} Depending on the location of the clients, you must ensure that ingress routing tables exist (as described in Table 1).

   | Client location | Routing table type | Traffic source |
   |----|----|----|
   | On-prem | Ingress | Direct link |
   | Another VPC or classic infrastructure | Ingress | Transit gateway |
   | Another availability zone of the same VPC | Ingress | VPC zone |
   {: caption="Table 1: Traffic sources that require ingress custom routing tables." caption-side="bottom"}

   For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
   {: note}  
* You can have a maximum of 128 direct server return configurations for each backend member VSI.
* When a member target instance is deleted, a Network load balancer pool member is not automatically deleted. 

## Known limitations for {{site.data.keyword.cloud}} Private Path network load balancers
{: #limitations-private-path-network-load-balancers}

The beta release of IBM Cloud Private Path services is only available to allowlisted users. Contact your IBM Support representative if you are interested in getting early access to this beta offering.
{: beta}

* Access to a Virtual Private Endpoint gateway associated with Private Path Network load balancer from Direct Link or Transit Gateway is not supported.
* Private Path network load balancer Pool Members on Bare Metal are not supported.
* Private Path network load balancer Pool Members must be running in a VPC Virtual Server Instance. On-prem members are not supported.
* Access to Private Path network load balancer from remote regions is not supported. The consumer Virtual Private Endpoint gateway and the Private Path network load balancer instance must reside in same region. 
* Access to Private Path network load balancers from CSE (classic) is not supported.
* Access to a Virtual Private Endpoint gateway associated with Private Path network load balancer from on-prem via direct-link or a different VPC via transit gateway is not supported.
* Security Groups and Network Access Control List (NACL) on the load balancer itself are not supported.
* UDP is not supported in datapath.
* Autoscaler integration is not supported.

### Related Links
{: #nlb-limitations-related-links}

* [Private Path service limitations](/docs/vpc?topic=vpc-ppsg-limitations)
