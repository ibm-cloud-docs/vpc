---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-24"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Listing routing tables for a VPC
{: #list-routing-tables-for-vpc}

You can list routing tables for a VPC by using the UI, CLI, API, or Terraform.
{: shortdesc}

## Listing routing tables for a VPC in the UI
{: #cr-list-tables-using-the-ui}
{: ui}

To list the routing tables for a VPC in the UI, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, Select the Menu icon ![Navigation Menu](images/menu_icon.png), then click **Infrastructure > Network > Routing tables**  in the Network section. The Routing tables for VPC page appears.
1. From the Virtual private cloud drop-down list, select the VPC that you want to list routing tables for.

   Column descriptions are as follows:

   * **Name** - Indicates the name of the routing table. Click this link to see the details of the routing table. You can also edit and change this name.

      The `VPC default tag` specifies the default routing table of the specified VPC.

   * **Status** - Indicates the status of the routing table.
   * **Accepts routes from** - Specifies whether traffic is accepted from a VPN server or VPN gateway.
   * **Traffic source** - Specifies the source of traffic (for example, a **Direct link** or **VPC zone**).
   * **Routes** - States the number of routes that are attached to the routing table.
   * **Attached subnets** - Indicates the number of subnets that are attached to the routing table.

1. From the Routing tables for VPC page, you can create, delete, and view the details of a routing table.

The Actions menu ![Actions menu](images/overflow.png) is used to delete a routing table. Keep in mind that you can do this action only on routing tables without attached subnets.
{: note}

## Listing routing tables for a VPC from the CLI
{: #cr-list-tables-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To list the routing tables for a VPC from the CLI, run the following command:

```sh
ibmcloud is vpc-routing-tables VPC [--json]
```
{: pre}

Where:

`VPC`
:   Is the ID or name of the VPC.

`--json`
:   Formats output in JSON.

## Listing routing tables for a VPC with the API
{: #cr-list-tables-using-the-api}
{: api}

To list the routing tables for a VPC with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the `VpcId` value in a variable to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    ```
    {: pre}

1. List all routings tables for a VPC:

   ```curl
   curl -X GET "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables?version=$api_version&generation=2" \
      -H "Authorization: $iam_token"
   ```

## Listing routing tables with Terraform
{: #cr-list-routing-tables-terraform}
{: terraform}

To list all routing tables or a specific routing table with Terraform, follow these steps:

1. Set up your Terraform environment.
1. Use one of the following examples:

   * To list all routing tables for a VPC:

      ```terraform
      data "ibm_is_vpc_routing_tables" "example" {
        vpc = ibm_is_vpc.example.id
      }
      ```

      For more information about the `ibm_is_vpc_routing_tables` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_vpc_routing_tables).{: external}

   * To retrieve a single routing table specified by the identifier:

      ```terraform
      data "ibm_is_vpc_routing_table" "example_routing_table" {
        vpc                 = ibm_is_vpc.example_vpc.id
        routing_table     = ibm_is_vpc_routing_table.example_rt.routing_table
      }
      ```

      For more information about the `ibm_is_vpc_routing_table` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_vpc_routing_table).{: external}

   * To retrieve the default routing table for the VPC specified by the identifier:

      ```terraform
      data "ibm_is_vpc_default_routing_table" "example" {
        vpc = ibm_is_vpc.example.id
      }
      ```

      For more information about the `ibm_is_vpc_default_routing_table` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_vpc_default_routing_table).{: external}
