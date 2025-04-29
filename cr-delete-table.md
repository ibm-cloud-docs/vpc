---

copyright:
  years: 2020, 2025
lastupdated: "2025-04-29"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a routing table
{: #delete-routing-table}

You can delete a routing table for an IBM Cloud service by using the console, CLI, API, or Terraform.
{: shortdesc}

## Deleting a routing table in the console
{: #cr-delete-table-using-the-ui}
{: ui}

To delete a routing table in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation Menu** ![Menu icon](images/menu_icon.png), then click **Infrastructure > Network > Routing tables**. The routing tables for VPC page appear.
2. Click the Actions menu ![Actions menu](images/overflow.png) next to the routing table that you want to delete, then click **Delete**. Click **Delete** again to confirm the deletion. Alternatively, you can click **Actions > Delete** from the routing table details page.

   You can delete a routing table only when it does not have any attached subnets and is not the default for its VPC. If the routing table that you want to delete is attached to a subnet, you can detach it by either reassigning the routing table to another subnet or by deleting the subnet. To reassign the subnet, go to the details page of the routing table, click the **Subnets** tab, and use the Actions menu ![Actions menu](images/overflow.png) to reassign the subnet to another routing table. To delete the subnet, go to the list of subnets, and click the Actions menu ![Actions menu](images/overflow.png), and select **Delete**.
   {: important}

## Deleting a routing table from the CLI
{: #cr-delete-table-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To delete a routing table from the CLI, run the following command:

```sh
ibmcloud is vpc-routing-table-delete VPC ROUTING_TABLE [-f, --force]
```
{: pre}

Where:

`VPC`
:   Is the ID of the VPC.

`ROUTING_TABLE`
:   Is the ID of the VPC routing table.

`-f, --force`
:   Forces the operation without confirmation.

## Deleting a routing table with the API
{: #cr-delete-table-using-the-api}
{: api}

To delete a routing table with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
2. Store the following values in variables to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    export RoutingTableId=<your_routing_table_id>
    ```
    {: codeblock}

3. Delete a routing table:

   ```sh
   curl -X DELETE "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables/$RoutingTableId?version=$api_version&generation=2" \
     -H "Authorization: $iam_token"
   ```
   {: codeblock}

## Deleting a routing table with Terraform
{: #cr-delete-route-table-terraform}
{: terraform}

To delete a routing table with Terraform, follow these steps:

1. Set up your Terraform environment.
1. Delete a routing table:

   ```terraform
   terraform destroy --target="ibm_is_vpc_routing_table.example"
   ```

      For more information about the `terraform destroy` command, see the [Terraform Registry](https://developer.hashicorp.com/terraform/tutorials/state/resource-targeting#destroy-your-infrastructure).{: external}
