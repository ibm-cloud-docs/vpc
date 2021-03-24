---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-25"

keywords: custom routes

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:term: .term}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating a route
{: #create-vpc-route}

You can control the flow of network traffic in your VPC by configuring routes. Use VPC routes to specify the next hop for packets, based on their destination addresses.
{: shortdesc}

Multiple routing tables can exist for each zone in your VPC. When a packet leaves a subnet, the system evaluates it against the routing table in the subnet's zone to determine where to send the packet next. Each routing table has a default route, but you can add custom routes to your routing tables.

A VPC route has three main components:

* The destination CIDR
* The next hop where the packet will route
* The zone

Any traffic that originates in the specified zone of the VPC and has a destination address within the specified destination CIDR routes to the next hop. If the destination address is within the destination CIDR for multiple VPC routes, the most specific route is used. If there are two or more equally specific routes, the traffic is round-robin that is distributed between each route.

Each route has a destination property, which includes a prefix length (`/24` in `10.2.0.0/24`). The number of unique prefix lengths that are supported per custom routing table is 14. Multiple routes with the same prefix count as only one unique prefix.

VPC address prefixes are no longer restricted to RFC-1918 addresses. You must now configure VPCs that use both non-RFC-1918 addresses and have public connectivity (floating IPs or public gateways) using a custom route that contains the new `Delegate-VPC` action. You must specify this action for destination CIDRs that are non-RFC-1918 compliant and also outside of the VPC, such as for destinations that are reachable through Direct Link (2.0), Transit Gateway, or VPC classic access. For more information about when to use the `Delegate-VPC` action, see [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana). 
{: note}

The `Delegate-VPC` action is required if both are true:
   * The VPC uses non-RFC-1918 addresses
   * The VPC has public connectivity

The `Delegate-VPC` action is NOT required if:
   * The VPC uses only RFC-1918 addresses
   * The VPC has no public connectivity

You can create a route for an IBM Cloud service by using the UI, CLI, or API.

## Creating a route using the UI
{: #cr-route-using-the-ui}
{: ui}

To create a route by using the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Routing tables** in the Network section. The Routing tables for VPC page appear.
1. Select the VPC associated with the routing table you want to view. Then, click the name of the routing table to show its details.
1. Scroll to the Routes section and click **Create**.

   ![New route side panel](./images/cr-create-route.png)
1. In the New route side panel, enter the following information:

   * **Destination CIDR** - The destination CIDR of the route.
   * **Action** - Values are:

      * **Delegate-VPC** - Delegates to the system's built-in routes, ignoring internet-bound routes. Required if the VPC uses non-RFC-1918 addresses and also has public connectivity. See [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana) for details.
      * **Delegate** - Routes the packet by using the system routing table.
      * **Deliver** - Routes the packet to the next hop target. You can add multiple routes with the same address prefix. The virtual router performs equal-cost, multi-path routing (ECMP) using the different next hop IP addresses.
      * **Drop** - Drops the packet.

   * Type - (Egress traffic type only) Choose either **IP Address** or **VPN connection**. If you select **IP Address**, enter the **Next hop**. If you choose **VPN Connection**, select from the list of available VPN gateways or connections.

   Traffic egressing a subnet routes that use the custom routing table that is associated with the subnet. If no matching route is found in a custom routing table, routing continues using the VPC system routing table. You can avoid this behavior with a custom routing table default route with an action of **Drop**.
   {: note}

1. Click **Save** to save the new route. The route appears on the Routing table details page.

## Creating a route using the CLI
{: #cr-route-using-the-cli}
{: cli}

To create a VPC route by using the CLI, run the following command:

```
ibmcloud is vpc-routing-table-route-create VPC ROUTING_TABLE --zone ZONE_NAME --destination DESTINATION_CIDR --next-hop NEXT_HOP [--action delegate_vpc | delegate | deliver | drop] [--name NAME] [--json]
```
{: pre}

Where:

* **VPC** is the ID of the VPC.
* **ROUTING_TABLE** is the ID of the VPC routing table.
* **--zone** is the name of the zone.
* **--action** is the action to perform with a packet that matches the route. Actions are **delegate_vpc**, **delegate**, **deliver**, and **drop**.
* **--destination** is the destination CIDR of the route. At most, two routes per zone in a table can have the same destination, and only if both routes have an action of **deliver**.
* **--next-hop** - If the action is **deliver**, this is the IP address or VPN connection ID of the next hop to which to route packets.
* **--name** is the name of the VPC routing table.
* **--json** formats the output in JSON.

## Creating a route using the API
{: #cr-route-using-the-api}
{: api}

To create a destination route by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
2. Store values for the following variables to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    export RoutingTableId=<your_routing_table_id>
    ```
    {: codeblock}

3. Create a route:

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables/$RoutingTableId/routes?version=$api_version&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
        "name": "my-new-route",
        "zone": {"name": "us-south-2"},
        "action": "deliver",
        "destination": "10.10.10.0/24",
        "next_hop": {"address": "10.0.0.3"}
      }'
   ```
   {: codeblock}
