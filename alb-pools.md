---

copyright:
  years: 2020, 2026
lastupdated: "2026-05-02"

keywords: application load balancer, public, private, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

The {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) pool is a group of back-end targets that receive the inbound customer traffic from the load balancer and provide your outbound traffic response. The pool has a protocol, a load-balancing algorithm, the back-end targets (VPC instances), the health checks for the back-end targets, and the session stickiness.
{: shortdesc}

# Working with application load balancer pools
{: #alb-pools}

Multiple pools can be associated with a listener when the secondary pools are configured as `failsafe_policy.target`. Duplicate pool assignments are not allowed; each pool must be unique per listener. This feature is available only for `Application` family load balancers. When using a `failsafe_policy.target`, the `protocol` must match the primary pool or use an allowed compatible protocol. At this time, only `http` and `https` are considered compatible.

In a load balancer configuration, a listener is considered the parent resource. You can associate pools with that listener in two ways, by referencing them directly or indirectly. For direct association, configure the pool as the listener’s `default_pool`. For indirect association, reference the pool from another pool through a `failsafe_policy.target` relationship, ensuring that the other pool is already linked to the listener.
{: note}

You can configure pools when [creating a application load balancer](/docs/vpc?topic=vpc-load-balancers&interface=ui), or later with the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Application** > **Load balancers**.
1. Click the application load balancer that you want to change.
1. On the application load balancer's details page, click the back-end pools tab and select the pool that you want to edit.
1. Select the options for your pool:
   * **Pool Name**: The name for your pool. Ideally, a name that describes the function that is performed by this pool.
   * **Protocol**: Select the protocol for your instances in this pool. The protocol of the pool must match the protocol of its associated listener. For example, if an HTTPS or HTTP protocol is selected for the listener, the protocol of the pool must be HTTP. Similarly, if the listener protocol is TCP, the protocol of the pool must be TCP.

   HTTP sends data as plain text that can be intercepted and is considered insecure. It is recommended to choose protocol as `https` instead of `http`.
   For more details see: https://www.cloudflare.com/learning/ssl/why-is-http-not-secure/
   {: note}
   
   * **Method**: Select how you want the load balancer to distribute traffic across the instances in the pool:
       * **Round robin:** Forward requests to each instance in turn. All instances receive approximately an equal number of client connections.
       * **Weighted round robin:** Forward requests to each instance in proportion to its assigned weight. For example, you have instances A, B, and C, and their weights are set to `60`, `60` and `30`. Instances A and B receive an equal number of connections, and instance C receives half as many connections.
       * **Least connections:** Forward requests to the instance with the least number of connections at the current time.
   * **Session stickiness**: Whether all requests during a user's session are sent to the same instance.
   * **Health check**: Configure how the load balancer checks the health of the instances. For information about configuring health checks, see [Working with health checks](/docs/vpc?topic=vpc-alb-health-checks#lb-health-checks).
1. You can add a backup pool to any existing pool by selecting `failsafe policy` with following menu options. Make sure that, there is at least one pool already existing in the load balancer.
   * **Action**: The action to perform if primary pool is unhealthy. One of: `forward` or `drop` or `fail`
      * When editing a back-end pool in a load balancer, you can specify one of the following failsafe policy actions:
         * **Forward:** - The load balancer routes requests to a designated backup pool. This provides a clean failover path to another set of application servers. You must have an  existing backup pool configured and ready to receive traffic.
         * **Drop:** -  The load balancer drops all incoming requests, and the client receives no response.
         * **Fail:** - The load balancer rejects requests with an HTTP 503 ("Service Unavailable") status code, informing the client that the service is temporarily down.
   * **Target**: The selection of the backup pool is done here. If a pool specified, then the `action` must be `forward`

If instances in the pool are unhealthy and you believe that your application is running fine, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
{: tip}
