---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-18"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing details of a routing table
{: #view-details-routing-table}

You can view details of a routing table by using the UI, CLI, or API. From the Routing table details page, you can also view details of attached routes and subnets, as well as navigate to other pages to view VPC details, manage address prefixes, and more.
{: shortdesc}

## Viewing details of a routing table in the UI
{: #cr-routing-table-using-the-ui}
{: ui}

To view the details of a routing table in the UI, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the Navigation Menu ![Navigation Menu](/images/menu_icon.png), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>Routing tables** in the Network section. The Routing tables for VPC page appears.

1. Click the name of the routing table that you want details for. The Routing table details page appears.

Field descriptions are as follows.

| Field | Description |
|-------|-------------|
| Name | Click the Edit icon ![Edit icon](/images/edit.png) to update the name. |
| Routing table ID | The ID of this routing table. |
| Virtual private cloud | The VPC for this routing table. |
| Created date | The origination date of the routing table.|
| Routes | The number of destination routes. |
| Default | Indicates whether this is the default routing table. |
| Attached subnets | The number of attached subnets. Click the Subnets tab to see subnet details. |
{: caption="Table 1. Routing table details" caption-side="bottom"}

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

* **VPC** is the ID of the VPC.
* **ROUTING_TABLE** is the ID of the VPC routing table.
* **SUBNET** is the ID of the subnet.
* **--json** formats output in JSON.

## Viewing details of a routing table with the API
{: #cr-routing-table-using-the-api}
{: api}

To view the details of a routing table by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
2. Store the following values in variables to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    export RoutingTableId=<your_routing_table_id>
    ```
    {: codeblock}

3. View details of a routing table:

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
