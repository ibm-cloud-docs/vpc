---

copyright:
  years: 2020, 2023
lastupdated: "2023-03-15"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating a routing table
{: #update-vpc-routing-table}

You can update a routing table using the UI, CLI, and API.
{: shortdesc}

## Updating a routing table in the UI
{: #cr-updating-the-ui}
{: ui}

To update a routing table in the UI, follow these steps:

1. Make sure to review [Limitations and guidelines](/docs/vpc?topic=vpc-about-custom-routes&interface=ui#limitations-custom-routes).
1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the Navigation Menu ![Navigation Menu](/images/menu_icon.png), then click **VPC Infrastructure > Routing tables** in the Network section. The Routing tables for VPC page appears.
1. Locate the routing table in the table. You can:

   * Use the Actions menu ![Actions menu](images/overflow.png) to rename or delete the routing table.
   * Click the routing table's name to view its details page. From here, you can click the Edit icon ![Edit icon](/images/edit.png) to rename the routing table, click the Actions menu to delete the routing table, or click the Edit icon ![Edit icon](/images/edit.png) to edit the traffic configuration and manage routes.

## Updating a routing table from the CLI
{: #cr-update-the-cli-ct}
{: cli}

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

To update a routing table by using the CLI, run the following command:

```sh
ibmcloud is vpc-routing-table-update VPC ROUTING_TABLE [--name NEW_NAME] [--direct-link-ingress false | true] [--internet-ingress, --internet false | true] [--transit-gateway-ingress false | true] [--vpc-zone-ingress false | true] [--accept-routes-from-resource-type-filters, --ar-rtf vpn_server | vpn_gateway | --clean-all-accept-routes-from-filters, --cl-arf] [--advertise_routes_to TARGETS] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`VPC`
:   Is the ID or name of the VPC.

`ROUTING_TABLE`
:   Is the ID or name of the VPC routing table.

`--name`
:   Is the new name of the routing table.

`--direct-link-ingress, ``direct-link`
:   Optional. If set to **true**, the routing table is used to route traffic that originates from {{site.data.keyword.cloud_notm}} Direct Link to this VPC. For the routing to succeed, the VPC must not already have a routing table with this property set to **true**. One of: **false**, **true**.

`--internet-ingress, --internet`
:   Indicates whether this routing table is used to route traffic that originates from the internet. Updating to **true** selects this routing table, provided no other routing table in the VPC already has this property set to **true**. Updating to **false** deselects this routing table. One of: **false**, **true**.

`--transit-gateway-ingress, --transit-gateway`
:   If set to **true**, this routing table is used to route traffic that originates from Transit Gateway to this VPC. For the routing to succeed, the VPC must not already have a routing table with this property set to **true**. One of: **false**, **true**.

`--vpc-zone-ingress, --vpc-zone`
:   If set to **true**, this routing table is used to route traffic that originates from subnets in other zones in this VPC. For the routing to succeed, the VPC must not already have a routing table with this property set to **true**. One of: **false**, **true**.

`--accept-routes-from-resource-type-filters, --ar-rtf`
:   The comma-separated resource type filters that can create routes in this routing table. All learned routes from resources that match a resource filter are removed when an existing resource filter is removed. One of: **vpn_server**, **vpn_gateway**.

`--clean-all-accept-routes-from-filters, --cl-arf`
:   Remove all accept routes from filters and delete all learned routes from the routing table.

`--advertise_routes_to TARGETS`
:   Is a comma-separated list of advertisement targets for routes in this routing table. Currently, `direct_link` and `transit_gateway` are the allowed values. `direct_link` requires `direct-link-ingress` to be set to **true**. `transit_gateway` requires `transit—gateway-ingress` to be set to **true**. All routes in the routing table with this option set to **true** are advertised. Optional.

`--output` 
:   Is the output format. One of: **JSON**.

`-q, --quiet` 
:   Suppresses verbose output.

Routes with an action of **deliver** are treated as **drop** unless the next-hop is an IP address that is bound to a network interface on a subnet in the route’s zone. Hence, if an incoming packet matches a route with a next-hop of an internet-bound IP address or a VPN gateway connection, the packet is dropped.
{: important}

You can set an ingress option to **true** on only one routing table per VPC, and then only if that routing table is not attached to any subnets.
{: note}

### CLI examples
{: #routing-table-update-examples-cli}

```sh
ibmcloud is vpc-routing-table-update 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 72b27b5c-f4b0-48bb-b954-5becc7c1d456 --name my-renamed-vpc-routing-table --output JSON
```

```sh
ibmcloud is vpc-routing-table-update my-vpc my-vpc-routing-table --name my-renamed-vpc-routing-table --output JSON
```

```sh
ibmcloud is vpc-routing-table-update vpc-doloremque-6364-us-east  test-vpc-cli-routing-tb2 --direct-link-ingress true --internet-ingress false --transit-gateway-ingress true  --vpc-zone-ingress false
```

```sh
ibmcloud is vpc-routing-table-update 979b4bc6-f018-40a2-92f5-0b1cf777b55d  27415d55-9d3b-4adb-a993-236ef59a45ec --direct-link-ingress false --internet-ingress false   --transit-gateway-ingress false  --vpc-zone-ingress false
```

## Updating a routing table with the API
{: #cr-update-the-api-ct}
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

    ```curl
    curl -X POST -sH "Authorization:${iam_token}" \
    "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables?version=$api_version&generation=2" \
    -d '{"name": "test-routing-table","resource_group": {"id": "'$ResourceGroupId'"}}'
    ```
    {: codeblock}

    Ingress routing table:

    ```curl
       curl -X POST "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables?version=$api_version&generation=2" \
       -H "Authorization: $iam_token" \
       -d '{
             "name": "my-ingress-routing-table",
             "route_direct_link_ingress": true
           }'
    ```
    {: codeblock}

    ```curl
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
