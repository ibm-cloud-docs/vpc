---

copyright:
  years: 2020, 2026
lastupdated: "2026-03-30"

keywords: network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc network

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Working with network load balancer pools
{: #nlb-pools}

The {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) pool is a group of back-end targets that receive the inbound customer traffic from the load balancer and provide your outbound traffic response. The pool has a protocol, a load-balancing algorithm, the back-end targets (VPC instances), the health checks for the back-end targets, and the session stickiness. `Network` family load balancers do not support associating more than one pool with a single listener.
{: shortdesc}

In a load balancer configuration, a listener is considered the parent resource. You can associate pools with that listener in two ways, by referencing them directly or indirectly. For direct association, configure the pool as the listener’s `default_pool`. For indirect association, reference the pool from another pool through a `failsafe_policy.target` relationship, ensuring that the other pool is already linked to the listener.
{: note}

You can configure pools when [creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer), or later with the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Click the network load balancer that you want to change.
1. On the network load balancer's details page, click the back-end pools tab and select the pool that you want to edit.
1. Select the options for your pool:

   * **Pool Name**: The name for your pool. Ideally, a name that describes the function that is performed by this pool.
   * **Protocol**: The network traffic protocol for your traffic.
   * **Method**: The load-balancing algorithm for the pool.
   * **Session stickiness**: Whether all requests during a user's session are sent to the same instance.
   * **Health check**: For information about configuring health checks, see [Working with health checks](/docs/vpc?topic=vpc-nlb-health-checks#nlb-health-checks).

1. You can add a backup pool to any existing pool by selecting `failsafe policy` with following menu options. Make sure that, there is at least one pool already existing in the load balancer.
   * **Action**: The action to perform if primary pool is unhealthy. One of: `forward` or `drop`
      * When editing a back-end pool in a load balancer, you can specify one of the following failsafe policy actions:
         * **Forward:** - The load balancer routes requests to a designated backup pool. This provides a clean failover path to another set of application servers. You must have an existing backup pool configured and ready to receive traffic.
         * **Bypass:** - The load balancer sends requests directly to the member's destination IP addresses, bypassing the load balancer completely. This option is typically used in specific networking setups, such as with network load balancers and virtual network function (VNF) devices.
         * **Drop:** -  The load balancer drops all incoming requests, and the client receives no response.
   * **Target**: The selection of the backup pool is done here. If a pool specified, then the `action` must be `forward`

If instances in the pool are unhealthy and you believe that your application is running without issues, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
{: tip}
