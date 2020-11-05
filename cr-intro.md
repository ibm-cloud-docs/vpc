---

copyright:
  years: 2020
lastupdated: "2020-10-30"

keywords: custom routes

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

# About routing tables and routes
{: #about-custom-routes}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) automatically generates a default route for the VPC to manage traffic in the zone. One routing table exists for each zone that is used by a VPC in a region. You can create a custom route in your VPC, but only within that particular VPC routing table.
{:shortdesc}

VPCs use routing tables to manage network traffic on its subnets. Each VPC has a default routing table assigned to it; subnets use this routing table by default. If you need to manage traffic differently for certain subnets, you have the option to create your own routing table.

You can define routes within any routing table to shape traffic any way that you want. Each subnet has one routing table assigned to it, which is responsible for managing the subnet's traffic. You can change the routing table that a subnet uses to manage its traffic at any time.

This VPC service also allows the use of Network Functions Virtualization (NFV) for advanced networking services, such as third-party routing, firewalls, local/global load balancing, web application firewalls, and more. Custom routing tables are also currently being integrated in {{site.data.keyword.cloud_notm}} services.
{: note}

## Limitations and guidelines
{: #limitations-custom-routes}

The following limitations and guidelines apply to {{site.data.keyword.cloud_notm}} Custom Routes for VPC:

* You can use a maximum of 14 unique prefix lengths per custom route table. You can have multiple routes with the same prefix that counts as only one unique prefix. For example, you could have multiple routes with a `/28` prefix and this would count as one unique prefix.
* Network packets traverse the routes in a routing table as they leave subnets where they are attached (egress flows), and not as they enter subnets where they are attached (ingress flows).
* Currently, routes added to a routing table cannot be exported to Transit Gateway or Direct Link connections.
* Reachability of a next hop IP address in a custom route is not a determining factor in whether or not the route is used for forward traffic. This can cause issues when multiple routes with the same prefix (but different next hop IP addresses) are used, as traffic to unreachable next hop IP addresses might not be delivered.
* When creating a route for a VPN connection, you must enter an IP address for the next hop. The VPN gateway must be in the same zone as the source prefix. A VPN gateway as the next hop in a different zone than the source prefix is not supported.
* When you add a destination route, you must select a zone. However, the next hop doesn't have to be in the same zone.
* The implicit router will perform equal-cost multi-path (ECMP) routing (multiple routes with the same destination, but different next hop addresses), but with the following limitations:
   * This only applies to routes with an action of deliver.
   * Only two identical destination routes are permitted per zone, and each must have different next hop address.
   * When ECMP is used, the return path might not take the same path.

## Related links
{: #related-links-custom-routes}

These links provide additional information about {{site.data.keyword.cloud_notm}} Custom Routes for VPC.

* [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#custom-routes-section)
* [VPC API reference](https://{DomainName}/apidocs/vpc)
* [Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-custom-routes)
* [Required permissions for routing tables and routes](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls)
* [Quotas and service limits](/docs/vpc?topic=vpc-quotas#routing-tables-routes-quotas)
