---

copyright:
  years: 2020, 2024
lastupdated: "2024-07-25"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing details of a routing table
{: #view-details-routing-table}

You can view details of a routing table by using the UI, CLI, API, or Terraform. From the Routing table details page in the console, you can also view details of attached routes and subnets, as well as go to other pages to view VPC details, manage address prefixes, and more.
{: shortdesc}

## Viewing details of a routing table in the UI
{: #cr-routing-table-using-the-ui}
{: ui}

To view the details of a routing table in the UI, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, Select the menu icon ![Navigation menu](images/menu_icon.png), then click **VPC Infrastructure > Routing tables** in the Network section. The Routing tables for VPC page appears.

1. Click the name of the routing table that you want details for. The Routing table details page appears.

Field descriptions are as follows.

| Field | Description |
|-------|-------------|
| Name | Click the Edit icon ![Edit icon](images/edit.png) to update the name. |
| Routing table ID | The ID of this routing table. |
| Virtual private cloud | The VPC for this routing table. |
| Created date | The origination date of the routing table.|
| Routes | The number of destination routes. |
| Default | Indicates whether the routing table is the default routing table. |
| Attached subnets | In the Traffic section, view the number of attached subnets. Click the Subnets tab to see subnet details. |
{: caption="Table 1. Routing table details and Traffic" caption-side="bottom"}

For descriptions of the routing table columns, see [Listing routes of a routing table](/docs/vpc?topic=vpc-list-routes-routing-table).
{: note}

## Viewing details of a routing table from the CLI
{: #cr-routing-table-using-the-cli}
{: cli}

To view details of a specific routing table:

```sh
ibmcloud is vpc-routing-table VPC ROUTING_TABLE [--json]
```
{: pre}

To view details of the default routing table:

```sh
ibmcloud is vpc-default-routing-table VPC [--json]
```
{: pre}

To view details of a routing table that is attached to the subnet:

```sh
ibmcloud is subnet-routing-table SUBNET [--json]
```
{: pre}

Where:

`VPC`
:   Is the ID or name of the VPC.

`ROUTING_TABLE`
:   Is the ID or name of the VPC routing table.

`SUBNET`
:   Is the ID or name of the subnet.

`--json`
:   Formats output in JSON.

## Viewing details of a routing table with the API
{: #cr-routing-table-using-the-api}
{: api}

To view the details of a routing table with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    export RoutingTableId=<your_routing_table_id>
    ```
    {: codeblock}

1. View details of a routing table:

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables/$RoutingTableId?version=$api_version&generation=2" \
        -H "Authorization: $iam_token"
   ```
   {: codeblock}

   To view the routing table that is attached to a subnet:

   ```sh
   export SubnetId=<your_subnet_id>
   curl -X GET "$vpc_api_endpoint/v1/subnets/$SubnetId/routing_table?version=$api_version&generation=2" \
        -H "Authorization: $iam_token"
   ```
   {: codeblock}

## Viewing details of a routing table with Terraform
{: #cr-view-details-terraform}
{: terraform}

To view details of all or a specific routing table with Terraform, follow these steps:

1. Set up your Terraform environment.
1. Use one of the following examples:

   * To list and view details of all routing tables for a VPC:

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
