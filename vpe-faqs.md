---

copyright:
  years: 2020, 2025
lastupdated: "2025-10-13"

keywords: virtual private endpoint, FAQs, VPE, endpoint gateway

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for virtual private endpoints
{: #faqs-vpe}

You might encounter the following frequently asked questions when you use {{site.data.keyword.cloud_notm}} Virtual Private Endpoints (VPE) for VPC.

## Can I map {{site.data.keyword.cloud_notm}} services to a VPE from the service catalog?
{: #faq-cannot-map-services}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} services cannot be mapped to a VPE from the service catalog during the time of purchase.

## Can I map an {{site.data.keyword.cloud_notm}} service to a Public endpoint?
{: #faq-map-cloud-service-public-endpoint}
{: faq}
{: support}

Public endpoints of {{site.data.keyword.cloud_notm}} services are not eligible for VPE. VPE can be mapped only to a private endpoint of {{site.data.keyword.cloud_notm}} services.

## Is a VPE created in high-availability mode?
{: #faq-vpe-ha-mode}
{: faq}
{: support}

A VPE is not created in high-availability (HA) mode, by default. HA comes primarily from the {{site.data.keyword.cloud_notm}} service.

## Can I access an IBM Cloud service by using a private service endpoint IP address?
{: #faq-access-using-cse-adn}
{: faq}
{: support}

When an {{site.data.keyword.cloud_notm}} service is created, {{site.data.keyword.cloud_notm}} DNS Services are automatically set up to resolve the IBM Cloud service FQDN to the {{site.data.keyword.cloud_notm}} private service address.

When a VPE is created, VPE assigns a reserved IP with which you can access the {{site.data.keyword.cloud_notm}} service. It is recommended to use the reserved IP instead of the {{site.data.keyword.cloud_notm}} private service endpoint.

## Does mapping an IBM Cloud service to an IP address on a VPC network make the service private?
{: #faq-map-cloud-service-make-service-private}
{: faq}

Mapping an {{site.data.keyword.cloud_notm}} service to an IP address on a VPC network does not make the service private. For example, if a service has
a public endpoint, you can still access the public endpoint after the service is mapped.

## Does controlling access to an IP on a VPC network that is mapped to a service control access to the mapped service?
{: #faq-access-mapped-service}
{: faq}

Controlling access to an IP address on a VPC network that is mapped to an {{site.data.keyword.cloud_notm}} service does not control the access to the mapped service itself.

## Is there a limit to the total number of concurrent active connections to an endpoint gateway?
{: #faq-reserved-ips-and-number-ports}
{: faq}

Connections to a reserved IP address that is bound to the endpoint gateway are source NATed at the VPE gateway. The source IP becomes the reserved IP bound to the endpoint gateway, and a UDP/TCP source port is allocated for each connection. The total number of TCP/UDP ports is limited, and since every active TCP/UDP connection might consume a source port, the total number of active connections through a VPE gateway is limited.

To avoid port exhaustion, consider techniques such as long-running connections and connection-pooling to reduce the total number of active connections to a VPE.

## How many IP addresses can I use for NAT operations on the VPC gateway?
{: #faq-ip-nat-operations}
{: faq}

A finite pool of IP addresses is used for NAT operations on the VPC gateway. One IP address is required per VPC per zone.

## Related links
{: #vpe-faq-related-links}

[Planning for virtual private endpoint gateways](/docs/vpc?topic=vpc-vpe-planning-considerations)
