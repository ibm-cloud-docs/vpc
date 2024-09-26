---

copyright:
  years:  2021, 2023
lastupdated: "2024-09-26"

keywords: IBM Cloud monitoring, vpc monitoring, dashboards, dashboard

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# IBM Cloud VPC monitoring dashboards
{: #ibm-monitoring}

You can monitor various aspects of VPC services with {{site.data.keyword.monitoringlong}} dashboards. For more information about {{site.data.keyword.monitoringlong_notm}}, see the {{site.data.keyword.monitoringlong_notm}} [Getting started tutorial](/docs/monitoring?topic=monitoring-getting-started).

## VPC service metric definitions
{: #vpc-metric-definitions}

Each monitoring dashboard has the relevant metrics for its associated service as defined in Table 1.

| Dashboard name | Metric definition dictionary |
|----------|----------|
| Virtual Server for VPC Overview | [VPC virtual server instances metrics definitions](/docs/vpc?topic=vpc-vpc-monitoring-metrics) |
| VPC Gen 2 VPN | [Monitoring VPN for VPC metrics](/docs/vpc?topic=vpc-vpn-monitoring-metrics&interface=ui) |
| VPC Gen 2 Load Balancer | [Monitoring Application Load Balancer for VPC metrics](/docs/vpc?topic=vpc-monitoring-metrics-alb) |
| VPC Gen 2 Load Balancer | [Monitoring Network Load Balancer for VPC metrics](/docs/vpc?topic=vpc-nlb_monitoring-metrics) |
| Flow Logs for VPC Overview | [Monitoring flow logs for VPC metrics](/docs/vpc?topic=vpc-fl-monitoring-metrics) |
| VPC Infrastructure Service Resource Quota Overview | [VPC quota metrics definitions](/docs/vpc?topic=vpc-vpc-quota-metrics) |
| {{site.data.keyword.filestorage_vpc_short}} | [Monitoring metrics for {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-fs-vpc-monitoring-sysdig) |
{: caption="Table 1: VPC metric definitions" caption-side="bottom"}

To see a list of Cloud services outside of VPC that offer monitoring, see [Cloud services](/docs/monitoring?topic=monitoring-cloud_services).

## View a specific monitoring dashboard
{: #view-specific-dashboard}

1. To view a specific dashboard on the monitoring, first go to the [monitoring UI](/observe/monitoring). For more information, see [Navigating to the web UI](/docs/monitoring?topic=monitoring-launch) for Monitoring.
2. In the Monitoring UI, select the instance that you want to view monitoring metrics for. Then, select Open Dashboard.
3. Select the Dashboards tile, the Dashboards menu expands.
4. Under DASHBOARD TEMPLATES, expand the `IBM` tab to select the dashboard you want to access. The names of available dashboards are detailed in Table 1.

## Limitations of {{site.data.keyword.filestorage_vpc_short}} monitoring
{: #fs-metrics-limitations}

The system queries file share information hourly. When you create a file share or increase its capacity, it can take up to an hour for this information to appear on the monitoring dashboards.
