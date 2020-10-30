---

copyright:
  years: 2020
lastupdated: "2020-10-30"

keywords: virtual private endpoints, endpoint gateway, vpe
subcollection: vpc

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

# About virtual private endpoint gateways
{: #about-vpe}

{{site.data.keyword.cloud}} Virtual Private Endpoints (VPE) for VPC enables you to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choosing, allocated from a subnet within your VPC.
{:shortdesc}

VPE is an evolution of the private connectivity to {{site.data.keyword.cloud_notm}} services. VPEs are virtual IP interfaces that are bound to an endpoint gateway created on a per service, or service instance, basis (depending on the service operation model). The endpoint gateway is a virtualized function that scales horizontally, is redundant and highly available, and spans all availability zones of your VPC. Endpoint gateways enable communications from virtual server instances within your VPC and {{site.data.keyword.cloud}} service on the private backbone. VPE for VPC gives you the experience of controlling all the private addressing within your cloud.

Similar to service endpoints, VPE for VPC provides private connectivity to IBM services, but within the VPC network of your choosing.
{: important}

## Overview of features
{: #vpe-feature-overview}

The features of VPE for VPC include:

* Public connectivity is not required and has no public data egress charges.
* Reaches {{site.data.keyword.cloud_notm}} assets through a private service provider.
* A VPE lives in your network address space, extending your private and multicloud into the {{site.data.keyword.cloud_notm}}.
* You can apply security through Network Access Control Lists (NACLs).
* The endpoint IP deploys in a customer-defined, virtual network.
* Includes platform integration to VPC - Identity and Access Management (IAM), Ghost, and ACLs.
* Access to new endpoints is achieved through the UI, CLI, and API.
* You can map a new endpoint to an existing service, as well as map to a shared endpoint. 

Currently, Network Time Protocol (NTP) is the only service supported.  
{: note}

## VPE connectivity patterns
{: #vpe-connectivity patterns}

VPE for VPC IP addresses use a multi-zone-region, logical endpoint gateway to connect to a service endpoint on the {{site.data.keyword.cloud_notm}} private backbone. The endpoint gateway is designed to support the best practice of binding a single IP from each zone of the VPC. You can create an endpoint gateway with  zero IP addresses and bind IPs as each zone is brought online.

As more {{site.data.keyword.cloud_notm}} services are enabled for VPE for VPC, each service instance will require you to configure its endpoint gateway, but will leverage the same topologies and best practices. For additional provisioning and best practices guidelines, refer to the documentation provided by the individual service.

### Single-zone topology

  ![VPE single-zone topology](/images/vpe-single-zone.png)

### Multi-zone topology

  ![VPE multi-zone topology](/images/vpe-multi-zone.png)

## Getting started
{: #vpe-getting-started}

To configure a virtual private endpoint, follow these steps:

1. List the available services, including {{site.data.keyword.cloud_notm}} infrastructure services available (by default) for all VPC users.
1. Review planning considerations. See [Planning for virtual private endpoint gateways](/docs/vpc?topic=vpc-planning-considerations) for details.
1. Create an endpoint gateway for each service that you want to be privately available to the VPC.
   See [Creating an endpoint gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway) for details.  
1. Bind a reserved IP address to the endpoint gateway.
   See [Binding and unbinding a reserved IP address](/docs/vpc?topic=vpc-bind-unbind-reserved-ip) for details.

After you create the endpoint gateway, virtual server instances in the VPC can access the {{site.data.keyword.cloud_notm}} service privately through it.

## Related links

These links provide additional information about {{site.data.keyword.cloud}} VPE for VPC:

* [VPE CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpe-clis)
* [VPE API reference](https://{DomainName}/apidocs/vpc)
* [FAQs for virtual private endpoints](/docs/vpc?topic=vpc-faqs-vpe)
* [Troubleshooting virtual private endpoints](/docs/vpc?topic=vpc-vpc-troubleshooting-vpe)
* [VPE Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-vpe) 
