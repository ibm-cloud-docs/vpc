---

copyright:
  years: 2020, 2026
lastupdated: "2026-06-17"

keywords: application load balancer, public, private, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Working with application load balancer pools
{: #alb-pools}

The {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) pool is a group of back-end targets that receive the inbound traffic from the load balancer and provide your outbound traffic response. The pool includes a protocol, a load-balancing algorithm, back-end targets (VPC instances), health checks for the back-end targets, and session stickiness.
{: shortdesc}

In a load balancer configuration, a listener is considered the parent resource. You can associate pools with that listener in two ways, by referencing them directly or indirectly. For direct association, configure the pool as the listener’s `default_pool`. For indirect association, reference the pool from another pool through a `failsafe_policy.target` relationship, ensuring that the other pool is already linked to the listener.
{: note}

The following conditions apply to all application load balancer pools:

- You can associate multiple pools with a listener when secondary pools are configured as `failsafe_policy.target`.
- Duplicate pool assignments are not allowed; each pool must be unique per listener.
- This feature is available only for `Application` family load balancers.
- When you use a `failsafe_policy.target`, the `protocol` must match the primary pool protocol or use an allowed compatible protocol. Currently, only `http` and `https` are considered compatible.

You can configure pools when [creating a application load balancer](/docs/vpc?topic=vpc-load-balancers&interface=ui), or later with the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Click the application load balancer that you want to modify.
1. On the application load balancer's details page, click the **Back-end pools** tab, and then select the pool that you want to edit.
1. Configure the following options for your pool:
   * **Pool Name**: The name for your pool. Ideally, a name that describes the function that is performed by this pool.
   * **Protocol**: Select the protocol for your instances in this pool. The protocol of the pool must match the protocol of its associated listener. For example, if the listener uses HTTP, the pool must also use HTTP. Similarly, if the listener uses TCP, the pool must also use TCP.

   HTTP sends data as plain text that can be intercepted and is considered insecure. It is recommended to choose protocol as `https` instead of `http`.
   For more details see: https://www.cloudflare.com/learning/ssl/why-is-http-not-secure/
   {: note}

   * **Method**: Select how the load balancer distributes traffic across the instances in the pool:
       * **Round robin:** Forward requests to each instance in turn. All instances receive approximately an equal number of client connections.
       * **Weighted round robin:** Forward requests to each instance in proportion to its assigned weight. For example, you have instances A, B, and C, and their weights are set to `60`, `60` and `30`. Instances A and B receive an equal number of connections, and instance C receives half as many connections.
       * **Least connections:** Forward requests to the instance with the least number of connections at the current time.
   * **Session stickiness**: Select Whether all requests during a user's session are sent to the same instance.
   * **Health check**: Configure how the load balancer checks the health of the instances. For information about configuring health checks, see [Working with health checks](/docs/vpc?topic=vpc-alb-health-checks#lb-health-checks).
1. Select **Request settings (optional)** for your health checks. If you do not specify values, the default health check settings are used. The following options are available:
    * **Request Method**: Choose one of `GET` or `POST`. You can customize Request headers for both methods. You can optionally customize a Request body when you use the `POST` method.
    * **Request Body**: Specify the HTTP request body to use for health checks. If no value is specified, health check requests do not include a request body.
    * **Host header**: Include host header to ensure that the request uses HTTP/1.1 protocol. Otherwise, the system defaults to HTTP/1.0.
    * **Add other request headers**: Add one or more additional request headers.
       - **Header name**: For `GET` request method, choose one of `content-type`, `accept`, `authorization`, `cookie`, `origin`, `referrer`, or `user-agent`. For `POST` request method, choose one of `content-type`, `content-length`, `application-json`, or `accept-encoding`.
       - **Value**: Enter the value that corresponds to the specified Header name.
1. Select **Response settings (optional)** for your health checks. Customize successful response values during health checks. If you do not specify values, the default health check settings are used. The following options are available:
    * **Response body**: Enter response body text.
    * **Response code**: You can specify multiple comma separated values within the range of `100`-`599`. To specify a range, use `XX`. For example, `2XX` matches response in the range `200`-`299`.
1. To add a backup pool to an existing pool, configure a `failsafe policy`. Ensure that at least one other pool already exists in the load balancer.
   * **Action**: The action to perform when the primary pool becomes unhealthy. When you edit a back-end pool in a load balancer, you can specify one of the following `failsafe policy` actions:
      * **Forward**: - The load balancer routes requests to a designated backup pool. This action provides a clean failover path to another set of application servers. You must have an existing backup pool configured and ready to receive traffic.
      * **Drop**: - The load balancer drops all incoming requests, and the client receives no response.
      * **Fail**: - The load balancer rejects requests with an HTTP 503 ("Service Unavailable") status code, informing the client that the service is temporarily down.
   * **Target**: The selection of the backup pool is done here. If you specify a target pool, then the `action` value must be `forward`

If instances in the pool are unhealthy and your application is running fine, verify the health protocol and health path values. Also verify that any security groups attached to the instances allow traffic between the load balancer and the instances.
{: tip}

## Add members to application load balancer pools
{: #alb-pools-add-members}

You can add members to application load balancer pools after [creating a application load balancer](/docs/vpc?topic=vpc-load-balancers&interface=ui), with the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Click the application load balancer that you want to modify.
1. On the application load balancer's details page, click the **Back-end pools** tab and then select the pool that you want to edit.
1. Select the **Members** tab and then click **Attach Members +**.
1. Configure the following member options:
   * **Member type**: Select one of `Compute server instances` or `Other`. Compute devices include Virtual Server Instances and Bare Metal Servers within the selected VPC. To attach other server instances, such as in PowerVS, choose `Other`.
   * **Add member details**:
      * **Type**: Select one of `IP address` or `FQDN`.
      * **Address value**: For `IP address` type, enter an IP address. for `FQDN`, enter the domain name.
      * **Server Port**: Enter server port value.
1. Click **Add +** to add a new member. Repeat the previous steps to create any additional new members before attaching all new members.
1. Click **Attach** to save and attach your members.
