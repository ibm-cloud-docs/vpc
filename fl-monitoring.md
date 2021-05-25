---

copyright:
  years: 2020,2021
lastupdated: "2021-05-25"

keywords:

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

# Monitoring flow logs for VPC metrics
{: #fl-monitoring-metrics}

Use {{site.data.keyword.mon_full}} to gain operational visibility into the performance and health of your applications, services, and platforms. It offers administrators, DevOps teams, and developers full stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards.
{:shortdesc}

## Platform metrics overview
{: platform-metrics-overview}

You can view platform metrics when you enable {{site.data.keyword.mon_full_notm}} on your {{site.data.keyword.cloud_notm}} platform. A monitoring instance must be configured in a region to monitor these metrics. For more information, see [Enabling platform metrics](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-platform_metrics_enabling).

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
{: caption="Table 1: Flow log bytes sent" caption-side="top"}

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
{: caption="Table 2: Global attributes" caption-side="top"}

### Additional attributes
{: additional-attributes-fl}

The following attributes are available for segmenting one or more attributes as described in the preceding reference. See the individual metrics for segmentation options.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `IBM IS Generation 2` | `ibm_is_generation` | IBM IS Generation ( `2` for Gen. 2) |
| `Flow log collector VPS attachment` | `ibm_flow_log_collector_instance` | The virtual server instance ID where the flow log collector is attached. |
{: caption="Table 3: Additional attributes" caption-side="top"}
