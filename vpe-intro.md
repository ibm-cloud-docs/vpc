---

copyright:
  years: 2020
lastupdated: "2020-09-03"

keywords: secure, region, zone, subnet, public gateway, floating IP, NAT, lbaas, vpnaas, lb, vpn, load balancer, virtual private network
subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:beta: .beta}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# About virtual private endpoints (Beta)
{: #about-vpe}

{{site.data.keyword.cloud}} Virtual Private Endpoint (VPE) for VPC enables you to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC virtual network by using the IP addresses of your choosing, allocated from a subnet within your VPC.
{:shortdesc}

Endpoint gateways are only available to accounts with special approval to preview this beta feature. Contact your IBM Sales representative if you are interested in getting access.
{:beta}

VPE is an evolution of the private connectivity to IBM Cloud services. VPEs are virtual IP interfaces that are bound to an endpoint gateway created on a per service, or service instance basis. The endpoint gateway is a virtualized function that scales horizontally, is redundant and highly available, and spans all availability zones of your VPC. Endpoint gateways enable communications from virtual server instances within your VPC and {{site.data.keyword.cloud}} service on the private backbone without adding availability or bandwidth constraints. VPE gives you the experience of controlling all the private addressing within your cloud.

A VPE is the equivalent to a [Cloud Service Endpoint (CSE)](/docs/account?topic=account-vrf-service-endpoint#service-endpoint), but in VPC infrastructure. Your account does not need to be enabled for virtual routing and forwarding (VRF) in VPC infrastructure to configure access to a VPE.
{: important}

**Notes**:

* Beta users can connect to service instances deployed in IBM Cloud service regions: Dallas, Washington D.C., London, Frankfurt, and Tokyo.
* A reserved IP address bound to an endpoint gateway can receive traffic from another zone of the same VPC.
* You can bind more than one IP address to an endpoint gateway; however, the current limit is one IP address per zone.
* You can access IBM Cloud services by using either CSEs or VPEs. However, if you want your VPC to enforce a certain behavior or discipline, it is recommended to block direct access to CSE IP addresses using network ACLs (NACLs). For more information, see [Configuring ACLs for use with virtual private endpoints](/docs/vpc?topic=vpc-vpe-configuring-acls).

## Overview of features
{: #vpe-feature-overview}

VPE for VPC offers the following features:

* Scales elastically for bursting and performance management.
* Does not require public connectivity and has no public data egress charges.
* Reach {{site.data.keyword.cloud_notm}} assets through a private service provider or, when combined with DNS Services, private fully qualified domain name within a VPC:
   * A VPE lives in your network address space, extending your private and multicloud into the {{site.data.keyword.cloud_notm}}.
   * You can apply security through Access Control Lists (ACLs).
   * The endpoint IP is deployed in a customer-defined virtual network.
   * Customer-driven and controlled, including ACLs.

* Platform integration to VPC - Identity and Access Management (IAM), Ghost, and ACLs.
* VPE for VPC supports:
   * New endpoints through the UI, CLI, and API.
   * Mapping a new endpoint to an existing service.
   * NAT-able (Network Address Translation) services only.
   * Mapping to a shared endpoint.
   * IP address only.

## Pricing
{: #vpe-pricing}

Customers are not charged for endpoint gateway instances, or bandwidth traversing the endpoint gateway to or from the service instance.

## Getting started
{: #vpe-getting-started}

To configure a virtual private endpoint, follow these steps:

1. List available services, including:

   * {{site.data.keyword.cloud_notm}} service instances available in the account.
   * {{site.data.keyword.cloud_notm}} infrastructure services available (by default) for all VPC users.

1. Create an endpoint gateway for each service that you want available privately to the VPC. See [Creating an endpoint gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway) for details.

1. Bind a reserved IP address to the endpoint gateway. See [Binding and unbinding a reserved IP address](/docs/vpc?topic=vpc-bind-unbind-reserved-ip) for details.

After the endpoint gateway is created, virtual server instances in the VPC can access the {{site.data.keyword.cloud_notm}} service privately through the endpoint gateway.

## Related links

These links provide additional information about the {{site.data.keyword.cloud}} Virtual Private Endpoints for VPC beta release.

* [VPE CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpe-clis)
* [VPE API reference (for allowlisted Beta participants only)](https://{DomainName}/apidocs/vpe-beta)
* [FAQs for virtual private endpoints](/docs/vpc?topic=vpc-faqs-vpe)
* [Troubleshooting virtual private endpoints](/docs/vpc?topic=vpc-vpc-troubleshooting-vpe)
* [VPE Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-vpe)
