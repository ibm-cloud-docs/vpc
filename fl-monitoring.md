---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-29"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring flow logs for VPC metrics
{: #fl-monitoring-metrics}

Use {{site.data.keyword.mon_full}} to gain operational visibility into the performance and health of your applications, services, and platforms. It offers administrators, DevOps teams, and developers full stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards.
{: shortdesc}

## Platform metrics overview
{: #fl-platform-metrics-overview}

You can view platform metrics when you enable {{site.data.keyword.mon_full_notm}} on your {{site.data.keyword.cloud_notm}} platform. A monitoring instance must be configured in a region to monitor these metrics. For more information, see [Enabling platform metrics](/docs/monitoring?topic=monitoring-platform_metrics_enabling).

Before you enable {{site.data.keyword.mon_full_notm}} on your platform, keep in mind:

* You can configure only one instance of the {{site.data.keyword.mon_full_notm}} service per region to collect platform metrics.
* Platform metrics are regional. Metrics are monitored only from {{site.data.keyword.mon_full_notm}} services, which are in the same region of the instance that you want to monitor.
* Metrics are collected automatically and are available for monitoring through the {{site.data.keyword.mon_full_notm}}-enabled instance.

### Metric: Flow log bytes sent
{: #flow-log-bytes-sent}

The number of bytes sent to Cloud Object Storage from a flow log collector.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_flow_logs_cos_sent_byte`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation 2, flow log collector virtual server attachment` |
{: caption="Flow log bytes sent" caption-side="bottom"}

## Attributes for segmentation
{: #segmentation-attributes-fl}

### Global attributes
{: #global-segmentation-attributes-fl}

The following attributes are available for segmenting the previously listed metric.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Cloud type` | `ibm_ctype` | Type of IBM Cloud. Cloud type values are `public`, `dedicated`, or `local`. |
| `Location` | `ibm_location` | Location of the monitored resource. This value can be a region, data center, or global. |
| `Resource` | `ibm_resource` | Resource being measured by the service. Typically, this value is an identifying name or GUID. |
| `Resource type` | `ibm_resource_type` | Type of resource measured by the service. |
| `Scope` | `ibm_scope` | Scope of the account, organization, or space GUID that is associated with this metric. |
| `Service name` | `ibm_service_name` | Name of the service generating this metric. |
{: caption="Global attributes" caption-side="bottom"}

### Additional attributes
{: #additional-attributes-fl}

The following attributes are available for segmenting one or more attributes as described in the preceding reference. See the individual metrics for segmentation options.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `IBM IS Generation 2` | `ibm_is_generation` | IBM IS Generation ( `2` for Gen. 2) |
| `Flow log collector VPS attachment` | `ibm_is_flow_log_collector_instance` | The virtual server instance ID where the flow log collector is attached. |
{: caption="Additional attributes" caption-side="bottom"}

## Enabling metrics monitoring
{: #-fl}

To receive monitoring metrics, you must set up your {{site.data.keyword.mon_full_notm}} instance.

To receive monitoring metrics, use the following steps:

1. Navigate to the [metrics monitoring portal](/observe/monitoring){: external} and click **Create**.

1. Select a region for your monitoring instance.

   The region needs to match the location of your existing VPN gateway.
   {: important}

1. Choose your pricing plan.

   Pricing plan details are explained in the selection window. Select the plan that best meets your requirements.

1. Provide a unique service name for your instance. The name can be any name that you want. The name has no impact on functionality.

   Do not give multiple monitoring instances the same name.
   {: important}

1. Optionally, select a resource group. A resource group organizes account resources in customizable groupings. Any account resource that is managed by using {{site.data.keyword.iamlong}} (IAM) access control belongs to a resource group within your account.

   If you do not have any pre-configured resource groups, or have no reason to share this resource selectively, use the default selection.
   {: note}

   If your account has multiple resource groups, you can choose which group has access to this {{site.data.keyword.mon_full_notm}} instance. By using this selective access, metrics can be available to some resource groups and not to others.
   {: tip}

1. Check the **Enable Platform Metrics** checkbox. You must select this option to receive metrics from your VPN gateway.

1. Click **Create**. You are taken back to the monitoring metrics home page.

Within a few minutes, your new monitoring instance is displayed with several configurations. You might have to refresh your browser to see it.

## Accessing and viewing metrics
{: #viewing-metrics-fl}

To view metrics for a specific flow log, follow these steps:

1. From the Flow logs for VPC page, click the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg) associated with the flow log. Then, select **Monitoring** from the menu.
1. Launch the Monitoring dashboard.
