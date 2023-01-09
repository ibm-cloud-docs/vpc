---

copyright:
  years: 2020, 2022
lastupdated: "2022-10-05"

keywords: VPE, virtual private endpoints, limitations, endpoint gateway

---

{{site.data.keyword.attribute-definition-list}}

# Virtual private endpoint limitations
{: #limitations-vpe}

* The following items are not supported for {{site.data.keyword.cloud_notm}} Virtual Private Endpoints for VPC:

   * Services that are in zones and regions other than [IBM Cloud Multi-Zone Regions (MZRs)](/docs/overview?topic=overview-locations#mzr-table) 
   * {{site.data.keyword.cloud_notm}} Flow Logs for VPC

* The following items are architectural restrictions:

   * Virtual private endpoints support only IPv4 addressing.
   * Each endpoint gateway is bound to a single VPC network.
   * Each endpoint gateway to a service mapping varies based on the service that you are enabling. For best practices, consult the documentation for the specific {{site.data.keyword.cloud_notm}} service that you are enabling.
