---

copyright:
  years: 2020
lastupdated: "2020-10-30"

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

# Creating a route
{: #create-vpc-route}

You can control the flow of network traffic in your VPC by configuring routes. Use VPC routes to specify the next hop for packets, based on their destination addresses.
{: shortdesc}

Multiple route tables can exist for each zone in your VPC. When a packet leaves a subnet, the system evaluates it against the route table in the subnet's zone to determine where to send the packet next. Each route table has a default route, but you can add custom static routes to your routing tables.

A VPC route has three main components:

* The destination CIDR
* The next hop where the packet will route
* The zone

Any traffic that originates in the specified zone of the VPC and has a destination address within the specified destination CIDR routes to the next hop. If the destination address is within the destination CIDR for multiple VPC routes, the most specific route is used. If there are two or more equally specific routes, the traffic is round-robin distributed between each route.

Each route has a destination property, which includes a prefix length (`/24` in `10.2.0.0/24`). The number of unique prefix lengths that are supported per custom route table is 14. Multiple routes with the same prefix count as only one unique prefix.
{: important}

You can create a route for an IBM Cloud service by using the UI, CLI, or API.

## Using the UI
{: #cr-route-using-the-ui}

To create a route by using the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Routing tables** in the Network section. The Routing tables for VPC page appears.

2. To add a route, click **New route**. The Routing table details page shows.

   ![New route side panel](./images/cr-create-route.png)

3. In the New route side panel, enter the following information:

   * **Destination CIDR** - The destination CIDR of the route.
   * **Action** - Values are:<ul><li>**Deliver** - Routes the packet to the next hop target. You can add multiple routes with the same address prefix. The virtual router performs equal-cost, multi-path routing (ECMP) using the different next hop IP addresses.</li><li>**Drop** - Drops the packet.</li><li>**Delegate** - Routes the packet using the system routing table.</li></ul>
   * Type - Choose either **IP Address** or **VPN Connection**. If you select **IP Address**, enter the **Next hop**. If you choose **VPN Connection**, select from the list of available VPN gateways or connections.

Traffic egressing a subnet routes using the custom route table associated with the subnet. If no matching route is found in a custom route table, routing continues using the VPC system route table. You can avoid this behavior with a custom route table default route with an action of **Drop**.
{: note}

4. Click **Save** to save the new route. The route appears on the Routing table details page.

## Using the CLI
{: #cr-route-using-the-cli}

To create a VPC route by using the CLI, run the following command:

```
ibmcloud is vpc-routing-table-route-create VPC ROUTING_TABLE \
--zone ZONE_NAME --destination DESTINATION_CIDR \
--next-hop NEXT_HOP [--action delegate | deliver | drop] \
[--name NAME] \
[--json]
```

Where:

* **VPC** is the ID of the VPC.
* **ROUTING_TABLE** is the ID of the VPC routing table.
* **--zone** is the name of the zone.
* **--action** is the action to perform with a packet that matches the route. Actions are **delegate**, **deliver**, and **drop**.
* **--destination** is the destination CIDR of the route. At most, two routes per zone in a table can have the same destination, and only if both routes have an action of **deliver**.
* **--next-hop** - If the action is **deliver**, this is the IP address or VPN connection ID of the next hop to which to route packets.
* **--name** is the name of the VPC routing table.
* **--json** formats the output in JSON.

## Using the API
{: #cr-route-using-the-api}

To create a destination route by using the API:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
2. Store values for the following variables to be used in the API command:

    ```
    export VpcId=<your_vpc_id>
    export RoutingTableId=<your_routing_table_id>
    ```
    {: codeblock}

3. Create a route:

   ```
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
