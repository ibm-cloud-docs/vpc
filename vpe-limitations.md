---

copyright:
  years: 2020
lastupdated: "2020-10-30"

keywords: vpe, virtual private endpoints, limitations, endpoint gateway

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Virtual private endpoint limitations
{: #limitations-vpe}

The following items are not supported for {{site.data.keyword.cloud}} Virtual Private Endpoints for VPC:
{: shortdesc}

* {{site.data.keyword.cloud_notm}} Security Groups and Flow Logs for VPC
* VPE access over {{site.data.keyword.cloud_notm}} Direct Link (2.0)
* VPE access over {{site.data.keyword.cloud_notm}} Transit Gateway  
* Services that are in zones and regions other than [IBM Cloud Multi-Zone Regions (MZRs)](/docs/overview?topic=overview-locations#mzr-table)

The following items are architectural restrictions:

* Virtual private endpoints support only IPv4 addressing.
* Each endpoint gateway is bound to a single VPC network.
* Each endpoint gateway to a service mapping varies based on the service that you are enabling. For best practices, consult the documentation for the specific {{site.data.keyword.cloud_notm}} service that you are enabling.
