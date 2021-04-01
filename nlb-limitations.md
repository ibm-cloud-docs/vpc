---

copyright:
  years: 2018, 2021
lastupdated: "2021-03-23"

keywords:  

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.nlb_full}} limitations
{: #nlb-limitations}

Known limitations for {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) are as follows:

* The NLB requires the member and port combination to be unique. In other words, a member with instance X and port Y cannot be added to a pool if a member with instance X and port Y exists in another pool.
* There is a one-to-one mapping between listener and pool.
* All members that are associated with a network load balancer must be in the same zone as the load balancer.
* Two members with the same instance X and same port Y cannot exist at the same time for an NLB. This case is not supported and your traffic might not be routed correctly.
* Currently, the creation of private network load balancers is supported in LON, OSA, TOK, and WDC. 
* For Private NLB, the NLB service might add rules to custom routing tables to ensure service availability for some failure conditions. As a result, if the client is outside the zone and/or VPC of the NLB, you must add an ingress custom routing table to the VPC where the NLB resides with the proper traffic source selected.

   For Private NLB, depending on the location of the clients, you must ensure that ingress routing tables exist (as described in Table 1).

| Client Location | Routing Table Type	| Traffic Source |
|----|----|----|
| On-premises | Ingress | Direct Link |
| Another VPC or classic infrastructure | Ingress | Transit Gateway |
| Another Availability Zone of the same VPC |	Ingress	| VPC zone |
{: caption="Table 1: Traffic sources that require ingress custom routing tables" caption-side="top"}

   For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
   {: note}
