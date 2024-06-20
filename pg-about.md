---
copyright:
  years: 2022, 2024
lastupdated: "2024-06-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About public gateways
{: #about-public-gateways}

A public gateway enables a subnet and all its attached virtual server instances to connect to the internet. By default, subnets do not have access to the public internet. After a subnet is attached to a public gateway, all instances in that subnet can connect to the internet.
{: shortdesc}

Only one public gateway per zone is allowed in a VPC, but that public gateway can be attached to multiple subnets in the zone.
{: important}

## Comparing public gateways and floating IP addresses
{: #pg-comparison}

In addition to applying a public gateway to your VPC, you can assign floating IP addresses to any of your virtual server instances to enable them to be reachable from the internet, independent of whether its subnet is attached to a public gateway.

A single public gateway can grant multiple virtal server instances external connectivity for the same cost as one floating IP address.
{: note}

A public gateway only provides virtual server instances outbound connectivity, whereas a floating IP address provides virtual server instances outbound and inbound connectivity. A floating IP address exposes a service on the internet to inbound activity.

 Your public gateway assigns a floating IP that incurs a monthly charge. If you don't already have a paid account, you must upgrade to a paid account to create a public gateway.
 {: important}

All of your public gateway traffic flows out of the floating IP address associated with your public gateway. For example, if you have external services, this floating IP address is used to allow external services access to resources connected to your public gateway. The source address of your public gateway is always going to be the associated IP address.

Floating IP addresses and public gateways are independent objects. If a virtual server instance is assigned both a public gateway and floating IP address, the floating IP address always takes precedence on a virtual server instance. If you remove the floating IP address from the instance, then the server uses an assigned public gateway for external connectivity. You can add and remove floating IP addresses and public gateways at any time from a virtual server instance.

## Getting started
{: #pg-getting-started}

To get started using {{site.data.keyword.cloud}} public gateways, follow these steps:

1. Complete any [prerequisites](/docs/vpc?topic=vpc-create-public-gateways&interface=ui#pg-before-you-begin) before creating or connecting to a public gateway.
1. Decide if you need a public gateway and [Create your public gateway](/docs/vpc?topic=vpc-create-public-gateways&interface=ui#pg-creating-api).
1. Review the public gateway that you've created in the [{{site.data.keyword.cloud_notm}} console](/login){: external}.

## Public gateway use cases
{: #pg-use-cases}

Creating a public gateway is a standard way for a customer to acquire external connectivity for their services.

### Use case 1: External connectivity
{: #pg-use-case-external-connectivity}

You can create and assign a public gateway to a virtual server instance to provide your service with outbound connectivity to a third-party vendor service or external logging service. One public gateway can be assigned to multiple virtual server instances and subnets.

The following diagram demonstrates the difference in applying external connectivity to a service through a public gateway and a floating IP address. In this scenario, three virtual server instances are connecting to services and customers through a public gateway and floating IP address.

External service 1 and External service 2 (logging) receive outbound traffic from Virtual server instance 1 and Virtual server instance 2 through a single Public gateway connection. Floating IP address 1 associated with this public gateway connection allows the Virtual server instances to access External service 1 by IP address through a firewall. The External customer sends and receives traffic to and from Virtual server instance 3 through floating IP address 2:

 ![Examples of external connectivity](./images/public-gateway.svg "Examples of external connectivity"){: caption="Figure 1. Examples of external connectivity" caption-side="bottom}

## Related Links
{: #pg-related-links}

These links provide additional information about {{site.data.keyword.cloud_notm}} public gateways.

* [VPC external connectivity options](/docs/vpc?topic=vpc-about-networking-for-vpc#public-gateway-for-external-connectivity)
* [Create VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli)
* [Public gateway prerequisites](/docs/vpc?topic=vpc-create-public-gateways&interface=ui#pg-before-you-begin)
* [Create your public gateway](/docs/vpc?topic=vpc-create-public-gateways&interface=ui#pg-creating-api)
