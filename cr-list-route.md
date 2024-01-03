---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-18"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Listing routes of a routing table
{: #list-routes-routing-table}

You can list the routes of a VPC routing table by using the UI, CLI, or API.
{: shortdesc}

## Listing routes of a routing table in the UI
{: #cr-list-using-the-ui}
{: ui}

To list the routes of a VPC routing table in the UI, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the Navigation Menu ![Navigation Menu](/images/menu_icon.png), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>Routing tables** in the Network section. The Routing tables for VPC page appears.
1. Click the routing table name or number of routes that are associated with the routing table. The Routing table details page appears, listing the routes associated with the routing table.

Descriptions of these columns are as follows:

| Column | Description |
|-------|-------------|
| Name  | Name of the route. |
| Destination | Destination CIDR of the route. |
| State | The lifecycle state. Custom route states are:  \n * **Pending** - In the process of being provisioned in the specified VPC and zone.  \n * **Stable** - Provisioning was successful.  \n * **Deleting** - A **Stable** or **Failed** route is in the process of being deleted.  \n * **Deleted** - Deletion was successful.  \n * **Failed** - The route is not functional. From this state, you can delete only the route.|
| Zone  |  xxx |
| Action | Values are:  \n * **Deliver** - Routes the packet to the next hop target. You can add multiple routes with the same address prefix. The virtual router performs equal-cost, multi-path routing (ECMP) by using the different next hop IP addresses.  \n * **Drop** - Drops the packet.  \n * **Delegate** - Routes the packet by using the system routing table. 1 |
| Next hop | The IP address of the next hop to which to route packets. |
| Route origin | Origin of the route. |
{: caption="Table 1. Destination routes details" caption-side="bottom"}

1 - A system routing table is maintained for each VPC. A VPC can have a presence in multiple zones, and the VPC's system routing table is different in each zone. It is used for routing traffic when no matching route is found in the custom routing table that is associated with the subnet of which the traffic is egressing.

## Listing routes of a routing table from the CLI
{: #cr-view-details-route-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To view details of a route by using the CLI, run the following command:

```sh
ibmcloud is vpc-routing-table-route VPC ROUTING_TABLE ROUTE [--json]
```
{: pre}

Where:

* **VPC** is the ID of the VPC.
* **ROUTING_TABLE** is the ID of the VPC routing table.
* **ROUTE** is the ID of the VPC route.
* **--json** formats output in JSON.

## Listing routes of a routing table with the API
{: #cr-view-details-route-using-the-api}
{: api}

To view details of a route with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API command:

   ```sh
   export VpcId=<your_vpc_id>
   export RoutingTableId=<your_routing_table_id>
   export RouteId=<your_route_id>
   ```
   {: codeblock}

1. View details of a specific route:

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables/$RoutingTableId/routes/$RouteId?version=$api_version&generation=2" \
      -H "Authorization: $iam_token"
   ```
   {: codeblock}
