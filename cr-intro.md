---

copyright:
  years: 2020, 2022
lastupdated: "2022-10-03"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About routing tables and routes
{: #about-custom-routes}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) automatically generates a default routing table for the VPC to manage traffic in the zone. By default, this routing table is empty. You can add routes to the default routing table, or create one or more custom routing tables and then add routes. For example, if you want a specialized routing policy for a specific subnet, you can create a routing table and associate it with one or more subnets. However, if you want to change the default routing policy that affects all subnets using the default routing table, then you should add routes to the default routing table.
{: shortdesc}

The default routing table functions the same as other routing tables, except that it is created automatically, and is used when you create a subnet without specifying a routing table.
{: note}

You can define routes within any routing table to shape traffic the way that you want. Each subnet has one routing table assigned to it, which is responsible for managing the subnet's traffic. You can change the routing table that a subnet uses to manage its egress traffic at any time.

This service also allows the use of Network Functions Virtualization (NFV) for advanced networking services, such as third-party routing, firewalls, local/global load balancing, web application firewalls, and more. Custom routing tables are also currently being integrated in {{site.data.keyword.cloud_notm}} services.

## Egress and ingress routing
{: #egress-ingress-overview}

When you create a routing table, you can select one of the following traffic types:

* **Egress routes** control traffic, which originates within a subnet and travels towards the public internet, or to another VM in same or different zone.

* **Ingress routes** enable you to customize routes on incoming traffic to a VPC from traffic sources external to the VPC's availability zone (IBM Cloud Direct Link, IBM Cloud Transit Gateway, another availability zone in the same VPC, or the public internet). You must choose at least one traffic source to enable ingress routing.

   Only one custom routing table can be associated with a particular ingress traffic source. However, you can use different routing tables for different traffic sources. For example, routing table A might use Transit Gateway and VPC Zone, while routing table B uses Direct Link.
   {: note}

## System routing table
{: #system-routing-table}

You cannot configure a routing table to use the system routing table; it is populated automatically. The system routing table is used when no matching route is found in the custom routing table that is associated with the subnet where the traffic is egressing. If no match is found, the packet is dropped.

A system routing table is maintained for each VPC. A VPC can have a presence in multiple zones, and the VPC's system routing table is different in each zone. For ingress routing, the system routing table contains only routes to each network interface in the VPC’s zone.

This behavior can be avoided with a custom route table default route with an action of `drop`.
{: tip}

The system route table contains:

* Routes to the CIDR of each subnet in the VPC
* Routes to subnets within the zone (statically maintained)
* Routes to subnets in other zones that are learned through BGP
* Dynamic routes learned through BGP (for example, Direct Link) 
* The default route for internet traffic (used when a public gateway or floating IP is associated with the VPC)
* Routes to the classic infrastructure service network CIDRs (used when a service gateway is associated with the VPC), for example:

    `10.0.0.0/14`, `10.200.0.0/14`, `10.198.0.0/15`, and `10.254.0.0/16` are classic infrastructure service network CIDRs.
    {: note}

## Use cases
{: #use-cases}

The following use cases illustrate different routing scenarios.

### Use case 1: Edge proxy firewall
{: #edge-proxy-firewall-use-case}

![Edge proxy firewall use case](/images/cr_use_case_1.png){: caption="Figure 1. Edge proxy firewall use case" caption-side="bottom}

| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| - | - | - | - |
| - | - | - | - |
{: caption="Table 1. Default route table, edge proxy firewall" caption-side="bottom"}

| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| `10.10.0.0/16` | Delegate |  | Dallas DC 1|
| `10.11.0.0/16` | Delegate |  | Dallas DC 1|
| `0.0.0.0/0` | Deliver | `10.10.1.5` | Dallas DC 1|
{: caption="Table 2. Proxy route table, edge proxy firewall" caption-side="bottom"}

#### Objective: Edge proxy firewall
{: #objective-edge-proxy-firewall}

Secure flows to the public internet from behind a client-defined gateway, proxy, or firewall appliance, while also allowing resources on the other subnets to flow through the VPC-managed public gateway.

Using VPC custom routes with per subnet egress routing, you can create a table to override the system-implicit routing with the VPC network. In this example, the default table created by the system, where all subnets are assigned at creation, is unchanged. A table to direct traffic from subnet `10.10.1.0/24` through an edge proxy is created as well.

Because the destination for the proxied flows are the public internet and, typically, follow the default route (`0.0.0.0/0`), you must first exempt flows toward internally-reachable private networks using the Delegate function of custom routes. Resources in subnet `10.10.3.0/24` continue to use the public gateway that is attached to that subnet. You can achieve further isolation by disconnecting the public gateway, or removing it from the VPC altogether.

While the instances that use the proxy share a common subnet with the proxy in this example, you are not required to do so. Custom routes also enable you to specify a next-hop IP on a subnet that is not connected to the instance. This allows you to scale horizontally without concern for the subnet sizings of Edge services. For example, a proxy route table can be attached to subnet `10.10.3.0/24` and directs all public flows to the NFV proxy.

#### Functions used in an Edge proxy firewall
{: #functions-used-in-edge-proxy-firewall}

This use case uses the following functions of Edge proxy firewalls:

* Disabling the IP spoofing check on `10.10.0.5` and `10.10.1.5` interfaces to enable source address preservation. This action requires elevated IAM permissions on the instance.
* A public gateway for outbound flows with resources that are not directed through the NFV proxy.
* The floating IP attached to the `10.10.0.5` interface to enable inbound and outbound public flows directly to the NFV proxy.
* Adding stateful security groups and network access control lists (ACLs) to the instances interfaces and VPC subnets, respectively, to provide additional isolation resources within the VPC.

### Use case 2: Public load balancer
{: #public-load-balancer-use-case}

![Public load balancer use case](/images/cr_use_case_2.png){: caption="Figure 2. Public load balancer use case" caption-side="bottom}

| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| -  |  - | -  | -  |
| -  | - | -  | - |
{: caption="Table 3. Default route table, public load balancer" caption-side="bottom"}

| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| `10.10.0.0/16` | Delegate |  | Dallas DC 1|
| `10.11.0.0/16` | Delegate |  | Dallas DC 1|
| `166.26.0.0/16` * | Delegate |  | Dallas DC 1|
| `166.8.0.0/14` * | Delegate |  | Dallas DC 1|
| `0.0.0.0/0` | Deliver | `10.10.1.5` | Dallas DC 1|
{: caption="Table 4. Web route table, public load balancer" caption-side="bottom"}

#### Objective: public load balancer
{: #objective-public-load-balancer}

Hosting web applications using a client-defined application or network load balancer.

Utilizing VPC custom routes with per-subnet egress routing, you can create a table to override the system-implicit routing with the VPC network without updating routing information on each instance. In this example, the source IP from the internet users to the web layer is preserved. The web-to-app and app-to-db layers communicate using implicit VPC routing and continue to use the public gateway for internet egress.

Route delegation is required for internal/private networks within the VPC and IBM service networks over the private backbone. To avoid the need for delegation, web layer servers can be attached to the app layer subnet, and routing changed on the web instances to use the app subnet gateway as the next hop for the internal VPC and cloud service networks.

#### Functions used in public load balancers
{: #functions-used-in-public-lb}

This use case uses the following functions of public load balancers:

* Disabling the IP spoofing check on `10.10.0.5` and `10.10.1.5` interfaces to enable source address preservation. This action requires elevated IAM permissions on the instance.
* A public gateway for outbound flows for resources that are not directed through the NFV proxy.
* The floating IP attached to the `10.10.0.5` interface to enable inbound and outbound public flows directly to the NFV load balancer.
* Adding stateful security groups and network access control lists (ACLs) to the instances interfaces and VPC subnets, respectively, to provide additional isolation resources within the VPC.

### Use case 3: VPN
{: #use-case-3}

To have a VPN gateway as the next hop in a VPC routing table, the gateway is created in a dedicated subnet. This subnet is used only for the VPN gateway, and associated with the VPN gateway subnet with a different routing table when there is a route `0.0.0.0/0` and the next hop is a VPN connection. 

For example:

* Subnet A is used to host the virtual servers and is associated with the default routing table. There is a route `0.0.0.0/0`, and the next hop is a VPN connection.
* Subnet B is used to host the VPN gateway and creates a new routing table (routing table B). It associates subnet B with routing table B. By default, you do not need any routes in routing table B.

To have connectivity between subnets when having route `0.0.0.0/0` and the next hop is a VPN connection, as above, you can add the following delegate routes:

```sh
destination CIDR==zone3 VPC prefix, action==delegate, location==zone1
destination CIDR==zone1 VPC prefix, action==delegate, location==zone3
```
{: pre}

## Limitations and guidelines
{: #limitations-custom-routes}

The following limitations and guidelines apply to {{site.data.keyword.cloud_notm}} Custom Routes for VPC.

### General limitations
{: #routes-general}

* Currently, you cannot use a custom routing table for both ingress (attached to a traffic source) and egress (attached to a subnet) traffic. In addition, the default custom routing table cannot be associated with an ingress traffic source.
* The ability to reach a next hop IP address in a custom route is not a determining factor in whether the route is used for forward traffic. This can cause issues when multiple routes with the same prefix (but different next hop IP addresses) are used, as traffic to unreachable next hop IP addresses might not be delivered.
* {{site.data.keyword.cloud_notm}} VPC permits the use of RFC-1918 and IANA-registered IPv4 address spaces privately within your VPC, with some exceptions in the IANA special-purpose ranges, and select ranges assigned to {{site.data.keyword.cloud_notm}} services. When using IANA-registered ranges within your enterprise, and within VPCs in conjunction with {{site.data.keyword.cloud_notm}} Transit Gateway or {{site.data.keyword.cloud_notm}} Direct Link, you must install custom routes in each zone. For more information, see [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana).

### Egress routes
{: #routes-egress-routes}

For egress custom routes, when you add a destination route, you must select a zone. However, the next hop doesn't have to be in the same zone. For ingress custom routes, the next hop must be in the same zone.

### Equal-Cost Multi-Path (ECMP)
{: #routes-ecmp}

The implicit router performs ECMP routing (multiple routes with the same destination, but different next hop addresses) with the following limitations:

* This only applies to routes with an action of `deliver`.
* Only two identical destination routes are permitted per zone, and each must have different next hop addresses.
* When ECMP is used, the return path might not take the same path.

### Ingress routes
{: #routes-ingress}


* Currently, public ingress routing (`public internet` traffic choice) is available in the UI only. CLI and API are forthcoming. 
* Each ingress source type can be associated with up to one ingress route table per VPC, however, a VPC can have multiple ingress route tables and each ingress route table can have one or more ingress types associated.
* Ingress traffic from a particular traffic source is routed using the routes in the custom routing table that is associated with that traffic source.
* Custom routes in a custom routing table associated with an ingress traffic source, and with an action of `deliver`, must have a next hop IP contained by one of the address prefixes of the VPC in the availability zone where the route is added. In addition, the next hop IP must be configured on a virtual server interface in the VPC and availability zone where the route is targeted.  
* Ingress traffic from a particular traffic source is routed using the routes in the custom routing table that is associated with that traffic source. If no matching route is found in a custom routing table, routing continues by using the VPC system routing table. You can avoid this behavior with a custom routing table's default route with an action of **drop**.
* An ingress custom routing table containing any custom routes with destination IP (FIP) should be defined in the same zone as the virtual server instance that a FIP is associated with.
* Floating IPs that are bound to an IBM Cloud VPN Gateway may not be used as a Destination for a Custom Route in an Ingress Route Table when the Public Internet (`route_internet_ingress`) source type is enabled.

### Unique prefix lengths
{: #routes-unique-prefix-lengths}

You can use a maximum of 14 unique prefix lengths per custom routing table. You can have multiple routes with the same prefix that count as only one unique prefix. For example, you might have multiple routes with a `/28` prefix. This counts as one unique prefix.

### VPN 
{: #routes-vpn}

* When creating a route for a static, route-based VPN connection, you must enter the VPN connection ID for the next hop. The VPN gateway must be in the same zone as the subnet to which the routing table is associated. It is not recommended that you define a VPN gateway as the next hop in a zone that is different than the subnet that is associated with the routing table.
* A route with a VPN connection as the next-hop takes effect only when the VPN connection is active. This route is skipped during the routing lookup if the VPN connection is down.
* A custom routing table that contains custom routes with a next hop that is associated with a VPN connection cannot be associated with an ingress traffic source.
* Custom routes are only supported on route-based VPNs. If you are using policy-based VPNs, the routes are created automatically by the VPN service in the default routing table.

## Related links
{: #related-links-custom-routes}

These links provide additional information about {{site.data.keyword.cloud_notm}} routing tables and routes for VPC:

* [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#custom-routes-section)
* [VPC API reference](/apidocs/vpc)
* [Activity Tracker events](/docs/vpc?topic=vpc-at-events#events-custom-routes)
* [Routing tables for VPC infrastructure resources for Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpc_routing_table){: external} (VPC infrastructure > Resources)
* [Required permissions for routing tables and routes](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls)
* [Quotas and service limits](/docs/vpc?topic=vpc-quotas#routing-tables-routes-quotas)
* Ingress traffic sources: IBM Cloud [Direct Link](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) and [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started)
