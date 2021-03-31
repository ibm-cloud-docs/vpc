---

copyright:
  years: 2019, 2021
lastupdated: "2021-03-30"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Using {{site.data.keyword.la_full_notm}} to view VPN logs
{: #using-log-analysis-to-view-vpn-logs}

You can use {{site.data.keyword.la_full}} to view application and connection logs from your {{site.data.keyword.cloud}} {{site.data.keyword.vpn_vpc_short}} gateway.
{: shortdesc}

## Before you begin
{: #log-analysis-preparation}

To initiate sending VPN logs to {{site.data.keyword.la_full_notm}}, you need the following first:

* An {{site.data.keyword.la_full_notm}} instance (log retention is recommended)
* A VPN gateway

   After the VPN gateway gets provisioned, note the ID and region.
   {: tip}

## Sending VPN logs to {{site.data.keyword.la_full_notm}}
{: #sending-vpn-logs}

To send {{site.data.keyword.cloud_notm}} {{site.data.keyword.vpn_vpc_short}} logs in to an instance of {{site.data.keyword.la_full_notm}}, complete the following steps:

1. From the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) &gt; **Observability** to access the Observability dashboard.
1. Select **Logging**. The list of logging instances appears.
1. Click **Configure platform services logs**.
1. Select the region where the VPN gateway is provisioned and the logging instance that the logs should be sent to. This ensures logs from all VPN gateways in the selected region are sent to the chosen logging instance.
1. Click **Save**.

## Viewing VPN logs in {{site.data.keyword.la_full_notm}}
{: #viewing-vpn-logs}

View the {{site.data.keyword.cloud_notm}} {{site.data.keyword.vpn_vpc_short}} logs in the {{site.data.keyword.la_full_notm}} instance. You can apply the source filter for **is.vpn** to filter logs from {{site.data.keyword.vpn_vpc_short}}.

![Source filter](images/vpc-vpn-logdna-source-filter.png)

Additionally, you can enter the VPN gateway ID in the Search field to filter the logs specific to a VPN gateway.
