---

copyright:
  years: 2020, 2025
lastupdated: "2025-10-15"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a route
{: #delete-vpc-route}

You can delete a route for an IBM Cloud service by using the console, CLI, API, or Terraform.
{: shortdesc}

## Deleting a route in the console
{: #cr-delete-using-the-ui}
{: ui}

To delete a route in the console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Routing tables**. The Routing tables for VPC page appears.
2. Click the number of routes, or the routing table name that contains the route.
3. Click the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions") next to the route that you want to delete, then click **Delete**. Click **Delete** again to confirm the deletion.

## Deleting a route from the CLI
{: #cr-delete-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To delete a route from the CLI, run the following command:

```sh
ibmcloud is vpc-routing-table-route-delete VPC ROUTING_TABLE ROUTE [-f, --force]
```
{: pre}

Where:

`VPC`
:   Is the ID or name of the VPC.

`ROUTING_TABLE`
:   Is the ID or name of the VPC routing table.

`ROUTE`
:   Is the ID or name of the VPC route.

`-f, --force`
:   Forces the operation without confirmation.


## Deleting a route with the API
{: #cr-delete-using-the-api}
{: api}

To delete a route with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    export RoutingTableId=<your_routing_table_id>
    export RouteId=<your_route_id>
    ```
    {: codeblock}

1. Delete a route:

   ```sh
   curl -X DELETE "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables/$RoutingTableId/routes/$RouteId?version=$api_version&generation=2" \
      -H "Authorization: $iam_token"
   ```
   {: codeblock}

## Deleting a route with Terraform
{: #cr-delete-route-terraform}
{: terraform}

To delete a route with Terraform, follow these steps:

1. Set up your Terraform environment.
1. Delete a route:

      ```terraform
      terraform destroy --target="ibm_is_vpc_routing_table_route.example"
      ```

      For more information about the `terraform destroy` command, see the [Terraform Registry](https://developer.hashicorp.com/terraform/tutorials/state/resource-targeting#destroy-your-infrastructure).{: external}
