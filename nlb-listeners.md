---

copyright:
  years: 2020
lastupdated: "2022-12-12"

keywords: network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Working with listeners
{: #nlb-listeners}

{{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) listeners are used to configure how the load balancer receives your traffic. The attributes that you can define on a listener are protocols, ports, and pools.

You can configure listeners when [creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer), or later with the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.

2. Select the Navigation Menu icon ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click **VPC Infrastructure > Load balancers**.

3. Click the NLB that you want to change.

4. On the network load balancer details page, click the front-end listeners tab, then select the listener that you want to edit.

5. Select the new options for your listener. You have the following options:

   * **Protocol**: The network protocol for the inbound customer traffic.
   * **Port**: The port that the load balancer receives your inbound customer traffic.
   * **Back-end pool**: The pool the load balancer distributes to the inbound customer traffic.

When you use the UI, you must create a pool before you create a listener. You cannot set the pool of an existing listener to empty.
{: tip}
