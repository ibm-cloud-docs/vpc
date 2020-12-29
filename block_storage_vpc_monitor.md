---

copyright:
  years: 2019, 2020
lastupdated: "2020-12-29"

keywords:

subcollection: vpc


---

{:shortdesc: .Shortdesc}
{: codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .Aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Monitoring block storage volumes
{: #monitor-block-storage-vpc}

To make sure you're always aware of any issues with your block storage volumes, you can monitor IOPS, latency, and throughput for your block storage volumes over time.
{:shortdesc}

## Procedure
{: monitor-volume-procedure}

To monitor your block storage volume in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. In {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. From the list of all block storage volumes, click the name of the volume that you want to monitor.
1. Select **Monitoring** in the navigation pane.

The monitoring page for the volume is displayed, where you specify the type of monitoring you want and time range in which to monitor. Table 1 describes these settings. After you make your selections, the graphs are automatically populated.

| Setting | Description |
|---------|-------------|
| View Graph | Select any combination of IOPS, latency, or throughput for monitoring data by category. By default, all graphs are selected. |
| View history | Select the time period for which you are monitoring the volume. By default, the last 7 days are monitored. You can select different predefined timeframe or specify a custom date range.  For custom, use the "To" and "From" settings. |
| From | When you specify a custom date range, click the calendar icon and select a start date. |
| To | Select an end date for the monitoring range. |
| Time Zone (UTC) | Specify the time zone as an integral of UTC.  For example, Eastern Standard time in the US is -05 UTC. |
{: caption="Table 1. Block storage volume monitoring settings" caption-side="top"}

## IOPS, Latency, and Throughput Graphs
{: monitor-volume-metrics}

By default, all **reads** from the volume are shown in the resulting graphs. Under **Available metrics**, you can change the view by selecting the type of metric per graph (read, write, or total):

* IOPS - View all reads, writes, or total read/write metrics. IOPS are shown in integrals of .05/IOPS.
* Latency - View either reads or writes. Latency is shown in integrals of 5 ms.
* Throughput  - View all reads, writes, or total read/write metrics. Throughput is shown in integrals of .005 KB.

You can save a snapshot of the metrics in a CSV file. Click **Download CSV**.

## Next steps
{: #next-step-monitor-block-storage-vpc}

* [View details about a block storage volume](/docs/vpc?topic=vpc-viewing-block-storage)
* [Manage block storage volumes](/docs/vpc?topic=vpc-managing-block-storage)
