---

copyright:
  years: 2020, 2026
lastupdated: "2026-06-16"

keywords: application load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Working with health checks
{: #alb-health-checks}

{{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) conducts periodic health checks to monitor the health of the back-end ports and forwards client traffic only to healthy targets. If a back-end server port is found to be unhealthy, the load balancer stops forwarding new connections to that port. The load balancer continues to monitor unhealthy ports and automatically resumes forwarding traffic when they become healthy again by successfully passing the configured health checks.
{: shortdesc}

You can configure health checks when [creating an application load balancer](/docs/vpc?topic=vpc-load-balancers&interface=ui), or later by performing the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.
1. Click the name of the load balancer that you want to change.
1. On the Load balancer details page, click the **Back-end pools** tab, then select the pool that you want to edit.
1. Click the name of the back-end pool that you want to edit, or click **Edit** from the Actions menu. 

   Configure the health check settings. The following options are available:

    * **Health check path**: Health path is applicable only if HTTPS is selected as the health check protocol. Specifies the URL path that the load balancer uses when sending health check requests to pool members. By default, health checks are sent to the root path (/).
    * **Health protocol**: The protocol used by the load balancer to send health check messages to the instances in the pool.
    * **Health port**: The port on which to send health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
    * **Interval**: The number of seconds between consecutive health check attempts. By default, health checks are run every 5 seconds.
    * **Timeout (sec)**: The maximum time that the load balancer waits for a health check response. By default, the timeout is 2 seconds.
    * **Max retries**: The number of consecutive failed health check attempts that are allowed before a target is marked unhealthy. By default, a target is marked unhealthy after two failed health checks. The load balancer continues monitoring unhealthy targets and resumes forwarding traffic after they successfully pass two consecutive health checks.
1. Select optional request settings for your health checks. You have the following options:
    * **Request settings (optional)**: Customize successful request values during health checks. If unspecified, health checks use default values.
    * **Request method**: Choose one of `GET` or `POST`. You can customize `request.headers` for both methods. You can optionally customize a `request.body` while using the `POST` method.
    * **Request body**: Specify the HTTP request body to use for health checks. If unspecified, health check requests do not have a request body.
    * **Host header**: Specify a host header to ensure request uses HTTP/1.1 protocol. Otherwise, the system defaults to HTTP/1.0.
    * **Add other request headers**: Add one or more additional request headers.
    * **Header name**: For a `GET` request method, choose one of `content-type`, `accept`, `authorization`, `cookie`, `origin`, `referrer`, or `user-agent`. For a `POST` request method, choose one of `content-type`, `content-length`, `application-json`, or `accept-encoding`.
    * **Value**: Enter the value that corresponds with the specified Header name.
1. Select optional response settings for your health checks. You have the following options:
    * **Response settings (optional)**: Customize successful response values during health checks. If unspecified, health checks will use default values.
    * **Response body**: Enter response body text.
    * **Response code**: You can specify multiple comma separated values within the range of 100-599. To specify a range, use XX. For example, 2XX for 200-299.

If instances in the pool are marked unhealthy but your application appears to be functioning correctly, double-check the health protocol and health check path settings. Also verify that any security groups attached to the instances allow traffic between the load balancer and the instances.
{: tip}

Health check definitions are mandatory for back-end pools. Health checks can be configured on back-end ports, or on a separate health check port based on the application.
{: important}

The health checks for HTTP, HTTPS, and TCP ports are conducted as follows:

* **HTTP:** An `HTTP GET` request against a pre-specified URL is sent to the back-end server health check port. The server port is marked healthy after receiving a `200 OK` response. The default `GET` health path is "/".

* **HTTPS:** Similar to an HTTP health check, an `HTTPS GET` request is sent to the configured health check port and URL path. Health checks use HTTPS to encrypt traffic to back-end servers. A back-end server is deemed healthy after receiving a `200 OK` response. The default `GET` health path is "/".

   

* **TCP:** The load balancer attempts to open a TCP connection with the back-end server on the specified TCP port. The server port is marked healthy if the connection attempt is successful, and the connection is closed.

By default, health checks run every 5 seconds on the same port on which traffic is sent to the instance. By default, the load balancer waits 2 seconds for a response to the health check, and an instance is no longer considered healthy after two failed health checks.
