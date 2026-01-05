---

copyright:
  years: 2020, 2026
lastupdated: "2026-01-05"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a routing table
{: #create-vpc-routing-table}

Create a routing table to define rules to forward network traffic along the best path toward its destination. For example, a routing table provides information for sending a data packet to the next hop on its route across the network.
{: shortdesc}

## Before you begin
{: #routing-planning-guidelines}

Before you create a routing table, make sure that you have at least one VPC and review and adhere to routing table [limitations and guidelines](/docs/vpc?topic=vpc-about-custom-routes&interface=ui#limitations-custom-routes).

You can create a routing table for an {{site.data.keyword.cloud_notm}} service by using the console, CLI, API, or Terraform.

## Creating a routing table in the console
{: #cr-using-the-ui}
{: ui}

To create a routing table in the console, follow these steps:

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Routing tables**. The Routing tables for VPC page appears.
1. Click **Create** in the upper right of the page.
1. In the Routing table for VPC provisioning page, complete the following information:

   * Enter a unique name for your routing table.
   * Select the Virtual Private Cloud that you want to associate with the routing table.
   * **Tags** - (Optional) Add tags to help you organize and find your resources. You can add more tags later. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Access management tags** - (Optional) Add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Virtual private cloud** - Select your VPC.
   * In the Traffic section, you can select from these optional features:

      * **Accepts routes from** (optional) -  Choose which resources can create routes in the routing table. You can select the switch for **VPN server**, **VPN gateway**, or both.

         For policy-based VPN gateways, routes are not automatically discovered. The routing table must be configured to accept routes from the VPN gateway, and propagated routes are limited to the CIDR prefixes defined in the VPN policy. Route-based VPN gateways with BGP dynamically advertise learned routes. For more information about defining CIDR prefixes for policy-based VPNs, see [Adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections).

         VPN server routes are derived from the client address pool and any configured client routes and are propagated when the routing table is configured to accept routes from the VPN server.
         {: note}

      * **Traffic source** (optional) - Select the traffic source to use this routing table to route its traffic to the VPC.

         * **Direct Link** - Allows ingress traffic from an [IBM Cloud Direct Link](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) Dedicated or Connect connection to an on-premises location. Optionally, you can advertise routes to a direct link, which are not in the address prefix range of the VPC.
         * **Transit gateway** - Allows ingress traffic from an [IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started) to another VPC or classic infrastructure. Optionally, you can advertise routes to a transit gateway, which are not in the address prefix range of the VPC.
         * **VPC zone** - Allows ingress traffic to another availability zone of the same VPC.
         * **Public internet** - Allows public internet ingress traffic that is destined to a floating IP to be routed to a VPC next-hop IP.

1. Click **Create routing table**.

## Creating a routing table from the CLI
{: #cr-using-the-cli-ct}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a routing table from the CLI, run the following command:

```sh
ibmcloud is vpc-routing-table-create VPC [--name NAME] [--direct-link-ingress false | true] [--internet-ingress, --internet false | true] [--transit-gateway-ingress false | true] [--vpc-zone-ingress false | true] [--accept-routes-from-resource-type-filters, --ar-rtf vpn_server | vpn_gateway] [--advertise-routes-to direct_link | transit_gateway | direct_link, transit_gateway] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`VPC`
:   Is the ID or name of the VPC.

`--name`
:   Is the name of the VPC routing table.

`--direct-link-ingress, --direct-link`
:   Optional. If set to `true`, this routing table is used to route traffic that originates from {{site.data.keyword.cloud_notm}} Direct Link to this VPC. For the routing to succeed, the VPC must not already have a routing table with this property set to `true`. One of: `false`, `true`.

`--internet-ingress, --internet`
:   Indicates whether this routing table is used to route traffic that originates from the internet. Updating to `true` selects this routing table, provided no other routing table in the VPC already has this property set to `true`. Updating to `false` deselects this routing table. One of: `false`, `true`.

`--transit-gateway-ingress, --transit-gateway`
:   If set to `true`, this routing table is used to route traffic that originates from Transit Gateway to this VPC. For the routing to succeed, the VPC must not already have a routing table with this property set to `true`. One of: `false`, `true`.

`--vpc-zone-ingress, --vpc-zone`
:   Optional. If set to `true`, this routing table is used to route traffic that originates from the public internet. For the routing to succeed, the VPC must not have an existing routing table with this property set to `true`. One of: `false`, `true`.

`--accept-routes-from-resource-type-filters, --ar-rtf`
:   Comma-separated resource type filters that can create routes in this routing table. One of: `vpn_server`, `vpn_gatewa`.

`--advertise_routes_to TARGETS`
:   Optional. Is a comma-separated list of advertisement targets for routes in this routing table. Currently, `direct_link` and `transit_gateway` are the allowed values. `direct_link` requires `direct-link-ingress` to be set to `true`. `transit_gateway` requires `transit—gateway-ingress` to be set to `true`. All routes in the routing table with the `advertise` option set to `true` are advertised to the ingress sources specified by 'advertise_routes_to'.

`--output`
:   Is the output format. One of: **JSON**.

`-q, --quiet`
:   Suppresses verbose output.


Routes with an action of *deliver* are treated as *drop* unless the next-hop is an IP address that is bound to a network interface on a subnet in the route’s zone. Hence, if an incoming packet matches a route with a next-hop of an internet-bound IP address or a VPN gateway connection, the packet is dropped.
{: important}

You can set an ingress option to `true` on only one routing table per VPC, and then only if that routing table is not attached to any subnets.
{: note}

### CLI examples
{: #routing-table-create-examples-cli}

`ibmcloud is vpc-routing-table-create 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 --name my-vpc-routing-table -—advertise-routes-to direct_link --direct-link-ingress true -—output JSON`

`ibmcloud is vpc-routing-table-create my-vpc --name my-vpc-routing-table --advertise-routes-to transit_gateway —-transit-gateway-ingress true --output JSON`

`ibmcloud is vpc-routing-table-create my-vpc --name my-vpc-routing-table --advertise-routes-to direct_link, transit_gateway  --direct-link-ingress true —transit-gateway-ingress true -—output JSON`

`ibmcloud is vpc-routing-table-create 979b4bc6-f018-40a2-92f5-0b1cf777b55d --name test-vpc-cli-routing-tb1 --direct-link-ingress false --internet-ingress false   --transit-gateway-ingress false  --vpc-zone-ingress true`


## Creating a routing table with the API
{: #cr-using-the-api-ct}
{: api}

To create a routing table with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the `VpcId` value in a variable to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    ```
    {: codeblock}

1.  Create a routing table.

    Egress routing table:

    ```sh
    curl -X POST \
    "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables?version=$api_version&generation=2" \
    -H "Authorization: ${iam_token}" \
    -d '{
          "name": "test-routing-table"
        }'
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

For more information and optional parameters, see [Create a routing table for a VPC](/apidocs/vpc/latest#create-vpc-routing-table) in the VPC API Reference.

## Creating a routing table with Terraform
{: #cr-create-terraform}
{: terraform}

To create a routing table with Terraform, follow these steps:

1. Set up your Terraform environment.
1. Use one of the following examples:

   * To create a routing table:

      ```terraform
      resource "ibm_is_vpc_routing_table" "example" {
        vpc                           = ibm_is_vpc.example.id
        name                          = "example-vpc-routing-table"
        route_direct_link_ingress     = true
      }
      ```

   * To create a routing table that accepts routes that are created from a VPN server:

      ```terraform
      resource "ibm_is_vpc_routing_table" "example" {
        vpc                              = ibm_is_vpc.example.id
        name                             = "example-vpc-routing-table"
        route_direct_link_ingress        = true
        accept_routes_from_resource_type = ["vpn_server"]
      }
      ```

   * To create a routing table that routes traffic that originates from IBM Cloud Direct Link to this VPC:

      ```terraform
      resource "ibm_is_vpc_routing_table" "is_vpc_routing_table_instance" {
        vpc                           = ibm_is_vpc.example.id
        name                          = "example-vpc-routing-table"
        route_direct_link_ingress     = true
        route_transit_gateway_ingress = false
        route_vpc_zone_ingress        = false
        advertise_routes_to           = ["direct_link", "transit_gateway"]
      }
      ```

   * To create a routing table that includes user tags and access tags:

      ```terraform
      resource "ibm_is_vpc_routing_table" "example" {
        tags                          = ["rt-tag"]
        access_tags                   = ["access:dev"]
        vpc                           = ibm_is_vpc.example.id
        name                          = "example-vpc-routing-table"
        route_direct_link_ingress     = true
        route_transit_gateway_ingress = false
        route_vpc_zone_ingress        = false
      }
      ```

      The CRN and resource group is included in the responses and data sources.

      ```terraform
      resource "ibm_is_subnet" "example" {
        name               = "example-subnet"
        vpc                = ibm_is_vpc.example.id
        zone               = "us-south-1"
        ipv4_cidr_block    = "10.240.0.0/24"
        routing_table_crn  = ibm_is_vpc_routing_table.example.crn
      }
      ```

For documentation about the `ibm_is_vpc_routing_table` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpc_routing_table).{: external}
