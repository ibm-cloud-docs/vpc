---

copyright:
  years: 2020
lastupdated: "2020-08-10"

keywords: virtual private endpoint, faq, faqs, frequently asked questions, vpe, endpoint gateway

subcollection: vpc

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:beta: .beta}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# FAQs for Virtual Private Endpoints (Beta)
{: #faqs-vpe}

You might encounter the following frequently asked questions when using {{site.data.keyword.cloud_notm}} Virtual Private Endpoint (VPE) for VPC.

## Can I map IBM Cloud services to a VPE from the service catalog?
{: #faq-cannot-map-services}
{: faq}
{: support}

IBM Cloud services cannot be mapped to a VPE from the service catalog during the time of purchase.

## Can I map an IBM Cloud service to a Public endpoint?
{ #faq-map-cloud-service-public-endpoint}
{: faq}
{: support}

Public endpoints of IBM Cloud services are not eligible for VPE. VPE can only be mapped to a private endpoint of IBM Cloud services.

## Is a VPE created in high-availability mode?
{: #faq-vpe-ha-mode}
{: faq}
{: support}

A VPE is not created in high-availability (HA) mode, by default. HA comes primarily from the IBM Cloud services.

## Can I access an IBM Cloud service using either a CSE or ADN IP address?
{: #faq-access-using-cse-adn}
{: faq}
{: support}

When a VPE project deploys, any IBM Cloud service that is mapped to an IP address on a VPC network cannot be accessed directly using either an IBM Cloud Service Endpoint (CSE) or Application Delivery Network (ADN) IP address. Failing to do so results in unexpected connectivity issues.

## Does mapping an IBM Cloud service to an IP address on a VPC network make the service private?
{: #faq-map-cloud-service-make-service-private}
{: faq}

Mapping an IBM Cloud service to an IP address on a VPC network does not make the service private. For example, if a service has
a public endpoint, you can still access the public endpoint after the service is mapped.

## Does controlling access to an IP on a VPC network mapped to a service control access to the mapped service?
{: #faq-access-mapped-service}
{: faq}

Controlling access to an IP address on a VPC network mapped to an IBM Cloud service does not control the access to the mapped service itself.

## Is there a limit to the number of IP addresses I can bind to an endpoint gateway?
{: #faq-reserved-ips-and-number-ports}
{: faq}

When the reserved IP address bound to the endpoint gateway is source NATed on the VPC gateway, it is done using IP masquerading on the port. As the number of IP addresses bound to the endpoint gateway grows, the number of available ports to masquerade might become a concern.

## How many IP addresses can I use for NAT operations on the VPC gateway?
{: #faq-ip-nat-operations}
{: faq}

A finite pool of IP addresses is used for NAT operations on the VPC gateway. One IP address is required per VPC per zone.  
