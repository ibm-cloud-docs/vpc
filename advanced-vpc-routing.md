---

copyright:
  years: 2019, 2020
lastupdated: "2019-11-03"

keywords: secure, region, zone, subnet, routing, terminology, public gateway, floating IP, NAT, API

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
{:DomainName: data-hd-keyref="DomainName"}



# Setting up advanced routing
{: #advanced-routing}

You can control the flow of network traffic in your VPC by configuring routes. Use routes to specify the next hop for packets, based on their destination addresses.
{:shortdesc}

Separate route tables exist for each zone in your VPC. When a packet leaves a subnet, it is evaluated against the route table in the subnet's zone to determine where to send the packet next. Each route table has a default route, but you can add custom static routes to your routing tables. 

A VPC route has three main components: the destination CIDR, the next hop to which to route packets, and the zone. Any traffic that originates in the specified zone of the VPC and has a destination address within the specified destination CIDR is routed to the next hop. If the destination address is within the destination CIDR for multiple VPC routes, the most specific route is used. If there are two or more equally specific routes, the traffic is round-robin distributed between each route.

## Creating VPC routes
{: #create-route}

### Using the {{site.data.keyword.cloud_notm}} console to create VPC routes
{: #create-a-route-from-the-user-interface}

Go to the details page for a VPC and click **Routes** in the navigation.

### Using the CLI to create VPC routes
{: #create-a-route-using-the-cli}

Use the [ibmcloud is vpc-route-create](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpc-route-create) command.

### Using the API to create VPC routes
{: #create-a-route-using-the-api}

Use the [routes](https://{DomainName}/apidocs/vpc#create-a-route-on-your-vpc) API.

## Limitations
{: #routing-limitations}

- Currently, custom default static routes are not available. Only non-default static routes are supported.
- You can set the next hop to be an IP address only.
