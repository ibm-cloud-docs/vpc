---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-07"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About routing tables and routes
{: #about-custom-routes}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) automatically generates a default routing table for the VPC to manage traffic in the zone. By default, this routing table is empty. You can add routes to the default routing table, or create a custom routing table, and then add routes to the new table. For example, if you want a specialized routing policy for a specific subnet, you can create a routing table and associate it with one or more subnets. However, if you want to change the default routing policy (which affects all subnets that use the default routing table), then you should add routes to the default routing table.
{: shortdesc}

The default routing table functions the same as other routing tables, except that it is created automatically, and is used when you create a subnet without specifying a routing table.
{: note}

You can define routes within any routing table to shape traffic the way that you want. Each subnet has one routing table assigned to it, which is responsible for managing the subnet's traffic. You can change the routing table that a subnet uses to manage its traffic at any time.

This service also allows the use of Network Functions Virtualization (NFV) for advanced networking services, such as third-party routing, firewalls, local/global load balancing, web application firewalls, and more. Custom routing tables are also currently being integrated in {{site.data.keyword.cloud_notm}} services.

## Egress and ingress routing
{: #egress-ingress-overview}

When you create a routing table, you can select one of the following traffic types:

* **Egress** routes control traffic, which originates within a private network and travels towards the public internet, or to another VM in same or different zone.

* **Ingress** routes enable you to customize routes on incoming traffic to a VPC from traffic sources external to the VPC's availability zone (IBM Cloud Direct Link 2.0, IBM Cloud Transit Gateway, or another availability zone in same VPC). You must choose at least one traffic source to enable ingress routing.

   Only one custom routing table can be associated with a particular ingress traffic source. However, you can use different routing tables for different traffic sources. For example, routing table A might use Transit Gateway and VPC Zone, and routing table B use Direct Link 2.0.
   {: note}

## System routing table
{: #system-routing-table}

You cannot configure a routing table to use the system routing table; it is populated automatically. The system routing table is used when no matching route is found in the custom routing table that is associated with the subnet where the traffic is egressing. If no match is found, the packet is dropped.

A system routing table is maintained for each VPC. A VPC can have a presence in multiple zones, and the VPC's system routing table is different in each zone. For ingress routing, the system routing table contains only routes to each network interface in the VPC’s zone.

   This behavior can be avoided with a custom route table default route with an action of `drop`.
   {: note}

The system route table contains:

* Routes to the CIDR of each subnet in the VPC
* Routes to subnets within the zone (statically maintained)
* Routes to subnets in other zones that are learned via BGP
* Dynamic routes learned via BGP (for example, Direct Link)
* Routes to the SoftLayer service network CIDRs (used when a service gateway is associated with the VPC)
* The default route for internet traffic (used when a public gateway or floating IP is associated with the VPC)

## Limitations and guidelines
{: #limitations-custom-routes}

The following limitations and guidelines apply to {{site.data.keyword.cloud_notm}} Custom Routes for VPC:

* You can use a maximum of 14 unique prefix lengths per custom routing table. You can have multiple routes with the same prefix that counts as only one unique prefix. For example, you might have multiple routes with a `/28` prefix. This counts as one unique prefix.
* Reachability of a next hop IP address in a custom route is not a determining factor in whether the route is used for forward traffic. This can cause issues when multiple routes with the same prefix (but different next hop IP addresses) are used, as traffic to unreachable next hop IP addresses might not be delivered.
* When creating a route for a static, route-based VPN connection, you must enter the VPN connection ID for the next hop. The VPN gateway must be in the same zone as the subnet to which the routing table is associated with. A VPN gateway as the next hop in a different zone than the subnet that is associated with the routing table is not recommended.
* A custom routing table that contains custom routes with a next hop that is associated with a VPN connection cannot be associated with an ingress traffic source.
* For egress custom routes, when you add a destination route, you must select a zone. However, the next hop doesn't have to be in the same zone. For ingress custom routes, the next hop must be in the same zone.
* The implicit router performs equal-cost multi-path (ECMP) routing (multiple routes with the same destination, but different next hop addresses) with the following limitations:

   * This only applies to routes with an action of deliver.
   * Only two identical destination routes are permitted per zone, and each must have different next hop addresses.
   * When ECMP is used, the return path might not take the same path.

* Custom routes in a custom routing table associated with an ingress traffic source, and with an action of **deliver**, must have a next hop IP contained by one of the root prefixes of the VPC in the availability zone where the route is added. In addition, the next hop IP must be configured on a virtual server interface in the VPC and availability zone where the route is targeted.
* Ingress traffic from a particular traffic source is routed using the routes in the custom routing table that is associated with that traffic source.
* Currently, you cannot use a custom routing table for both ingress (attached to a traffic source) and egress (attached to a subnet) traffic. In addition, the default custom routing table cannot be associated with an ingress traffic source.
* {{site.data.keyword.cloud_notm}} VPC permits the use of RFC-1918 and IANA-registered IPv4 address space, privately within your VPC, with some exceptions in the IANA special-purpose ranges, and select ranges assigned to {{site.data.keyword.cloud_notm}} services. When using IANA-registered ranges within your enterprise, and within VPCs in conjunction with {{site.data.keyword.cloud_notm}} Transit Gateway or {{site.data.keyword.cloud_notm}} Direct Link, custom routes must be installed in each zone. For more information, see [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana).

## Related links
{: #related-links-custom-routes}

These links provide additional information about {{site.data.keyword.cloud_notm}} routing tables and routes for VPC.

* [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#custom-routes-section)
* [VPC API reference](https://{DomainName}/apidocs/vpc)
* [Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-custom-routes)
* [Routing tables for VPC infrastructure resources for Terraform](/docs/terraform?topic=terraform-vpc-gen2-resources#vpc-routing-table)
* [Required permissions for routing tables and routes](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls)
* [Quotas and service limits](/docs/vpc?topic=vpc-quotas#routing-tables-routes-quotas)
* Ingress traffic sources: IBM Cloud [Direct Link 2.0](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) and [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started)
