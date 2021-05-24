---

copyright:
  years: 2020,2021
lastupdated: "2021-05-21"

keywords: custom routes

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:note: .note}
{:tip: .tip}
{:important: .important}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Listing routing tables for a VPC
{: #list-routing-tables-for-vpc}

You can list routing tables for a VPC by using the UI, CLI, or API.
{: shortdesc}

## Listing routing tables for a VPC by using the UI
{: #cr-list-tables-using-the-ui}
{: ui}

To list the routing tables for a VPC by using the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Routing tables** in the Network section. The Routing tables for VPC page appears.

   ![Routing tables for VPC page](./images/cr-routing-tables-page.png)

1. From the Virtual private cloud drop-down list, select the VPC that you want to list routing tables for.

   Column descriptions are as follows:

   * **Name** - Indicates the name of the routing table. Click this link to see the details of the routing table. You can also edit and change this name.
   * **Default** - Specifies the default routing table of the specified VPC.
   * **Date created** - Shows the date that the routing table was created.
   * **Routes** - States the number of routes that are attached to the routing table.
   * **Attached subnets** - Indicates the number of subnets that are attached to the routing table.

1. From the Routing tables for VPC page, you can create, delete, and view the details of a routing table.

The overflow menu ![overflow menu](images/overflow.png) is used to delete a routing table. Keep in mind that you can do this action only on routing tables without attached subnets.
{: note}

## Listing routing tables for a VPC by using the CLI
{: #cr-list-tables-using-the-cli}
{: cli}

To list the routing tables for a VPC by using the CLI, run the following command:

```
ibmcloud is vpc-routing-tables VPC [--json]
```
{: pre}

Where:

* **VPC** is the ID of the VPC.
* **--json** formats output in JSON.


## Listing routing tables for a VPC by using the API
{: #cr-list-tables-using-the-api}
{: api}

To list the routing tables for a VPC by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the `VpcId` value in a variable to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    ```
    {: pre}

1. List the routing tables for a VPC:

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables?version=$api_version&generation=2" \
     -H "Authorization: $iam_token"

   ```
   {: codeblock}
