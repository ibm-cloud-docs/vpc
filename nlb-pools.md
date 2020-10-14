---

copyright:
  years: 2020
lastupdated: "2020-07-10"

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

# Working with pools
{: #nlb-pools}

The {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) pool is a group of backend targets that receive the inbound customer traffic from the load balancer and provide your outbound traffic response. The pool has a protocol, a load balancing algorithm, the back end targets (VPC instances), the health checks for the back end targets, and the session stickiness.
{: shortdesc}

You can configure pools when [creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer), or afterwards using the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){:external} and log in to your account.

2. Select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) from the upper left, then click **VPC Infrastructure > Load balancers**.

3. Click on the network load balancer you want to change.

4. On the network load balancer details page, click the back-end pools tab and select the pool you want to edit.

5. Select the new options for your pool. You have the following options:

  * **Pool Name**: The name for your pool. Ideally, a name that describes the function performed by this pool.
  * **Protocol**: The network traffic protocol for your traffic.
  * **Method**: The load balancing algorithm for the pool.
  * **Session stickiness**: Whether or not all requests during a user's session are sent to the same instance.
  * **Health check**: For information about configuring health checks, see [Working with health checks](/docs/vpc?topic=vpc-nlb-health-checks#nlb-health-checks).
