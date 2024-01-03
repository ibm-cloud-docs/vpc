---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-18"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Listing routing tables for a VPC
{: #list-routing-tables-for-vpc}

You can list routing tables for a VPC by using the UI, CLI, or API.
{: shortdesc}

## Listing routing tables for a VPC in the UI
{: #cr-list-tables-using-the-ui}
{: ui}

To list the routing tables for a VPC in the UI, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the Navigation Menu ![Navigation Menu](/images/menu_icon.png), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>Routing tables** in the Network section. The Routing tables for VPC page appears.
1. From the Virtual private cloud drop-down list, select the VPC that you want to list routing tables for.

   Column descriptions are as follows:

   * **Name** - Indicates the name of the routing table. Click this link to see the details of the routing table. You can also edit and change this name.
   * **Default** - Specifies the default routing table of the specified VPC.
   * **Traffic type** - Specifies the type of traffic.  For example, **Egress**. 
   * **Routes** - States the number of routes that are attached to the routing table.
   * **Attached subnets** - Indicates the number of subnets that are attached to the routing table.

1. From the Routing tables for VPC page, you can create, delete, and view the details of a routing table.

The Actions menu ![Actions menu](images/overflow.png) is used to delete a routing table. Keep in mind that you can do this action only on routing tables without attached subnets.
{: note}

## Listing routing tables for a VPC from the CLI
{: #cr-list-tables-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To list the routing tables for a VPC by using the CLI, run the following command:

```sh
ibmcloud is vpc-routing-tables VPC [--json]
```
{: pre}

Where:

* **VPC** is the ID of the VPC.
* **--json** formats output in JSON.

## Listing routing tables for a VPC with the API
{: #cr-list-tables-using-the-api}
{: api}

To list the routing tables for a VPC by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the `VpcId` value in a variable to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    ```
    {: pre}
