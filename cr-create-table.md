---

copyright:
  years: 2020
lastupdated: "2020-10-30"

keywords: custom routes

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:DomainName: data-hd-keyref="DomainName"}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:term: .term}
{:generic: data-hd-programlang="generic"}
{:download: .download}

# Creating a routing table
{: #create-vpc-routing-table}

You can create a routing table for an IBM Cloud service by using the UI, CLI, or API.
{: shortdesc}

Prior to creating a routing table, ensure that you have at least one VPC.
{: important}

## Using the UI
{: #cr-using-the-ui}

To create a routing table by using the {{site.data.keyword.cloud_notm}} console, follow these steps: 

1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Routing tables** in the Network section. The Routing tables for VPC page appears.

     ![Routing tables for VPC page](./images/cr-routing-tables-page.png)

2. Click **New routing table** in the upper right of the page.
3. In the New routing table for VPC provisioning page, complete the following information:

   * Enter a unique name for your routing table.
   * Select a virtual private cloud.

  ![Routing table creation page](./images/cr-routing-table-create.png)

   A Summary panel shows the total estimated monthly cost for your review.

   There is no charge for custom routing tables and associated routes.
   {: note}

4. Read and agree to the **Terms and Conditions**, then click **Create** to create the routing table.  

## Using the CLI
{: #cr-using-the-cli}

To create a routing table by using the CLI, run the following command:

```
ibmcloud is vpc-routing-table-create VPC \
[--name NAME] \
[--json]
```

Where:

* **VPC** is the ID of the VPC.
* **--name** is the name of the VPC routing table.
* **--json** formats the output in JSON.

##Using the API
{: #cr-using-the-api}

To create a routing table by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the `VpcId` and `ResourceGroupId` values in a variable to be used in the API command:

    ```
    export VpcId=<your_vpc_id>
    export ResourceGroupId=<your_resource_group_id>
    ```
    {: codeblock}

1.  Create a routing table:

   ```
   curl -X POST -sH "Authorization:${iam_token}" \
   "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables?version=$api_version&generation=2" \
   -d '{"name": "test-routing-table","resource_group": {"id": "'$ResourceGroupId'"}}' | jq
   ```
   {: codeblock}

   
