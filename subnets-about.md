---

copyright:
  years: 2022
lastupdated: "2022-11-09"

keywords: subnets

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About subnets
{: #about-subnets-vpc}

{{site.data.keyword.cloud}} has specific terminology for the types, and uses, of subnets. Knowing their intended use helps you understand how best to use them in your cloud infrastructure.
{: shortdesc}

To understand subnets and subnetting in general, review [Subnetwork](https://en.wikipedia.org/wiki/Subnetwork){: external}.
Additionally, subnets are referred to in [CIDR notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing){: external}.

Subnets are networks created within a VPC. Subnets are a fundamental mechanism within VPC used to allocate addresses to individual resources (such as Virtual server instances), and enable various controls to these resources through the use of network ACLs, routing tables, resource groups.

Subnets are bound to a single zone; however, they can reach all other subnets within a VPC, across a region. They are created from a larger address space within the VPC called an address prefix; and you can provision multiple subnets per address prefix.

## Subnets features
{: #subnets-vpc-features}

Subnets have the following features:

* Routable throughout your VPC across an entire region and further on-prem through Transit Gateway and Direct Link.
* Provide unique IP addresses to VPC resources including:
   * [Bare metal servers](/docs/vpc?topic=vpc-planning-for-bare-metal-servers)
   * [Virtual server instances](/docs/vpc?topic=vpc-vsi_best_practices)
   * [Virtual private endpoints](/docs/vpc?topic=vpc-about-vpe)
   * Appliances and virtual appliances
* IP range selection: Provision subnets of any size, as small as `/29` and up to `/9` from any IP address space.
* [Network ACLs](/docs/vpc?topic=vpc-using-acls): Access control lists (ACLs) can be used to control all incoming and outgoing traffic in IBM Cloud VPC.
* [Public gateways](/docs/vpc?topic=vpc-about-public-gateways&interface=ui): Attaching a public gateway allows all attached resources to communicate with the public internet.
* [Routing tables](/docs/vpc?topic=vpc-about-custom-routes): IBM Cloud VPC automatically generates a default routing table for the VPC to manage traffic in the zone. By default, this routing table is empty. You can add routes to the default routing table, or create a custom routing table, and then add routes to the new table.
