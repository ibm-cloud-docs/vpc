---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-30"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a route
{: #create-vpc-route}

You can control the flow of network traffic in your VPC by configuring routes. Use VPC routes to specify the next hop for packets, based on their destination addresses.
{: shortdesc}

Multiple routing tables can exist for each zone in your VPC. For egress traffic, when a packet leaves a subnet, the system evaluates its destination against the routing table in the subnet's zone to determine where to send the packet next. Each VPC has a default routing table that is attached to every subnet, unless the subnet was explicitly attached to a different one by the user.

A VPC custom route has four main components:

* The destination CIDR
* The next hop where the packet will route (when the action is `Deliver`)

   You can edit the next hop IP address for existing routes.
   {: note}

* The zone
* Action

Any traffic that originates in the specified zone of the VPC and has a destination address within the specified destination CIDR routes to the next hop. If the destination address is within the destination CIDR for multiple VPC routes, the most specific route is used. If the VPC has two or more equally specific routes, the traffic is round-robin that is distributed between each route.

Each route has a destination property, which includes a prefix length (`/24` in `10.2.0.0/24`). The number of unique prefix lengths that are supported per custom routing table is 14. Multiple routes with the same prefix count as only one unique prefix.

VPC address prefixes are no longer restricted to RFC-1918 addresses. You must now configure VPCs that use both non-RFC-1918 addresses and have public connectivity (floating IP addresses or public gateways) by using a custom route that contains the new `Delegate-VPC` action. You must specify this action for destination CIDRs that are non-RFC-1918 compliant and also outside of the VPC, such as for destinations that are reachable through Direct Link, Transit Gateway, or VPC classic access. For more information about when to use the `Delegate-VPC` action, see [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana).
{: note}

The `Delegate-VPC` action is required if both are true:
* The VPC uses non-RFC-1918 addresses
* The VPC has public connectivity

The `Delegate-VPC` action is NOT required if:
* The VPC uses only RFC-1918 addresses
* The VPC has no public connectivity

You can create a route for an IBM Cloud service by using the console, CLI, API, or Terraform.

## Creating a route in the console
{: #cr-route-using-the-ui}
{: ui}

To create a route in the console, follow these steps:

1. Make sure to review [Limitations and guidelines](/docs/vpc?topic=vpc-about-custom-routes&interface=ui#limitations-custom-routes).
1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Routing tables**. The Routing tables for VPC page appear.
1. Select the VPC associated with the routing table that you want to view. Then, click the name of the routing table to show its details.
1. Scroll to the Routes section and click **Create**.
1. In the Create route side panel, enter the following information:

   * **Zone** - Select an availability zone for your route.
   * **Name** - Type a name for the new route.
   * **Destination CIDR** - The destination CIDR of the route (for example, `10.0.0.0/16`).
   * **Priority** - Enter a priority from `0` to `4`. The default value is `2`.
      For more information, see [Determining route preference](/docs/vpc?topic=vpc-about-custom-routes#cr-determining-route-pref).
   * **Action** - The action to perform with a packet that matches the route. Values are:

      * **Delegate** - Routes the packet by using the system routing table.
      * **Delegate-VPC** - Delegates to the system's built-in routes, ignoring internet-bound routes. Required if the VPC uses non-RFC-1918 addresses and also has public connectivity. See [Routing considerations for IANA-registered IP assignments](/docs/vpc?topic=vpc-interconnectivity#routing-considerations-iana) for details.
      * **Deliver** - Routes the packet to the next hop target. You can add multiple routes with the same address prefix. The virtual router performs equal-cost, multi-path routing (ECMP) by using the different next hop IP addresses.
      * **Drop** - Drops the packet.

   Traffic egressing a subnet uses the custom routing table that is associated with the subnet. If no matching route is found in a custom routing table, routing continues by using the VPC system routing table. You can avoid this behavior with a custom routing table default route with an action of **Drop**.
   {: note}

   * **Type** - Select either **IP address** or **VPN connection**.
   * **Next hop (IP address)** - Enter the next hop.

      You can modify the next hop IP address for existing routes.
      {: note}

   * **Advertise** - Advertise to an ingress source. Currently, Direct Link and Transit Gateway are supported. Optional.

1. Click **Save** to save the new route. The route appears on the Routing table details page.

## Creating a route from the CLI
{: #cr-route-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a VPC route from the CLI, run the following command:

```sh
ibmcloud is vpc-routing-table-route-create VPC ROUTING_TABLE --zone ZONE_NAME --destination DESTINATION_CIDR [--action delegate_vpc | delegate | deliver | drop] [--priority PRIORITY] [--next-hop NEXT_HOP [--vpngw VPNGW]][--advertise true | false] [--name NAME] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

`VPC`
:   Is the ID or name of the VPC.

`ROUTING_TABLE`
:   Is the ID or name of the VPC routing table.

`--zone`
:   Is the name of the zone.

`--destination`
:   Is the destination CIDR of the route. At most, two routes per zone in a table can have the same destination, and only if both routes have an action of **deliver**.

`--action`
:   Is the action to perform with a packet that matches the route. Actions are **delegate_vpc**, **delegate**, **deliver**, and **drop**.

`--priority`
:   Is the route's priority. Values are `0`, `1`, `2`, `3`, and `4`. The default value is `2`.  Lesser values have higher priority. If a custom routing table contains routes with the same destination, the route with the highest priority (smallest value) is selected.

`--next-hop`
:   If the action is **deliver**, enter the IP address or VPN connection ID of the next hop to which to route packets.

`--vpngw`
:   Is the name of the VPN gateway.

`--advertise`
:   Advertise to a direct link, transit gateway, or both ingress sources. One of **true**, **false**.

`--name`
:   Is the name of the VPC routing table.

`--output`
:   Formats the output in JSON.

`--q, quiet`
:   Causes the command to run silently and does not generate any output.



### CLI examples
{: #examples-priority}
{: cli}

`ibmcloud is vpc-routing-table-route-update 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 72b27b5c-f4b0-48bb-b954-5becc7c1d456 72b27b5c-f4b0-48bb-b954-5becc7c1d4ef --name my-vpc-route --priority 1`

`ibmcloud is vpc-routing-table-route-update 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 72b27b5c-f4b0-48bb-b954-5becc7c1d456 72b27b5c-f4b0-48bb-b954-5becc7c1d4ef --name my-vpc-route --next-hop 10.0.0.2`


## Creating a route with the API
{: #cr-route-using-the-api}
{: api}

To create a destination route with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store values for the following variables to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    export RoutingTableId=<your_routing_table_id>
    ```
    {: codeblock}

1. Create a route:

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/vpcs/$VpcId/routing_tables/$RoutingTableId/routes?version=$api_version&generation=2" \
   -H "Authorization: $iam_token" \
   -d '{
        "name": "my-new-route",
        "zone": {"name": "us-south-2"},
        "action": "deliver",
        "destination": "10.10.10.0/24",
        "next_hop": {"address": "10.0.0.3"}
      }'
   ```
   {: codeblock}

For more information and optional parameters, see [Create a route in a VPC routing table](/apidocs/vpc/latest#create-vpc-routing-table-route) in the VPC API Reference.

## Creating a route with Terraform
{: #cr-create-route-terraform}
{: terraform}

To create a route with Terraform, follow these steps:

1. Set up your Terraform environment.
1. Create a route:

   ```terraform
   resource "ibm_is_vpc_routing_table_route" "example" {
     vpc           = ibm_is_vpc.example.id
     routing_table = ibm_is_vpc_routing_table.example.routing_table
     zone          = "us-south-1"
     name          = "custom-route-2"
     destination   = "192.168.4.0/24"
     action        = "deliver"
     next_hop      = ibm_is_vpn_gateway_connection.example.gateway_connection // Example value "10.0.0.4"
   }
   ```

      For more information about the `ibm_is_vpc_routing_table_route` resource, see the [Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpc_routing_table_route).{: external}
