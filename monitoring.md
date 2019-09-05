---

copyright:
  years: 2019

lastupdated: "2019-08-29"

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
{:shortdesc}

## Before you begin
{: #before-monitoring}

Because the monitoring data for virtual server instances is stored in {{site.data.keyword.monitoringlong_notm}}, you must have an instance of the monitoring service in your account. 

To create a monitoring instance, you must have administrator access for the account.
{: important}

## Setting up the monitoring service for VPC
{: #setup-monitoring}

To set up monitoring for virtual server instances in your VPC:
1. [Provision an instance of the {{site.data.keyword.monitoringshort}} service](/docs/services/cloud-monitoring/tutorials?topic=cloud-monitoring-provision) in the Dallas region.

  To check whether you already have a monitoring instance that can be used with your VPC, go to the [Resource list ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/resources){: new_window} page in the IBM Cloud console and expand the **Cloud Foundry Services** section. If there is an existing monitoring instance in the Dallas region, you can skip to the next step.
  {: tip}

1. Grant other users access to the monitoring instance so they can view metrics in the {{site.data.keyword.cloud_notm}} console.
  1. Go to the [IAM Users ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam/users){: new_window} page in the IBM Cloud console and select the user whose access you want to configure.
  1. On the **Cloud foundry access** tab, click **Assign organization** and assign the **Auditor** role for the organization and space where the monitoring instance is provisioned.
  3. On the **Access policies** tab, click **Assign access** and then click **Assign access to resources**.
  4. Select **Monitoring** for the service, select your instance, and assign the **Viewer** role.

1. {: #authenticate}Authenticate to the monitoring instance. On the [Resource list ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/resources){: new_window} page, expand the **Cloud Foundry Services** section and open the monitoring instance. Click **Open Monitoring Dashboard** and then click **Launch** to open the IBM Cloud Monitoring dashboard. The monitoring instance will start to store metrics for the virtual server instances in your VPC.

## Viewing metrics
{: #monitoring-instances} 

You can view metrics about your virtual server instances over time in the {{site.data.keyword.cloud_notm}} console. Metrics are retained in the system for two weeks. If the monitoring instance was provisioned with the Premium plan, metrics are retained for 45 days.

To view metrics for your virtual server instances:
1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. Click the name of a virtual server instance that you want to monitor.
3. Select **Monitoring** in the navigation pane. 

It can take some time until metrics are displayed after the monitoring instance is provisioned. 
{: tip}

You can download selected metrics and graphs in CSV format. Scroll down to the bottom of the page and click **Download CSV**.

Periodically, you might receive an error on the Monitoring page and metrics can't be displayed because the connection with the monitoring instance timed out. To fix this problem, reauthenticate to the monitoring instance, as described in step 3 of [Setting up the monitoring service for VPC](#setup-monitoring).
{: important}



