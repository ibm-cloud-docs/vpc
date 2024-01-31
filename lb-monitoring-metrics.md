---

copyright:
  years: 2017, 2024
lastupdated: "2024-01-23"

keywords: l7, layer 7, monitor, metrics, connection

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring application load balancer metrics
{: #monitoring-metrics-alb}

{{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) monitoring metrics are provided with {{site.data.keyword.mon_full}}, a service that specializes in data aggregation, usage alerts, and in-depth visualizations. For more information, see [IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-getting-started).

Load balancers calculate the metrics and send those metrics to your monitoring instance, which reflects different types of use and traffic. You can visualize and analyze metrics from either the {{site.data.keyword.mon_full_notm}} dashboard, or its API.

## Metrics available by service plan
{: #lb-metrics-by-plan}

The supported monitoring metrics include:

* Active connections to your application load balancer at a given time
* Throughput of data passing through your load balancer over a given time
* Connection rate, or an analysis of when more or less connections are made to your load balancer
* Total number of HTTP/HTTPS requests received by the back-end
* Average response time for HTTP/HTTPS requests
* Count of responses with the HTTP 2xx back-end response code (successful request)
* Count of responses with the HTTP 3xx back-end response code (redirect)
* Count of responses with the HTTP 4xx back-end response code (client side failure)
* Count of responses with the HTTP 5xx back-end response code (server side error)
* Count of total back-end members in a pool
* Count of active back-end members in a pool

These metrics help track the traffic and usage patterns for your load balancers and can provide insight about peak traffic hours, usage drop offs, and overall usage patterns.

Each metric is composed of the following metadata types:

* Metric name - The name for the collected metric.
* Metric type - Metric type determines whether the metric value is a counter metric or a gauge metric. Each of these metrics is of the type `gauge`, which represents a single numerical value that can arbitrarily fluctuate over time.
* Value type - A unit of measurement for a specific metric. Examples include bytes or counts. A value type of `none` means that the metric value represents individual occurrences of that metric type.
* Segment - How you want to divide and display the monitoring metrics.

Only the resources in the same zone as the selected subnet can connect through this VPN gateway.

### Active connections
{: #lb_is_load_balancer_active_connections}

Active connections are the number of connections that are established on a load balancer at a specific time.

The active connection metric contains the following metadata:

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_active_connections` |
| Metric type | `gauge` |
| Value type | `none`  |
| Segment by | `Application load balancer appliance metrics` and `Application load balancer listener metrics` |
{: caption="Table 1: Application load balancer active connections metrics metadata" caption-side="bottom"}


### Connection rate
{: #lb_is_load_balancer_connection_rate}

Connection rate is the number of new incoming active connections per second to your load balancer.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_connection_rate`|
| Metric type | `gauge` |
| Value type  | `none` |
| Segment by | `Application load balancer appliance metrics` and `Application load balancer listener metrics` |
{: caption="Table 2: Application load balancer connection rate metric metadata" caption-side="bottom"}


### Throughput
{: #ibm_is_load_balancer_throughput}

Throughput is the amount of data that passes in and out of a load balancer over a period of time.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_throughput`|
| Metric type | `gauge` |
| Value type  | `byte` |
| Segment by | `Application load balancer appliance metrics` and `Application load balancer listener metrics` |
{: caption="Table 3: Application load balancer throughput metric metadata" caption-side="bottom"}

### Request Count
{: #ibm_is_load_balancer_request_count}

The request count is the total number of HTTP/HTTPS requests that are sent by the load balancer to the back-end servers per minute.
This metric is only available for HTTP and HTTPS listeners.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_request_count`|
| Metric type | `gauge` |
| Value type  | `none` |
| Segment by | `Application load balancer appliance metrics` and `Application load balancer listener metrics` |
{: caption="Table 4: Application load balancer request count metric metadata" caption-side="bottom"}

### Request Latency
{: #ibm_is_load_balancer_request_latency}

Request latency is the average of the response time of the last 1024 requests as seen by the load balancer for
a given HTTP/HTTPS listener. This metric requires at least 1024 responses to be received from the back-end for a
given listener for an accurate value to be reported. This metric is only available for HTTP and HTTPS listeners.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_request_latency`|
| Metric type | `gauge` |
| Value type  | `milliseconds` |
| Segment by | `Application load balancer appliance metrics` and `Application load balancer listener metrics` |
{: caption="Table 5: Application load balancer request latency metric metadata" caption-side="bottom"}

### HTTP 2xx response code count
{: #ibm_is_load_balancer_backend_http_2xx}

The HTTP 2xx response code count is the count of all HTTP response codes in the 200-299 range that is received
from the back-end per minute. This metric is only available for HTTP and HTTPS listeners.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_backend_http_2xx`|
| Metric type | `gauge` |
| Value type  | `none` |
| Segment by | `Application load balancer appliance metrics` and `Application load balancer listener metrics` |
{: caption="Table 6: Application load balancer HTTP 2xx response count metric metadata" caption-side="bottom"}

### HTTP 3xx response code count
{: #ibm_is_load_balancer_backend_http_3xx}

The HTTP 3xx response code count is the count of all HTTP response codes in the 300-399 range received
from the back-end per minute. This metric is only available for HTTP and HTTPS listeners.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_backend_http_3xx`|
| Metric type | `gauge` |
| Value type  | `none` |
| Segment by | `Application load balancer appliance metrics` and `Application load balancer listener metrics` |
{: caption="Table 7: Application load balancer HTTP 3xx response count metric metadata" caption-side="bottom"}

### HTTP 4xx response code count
{: #ibm_is_load_balancer_backend_http_4xx}

The HTTP 4xx response code count is the count of all HTTP response codes in the 400-499 range that is received
from the back-end per minute. This metric is only available for HTTP and HTTPS listeners.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_backend_http_4xx`|
| Metric type | `gauge` |
| Value type  | `none` |
| Segment by | `Application load balancer appliance metrics` and `Application load balancer listener metrics` |
{: caption="Table 8: Application load balancer HTTP 4xx response count metric metadata" caption-side="bottom"}

### HTTP 5xx response code count
{: #ibm_is_load_balancer_backend_http_5xx}

The HTTP 5xx response code count is the count of all HTTP response codes in the 500-599 range that is received
from the back-end per minute. This metric is only available for HTTP and HTTPS listeners.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_backend_http_5xx`|
| Metric type | `gauge` |
| Value type  | `none` |
| Segment by | `Application load balancer appliance metrics` and `Application load balancer listener metrics` |
{: caption="Table 9: Application load balancer HTTP 5xx response count metric metadata" caption-side="bottom"}

### Total Members
{: #ibm_is_load_balancer_backend_total_members}

The total members count is the number of members created in a pool. This metric is only available for TCP, HTTP, and HTTPS listeners.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_backend_total_members`|
| Metric type | `gauge` |
| Value type  | `none` |
| Segment by | `Application load balancer appliance metrics`, `Application load balancer listener metrics` and `Application load balancer pool metrics`|
{: caption="Table 10: Application load balancer total members count metric metadata" caption-side="bottom"}

### Active Members
{: #ibm_is_load_balancer_backend_active_members}

The active members count is the number of healthy members in a pool. This metric is only available for TCP, HTTP, and HTTPS listeners.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_backend_active_members`|
| Metric type | `gauge` |
| Value type  | `none` |
| Segment by | `Application load balancer appliance metrics`, `Application load balancer listener metrics` and `Application load balancer pool metrics`|
{: caption="Table 11: Application load balancer active members count metric metadata" caption-side="bottom"}

## Metric segmentation
{: #attributes-lb}

You can split the data into various visualizations in the {{site.data.keyword.mon_full_notm}} dashboard, allowing views of different metrics based on your preferences. For example, if you have multiple load balancers or accounts with different load balancers in each account, you might want to focus on a particular listener port.

As an example, you can segment the `active connections` by `Application load balancer listener port` to show how many active users are connected to the load balancer through each listener type. To illustrate this, let's assume that your load balancer has two different listener protocols one HTTP on port 80 and another for TCP on port 8080. The dashboard would contain different lines showing 10 users who are connected through HTTP on Port 80 in one color, and 6 users connected through TCP on port 8080 in another color.

### Global attributes
{: #lb-global-attributes}

The following attributes are available for segmenting the three metrics.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Resource` | `ibm_resource` | A load balancer's unique ID |
| `Scope` | `ibm_scope` | The account that is associated with a given load balancer |
| `Service name` | `ibm_service_name` | ibm-is-load-balancer |
{: caption="Table 12: Global attributes" caption-side="bottom"}

### Additional attributes
{: #lb-additional-attributes}

The following attributes are available to segment one or more of the global attributes. See the individual metrics for any segmentation options.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| Application load balancer appliance metrics | `ibm_is_load_balancer_appliance_ip` | The metrics coming from the load balancer back-end. Because the load balancer is highly available, multiple appliances support each load balancer for redundancy.  |
| Application load balancer listener metrics | `ibm_is_load_balancer_listener_port` | The metrics that are gathered from individual listeners and their ports. Configure the listeners in your load balancer settings. The monitoring metrics reflect the metrics coming from those listeners. |
| Application load balancer pool metrics | `ibm_is_load_balancer_pool_name` | The metrics that are gathered from individual listener ports and the health check port (if a health check port is configured) or the back-end member ports (if a health check port is not configured). You must configure the listeners, back-end pool with members and, if needed, the health check port in your load balancer settings. The monitoring metrics reflect the metrics coming from those listeners and health check port, or the back-end member ports. |
{: caption="Table 13: Additional attributes" caption-side="bottom"}

The following sample values are for the `ibm_is_load_balancer_pool_name` attribute in its different configurations. If the pool has no members, then this value will be in the format `Pool_ListenerPort`. If the pool has members and the health check port is configured, then this value will be in the format `Pool_ListenerPort:HealthCheckPort`. Otherwise, if the pool has members but no health check port configured, then the value will be `Pool_ListenerPort:Member1Port/Member2Port/...`.

| Listener Port | HealthCheck Port | Member1 Port | Member2 Port | Member3 Port | 'ibm_is_load_balancer_pool_name Value' |
| --- | --- | ---  | --- | --- | --- |
| 9999 | - | - | - | - | Pool_9999 |
| 80 | 80 | 80| 8090| - | Pool_80:80 |
| 8099 | 443 | 9080 | 7080 | 8888 | Pool_8099:443 |
| 8081 | - | 80| 8090| 9090 | Pool_8081:9090/8090/80 |
| 8088 | - | 9080 | 7080 | - | Pool_8088:9080/7080 |
{: caption="Table 14: Pool name attribute" caption-side="bottom"}

The displayed metrics contain a timestamp in UNIX epoch time and the metric value for the time interval ending at that timestamp. You can specify different scopes, as well as the time interval over which to report the metrics.

The supported listener protocols for IBM Cloud Application Load Balancer include:

* HTTP
* HTTPS
* TCP

Specifying a listener port filters the metric by that listener. For example, if you don't specify a port, and the metric is `Throughput`, then {{site.data.keyword.mon_full_notm}} reports the total throughput for all listener protocols. However, if the protocol is HTTP on port 80, then {{site.data.keyword.mon_full_notm}} reports the throughput for HTTP traffic only.

You can also specify the time interval over which to report your metrics. Time intervals that are supported in the dashboard are:

* 10 seconds
* 1 minute
* 10 minutes
* 1 hour
* 6 hours
* 2 weeks
* Custom

The number of data points you can report is roughly the same for each time interval. For example, if the interval is 1 hour, then each data point represents 5 minutes of data. If the interval is 2 weeks, then each data point represents 24 hours of data.

## Enabling metrics monitoring
{: #enable-metrics-monitoring}

To receive monitoring metrics, you must set up {{site.data.keyword.mon_full_notm}} with a monitoring instance.

To do so, follow these steps:

1. Navigate to the [metrics monitoring portal](/observe/monitoring){: external}, then click **Create a monitoring instance**.

1. Select a region for your monitoring instance.

   If you do not have an existing load balancer, see [Creating an IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancers&interface=ui) to provision one.
   {: tip}

   The region should match the location of your existing load balancer.
   {: important}

1. Choose your pricing plan.

   Pricing plan details are explained in the selection window. Select the plan that best meets your requirements.

1. Provide a service name for your instance. It can be any name that you want, and has no impact on functionality.

   Do not create multiple monitoring instances with the same name.
   {: important}

1. Optionally, select a resource group. A resource group is a way to organize account resources in customizable groupings. Any account resource that is managed by using IBM Cloud Identity and Access Management (IAM) access control belongs to a resource group within your account.

   If you do not have any pre-configured resource groups, or no reason to share this resource selectively, use the default selection.
   {: note}

   If your account has multiple resource groups, you can choose which one has access to this monitoring instance. This allows you to have metrics available to some resource groups and not to others.
   {: tip}

1. Select the **Enable Platform Metrics** checkbox. You must select this to receive metrics from your load balancer.

1. Click **Create**. You are taken back to the monitoring metrics home page.

Within a few minutes, your new monitoring instance is displayed with several configurations. You might have to refresh your browser to see it.  

## Working with the {{site.data.keyword.mon_full_notm}} dashboard
{: #working-with-monitoring-dashboard-alb}

To view and work with your metrics, follow these steps:

1. Navigate to the [metrics monitoring portal](/observe/monitoring){: external}.

1. Click **View IBM Cloud Monitoring** next to the service name of the monitoring instance that you want to work with.

   The first time that you access your monitoring instance, several windows display as part of the internal setup. Leave these selections with their default entries, and click through the pages until you reach the main page.
   {: note}

1. Select **Dashboards** on the left sidebar to open the IBM Load Balancer Monitoring Metrics dashboard. Then, click **Default Dashboards > IBM > Load Balancer Monitoring Metrics**. The default dashboard is not editable.

1. Eleven main metrics in the dashboard are shown: Throughput, Active Connections, Connection Rate, Request Count, Request Latency, HTTP_2xx Response Count, HTTP_3xx Response Count, HTTP_4xx Response Count, HTTP_5xx Response Count, Total Members and Active Members. You must create a custom dashboard to modify parameters and segment of your metrics by load balancer ID or listener port.

   You can choose what time window you'd like to see your metrics displayed for, using the bar at the end.
   {: tip}

## Creating a custom metrics dashboard
{: #creating-custom-metrics-dashboard-lb}

You can create your own dashboard to customize your monitoring metrics, such as viewing information about particular load balancers, or seeing traffic that comes only through particular listeners.

To customize your dashboard, follow these steps:

1. Navigate to the [metrics monitoring portal](/observe/monitoring){: external}.

1. Click **View IBM Cloud Monitoring** next to the service name of the monitoring instance you want to work with. The dashboard displays.

1. On the left sidebar, select **Dashboards**. Then, click the green **+** sign in the panel.

1. Select **Blank dashboard**, then select the type of visual representation you want.

   {{site.data.keyword.mon_full_notm}} offers eight different visualizations for your dashboard. Read the description for each visualization, then choose the one that best meets your requirements.

   **Line** ("View trends over time") is the easiest and most basic option. It is also the most frequently selected option. The examples in this topic show a Line-based visualization.
   {: note}

1. Configure your custom dashboard.

   * In the **Metrics** field, enter `ibm_is` to display the three load balancer metrics: `ibm_is_load_balancer_active_connections`, `ibm_is_load_balancer_connection_rate`, and `ibm_is_load_balancer_throughput`.

   You can monitor listener port traffic by enabling the `ibm_is_load_balancer_listener_port` metric.
   {: tip}

   * You can choose a scope to display in your dashboard by clicking **Override Dashboard Scope**. For example, you can display the metrics for a particular load balancer.

   * You can also set a segment to compare metrics across the scope that you defined. For example, you can look at throughput for a particular load balancer segmented by listener port.

1. Click **Save** for your new custom dashboard to be accessible.

   By default, the dashboard begins with the name "blank dashboard". You can change the name by selecting **Dashboards** from the sidebar, then clicking the Pencil icon next to the name.
   {: tip}

To return to the default dashboard at any time, select **Dashboards > Default Dashboards > IBM > Load Balancer Monitoring Metrics**.

## Working with {{site.data.keyword.mon_full_notm}} using the APIs
{: #metric-query-api}

You can also work with the monitoring instance by using the metric query APIs. You might want to do this if you need raw data points or want to consume your metrics from a command-line interface rather than using the {{site.data.keyword.mon_full_notm}} dashboard.

After creating your monitoring instance, you must collect the following two pieces of information.

* The Monitor API token
* The endpoint of your {{site.data.keyword.mon_full_notm}} instance

To collect this information and work with your monitoring instance using metric query API, follow these steps:

1. Access the [Monitoring home page](/observe/monitoring){: external}, and click **Open Dashboard** next to the instance you want to work with. After the dashboard displays, select your Account Profile icon on the left sidebar, then select **Settings**. Your account settings display.

1. Your Monitor API token is an alphanumeric string that is located in the **Monitor API Token** field. Click the **Copy** button to the right of the key to transfer it to your clipboard.

   Do not share this API token. Anyone who has this API token has full access to your metrics.
   {: important}

1. To get the endpoint of your {{site.data.keyword.mon_full_notm}} instance, navigate to your main dashboard in your browser. Then, select the URL to the dashboard, which appears similar to:

   ```sh
   https://us-south.monitoring.cloud.ibm.com/#/default-dashboard/ibm_is_load_balancer?last=3600
   ```
   {: pre}

   The first part of the URL (in this case, `us-south.monitoring.cloud.ibm.com`) is your endpoint. Make note of it.

1. After you have both the API token and the endpoint, you can format your POST request. The following POST request is an example, with all the parameters that you can modify. These parameters are:

   * The Monitor API token.
   * The endpoint of your monitoring instance.
   * The value for `ibm_resource` (this is the load balancer ID you want to see metrics for).

   If you want to see this metric for all of your load balancers, do not enter a value for the `scope` attribute. For example, use `"scope" : ""`.
   {: note}

   * The metric type that you want to see the results for. This example uses `ibm_is_load_balancer_throughput`, but `ibm_is_load_balancer_active_connections` and `ibm_is_load_balancer_connection_rate` are also valid options.
   * The `from` and `to` attributes define the times to focus the scan, set in Epoch Time and in microseconds.
   * The `sampling` and `value` attributes set the granularity of which data is returned in the POST request.

Because a large volume of data is stored in {{site.data.keyword.mon_full_notm}}, choosing the specific level of granularity is important. {{site.data.keyword.mon_full_notm}} can return only 600 data points at any time with a given request. As a result, the `sampling` and `value` attributes are important. Leaving these two lines out of your request returns an aggregate sum over that time period instead.

If the time range specified by `from` and `to` is large (for example, 4 days), but you define a `sampling` and `value` of 10 seconds, this means that you receive 4 days worth of data that is split into 10-second chunks. This is not a useful sampling due to the large amount of data returned. Specifying a larger chunk is recommended (for example, 1 hour instead of 10 seconds).
{: tip}


   ```sh
  curl \
  -H 'Authorization: Bearer <API_TOKEN>’ \
  -H 'Content-Type: application/json' \
  https://us-south.monitoring.cloud.ibm.com/api/data/batch  \
  -d '{
    "requests": [
        {
            "format": {
                "type": "data"
            },
            "scope": "ibm_resource = \"cfc851b1-f30b-4a06-b354-5b64526c0a69\"",
            "metrics": {
                "k0": "timestamp",
                “v1”: "ibm_is_load_balancer_throughput"
            },
            "time": {
                "from": 1584396900000000,
                "to": 1584402600000000,
                 “sampling”: 600000000
            },
            "group": {
                "by": [
                    {
                        "metric": "k0",
                        “value” : 600000000
                    }
                ],
                "aggregations": {
                    “v1”: "sum"
                },
                "groupAggregations": {
                    “v1”: "sum"
                }
            }
        }
    ]
}'
   ```
   {:  codeblock}
