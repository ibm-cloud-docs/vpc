---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-24"

keywords: network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc network

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Working with pools
{: #nlb-pools}

The {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) pool is a group of back-end targets that receive the inbound customer traffic from the load balancer and provide your outbound traffic response. The pool has a protocol, a load-balancing algorithm, the back-end targets (VPC instances), the health checks for the back-end targets, and the session stickiness.
{: shortdesc}

You can configure pools when [creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer), or later with the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the Menu icon ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click **Infrastructure > Network > Load balancers**.
1. Click the network load balancer that you want to change.
1. On the network load balancer's details page, click the back-end pools tab and select the pool that you want to edit.
1. Select the options for your pool:

   * **Pool Name**: The name for your pool. Ideally, a name that describes the function that is performed by this pool.
   * **Protocol**: The network traffic protocol for your traffic.
   * **Method**: The load-balancing algorithm for the pool.
   * **Session stickiness**: Whether all requests during a user's session are sent to the same instance.
   * **Health check**: For information about configuring health checks, see [Working with health checks](/docs/vpc?topic=vpc-nlb-health-checks#nlb-health-checks).
