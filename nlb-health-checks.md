---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-24"

keywords: network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Working with health checks
{: #nlb-health-checks}

The {{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) conducts periodic health checks to monitor the health of the back-end ports, and it forwards client traffic to them accordingly. If a back-end server port is found to be unhealthy, no new connections are forwarded to it. The load balancer continues to monitor the health of unhealthy ports, and it resumes their use if they become healthy again, which means that they successfully pass the user-defined health check attempts.
{: shortdesc}

You can configure health checks when [creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer), or later with the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
2. Select the Menu icon ![Navigation Menu icon](../../icons/icon_hamburger.svg), then click **Infrastructure > Network > Load balancers**.
3. Click the NLB you want to change.
4. On the network load balancer details page, click the back-end pools tab then select the pool that you want to edit.
5. Select the new options for your health checks. You have the following options:

   * **Health check path**: Health path is applicable only if HTTP is selected as the health check protocol. The health path specifies the URL used by the load balancer to send the HTTP health check requests to the instances in the pool. By default, health checks are sent to the root path (/).
   * **Health protocol**: The protocol used by the load balancer to send health check messages to the instances in the pool.
   * **Health port**: The port on which to send health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
   * **Interval**: Interval in seconds between two consecutive health check attempts. By default, health checks are sent every 5 seconds.
   * **Timeout (sec)**: Maximum amount of time the system waits for a response from a health check request. By default, the load balancer waits 2 seconds for a response.
   * **Max retries**: Maximum number of health check attempts that the load balancer makes before an instance is declared unhealthy. By default, an instance is no longer considered healthy after two failed health checks.

   Although the load balancer stops sending connections to unhealthy instances, the load balancer continues monitoring the health of these instances and resumes their use if they're found healthy again (that is, if they successfully pass two consecutive health check attempts).

If instances in the pool are unhealthy and you believe that your application is running fine, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
{: tip}

Health check definitions are mandatory for back-end pools.
{: important}

The health checks for HTTP and TCP ports are conducted as follows:

* **HTTP:** An `HTTP GET` request against a pre-specified URL is sent to the back-end server port. The server port is marked healthy after receiving a `200 OK` response. The default `GET` health path is `/` through the UI, and it can be customized.
* **TCP:** The network load balancer attempts to open a TCP connection with the back-end server on the specified TCP port. The server port is marked healthy if the connection attempt is successful, and the connection is closed.

By default, health checks are sent every five seconds on the same port on which traffic is sent to the instance. By default, the load balancer waits 2 seconds for a response to the health check, and an instance is no longer considered healthy after 2 failed health checks.
