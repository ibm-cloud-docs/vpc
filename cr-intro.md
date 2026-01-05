---

copyright:
  years: 2020, 2026
lastupdated: "2026-01-05"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About routing tables and routes
{: #about-custom-routes}

{{site.data.keyword.cloud}} Virtual Private Cloud (VPC) automatically generates a default routing table for the VPC to manage traffic in the zone. By default, this routing table is empty. You can add routes to the default routing table, or create one or more custom routing tables and then add routes. For example, if you want a specialized routing policy for a specific subnet, you can create a routing table and associate it with one or more subnets. However, if you want to change the default routing policy that affects all subnets that use the default routing table, then you should add routes to the default routing table.
{: shortdesc}

The default routing table functions the same as other routing tables, except that it is created automatically, and is used when you create a subnet without specifying a routing table.
{: note}

You can define routes within any routing table to shape traffic the way that you want. Each subnet is assigned to one routing table, which is responsible for managing the subnet's traffic. You can change the routing table that a subnet uses to manage its egress traffic at any time.

This service also allows the use of Network Functions Virtualization (NFV) for advanced networking services, such as third-party routing, firewalls, local/global load balancing, web application firewalls, and more. Custom routing tables are also currently being integrated in {{site.data.keyword.cloud_notm}} services.

## Egress and ingress routing
{: #egress-ingress-overview}

* **Egress routes** control traffic, which originates within a subnet and travels toward the public internet, or to another VM in the same or different zone.

* **Ingress routes** enable you to customize routes on incoming traffic to a VPC from traffic sources external to the VPC's availability zone (IBM Cloud Direct Link, IBM Cloud Transit Gateway, another availability zone in the same VPC, or the public internet).

   Only one custom routing table can be associated with a particular ingress traffic source. However, you can use different routing tables for different traffic sources. For example, routing table A might use Transit Gateway and VPC Zone, while routing table B uses Direct Link.
   {: note}

## Defining the system-implicit routing table
{: #system-implicit-routing-table}

You cannot configure a routing table to use the system-implicit routing table; it is populated automatically. The system-implicit routing table is used when no matching route is found in the custom routing table that is associated with the subnet where the traffic is egressing. If no match is found, the packet is dropped.

A system-implicit routing table is maintained for each VPC. A VPC can have a presence in multiple zones, and the VPC's system-implicit routing table is different in each zone. For ingress routing, the system-implicit routing table contains only routes to each network interface in the VPC’s zone.

This behavior can be avoided with a custom routing table default route with an action of `drop`.
{: tip}

The system-implicit routing table contains:

* Routes to the CIDR of each subnet in the VPC
* Routes to subnets within the zone (statically maintained)
* Routes to subnets in other zones that are learned through BGP
* Dynamic routes learned through BGP (for example, Direct Link and Transit Gateway)
* The default route for internet traffic (used when a public gateway or floating IP is associated with the VPC)
* Routes to the classic infrastructure service network CIDRs (used when a service gateway is associated with the VPC), for example:

    `10.0.0.0/14`, `10.200.0.0/14`, `10.198.0.0/15`, and `10.254.0.0/16` are classic infrastructure service network CIDRs.
    {: note}

## Determining route preference
{: #cr-determining-route-pref}

You can designate the route priority (`0`-`4`) of VPC routes to determine which routes have a higher priority when multiple routes exist for a specific destination. Existing routes and new routes, which are created without a priority value, are automatically set to the default priority (`2`).
{: shortdesc}

The route priority is considered on identical destinations only and is similar to any dynamically learned route.
{: note}

### Setting route priority examples
{: #examples-setting-route-priority}

- If you have a route with a `/32` prefix and priority of `1`, and a route with destination of `/31` and priority `0`, then `/32` is used first (longest prefix match). In other words, priority matters only when there is more than one route with the exact same prefix.
- If you have three routes with `0.0.0.0/0` (default route), only the route with the highest priority is used. If the selected route is removed (for example, through automation), the highest priority from the remaining two routes is selected.
- If the custom routing table has only one route with a specific prefix, that one route is always used regardless of its priority. Best route selection is performed for every specific prefix and picks the route with the highest priority.

Equal-Cost Multi-Path (ECMP) routing is used when two routes have the same destination and priority (extending the current behavior where ECMP is used for two routes with same destination). At most, two routes in a routing table are allowed to have the same destination and priority. For example, if the custom routing table has three routes with the same destination, one route with priority `1` and the other two routes with priority `4` (ECMP), the route with priority `1` is selected and the ECMP routes are ignored (no ECMP). Now, if the two routes with the same destination and priority have the highest priority, the system selects these two routes and performs ECMP. Then, if one of these routes is deleted, the route that still exists is selected and ECMP is not performed.

For route priority limitations, see [Limitations and guidelines](/docs/vpc?topic=vpc-about-custom-routes&interface=ui#route-rules-considerations).
{: note}

## Advertising routes
{: #rt-advertising-routes}

In the past, address prefixes within the root address prefix of the VPC were advertised to the Direct Link and Transit Gateway. However, you could not advertise your address prefixes outside the root address prefix of the VPC. Route advertisement adds this capability. For example, Figure 1 advertises the route `0.0.0.0/0` in all zones to a zone-specific next hop.

![Edge proxy firewall use case](images/advertise-routes.png){: caption="Advertising routes" caption-side="bottom}

For more information, see [Creating a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table&interface=ui) and [Updating a routing table](/docs/vpc?topic=vpc-update-vpc-routing-table&interface=ui).

### Advertising route considerations
{: #rt-ad-route-considerations}

Be aware of the following considerations when advertising routes:

* You can advertise routes outside the root address prefix that is assigned to the VPC, so you can advertise your external networks or networks residing outside the VPC to a Direct Link or Transit Gateway.

* If multiple Transit Gateways or Direct Links are connected to your VPC, you can use route filtering on individual Transit Gateway or Direct Link connections to fine-tune which routes are advertised to which connections.

* If multiple routes are advertised to a destination prefix with different priorities, the route with the higher priority (lesser priority value) is used as primary and the route with lesser priority (higher priority value) is used as backup.

* If you advertise two different routes, such as `172.16.0.0/31` through `10.1.1.0` and `172.16.0.0/32` through `10.1.2.0`, the route with a `/32` prefix always takes precedence over the route with a `/31` prefix. This is consistent with the "Longest Prefix Match" ruleset. A longer prefix to a host destination is always preferred over a narrower prefix.

* Currently, there is no mechanism to flag duplicate routes from a Transit Gateway or Direct Link. The Transit Gateway selects the best path by using the most specific prefix and shortest AS path, as preferred. Otherwise, the Transit Gateway selects the route that it received first. This route might not be the oldest route at the VPC side, and it can change if routes to the Transit Gateway are refreshed internally.

* Routes advertised from a VPN gateway depend on the VPN type. Policy-based VPN gateways do not dynamically advertise routes. Only the CIDR prefixes defined in the VPN policy are propagated, and the routing table must be configured to accept routes from the VPN gateway. Route-based VPN gateways with BGP advertise routes dynamically.

   When advertising routes from a routing table that includes VPN connectivity, ensure that the VPN gateway type supports the intended route propagation behavior.
   {: note}

## Use cases
{: #use-cases}

The following use cases illustrate different routing scenarios.

### Use case 1: Edge proxy firewall
{: #edge-proxy-firewall-use-case}

![Edge proxy firewall use case](images/cr_use_case_1.png){: caption="Edge proxy firewall use case" caption-side="bottom}

| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| `10.10.0.0/16` | Delegate |  | Dallas DC 1|
| `10.11.0.0/16` | Delegate |  | Dallas DC 1|
| `0.0.0.0/0` | Deliver | `10.10.1.5` | Dallas DC 1|
{: caption="Edge proxy firewall egress routing table" caption-side="bottom"}

#### Objective: Edge proxy firewall
{: #objective-edge-proxy-firewall}

Secure flows to the public internet from behind a client-defined gateway, proxy, or firewall appliance, while also allowing resources on the other subnets to flow through the VPC-managed public gateway.

By using VPC custom routes with per subnet egress routing, you can create a table to override the system-implicit routing within the VPC network. In this example, the default table that was created by the system, where all subnets are assigned at creation, is unchanged. A table to direct traffic from subnet `10.10.1.0/24` through an edge proxy (NFV Proxy) is created as well.

Because the destination for the proxied flows is the public internet and, typically, follow the default route (`0.0.0.0/0`), you must first exempt flows toward internally reachable private networks by using the Delegate function of custom routes. Resources in the subnet `10.10.3.0/24` continue to use the public gateway that is attached to that subnet.

While the instances that use the proxy share a common subnet with the proxy in this example, you are not required to do so. With custom routes, you can specify a next-hop IP of an instance that is connected to a different subnet. By doing this, you can scale horizontally without concern for the subnet sizings of Edge services. For example, a proxy routing table can be attached to the subnet `10.10.3.0/24` and directs all public flows to the NFV proxy.

#### Functions used in an Edge proxy firewall
{: #functions-used-in-edge-proxy-firewall}

This use case uses the following functions of Edge proxy firewalls:

* Disabling the IP spoofing check on `10.10.0.5` and `10.10.1.5` interfaces to enable source address preservation. This action requires elevated IAM permissions on the instance.
* A public gateway for outbound flows with resources that are not directed through the NFV proxy.
* The floating IP attached to the `10.10.0.5` interface to enable inbound and outbound public flows directly to the NFV proxy.
* Adding stateful security groups and network access control lists (ACLs) to the instances interfaces and VPC subnets, respectively, to provide additional isolation resources within the VPC.

### Use case 2: Public load balancer
{: #public-load-balancer-use-case}

![Public load balancer use case](images/cr_use_case_2.png){: caption="Public load balancer use case" caption-side="bottom}

| Destination | Action | Next hop | Location|
|-------------|--------|----------|---------|
| `10.10.0.0/16` | Delegate |  | Dallas DC 1|
| `10.11.0.0/16` | Delegate |  | Dallas DC 1|
| `161.26.0.0/16` * | Delegate |  | Dallas DC 1|
| `166.8.0.0/14` * | Delegate |  | Dallas DC 1|
| `0.0.0.0/0` | Deliver | `10.10.1.5` | Dallas DC 1|
{: caption="Public load balancer Web egress routing table" caption-side="bottom"}

#### Objective: public load balancer
{: #objective-public-load-balancer}

Hosting web applications that use a client-defined application or network load balancer.

By utilizing VPC custom routes with per-subnet egress routing, you can create a table to override the system-implicit routing with the VPC network without updating routing information on each instance. In this example, the source IP from the internet users to the web layer is preserved. The web-to-app and app-to-db layers communicate by using implicit VPC routing and continue to use the public gateway for internet egress.

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

To have connectivity between subnets when having route `0.0.0.0/0` and the next hop is a VPN connection, you can add the following delegate routes:

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
* Currently, return traffic might fail due to asymmetric routing. This issue impacts all services that depend on ECMP static routes. For example, suppose you create two ECMP routes in VPC A with the destination being `10.134.39.64/26`, and the next hops are `192.168.2.4` and `192.168.2.5` respectively. These next hops are NFV device IP addresses. When you send traffic from instance A in the VPC, the packet is randomly routed to one of the next hops, and there is no guarantee that the return traffic follows the same path as the forwarding traffic due to the ECMP hash algorithm on the other side. This phenomenon is known as _asymmetric routing_. When asymmetric routing occurs, the problem is not with the routing itself, but the security group drops the traffic even if the security group rules allow this traffic because the security group sees the traffic in one path only. In general, it is advisable to avoid the use of ECMP routes.
* The ability to reach a next hop IP address in a custom route is not a determining factor in whether the route is used for forward traffic. This can cause issues when multiple routes with the same prefix (but different next hop IP addresses) are used, as traffic to unreachable next hop IP addresses might not be delivered.
* {{site.data.keyword.cloud_notm}} VPC permits the use of RFC-1918 and IANA-registered IPv4 address spaces privately within your VPC, with some exceptions in the IANA special-purpose ranges, and select ranges assigned to {{site.data.keyword.cloud_notm}} services. When using IANA-registered ranges within your enterprise, and within VPCs in conjunction with {{site.data.keyword.cloud_notm}} Transit Gateway or {{site.data.keyword.cloud_notm}} Direct Link, you must install custom routes in each zone. For more information, see [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana).
* In the case where 2 different advertised routes exist to the same destination, the following limitations apply:
   - When the next-hop for both routes is in the same zone, the route with the higher priority (that is, a lesser value for `priority`) is used as the primary path and the route with the lesser priority (that is, a higher value for `priority`) is used as the backup path. If the priority for both routes is the same, ECMP is used to route the traffic equally across the 2 routes.
   - When the next-hop for both routes is in different zones, the Transit Gateway or Direct Link router chooses the best route. In this case, that is the oldest advertised route.

### Route priority
{: #route-rules-considerations}

If multiple routes exist with the same destination CIDR/prefix, the route with the highest priority value is used. Routes with the same destination CIDR/prefix and priority use ECMP routing.

### Egress routes
{: #routes-egress-routes}

For egress custom routes, when you add a destination route, you must select a zone. However, the next hop doesn't have to be in the same zone. For ingress custom routes, the next hop must be in the same zone.

### Equal-Cost Multi-Path (ECMP)
{: #routes-ecmp}

The implicit router performs ECMP routing (multiple routes with the same destination, but different next hop addresses) with the following limitations:

* This only applies to routes with an action of `deliver`.
* Only two identical destination routes are permitted per zone, and each must have different next hop addresses.
* When ECMP is used, the return path might not take the same path.
* ECMP only supports a next hop type of IP address.

### Ingress routes
{: #routes-ingress}

* Currently, public ingress routing (`public internet` traffic choice) is available in the console only. CLI and API are forthcoming.
* Each ingress source type can be associated with up to one ingress routing table per VPC, however, a VPC can have multiple ingress routing tables and each ingress routing table can have one or more ingress types associated.
* Ingress traffic from a particular traffic source is routed by using the routes in the custom routing table that is associated with that traffic source.
* Custom routes in a custom routing table associated with an ingress traffic source, and with an action of `deliver`, must have a next hop IP contained by one of the address prefixes of the VPC in the availability zone where the route is added. In addition, the next hop IP must be configured on a virtual server interface in the VPC and availability zone where the route is targeted.
* Ingress traffic from a particular traffic source is routed by using the routes in the custom routing table that is associated with that traffic source. If no matching route is found in a custom routing table, routing continues by using the VPC system routing table. You can avoid this behavior with a custom routing table's default route with an action of **drop**.
* An ingress custom routing table containing any custom routes with destination IP (FIP) should be defined in the same zone as the virtual server instance that a FIP is associated with.
* Floating IPs that are bound to an IBM Cloud VPN gateway cannot be used as a destination for a custom route in an ingress routing table when the Public Internet (`route_internet_ingress`) source type is enabled.

### Unique prefix lengths
{: #routes-unique-prefix-lengths}

You can use a maximum of 14 unique prefix lengths per custom routing table. You can have multiple routes with the same prefix that count as only one unique prefix. For example, you might have multiple routes with a `/28` prefix. This counts as one unique prefix.

### VPN
{: #routes-vpn}

* When you're creating a route for a static, route-based VPN connection, you must enter the VPN connection ID for the next hop. The VPN gateway must be in the same zone as the subnet to which the routing table is associated. It is not recommended that you define a VPN gateway as the next hop in a zone that is different than the subnet that is associated with the routing table.
* A route with a VPN connection as the next-hop takes effect only when the VPN connection is active. This route is skipped during the routing lookup if the VPN connection is down.
* A custom routing table that contains custom routes with a next hop that is associated with a VPN connection cannot be associated with an ingress traffic source.
* Custom routes are only supported on route-based VPNs. If you are using policy-based VPNs, the routes are created automatically by the VPN service in the default routing table.

## Related links
{: #related-links-custom-routes}

These links provide additional information about {{site.data.keyword.cloud_notm}} routing tables and routes for VPC:

* [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference#custom-routes-section)
* [VPC API reference](/apidocs/vpc)
* [Activity tracking events](/docs/vpc?topic=vpc-at_events#events-custom-routes)
* [Routing tables for VPC infrastructure resources for Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpc_routing_table){: external}
* [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started&interface=ui) (see Actions tab for [VPC roles and actions](/docs/account?topic=account-iam-service-roles-actions#is.vpc-roles))
* [Quotas and service limits](/docs/vpc?topic=vpc-quotas#routing-tables-routes-quotas)
* Ingress traffic sources: IBM Cloud [Direct Link](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) and [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started)
* Centralizing communication through a VPC transit hub and spoke architecture, [Part one](/docs/solution-tutorials?topic=solution-tutorials-vpc-transit1) and [Part two](/docs/solution-tutorials?topic=solution-tutorials-vpc-transit2).
