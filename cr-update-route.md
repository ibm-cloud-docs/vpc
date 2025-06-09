---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-09"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating a route
{: #update-vpc-route}

You can update a route for an IBM Cloud service by using the console, CLI, API, or Terraform.
{: shortdesc}

## Updating a route in the console
{: #cr-route-update-the-ui}
{: ui}

To update a route in the console, follow these steps:

1. Make sure to review [Limitations and guidelines](/docs/vpc?topic=vpc-about-custom-routes&interface=ui#limitations-custom-routes).
1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation Menu** ![Navigation Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Routing tables**. The Routing tables for VPC page appears.
1. Locate the routing table with the routes that you want to update, and click its name in the table.
1. In the Routes section, locate the route that you want to update in the table. Then, click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") to edit or delete the route. You can change the name, priority, advertise, and route type information.
1. Click **Save** to save your updates.

## Updating a route from the CLI
{: #cr-update-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To update a VPC route from the CLI, run the following command:

```sh
ibmcloud is vpc-routing-table-route-update VPC ROUTING_TABLE ROUTE --name NEW_NAME [--priority PRIORITY] [--next-hop NEXT_HOP [--vpngw VPNGW]] [--advertise true | false] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`VPC`
:   Is the ID or name of the VPC.

`ROUTING_TABLE`
:   Is the ID or name of the VPC routing table.

`ROUTE`
:   Is the ID or name of the VPC route.

`--name`
:   Is the new name of the route.

`--priority`
:   Is the route's priority. Lesser values have higher priority. If a custom routing table contains routes with the same destination, the route with the highest priority (smallest value) is selected.

`--next-hop`
:   If the action is `deliver`, this value is the IP address or VPN connection ID or name of the next hop to which to route packets.

`--advertise`
:   Advertise to a direct link, transit gateway, or both ingress sources. One of `true`, `false`.

`--output`
:   Formats the output in JSON.

`--q, quiet`
:   Causes the command to run silently and does not generate any output.

### CLI examples
{: #examples-cli-route-update}
{: cli}

`ibmcloud is vpc-routing-table-route-update 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 72b27b5c-f4b0-48bb-b954-5becc7c1d456 72b27b5c-f4b0-48bb-b954-5becc7c1d4ef --name my-vpc-route --priority 1`

`ibmcloud is vpc-routing-table-route-update 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 72b27b5c-f4b0-48bb-b954-5becc7c1d456 72b27b5c-f4b0-48bb-b954-5becc7c1d4ef --name my-vpc-route --next-hop 10.0.0.2`

`ibmcloud is vpc-routing-table-update 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 72b27b5c-f4b0-48bb-b954-5becc7d1d2d3 -—advertise_routes_to direct_link --direct-link-ingress true -—output JSON`

`ibmcloud is vpc-routing-table-update my-vpc my-vpc-routing-table -—advertise-routes-to transit_gateway  -—transit-gateway-ingress true --output JSON`

`ibmcloud is vpc-routing-table-update my-vpc my-vpc-routing-table -—advertise-routes-to direct_link, transit_gateway  --direct-link-ingress true —transit-gateway-ingress true -—output JSON`

## Updating a route with the API
{: #cr-route-update-the-api}
{: api}

To update a destination route with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store values for the following variables to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    export RoutingTableId=<your_routing_table_id>
    ```
    {: codeblock}

1. To update a route:

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/vpcs/$vpc_id/routing_tables/$routing_table_id/routes/$id?version=2023-11-28&generation=2" \
   -H "Authorization: Bearer $iam_token" \
   -d '{
      "name": "my-vpc-route-updated"
    }'
   ```
   {: codeblock}

## Updating a route with Terraform
{: #cr-update-route-terraform}
{: terraform}

To update a route with Terraform, follow these steps:

1. Set up your Terraform environment.
1. Update priority in the existing route resource:

   ```terraform
   resource "ibm_is_vpc_routing_table_route" "example" {
     vpc           = ibm_is_vpc.example.id
     routing_table = ibm_is_vpc_routing_table.example.routing_table
     zone          = "us-south-1"
     name          = "custom-route-2"
     destination   = "192.168.4.0/24"
     action        = "deliver"
     priority      = 4
     next_hop      = ibm_is_vpn_gateway_connection.example.gateway_connection // Example value "10.0.0.4"
   }
   ```

For documentation about the `ibm_is_vpc_routing_table_route` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpc_routing_table_route).{: external}
