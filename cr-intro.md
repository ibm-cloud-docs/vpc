---

copyright:
  years: 2020
lastupdated: "2020-12-03"

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

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) automatically generates a default routing table for the VPC to manage traffic in the zone. By default, this routing table is empty. You can add routes to the default routing table, or create one or more custom routing tables and then add routes. For example, if you want a specialized routing policy for a specific subnet, you can create a routing table and associate it with one or more subnets. However, if you want to change the default routing policy that affects all subnets using the default routing table, then you should add routes to the default routing table.
{:shortdesc}

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

Use the system routing table for routing traffic when no matching route is found in the custom routing table that is associated with the subnet where the traffic is egressing. A system routing table is maintained for each VPC. A VPC can have a presence in multiple zones, and the VPC's system routing table is different in each zone. For ingress routing, the system routing table contains only routes to each network interface in the VPC’s zone.

## Use cases
{: #use-cases}

**WORK-IN-PROGRESS**

### Use case 1:  Edge proxy firewall
{: #edge-proxy-firewall-use-case}

![Edge proxy firewall use case](/images/cr_use_case_1.png)   


| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| - | - | - | - |
| - | - | - | - |
{: caption="Table 1. Default route table, edge proxy firewall" caption-side="top"}


| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| `10.10.0.0/16` | Delegate |  | Dallas DC 1|
| `10.11.0.0/16` | Delegate |  | Dallas DC 1|
| `0.0.0.0/0` | Deliver | `10.10.1.5` | Dallas DC 1|
{: caption="Table 2. Proxy route table, edge proxy firewall" caption-side="top"}

####  Objective: Edge proxy firewall

Secure flows to public behind a client-defined gateway, proxy, or firewall appliance while allowing resources on the other subnets to flow through the VPC-managed public gateway.

Utilizing VPC Custom Routes with per subnet egress routing, you can create a table to override the system-implicit routing with the VPC network. In this example, the default table created by the system where all subnets are assigned at creation, is unchanged. A table to direct traffic from subnet `10.10.1.0/24` through an edge proxy is created.

Because the destination for the proxied flows are the public internet and, typically, follow the Default route (`0.0.0.0/0`), you must first exempt flows toward internally-reachable private network using the Delegate function of custom routes. Resources in subnet `10.10.3.0/24` continue to use the public gateway that is attached to that subnet. Achieve further isolation by disconnecting the public gateway, or removing it from the VPC altogether.

While this example has the instances that use the proxy share a common subnet with the proxy, it is not required to do so. Custom routes also enables you to specify a next-hop IP on a subnet that is not connected to the instance enabling you to scale horizontally without concern for the subnet sizings of Edge services. For example, the proxy route table can be attached to subnet `10.10.3.0/24` and it directs all public flows to the NFV proxy.

#### Functions used in Edge proxy firewall
- Disabling the IP spoofing check on `10.10.0.5` and `10.10.1.5` interfaces to enable source address preservation. This action requires elevated IAM permissions on the instance.
- Public gateway for outbound flows for resources that are not directed through the NFV proxy.
- Floating IP attached to the `10.10.0.5` interface to enable inbound and outbound public flows directy to the NFV proxy.
- Stateful security groups and network access control lists (ACLs) can be added to the instances interfaces and VPC subnets, respectively, to provide additional isolation resources within the VPC.

### Use case 2: Public load balancer
{: #public-load-balancer-use-case}


![Public load balancer use case](/images/cr_use_case_2.png)   

| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| -  |  - | -  | -  |
| -  | - | -  | - |
{: caption="Table 3. Default route table, public load balancer" caption-side="top"}


| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| `10.10.0.0/16` | Delegate |  | Dallas DC 1|
| `10.11.0.0/16` | Delegate |  | Dallas DC 1|
| `166.26.0.0/16`<sup>*</sup> | Delegate |  | Dallas DC 1|
| `166.8.0.0/14`<sup>*</sup> | Delegate |  | Dallas DC 1|
| `0.0.0.0/0` | Deliver | `10.10.1.5` | Dallas DC 1|
{: caption="Table 4. Web route table, public load balancer" caption-side="top"}

#### Objective: public load balancer
Hosting web applications using a client-defined application or network load balancer.

Utilizing VPC custom routes with per-subnet egress routing, you can create a table to override the system-implicit routing with the VPC network without updating routing information on each instance. In this example, the source IP from the internet users to the web layer is preserved. The web-to-app and app-to-db layers communicate using implicit VPC routing and continue to use the public gateway for internet egress.

Route delegation is required for internal/private networks within the VPC and IBM service networks over the private backbone. To avoid the need for delegation, web layer servers can be attached to the app layer subnet, and routing changed on the web instances to use the app subnet gateway as the next hop for the internal VPC and cloud service networks.

#### Functions used in public load balancer
- Disabling the IP spoofing check on `10.10.0.5` and `10.10.1.5` interfaces to enable source address preservation. This action requires elevated IAM permissions on the instance.
- Public gateway for outbound flows for resources that are not directed through the NFV proxy.
- Floating IP attached to the `10.10.0.5` interface to enable inbound and outbound public flows directy to the NFV load balancer.
- Stateful security groups and network access control lists (ACLs) can be added to the instances interfaces and VPC subnets, respectively, to provide additional isolation resources within the VPC.

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

* Custom routes in a custom routing table associated with an ingress traffic source, and with an action of **deliver**, must have a next hop IP contained by one of the root prefixes of the VPC in the availability zone where the route is added. In addition, the next hop IP must be configured on a virtual server interface in the VPC and availability zone.  
* Ingress traffic from a particular traffic source is routed using the routes in the custom routing table that is associated with that traffic source.
* Currently, you cannot use a custom routing table for both ingress (attached to a traffic source) and egress (attached to a subnet) traffic. In addition, the default custom routing table cannot be associated with an ingress traffic source.

## Related links
{: #related-links-custom-routes}

These links provide additional information about {{site.data.keyword.cloud_notm}} Custom Routes for VPC.

* [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#custom-routes-section)
* [VPC API reference](https://{DomainName}/apidocs/vpc)
* [Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-custom-routes)
* [Routing Tables for VPC infrastructure resources for Terraform](https://cloud.ibm.com/docs/terraform?topic=terraform-vpc-gen2-resources#vpc-routing-table)
* [Required permissions for routing tables and routes](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls)
* [Quotas and service limits](/docs/vpc?topic=vpc-quotas#routing-tables-routes-quotas)
* Ingress traffic sources: IBM Cloud [Direct Link 2.0](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) and [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started)
