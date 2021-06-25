---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-25"

keywords: VPE, virtual private endpoints, limitations, endpoint gateway

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

* The following items are not supported for {{site.data.keyword.cloud_notm}} Virtual Private Endpoints for VPC:

   * VPE access over {{site.data.keyword.cloud}} Direct Link is not supported. VPC access from another VPC, or other environments connected via Transit Gateway, is also not supported. VPE is accessible only from within the VPC where it is created.

      As a workaround, you can use a private application load balancer to access VPE from Direct Link, or other environments connected via Transit Gateway. For more information, see [Accessing private API endpoints from an on-premises network using IBM Cloud Direct Link](/docs/vpc?topic=vpc-end-to-end-private-connectivity).
      {: note}
   * {{site.data.keyword.cloud_notm}} Security Groups and Flow Logs for VPC
   * Services that are in zones and regions other than [IBM Cloud Multi-Zone Regions (MZRs)](/docs/overview?topic=overview-locations#mzr-table)

* The following items are architectural restrictions:

   * Virtual private endpoints support only IPv4 addressing.
   * Each endpoint gateway is bound to a single VPC network.
   * Each endpoint gateway to a service mapping varies based on the service that you are enabling. For best practices, consult the documentation for the specific {{site.data.keyword.cloud_notm}} service that you are enabling.
