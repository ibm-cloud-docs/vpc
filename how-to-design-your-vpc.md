---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-07-29"

keywords: VPC, subnet, address prefixes, design, addressing

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}


# Designing an addressing plan for a VPC 
{: #vpc-addressing-plan-design}

The first step in designing your {{site.data.keyword.vpc_full}} should be to design your addressing plan. A properly executed addressing plan has two goals:

* It meets the communication requirements of VPC instances.
* It maintains flexibility for future growth. 

This document gives an example of designing the addressing plan for a three-tiered web application, in which each tier is supported by multiple zones.
{:shortdesc}

Although each {{site.data.keyword.vpc_short}} is deployed to a specific region, the VPC can span all of the zones within that region. {{site.data.keyword.vpc_short}} defines a default address prefix for every zone. These address prefixes enable communication between VPC instances in different zones, by providing the zone-related routing information the [implicit router](/docs/vpc?topic=vpc-vpc-glossary#implicit-router) needs.

The same design steps are involved, no matter whether the application is contained completely on the cloud, or whether parts of the application are running in another location.
{: note}

## Design considerations and assumptions
{: #design-considerations-and-assumptions}

When designing the addressing plan for an application, the primary consideration is to keep the CIDR blocks used for creating subnets within a single zone as contiguous as possible. By doing so, the designer allows them to be summarized in a single address prefix, leaving room for future growth.

Another consideration is the number of available addresses a subnet may need for horizontal scaling. The following table lists the number of available addresses in a subnet, based on its specified CIDR block size:

| CIDR block size | Available Addresses |
| --------------- | ------------------- |
|      /22        |        1019         |
|      /23        |         507         |
|      /24        |         251         |
|      /25        |         123         |
|      /26        |          59         |
|      /27        |          27         |
|      /28        |          11         |

Based on these two considerations, here are the assumptions we make for this example:

* For this example, CIDR ranges from the 172.16.0.0/12 block of RFC 1918 addresses will be used for all subnets.
* We assume that the application's presentation layer is a thin veneer above a REST API. Therefore, horizontal scaling affects the middle tier more than it affects the presentation tier.

## Determine each tier's subnet size
{: #determine-each-tier-s-subnet-size}

The next step is to determine each tier's subnet size (in terms of available addresses). Each tier of the application has a presence in each zone, therefore each zone will require three subnets.

Here are the considerations we are using when planning each tier's subnet size:

* The database tier (the back end) is the least likely one to need dynamic scaling, so these subnets can be the smallest. That is, these subnets may contain the least number of available addresses. 
    * _This example uses a `/27` CIDR block, which allows for 27 addresses in this tier._
* The middle tier is the most likely to need dynamic scaling, so these subnets will be the largest. That is, they must contain the greatest number of available addresses. 
    * _This example uses a `/25` CIDR block, which allows for 123 addresses in this tier._
* The front-end tier fits in the middle; it won't need as many addresses as the middle tier, but it needs more than the database tier does. 
    * _This example uses a `/26` CIDR block, which allows for 59 addresses in this tier._

## Combining the subnets and selecting the address prefixes
{: #combining-the-subnets-and-selecting-the-address-prefixes}

To select an acceptable address prefix for each zone, you'll need a subnet size that's large enough to accommodate all three of the subnets in each tier, and still leave room for horizontal scaling and future expansion. 

A `/24` address prefix is the smallest prefix into which these three subnets can be combined (27 + 123 + 59). As a best practice, it is recommended to select the next larger subnet size, not the smallest. Assigning the next larger subnet size (`/23`) allows for horizontal scaling beyond the limits given previously, because it allows for adding new subnets to each layer, from within the same address prefix.

Now that we've determined the correct subnet size, the actual address prefixes can be assigned, one for each zone:

|  Zone  | Address Prefix  |
| ------ | --------------- |
| Zone 1 | `172.16.0.0/23` |
| Zone 2 | `172.16.2.0/23` |
| Zone 3 | `172.16.4.0/23` |

And from this basis, the 3 subnets within each zone can be assigned:

|  Zone  |   Tier   |    Subnet CIDR    |
| ------ | -------- | ----------------- |
| Zone 1 |  Middle  |  `172.16.0.0/25`  |
| Zone 1 |  Front   |  `172.16.1.0/26`  |
| Zone 1 | Database | `172.16.1.128/27` |
| Zone 2 |  Middle  |  `172.16.2.0/25`  |
| Zone 2 |  Front   |  `172.16.3.0/26`  |
| Zone 2 | Database | `172.16.3.128/27` |
| Zone 3 |  Middle  |  `172.16.4.0/25`  |
| Zone 3 |  Front   |  `172.16.5.0/26`  |
| Zone 3 | Database | `172.16.5.128/27` |

## Considerations for extending an existing infrastructure
{: #considerations-for-extending-an-existing-infrastructure}

When you're planning a VPC that extends an existing infrastructure, you can follow the previous steps, whether that infrastructure is your on-premise infrastructure, another VPC, or even another cloud. Keep in mind that you must not reuse existing address ranges. By avoiding address re-use, you can maximize your ability to take advantage of {{site.data.keyword.vpc_short}} features.
