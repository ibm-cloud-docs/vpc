---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-24"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching subnets to a routing table
{: #attach-subnets-routing-table}

When you view details of a routing table, you can also view details of attached subnets, or attach a subnet to apply routing table routes to a particular subnet. Note that a routing table can be associated with multiple subnets. However, each subnet can be assigned only one routing table.
By default, any subnet that is not associated with a routing table is associated with the default routing table. You can also reassign the routing table of a subnet to another subnet.
{: shortdesc}

You can attach a subnet to a routing table, or reassign a routing table to a particular subnet by using the UI, CLI, API, or Terraform.

## Attaching subnets to a routing table in the UI
{: #cr-attach-subnet-ui}
{: ui}

To attach a routing table to a subnet in the UI, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the menu icon ![Navigation menu](images/menu_icon.png), then click **Infrastructure > Network > Routing tables** in the Network section. The Routing tables for VPC page appears.
1. Click the name of the routing table in which you want to view subnet details. Alternatively, you can click the number of attached subnets.
1. From the Subnets tab, click **Attach subnet**. The Attach subnets to routing table side panel appears.
1. Select a subnet from the drop-down list. The current routing table, IP range, and location of the subnet shows.
1. Click **Attach**.

To reassign a routing table to a subnet, follow these steps:

1. From the routing table details page, click the Subnets tab.
1. Click the Actions menu ![Actions menu](images/overflow.png) next to the subnet, then click **Reassign routing table**.
1. From the Reassign subnet routing table side panel, click the subnet that you want to assign to this routing table.
1. Click **Reassign**.

## Attaching subnets to a routing table from the CLI
{: #cr-attach-subnets-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To attach subnets to a routing table from the CLI, run the following command:

```sh
ibmcloud is subnet-update SUBNET_ID --routing-table-id ROUTING_TABLE_ID
```
{: pre}

Where:

`SUBNET_ID`
:   Is the ID or name of the subnet you want to update.

`ROUTING_TABLE_ID`
:   Is the ID or name of the routing table that you want to assign the subnet to.

## Attaching subnets to a routing table with the API
{: #cr-attach-subnets-using-the-api}
{: api}

To attach subnets to a routing table with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the `VpcId` value in a variable to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    ```
    {: pre}

1. Attach a subnet to a routing table:

    ```sh
    curl -X PUT "$vpc_api_endpoint/v1/subnets/$subnet_id/routing_table?version=$api_version&generation=2"
    -H "Authorization: $iam_token"
    -d '{
      "id": "37491549-51c0-4acc-9e7b-9ab3628e1a68"
        }'
    ```
    {: codeblock}

## Attaching subnets to a routing table with Terraform
{: #cr-attach-subnet-terraform}
{: terraform}

To view details of all or a specific routing table with Terraform, follow these steps:

1. Set up your Terraform environment.
1. Use one of the following examples:

   * To attach a subnet to a routing table:

      ```terraform
      resource "ibm_is_subnet_routing_table_attachment" "example" {
        subnet        = ibm_is_subnet.example.id
        routing_table = ibm_is_vpc_routing_table.example.routing_table
      }
      ```

      For more information about the `ibm_is_subnet_routing_table_attachment` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_subnet_routing_table_attachment).{: external}

   * On an existing subnet resource, update the `routing_table` attribute that is specified by the identifier:

      ```terraform
      resource "ibm_is_subnet" "example" {
        name            = "example-subnet"
        vpc             = ibm_is_vpc.example.id
        zone            = "us-south-1"
        ipv4_cidr_block = "10.240.0.0/24"
        routing_table   = ibm_is_vpc_routing_table.example.routing_table
      }
      ```

      For more information about the `routing_table` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_subnet#routing_table).{: external}
