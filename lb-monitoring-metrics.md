
---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-01"

keywords: l7, Layer-7, monitor, metrics, throughput, connection

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:preview: .preview}
{:download: .download}
{:note: .note}
{:important: .important}

# Monitoring metrics using IBM Load Balancer for VPC with Sysdig
{: #monitoring-metrics-sysdig}

{{site.data.keyword.cloud}} load balancer for VPC monitoring metrics are provided with {{site.data.keyword.cloud}} Monitoring with Sysdig. Sysdig is a third-party monitoring tool that specializes in data aggregation, usage alerts, and in-depth visualizations. For more information, see [IBM Cloud Monitoring with Sysdig](https://www.ibm.com/cloud/sysdig). Load balancers calculate the metrics and send those metrics to your Sysdig instance, which reflects different types of use and traffic. You can visualize and analyze metrics from either the {{site.data.keyword.cloud}} Monitoring with Sysdig dashboard, or its API.

## Metrics available by service plan
{: metrics-by-plan}

The supported monitoring metrics include:

* Active connections to your load balancer at a given time.
* Throughput of data passing through your load balancer over a given time.
* Connection rate, or an analysis of when more or less connections are made to your load balancer.

These metrics help track the traffic and usage patterns for your load balancers and can provide insight about peak traffic hours, usage dropoffs, and overall usage patterns.

Each metric is composed of the following metadata types:

* Metric name - The name for the collected metric.
* Metric type - Metric type determines whether the metric value is a counter metric or a gauge metric. Each of these metrics is of the type `gauge`, which represents a single numerical value that can arbitrarily fluctuate over time. 
* Value type - A unit of measurement for a specific metric. Examples include bytes or counts. A value type of `none` means that the metric value represents individual occurrences of that metric type.
* Segment - How you want Sysdig to divide and display the monitoring metrics.

### Active connections
{: #ibm_is_load_balancer_active_connections}

_Active connections_ are the number of connections established on a load balancer at a specific time. 

The active connection metric contains the following metadata:

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_active_connections` |
| Metric type | `gauge` |
| Value type | `none`  |
| Segment by | `IBM Load Balancer for VPC appliance metrics` and `IBM Load Balancer for VPC listener metrics` |
{: caption="Table 1: IBM Load Balancer for VPC active connections metrics metadata" caption-side="top"}


### Connection rate
{: #ibm_is_load_balancer_connection_rate}

_Connection rate_ is number of new, incoming active connections per second to your load balancer.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_connection_rate`|
| Metric type | `gauge` |
| Value type  | `none` |
| Segment by | `IBM Load Balancer for VPC appliance metrics` and `IBM Load Balancer for VPC listener metrics` |
{: caption="Table 2: IBM Load Balancer for VPC connection rate metric metadata" caption-side="top"}


### Throughput
{: #ibm_is_load_balancer_throughput}

_Throughput_ is the amount of data that passes in and out of a load balancer over a period of time.

| Metadata | Description |
|----------|-------------|
| Metric name | `ibm_is_load_balancer_throughput`|
| Metric type | `gauge` |
| Value type  | `byte` |
| Segment by | `IBM Load Balancer for VPC appliance metrics` and `IBM Load Balancer for VPC listener metrics` |
{: caption="Table 3: IBM Load Balancer for VPC throughput metric metadata" caption-side="top"}

## Metric segmentation
{: attributes}

You can split the data that Sysdig presents into various visualizations in the Sysdig dashboard, allowing views of different metrics based on your preferences. For example, if you have multiple load balancers or accounts with different load balancers in each account, you might want to focus on a particular listener port.

As an example, you can segment the `active connections` by `IBM Load Balancer for VPC listener port` to show how many active users are connected to the load balancer through each listener type. To illustrate this, let's assume that your load balancer has two different listener protocols one HTTP on port 80 and another for TCP on port 8080. The dashboard would contain different lines showing 10 users who are connected through HTTP on Port 80 in one color, and 6 users connected through TCP on port 8080 in another color. 

### Global attributes
{: global-attributes}

The following attributes are available for segmenting the three Sysdig metrics.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Resource` | `ibm_resource` | A load balancer's unique ID |
| `Scope` | `ibm_scope` | The account associated with a given load balancer |
| `Service name` | `ibm_service_name` | ibm-is-load-balancer |
{: caption="Table 4: Sysdig global attributes" caption-side="top"}

### Additional attributes
{: additional-attributes}

The following attributes are available to segment one or more of the global attributes. See the individual metrics for any segmentation options.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| IBM Load Balancer for VPC appliance metrics | `ibm_is_load_balancer_appliance_ip` | The metrics coming from the load balancer backlend. Because the load balancer is highly available, multiple appliances support each load balancer for redundancy.  |
| IBM Load Balancer for VPC listener metrics | `ibm_is_load_balancer_listener_port` | The metrics that are gathered from individual listeners and their ports. Configure the listeners in your load balancer settings. The monitoring metrics reflect the metrics coming from those listeners. |
{: caption="Table 5: Sysdig additional attributes" caption-side="top"}

The displayed metrics contain a timestamp in UNIX epoch time and the metric value for the time interval ending at that timestamp. You can specify different scopes, as well as the time interval over which to report the metrics.

The supported listener protocols include:

* HTTP
* HTTPS
* TCP

Specifying a listener port filters the metric by that listener. For example, if you don't specify a port, and the metric is `Throughput`, then Sysdig reports the total throughput for all listener protocols. However, if the protocol is HTTP on port 80, then Sysdig reports the throughput for HTTP traffic only.

You can also specify the time interval over which to report your metrics. Time intervals that are supported in the Sysdig dashboard are:

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

To receive monitoring metrics, you must set up your {{site.data.keyword.cloud}} Monitoring with Sysdig instance.

To do so, follow these steps:

1. Navigate to the [metrics monitoring portal](https://cloud.ibm.com/observe/monitoring), then click **Create a monitoring instance**.

2. Select a region for your Sysdig instance.

  If you do not have an existing load balancer, see [Using an IBM Load Balancer for VPC for server load balancing](INSERT LINK FOR VPC CREATE LOAD BALANCER) to provision one.
  {: tip}

  The region must match the location of your existing load balancer.
  {: important}

3. Choose your pricing plan.

  Pricing plan details are explained in the selection window. Select the plan that best meets your requirements.

4. Provide a service name for your instance. It can be any name that you want, and has no impact on functionality.

  Do not create multiple Sysdig instances with the same name.
  {: important}

5. Optionally, select a resource group. A resource group is a way to organize account resources in customizable groupings. Any account resource that is managed by using IBM Cloud Identity and Access Management (IAM) access control belongs to a resource group within your account.

  If you do not have any pre-configured resource groups, or no reason to share this resource selectively, use the default selection.
  {: note}

  If your account has multiple resource groups, you can choose which one has access to this Sysdig instance. This allows you to have metrics available to some resource groups and not to others.
  {: tip}

6. Select the **Enable Platform Metrics** checkbox. You must select this to receive metrics from your load balancer.

7. Click **Create**. You are taken back to the monitoring metrics home page.

Within a few minutes, your new Sysdig instance is displayed with several configurations. You might have to refresh your browser to see it.  

## Working with the Sysdig dashboard

To view and work with your Sysdig metrics, follow these steps:

1. Navigate to the [metrics monitoring portal](https://cloud.ibm.com/observe/monitoring).

2. Click **View Sysdig** next to the service name of the Sysdig instance you want to work with.

  The first time that you access your Sysdig instance, several windows display as part of the internal setup. Leave these selections with their default entries, and click through the pages until you reach the Sysdig main page.
  {: note}

3. Select **Dashboards** on the left sidebar to open the IBM Load Balancer Monitoring Metrics dashboard. Then, click **Default Dashboards > IBM > Load Balancer Monitoring Metrics**. The default dashboard is not editable.

4. Three main metrics in the dashboard are shown: Throughput, Active Connections, and Connection Rate. To modify parameters and segment your metrics by load balancer ID or listener port, you must create a custom dashboard. 

  ![Sysdig dashboard](images/metrics_3.png "Sysdig dashboard")

  You can choose what time window you'd like to see your metrics displayed for, using the bar on the bottom.
  {: tip}

## Creating a custom metrics dashboard

You can create your own dashboard to customize your monitoring metrics, such as viewing information about particular load balancers, or only seeing traffic that comes through particular listeners.

To customize your dashboard, follow these steps:

1. Navigate to the [metrics monitoring portal](https://cloud.ibm.com/observe/monitoring).

2. Click **View Sysdig** next to the service name of the Sysdig instance you want to work with. The dashboard displays.

3. On the left sidebar, select **Dashboards**. Then, click the green **+** sign in the panel.

  ![Add dashboard](images/metrics_custom_db.png "Add dashboard")

4. Select **Blank dashboard**, then select the type of visual representation you want.

  Sysdig offers eight different visualizations for your dashboard. Read the description for each visualization, then choose the one that best meets your requirements.

  **Line** ("View trends over time") is the easiest and most basic option. It is also the most frequently selected option. The examples in this topic show a Line-based visualization.
  {: note}

5. Configure your custom dashboard.

  * In the **Metrics** field, enter `ibm_is` to display the three IBM Sysdig load balancer metrics: `ibm_is_load_balancer_active_connections`, `ibm_is_load_balancer_connection_rate`, and `ibm_is_load_balancer_throughput`.

  You can monitor listener port traffic by enabling the `ibm_cloud_load_balancer_listener_port` metric.
  {: tip}

  * You can choose a scope to display in your dashboard by clicking **Override Dashboard Scope**. For example, you can display the metrics for a particular load balancer.

  * You can also set a segment to compare metrics across the scope you have defined. For example, you can look at throughput for a particular load balancer segmented by listener port.

6. Click **Save** for your new custom dashboard to be accessible.

  By default, the dashboard begins with the name "blank dashboard". You can change the name by selecting **Dashboards** from the sidebar, then clicking the Pencil icon next to the name.
  {: tip}

To return to the default Sysdig dashboard at any time, select **Dashboards > Default Dashboards > IBM > Load Balancer Monitoring Metrics**.

## Working with Sysdig via API
{: #metric-query-api}

You can also work with the Sysdig instance by using the metric query API. You might want to do this if you need raw data points or want to consume your metrics from a command-line interface rather than using the Sysdig dashboard.

After creating your IBM Cloud Monitoring Sysdig instance, you must collect the following two pieces of information.

* The Sysdig Monitor API token
* The endpoint of your IBM Cloud Monitoring Sysdig instance

To collect this information and start working with your Sysdig instance using metric query API, follow these steps:

1. Access the [Monitoring home page](https://cloud.ibm.com/observe/monitoring), and click **View Sysdig** next to the instance you want to work with. After the Sysdig dashboard displays, select your Account Profile icon on the left sidebar, then select **Settings**. Your account settings display.

  ![Settings](images/metrics_settings.png "Settings")

2. Your Sysdig Monitor API token is an alphanumeric string that is located in the **Sysdig Monitor API Token** field. Click the **Copy** button to the right of the key to transfer it to your clipboard.

	Do not share this API token. Anyone who has this API token has full access to your metrics.
	{: important}

3. To get the endpoint of your IBM Cloud Monitoring Sysdig instance, navigate to your main Sysdig dashboard in your browser. Then, select the URL to the dashboard, which appears similar to:

  ```
  https://us-south.monitoring.cloud.ibm.com/#/default-dashboard/ibm_is_load_balancer?last=3600
  ```

  The first part of the URL (in this case, `us-south.monitoring.cloud.ibm.com`) is your endpoint. Make note of it.

4. After you have both the API token and the endpoint, you can format your POST request. The following POST request is an example, with all the parameters that you can modify. These parameters are:

  * The Sysdig Monitor API token.
  * The endpoint of your Sysdig instance.
  * The value for `ibm_resource` (this is the load balancer ID you want to see metrics for).

     If you want to see this metric for all of your load balancers, do not enter a value for the `scope` attribute. For example, use `"scope” : “”`.
     {: note}

  * The metric type that you want to see the results for. This example uses `ibm_is_load_balancer_throughput`, but `ibm_is_load_balancer_active_connections` and `ibm_is_load_balancer_connection_rate` are also valid options.
  * The `from` and `to` attributes define the times to focus the scan, set in Epoch Time and in microseconds.
  * The `sampling` and `value` attributes set the granularity of which data is returned in the POST request.

      Because a large volume of data is stored in Sysdig, choosing the specific level of granularity is important. Sysdig can return only 600 data points at any time with a given request. As a result, the `sampling` and `value` attributes are important. Leaving these two lines out of your request will return an aggregate sum over that time period instead.

      If the time range specified by `from` and `to` is large (for example, 4 days), but you define a `sampling` and `value` of 10 seconds, this means that you receive 4 days worth of data that is split into 10-second chunks. This is not a useful sampling due to the large amount of data returned. Specifying a larger chunk is recommended (for example, 1 hour instead of 10 seconds).
      {: tip}


  ```
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
            "scope": "ibm_resource=\"908461\"",
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
