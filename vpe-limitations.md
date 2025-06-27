---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-27"

keywords: VPE, virtual private endpoints, limitations, endpoint gateway

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for virtual private endpoints
{: #limitations-vpe}

* The following items are not supported for {{site.data.keyword.cloud}} Virtual Private Endpoints for VPC:

   * Services that are in zones and regions other than [IBM Cloud Multi-Zone Regions (MZRs)](/docs/overview?topic=overview-locations#table-mzr)
   * {{site.data.keyword.cloud_notm}} Flow Logs for VPC
   * {{site.data.keyword.cloud_notm}} Bare Metal Servers for VPC
   * VPE for User Datagram Protocol (UDP) services, for example, Network Time Protocol (NTP), is not accessible over {{site.data.keyword.cloud_notm}} Direct Link and {{site.data.keyword.cloud_notm}} Transit Gateway.

* The following items are architectural restrictions:

   * Virtual private endpoints support only IPv4 addressing.
   * Each endpoint gateway is bound to a single VPC network.
   * Each endpoint gateway to a service mapping varies based on the service that you are enabling. For best practices, consult the documentation for the specific {{site.data.keyword.cloud_notm}} service that you are enabling.
