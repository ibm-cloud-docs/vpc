---

copyright:
  years: 2019, 2020

lastupdated: "2020-03-03"

keywords: monitoring

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Monitoring your instances
{: #monitoring}

You can monitor CPU, volume, memory, and network usage of your virtual server instances after you set up an instance of the {{site.data.keyword.monitoringlong}} service. 
{: shortdesc}

## Before you begin
{: #before-monitoring}

Because the monitoring data for virtual server instances is stored in {{site.data.keyword.monitoringlong_notm}}, you must have an instance of the monitoring service in your account. 

To create a monitoring instance, you must have administrator access for the account.
{: important}

## Viewing metrics
{: #monitoring-instances} 

You can view metrics about your virtual server instances over time in the {{site.data.keyword.cloud_notm}} console. Metrics are retained in the system for two weeks. If the monitoring instance was provisioned with the Premium plan, metrics are retained for 45 days.

To view metrics for your virtual server instances:
1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. Click the name of a virtual server instance that you want to monitor.
3. Select **Monitoring** in the navigation pane. 

It can take some time until metrics are displayed after the monitoring instance is provisioned. 
{: tip}

You can download selected metrics and graphs in CSV format. Scroll to the end of the page and click **Download CSV**.

## Default metrics  
{: #default-metrics} 

The following metrics are available (by default) from the console. 

### CPU
{: #cpu-metrics}

| Metric Name | Description |
|----------|-------------|
| Average CPU [CPU number]| CPU utilization on the virtual server instance|
{: caption="Table 1: Default CPU metrics available from console" caption-side="top"}

### Volume
{: #volume-metrics}

| Metric Name | Description |
|----------|-------------|
| Disk [Disk name] Read | Number of read requests (IOPS) for the identified disk name |
| Disk [Disk name] Write| Number of write requests (IOPS) for the identified disk name|
{: caption="Table 2: Default volume metrics available from console" caption-side="top"}

Volume metrics are available in either IOPS requests or read/write requests by using the toggle selector. 
{: tip}

### Memory
{: #memory-metrics}

| Metric Name | Description |
|----------|-------------|
| Available | Total KB of memory used on a virtual server instance|
{: caption="Table 3: Default memory metrics available from console" caption-side="top"}

### Network
{: #network-metrics}

| Metric Name | Description |
|----------|-------------|
| KB in | Cumulative number of bytes received on a network interface since boot |
| KB out| Cumulative number of bytes transmitted on a network interface since boot |
{: caption="Table 3: Default network metrics available from console" caption-side="top"}

Network metrics are available in either packets or KB by using the toggle selector. 
{: tip}
