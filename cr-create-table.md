---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-10"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a routing table
{: #create-vpc-routing-table}

Create a routing table to define rules to forward network traffic along the best path toward its destination. For example, a routing table provides information for sending a data packet to the next hop on its route across the network.
{: shortdesc}

Custom routes and custom routing tables are only supported on route-based VPNs. If you are using policy-based VPNs, the routes are created automatically by the VPN service in the default routing table.
{: important}

Ingress traffic from a particular traffic source is routed by using the routes in the custom routing table that is associated with that traffic source. If no matching route is found in
a custom routing table, routing continues by using the VPC system routing table. You can avoid this behavior with a custom routing table's default route with an action of **drop**.
{: note}

Before creating a routing table, ensure that you have at least one VPC. 

You can create a routing table for an IBM Cloud service by using the UI, CLI, or API. 

## Creating a routing table using the UI
{: #cr-using-the-ui}
{: ui}

To create a routing table by using the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Routing tables** in the Network section. The Routing tables for VPC page appear.

   ![Routing tables for VPC page](./images/cr-routing-tables-page.png){: caption="Figure 1. Routing tables for VPC page" caption-side="bottom}

1. Click **Create** in the upper right of the page.
1. In the New routing table for VPC provisioning page, complete the following information:

   * Enter a unique name for your routing table.
   * Select the Virtual Private Cloud that you want to associate with the routing table.
   * Select a traffic type. You can choose **Egress** (default) or **Ingress**.

      If you select **Ingress**, you must select one or more traffic sources. Ingress routing tables support networks for Transit Gateway and Direct Link 2.0 that are associated with the VPC that the routing table is being created in.
      {: note}

      ![Routing table creation page](./images/cr-create-routing-table.png){: caption="Figure 2. Routing table creation page" caption-side="bottom}

1. Read and agree to the **Terms and Conditions**, then click **Create routing table**.  

## Creating a routing table using the CLI
{: #cr-using-the-cli}
{: cli}

To create a routing table by using the CLI, run the following command:

```sh
ibmcloud is vpc-routing-table-create VPC [--name NAME] [--direct-link-ingress false | true] [--transit-gateway-ingress false | true] [--vpc-zone-ingress false | true] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPC** is the ID of the VPC.
- **--name**: is the name of the VPC routing table.
- **--direct-link-ingress** - If set to **true**, this routing table is used to route traffic that originates from {{site.data.keyword.cloud_notm}} Direct Link 2.0 to this VPC. One of: **false**, **true**.
- **--transit-gateway-ingress** - If set to **true**, this routing table is used to route traffic that originates from {{site.data.keyword.cloud_notm}} Transit Gateway to this VPC. One of: **false**, **true**.
- **--vpc-zone-ingress** - If set to **true**, this routing table is used to route traffic that originates from subnets in other zones in this VPC. One of: **false**, **true**.
- **--output** is the output format. One of: **JSON**.
- **-q, --quiet** suppresses verbose output.

You can set an ingress option to **true** on only one routing table per VPC, and then only if that routing table is not attached to any subnets.
{: note}

## Creating a routing table using the API
{: #cr-using-the-api}
{: api}

To create a routing table by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the `VpcId` and `ResourceGroupId` values in a variable to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    export ResourceGroupId=<your_resource_group_id>
    ```
    {: codeblock}

1.  Create a routing table.

    Egress routing table:

    ```sh
    curl -X POST -sH "Authorization:${iam_token}" \
    "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables?version=$api_version&generation=2" \
    -d '{"name": "test-routing-table","resource_group": {"id": "'$ResourceGroupId'"}}'
    ```
    {: codeblock}

    Ingress routing table:   

    ```sh
       curl -X POST "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables?version=$api_version&generation=2" \
       -H "Authorization: $iam_token" \
       -d '{
             "name": "my-ingress-routing-table",
             "route_direct_link_ingress": true
           }'
    ```
    {: codeblock}

    ```sh
       curl -X POST "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables/$RoutingTableId/routes?version=$api_version&generation=2" \
       -H "Authorization: $iam_token" \
       -d '{
             "name": "my-ingress-routing-table",
             "zone": {
                       "name": "us-south-2"
                     },
             "action": "deliver",
             "destination": "<destination ingress CIDR>",
             "next_hop": {
                           "address": "<instance next hop IP address>"
                         }
          }'
    ```
    {: codeblock}
