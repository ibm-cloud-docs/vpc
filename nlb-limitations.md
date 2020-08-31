---

copyright:
  years: 2018, 2020
lastupdated: "2020-08-31"

keywords: network load balancer, limitations, issues, unique combinations, mapping, listener, pool, port

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

## Network load balancer limitations
{: #nlb-limitations}

Known limitations for network load balancers are as follows:

* The network load balancer requires the member and port combination to be unique. In other words, a member with instance X and port Y can not be added to a pool if a member with instance X and port Y already exists in another pool.
* There is a one-to-one mapping between listener and pool.
* All members associated with a network load balancer must be in the same zone as the load balancer.
* Two members with the same instance X and same port Y cannot exist at the same time for a network load balancer. This case is not supported and your traffic might not be routed correctly.
