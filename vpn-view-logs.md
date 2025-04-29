---

copyright:
  years: 2019, 2025
lastupdated: "2025-04-29"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using {{site.data.keyword.la_full_notm}} to view VPN logs
{: #using-log-analysis-to-view-vpn-logs}
{: ui} 

As of 28 March 2024, the {{site.data.keyword.la_full_notm}} service is deprecated and will no longer be supported as of 30 March 2025. Customers need to migrate to {{site.data.keyword.logs_full_notm}} before 30 March 2025. During the migration period, customers can use {{site.data.keyword.la_full_notm}} along with {{site.data.keyword.logs_full_notm}}. Logging is the same for both services. For information about migrating from {{site.data.keyword.la_full_notm}} to {{site.data.keyword.logs_full_notm}} and running the services in parallel, see [migration planning](/docs/cloud-logs?topic=cloud-logs-migration-intro).
{: deprecated} 

You can use {{site.data.keyword.la_full}} to view application and connection logs from your IBM Cloud VPN for VPC.
{: shortdesc}

## Before you begin
{: #log-analysis-preparation}

To initiate sending VPN logs to {{site.data.keyword.la_full_notm}}, you need the following first:

* An {{site.data.keyword.la_full_notm}} instance (log retention is recommended)
* A VPN gateway

   After the VPN gateway gets provisioned, note the ID and region.
   {: tip}

## Sending VPN logs to {{site.data.keyword.la_full_notm}} in the console
{: #sending-vpn-logs}
{: ui}

To send IBM Cloud VPN for VPC logs in to an instance of {{site.data.keyword.la_full_notm}}, follow these steps:

1. From the {{site.data.keyword.cloud_notm}} console, click the **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) > **Observability** to access the Observability dashboard.
1. Select **Logging**. The list of logging instances appears.
1. Click **Options** > **Edit platform**.
1. Select the region where the VPN gateway is provisioned and the logging instance that the logs should be sent to. This ensures logs from all VPN gateways in the selected region are sent to the chosen logging instance.
1. Confirm that you want to stop receiving platform logs to the previous region.
1. Click **Select**.

## Viewing VPN logs in {{site.data.keyword.la_full_notm}}
{: #viewing-vpn-logs}

To view the {{site.data.keyword.cloud_notm}} {{site.data.keyword.vpn_vpc_short}} logs in the {{site.data.keyword.la_full_notm}} instance:

1. Open the {{site.data.keyword.la_full_notm}} instance dashboard.
1. Apply the source filter for **is.vpn** to filter logs from {{site.data.keyword.vpn_vpc_short}}.
1. Click the **Sources** filter menu.
1. Click **Apply**.

Additionally, you can enter the VPN gateway ID in the Search field to filter the logs specific to a VPN gateway.
