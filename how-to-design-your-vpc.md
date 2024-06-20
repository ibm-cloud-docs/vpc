---

copyright:
  years: 2017, 2023
lastupdated: "2024-06-20"

keywords: subnet, address prefixes, design, addressing

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Designing an addressing plan for a VPC
{: #vpc-addressing-plan-design}

The first step in designing your {{site.data.keyword.vpc_full}} is designing your addressing plan.
{: shortdesc}

A properly designed addressing plan has two goals:

* It meets the communication requirements of VPC instances.
* It maintains flexibility for future growth.

This document gives an example of designing the addressing plan for a three-tiered web application, in which each tier is supported by multiple zones.

Although each {{site.data.keyword.vpc_short}} deploys to a specific region, the VPC can span all of the zones within that region. {{site.data.keyword.vpc_short}} defines a default address prefix for every zone. The address prefixes enable communication between VPC instances in different zones.

The same design steps are involved, no matter whether the application is contained completely on the cloud, or whether parts of the application are running in another location.
{: tip}

When you create VPC instances that you intend to interconnect by using [IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started), avoid selecting **Default address prefixes**. Create your VPC instances with nonoverlapping prefixes for [successful connectivity](/docs/transit-gateway?topic=transit-gateway-troubleshooting#overlapping-vpc-prefixes-and-classic-subnets).

When you create VPC instances that you also intend to interconnect with your IBM Cloud classic infrastructure by using [IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started), do not use IP addresses in your instances in the `10.0.0.0/14`, `10.200.0.0/14`, `10.198.0.0/15`, and `10.254.0.0/16` blocks. Also, avoid IP addresses from your classic infrastructure subnets.

For more information about designing your VPC instances for use with IBM Cloud Transit Gateway, see [Planning for IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-helpful-tips).
{: important}

## Design considerations and assumptions
{: #design-considerations-and-assumptions}

When you design the addressing plan for an application, the primary consideration is to keep the CIDR blocks used for creating subnets within a single zone as contiguous as possible. By doing so, you can summarize them in a single address prefix, leaving room for future growth.

Another consideration is the number of available addresses that a subnet might need for horizontal scaling. Table 1 lists the number of available addresses in a subnet, based on its specified CIDR block size:

| CIDR block size | Available addresses |
| --------------- | ------------------- |
|      /22        |        1019         |
|      /23        |         507         |
|      /24        |         251         |
|      /25        |         123         |
|      /26        |          59         |
|      /27        |          27         |
|      /28        |          11         |
{: caption="Table 1. Available addresses in a subnet based on CIDR block size" caption-side="bottom"}

Based on these two considerations, the following assumptions are made for this example:

* CIDR ranges from the `172.16.0.0/12` block of RFC 1918 addresses are used for all subnets.
* The application's presentation layer is a thin layer on a REST API. Therefore, horizontal scaling affects the middle tier more than it affects the presentation tier.

## Determine each tier's subnet size
{: #determine-each-tier-s-subnet-size}

The next step is to determine each tier's subnet size (in terms of available addresses). Because each tier of the application has a presence in each zone, each zone requires three subnets.

Consider the following information when you plan each tier's subnet size:

* The database tier (the back end) is the least likely to need dynamic scaling, so these subnets are the smallest. That is, these subnets can contain the least number of available addresses.
    * _This example uses a `/27` CIDR block, which allows for 27 addresses in this tier._
* The middle tier is the most likely to need dynamic scaling, so these subnets are the largest. That is, they must contain the greatest number of available addresses.
    * _This example uses a `/25` CIDR block, which allows for 123 addresses in this tier._
* The front-end tier fits in the middle; it doesn't need as many addresses as the middle tier, but it needs more than the database tier does.
    * _This example uses a `/26` CIDR block, which allows for 59 addresses in this tier._

## Combining the subnets and selecting the address prefixes
{: #combining-the-subnets-and-selecting-the-address-prefixes}

To select an acceptable address prefix for each zone, you need a subnet size that's large enough to accommodate all three of the subnets in each tier, and still leave room for horizontal scaling and future expansion.

A `/24` address prefix is the smallest prefix into which these three subnets can be combined (27 + 123 + 59). Select the next larger subnet size, not the smallest. Assigning the next larger subnet size (`/23`) allows for horizontal scaling beyond the limits that were given previously because it allows for adding new subnets to each layer, from within the same address prefix.

After you determine the correct subnet size, you can assign the actual address prefixes, one for each zone:

|  Zone  | Address prefix  |
| ------ | --------------- |
| Zone 1 | `172.16.0.0/23` |
| Zone 2 | `172.16.2.0/23` |
| Zone 3 | `172.16.4.0/23` |
{: caption="Table 2. Zones assigned address prefixes" caption-side="bottom"}

And from this basis, you can also assign the three subnets within each zone:

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
{: caption="Table 3. Subnets assigned within each zone" caption-side="bottom"}

## Considerations for extending an existing infrastructure
{: #considerations-for-extending-an-existing-infrastructure}

When you're planning a VPC that extends an existing infrastructure, you can follow the previous steps, whether that infrastructure is your on-premises infrastructure, another VPC, or even another cloud. Keep in mind that you must not reuse existing address ranges. By avoiding address reuse, you can maximize your ability to take advantage of {{site.data.keyword.vpc_short}} features.
