---

copyright:
  years: 2020, 2025
lastupdated: "2025-10-15"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Listing routes of a routing table
{: #list-routes-routing-table}

You can list the routes of a VPC routing table by using the console, CLI, API, or Terraform.
{: shortdesc}

## Listing routes of a routing table in the console
{: #cr-list-using-the-ui}
{: ui}

To list the routes of a VPC routing table in the console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Routing tables**. The Routing tables for VPC page appears.
1. Click the routing table name or number of routes that are associated with the routing table. The Routing table details page appears, listing the routes associated with the routing table.

Descriptions of these columns are as follows:

| Column | Description |
|-------|-------------|
| Name  | Name of the route. |
| Destination | Destination CIDR of the route. |
| State | The lifecycle state. Custom route states are:  \n * **Pending** - In the process of being provisioned in the specified VPC and zone.  \n * **Stable** - Provisioning was successful.  \n * **Deleting** - A **Stable** or **Failed** route is in the process of being deleted.  \n * **Deleted** - Deletion was successful. \n * **Failed** - The route is not functional. From this state, you can delete only the route.|
| Zone  |  Indicates the zone. |
| Action | Values are: \n * **Deliver** - Routes the packet to the next hop target. You can add multiple routes with the same address prefix. The virtual router performs equal-cost, multi-path routing (ECMP) by using the different next hop IP addresses. \n * **Drop** - Drops the packet. \n * **Delegate** - Routes the packet by using the system routing table.[^fn1] |
| Next hop | The IP address of the next hop to which to route packets. |
| Route origin | Origin of the route. |
{: caption="Destination routes details" caption-side="bottom"}

[^fn1]: A system routing table is maintained for each VPC. A VPC can have a presence in multiple zones, and the VPC's system routing table is different in each zone. It is used for routing traffic when no matching route is found in the custom routing table that is associated with the subnet of which the traffic is egressing.

## Listing routes of a routing table from the CLI
{: #cr-view-details-route-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To list all the routes of a VPC routing table, run the following command:

```sh
ibmcloud is vpc-routing-table-routes VPC ROUTING_TABLE [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`VPC`
:   Is the ID or name of the VPC.

`ROUTING_TABLE`
:   Is the ID or name of the VPC routing table.

`--output`
:   Specify the output format, only JSON is supported.

`--q, --quiet`
:   Specify verbose output.


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

1. List all routes in a routing table:

   ```curl
   curl -X GET "$vpc_api_endpoint/v1/vpcs/$vpc_id/routing_tables?version=$api_version&generation=2" \
   -H "Authorization: Bearer $iam_token"
   ```
   {: codeblock}

## Listing routes of a routing table with Terraform
{: #cr-list-route-terraform}
{: terraform}

To list routes of a routing table with Terraform, follow these steps:

1. Set up your Terraform environment.
1. Use one of the following examples:

   * To list all the routes:

      ```terraform
      data "ibm_is_vpc_routing_table_routes" "example" {
        vpc           = ibm_is_vpc.example.id
        routing_table = ibm_is_vpc_routing_tables.example.routing_table
      }
      ```

      For more information about the `ibm_is_vpc_routing_table_routes` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_vpc_routing_table_routes).{: external}

   * To retrieve a single route with an identifier:

      ```terraform
      data "ibm_is_vpc_routing_table_route" "example_route" {
        vpc             = ibm_is_vpc.example_vpc.id
        routing_table = ibm_is_vpc_routing_table.example_rt.routing_table
        route_id         = ibm_is_vpc_routing_table_route.example_route.route_id
      }
      ```

      For more information about the `ibm_is_vpc_routing_table_route` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_vpc_routing_table_route).{: external}
