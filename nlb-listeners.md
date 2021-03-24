---

copyright:
  years: 2020
lastupdated: "2020-12-30"

keywords: network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Working with listeners
{: #nlb-listeners}

{{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) listeners are used to configure how the load balancer receives your traffic. The attributes that you can define on a listener are protocols, ports, and pools.

You can configure listeners when [creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer), or afterward by using the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){:external} and log in to your account.

2. Select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) from the upper left, then click **VPC Infrastructure > Load balancers**.

3. Click the NLB you want to change.

4. On the network load balancer details page, click the front-end listeners tab, then select the listener that you want to edit.

5. Select the new options for your listener. You have the following options:

  * **Protocol**: The network protocol for the inbound customer traffic.
  * **Port**: The port that the load balancer receives your inbound customer traffic.
  * **Back-end pool**: The pool the load balancer distributes to the inbound customer traffic.

When you use the UI, you must create a pool before you create a listener. You cannot set the pool of an existing listener to empty.
{: tip}
