---

copyright:
  years: 2021, 2025
lastupdated: "2025-03-21"

keywords: file share, file storage, sysdig, platform metrics

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring metrics for {{site.data.keyword.filestorage_vpc_short}}
{: #fs-vpc-monitoring-sysdig}

{{site.data.keyword.mon_full}} is a third-party cloud-native, and container-intelligence management system that you can include as part of your {{site.data.keyword.cloud_notm}} architecture. Use it to gain operational visibility into the performance and health of your applications, services, and platforms. It offers administrators, DevOps teams, and developers full-stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards. {{site.data.keyword.mon_full_notm}} is operated by Sysdig in partnership with {{site.data.keyword.IBM_notm}}. 
{: shortdesc}

With {{site.data.keyword.mon_full}}, you can view utilization metrics that measure the amount of transmitted data (throughput) and the number of read and write operations (IOPS) that are performed on the share. It can help you to determine how much work is done by your application or workload. You can use this information to determine whether the IOPS value needs to be adjusted. Monitoring the available capacity of your share can help you identify the need for more storage before insufficient space can become a problem with writing data to the share or replication. Seeing these metrics can help you anticipate any changes in charges at the end of the billing period. 

## Platform metrics overview
{: #sysdig-ov}

You can view platform metrics when you enable {{site.data.keyword.mon_full_notm}} on your {{site.data.keyword.cloud_notm}} platform. A monitoring instance must be configured in a region to monitor these metrics. For more information, see [Enabling platform metrics](/docs/monitoring?topic=monitoring-platform_metrics_enabling).

Before you enable {{site.data.keyword.mon_full_notm}} on your platform, keep in mind the following points.

* You can configure only one instance of the {{site.data.keyword.mon_full_notm}} service per region to collect platform metrics.
* Platform metrics are regional. Metrics are monitored only from {{site.data.keyword.mon_full_notm}} services, which are in the same region of the instance that you want to monitor.
* Metrics are collected automatically and are available for monitoring through the {{site.data.keyword.mon_full_notm}}-enabled instance.

## Enabling platform metrics
{: #sysdig-enable}

To receive monitoring metrics, you must set up your {{site.data.keyword.mon_full_notm}} instance.

1. In the {{site.data.keyword.cloud_notm}} console, go to the **Navigation menu** ![menu icon](../../icons/icon_hamburger.svg) > **Observability**.
1. Click **Monitoring**.
1. Click **Create**.
1. Select a region for your monitoring instance.

   The region needs to match the location of your file share.
   {: important}

1. Choose your pricing plan. Pricing plan details are explained in the selection window. Select the plan that best meets your requirements.
1. Provide a unique service name for your instance. The name can be any name that you want. The name has no impact on functionality. 

   Do not give multiple monitoring instances the same name.
   {: important}

1. Optionally, select a resource group. A resource group organizes account resources in customizable groupings. Any account resource that is managed by using {{site.data.keyword.iamlong}} (IAM) access control belongs to a resource group within your account.
   - If you do not have any pre-configured resource groups, or have no reason to share this resource selectively, use the default selection.
   - If your account has multiple resource groups, you can choose which group has access to this {{site.data.keyword.mon_full_notm}} instance. By using this selective access, metrics can be available to some resource groups and not to others.

1. Check the **Enable Platform Metrics** checkbox. You must select this option to receive metrics from your VPN gateway.
1. Click **Create**. You are taken back to the monitoring metrics home page.

Within a few minutes, your new monitoring instance is displayed with several configurations. You might have to refresh your browser to see it.

It can take some time until all the metrics are displayed after the monitoring instance is provisioned.
{: note}

## Viewing metrics
{: #sysdig-view}

### Launching Sysdig web UI from the {{site.data.keyword.filestorage_vpc_short}} details page
{: #sysdig-view-ui}

To launch the Sysdig web UI from your {{site.data.keyword.filestorage_vpc_short}} share details page complete the following steps.
1. In the {{site.data.keyword.cloud_notm}} console, go to the **Navigation menu** ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Storage > File storage shares**.
2. Click the name of a file share that you want to monitor.
3. Click the **Monitoring** tab in the File share details page. On this tab, you can see the graphs for share usage, total throughput, and total IOPS. These graphs are available to you at no cost, even without an {{site.data.keyword.mon_full_notm}} instance.
4. Click **Launch monitoring** to launch the Sysdig web UI to view the other metrics, customize your dashboards, set up alerts.
5. Select the monitoring instance, and click **Open dashboard**.

### Launching Sysdig web UI from the Observability page
{: #sysdig-view-ob}

To launch the Sysdig web UI from the Observability page, complete the following steps.
1. In the {{site.data.keyword.cloud_notm}} console, go to the **Navigation menu** ![menu icon](../../icons/icon_hamburger.svg) > **Observability**.
2. Click **Monitoring**.
3. Select the monitoring instance, and click **Open dashboard**.

## Predefined dashboard for {{site.data.keyword.filestorage_vpc_short}}
{: #sysdig-dashboards}

When you launch the file share dashboard, you can see the graphs that show the following metrics:

* Share usage - available capacity, used capacity, total capacity
* Throughput - read, write, and maximum throughput
* IOPS - read, write, maximum
* Snapshot space - available and used capacity for snapshots. 

The displayed metrics contain a timestamp in UNIX epoch time and the metric value for the time intervals that end at that timestamp. You can specify different scopes, and the time interval over which to report the metrics. The following time intervals are supported in the {{site.data.keyword.mon_full_notm}} dashboard:

* 10 seconds
* 1 minute
* 10 minutes
* 1 hour
* 6 hours
* 2 weeks
* Custom

However, the system queries file share information hourly. Therefore, selecting an interval that is less than 1 hour is not recommended.

When you create a file share, the data for the new share can take up to an hour or an hour and 15 minutes to appear in the dashboard. Changes in usage and share characteristics, such as an increase in capacity or a change in IOPS, can take from 15 to 30 minutes to be reflected in the graphs. Changes in snapshot space (adding or deleting snapshots) is reflected within 15 minutes.
{: note}

You can also create custom dashboards through the web UI or programmatically. You can back up and restore dashboards by using Python scripts or the {{site.data.keyword.mon_full_notm}} REST API. You can also copy and share dashboards through the web UI.
For more information, see [Dashboards](/docs/monitoring?topic=monitoring-monitoring#monitoring_dashboards).

### Metrics available by service plan
{: #sysdig-metrics-by-plan}

The following metrics help track the IO activity and throughput that is handled by your file shares and can provide insight about peak utilization and overall usage status.

|Name                           | Description|
|-------------------------------|------------|
| `ibm_is_share_throughput_read`  | Current read throughput | 
| `ibm_is_share_throughput_write` | Current write throughput | 
| `ibm_is_share_throughput_max`   | Maximum throughput configured on the share |
| `ibm_is_share_iops_read`       | Current read IOPS|
| `ibm_is_share_iops_write`       | Current write IOPS| 
| `ibm_is_share_iops_max`         | Maximum IOPS configured on the share 
| `ibm_is_share_capacity_total`   | Total allocated capacity|
| `ibm_is_share_capacity_used`    | Current used capacity| 
| `ibm_is_share_mount_targets_count` | Number of Share mount targets |
| `ibm_is_share_snapshot_capacity_used`  | Current capacity used by snapshots |
| `ibm_is_share_snapshot_capacity_total`  | Available capacity for snapshots. |
{: caption="Information about share metrics." caption-side="bottom"}

### Example of file share metrics
{: #sysdig-metrics-examples}

```text
ibm_is_share_throughput_read{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_throughput_write{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_throughput_max{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_iops_read{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_iops_write{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_iops_max{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_capacity_total{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_capacity_used{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_mount_targets_count_count{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_snapshot_capacity_used{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
ibm_is_share_snapshot_capacity_total{ibm_ctype="public", ibm_is_generation="2", ibm_location="us-south-1", ibm_resource="032ab79f-f2f8-4e1b-a676-d0c3faf9659a", ibm_resource_type="share", ibm_scope="7e11faa285f74ed19cc89f5d6fecb212", ibm_service_name="is", ibm_is_resource_name="test-share-1"}
```
{: screen}

## {{site.data.keyword.filestorage_vpc_short}} metrics dictionary
{: #sysdig-metrics-dictionary}

Each metric is composed of the following metadata types:

* **Metric name** - Name of the collected metric.
* **Metric type** - Determines whether the metric value is a counter metric or a gauge metric. Each of these metrics is of the `gauge` type, which represents a single numerical value that can arbitrarily fluctuate over time.
* **Value type** -  The file share metrics are displayed as integer and floating point numbers.
* **Segment** - How you want {{site.data.keyword.mon_full_notm}} to divide and display the monitoring metrics.

| Metric name                           | Metric Type  | Metric value type |
|---------------------------------------|--------------|-------------------|
| `ibm_is_share_throughput_read`          | Gauge        |Floating point     | 
| `ibm_is_share_throughput_write`         | Gauge        |Floating point     | 
| `ibm_is_share_throughput_max`           | Gauge        |Floating point     | 
| `ibm_is_share_iops_read`                | Gauge        |Integer            | 
| `ibm_is_share_iops_write`               | Gauge        |Integer            | 
| `ibm_is_share_iops_max`                 | Gauge        |Integer            | 
| `ibm_is_share_capacity_total`          | Gauge        |Floating point     | 
| `ibm_is_share_capacity_used`            | Gauge        |Floating point     | 
| `ibm_is_share_mount_targets_count`      | Gauge        |Integer            |
| `ibm_is_share_snapshot_capacity_used`  | Gauge  |Floating point |
| `ibm_is_share_snapshot_capacity_total` | Gauge  |Floating point |
{: caption="Available metrics" caption-side="bottom"}

## Attributes for segmentation
{: #sysdig-attributes}

You can split the metrics that {{site.data.keyword.mon_full_notm}} presents into various visualizations in the {{site.data.keyword.mon_full_notm}} dashboard, allowing views of different metrics based on your preference.

### Global attributes
{: #sysdig-attributes-global}

The following attributes are available for segmenting the file share metrics.

| Attribute      | Attribute Name | Attribute Description |
|----------------|----------------|-----------------------|
| `Cloud type`   | `ibm_ctype`    | Type of IBM Cloud. Cloud type values are `public`, `dedicated`, or `local`. |
| `Location`     | `ibm_location` | Location of the monitored resource. This value can be a region, data center, or global. |
| `Resource`     | `ibm_resource` | Resource being measured by the service. Typically, this value is an identifying name or GUID. |
| `Resource type`| `ibm_resource_type` | Type of resource that is measured by the service. |
| `Scope`        | `ibm_scope`    | Scope of the account, organization, or space GUID that is associated with this metric. |
| `Service name` | `ibm_service_name` | Name of the service that is generating this metric. |
{: caption="Global attributes" caption-side="bottom"}

### Other attributes
{: #sysdig-attributes-add}

The following attributes are available for segmenting one or more attributes as described in the preceding reference. See the individual metrics for segmentation options.

| Attribute             | Attribute Name      | Attribute Description |
|-----------------------|---------------------|-----------------------|
| `IBM IS Generation 2` | `ibm_is_generation` | IBM IS Generation ( `2` for Gen. 2) |
{: caption="Table 4: Additional attributes" caption-side="bottom"}

## Monitoring alerts
{: #monitoring-alerts}

In the {{site.data.keyword.mon_full_notm}} service, you can configure single alerts and multi-condition alerts to notify about problems that might require attention. When an alert is triggered, you can be notified through 1 or more notification channels. An alert definition can generate multi-channel notifications. 

For example, when your file share is low on available capacity, it can cause problems with replication. So you might want to create an alert for when your file share reaches 75%, 90%, and 95% of capacity used. When the metric threshold is reached, you can be notified of it through email, Slack, PagerDuty, or another channel. For more information, see [Working with alerts and events](/docs/monitoring?topic=monitoring-alerts).
