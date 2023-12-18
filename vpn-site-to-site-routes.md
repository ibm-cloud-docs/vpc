---

copyright:
  years: 2022, 2023
lastupdated: "2023-12-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing VPN routes
{: #vpn-site-to-site-routes}

Each site-to-site VPN gateway uses a routing table to list destination network routes, where network traffic is directed.
{: shortdesc}

Private IPs are not static and can change at any time. Do not create routes and try to use the private IP addresses.
{: important}

## Planning consideration
{: #vpn-overlapping-cidrs}

Local and peer subnets of a VPN gateway connection should not overlap or conflict. For example, if you specify `192.168.0.0/24` as the local CIDR and `192.168.1.0/16` as the peer CIDR, the local CIDR is a subset of the peer CIDR because bitmask `24` is bigger than `16`. In this case, where should a packet with destination IP `192.168.0.100` be forwarded? Since the peer CIDR uses the entire subnet `192.168.1.0/16`, the `192.168.0.100` might be on the peer side, or it might be on the VPC side too.

However, exceptions can be made if you need to use an overlapping configuration. For example, let's say that you want to use `192.168.1.0/16` as a peer subnet, but not include `192.168.0.0/24` on the peer side:

* `192.168.0.0/24` is on the VPC.
* `192.168.1.0/24` and `192.168.2.0/24` through `192.168.255.0/24` are on the peer side, but `192.168.0.0/24` is not included.

In this special case, you must configure the route in your gateway and make sure `192.168.0.0/24` goes into the tunnel. Then, you must configure routes on the VPC side to make sure the traffic goes to the tunnel.

In another scenario, you might want to send all traffic from the VPC side to the on-premises side. To accomplish this, you can set peer CIDRs to `0.0.0.0/0` when creating a connection. When the connection is created successfully, the VPN service adds a `0.0.0.0/0` via `<VPN gateway private IP>` route into the default routing table of the VPC. However, this configuration can cause routing issues. For more information, see [Why aren't my VPN gateways or virtual server instances communicating?](/docs/vpc?topic=vpc-troubleshoot-routing-issues).

## Creating a route in the UI
{: #create-route-ui-s2s}
{: ui}

To create a route to specify how destination network traffic is directed, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Menu** icon ![menu icon](../icons/icon_hamburger.svg), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>VPNs** in the Network section.
1. Select **Site-to-site gateways** > **VPN gateways**, then select the VPN gateway where you want to add a route.
1. On the VPN gateway's detailspage, select the **Routes** tab.
1. Select **Create +**.
1. Give the route a name.
1. Specify a destination CIDR range for the destination network. For example:
   * For internet access, enter `0.0.0.0/0`.
   * For an on-premises network, enter the IPv4 CIDR range of the site-to-site gateway connection.
   * For the VPC subnet, enter the VPC subnet CIDR.
1. Choose an action:
   * **Deliver** - Use when the route destination is in the VPC, or an on-premises private subnet connected using VPN gateway.
   * **Drop** - Use when you want to block traffic from the client, to forward unwanted or undesirable network traffic to a null or "black hole" route.
   * **Translate** - Use to translate the source IP to the VPN gateway's private IP before it is sent out from the VPN gateway.
1. Click **Save**.

You can select **Edit** from the Actions menu of the VPN gateway route to change the name of your route.
{: note}

## Deleting a route in the UI
{: #delete-route-ui-s2s}
{: ui}

To delete a route by using the UI, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Menu** icon ![menu icon](../icons/icon_hamburger.svg), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>VPNs** in the Network section.
1. Select the **Site-to-site gateways** tab.
1. Select the site-to-site gateway where you want to delete a route, then select **Delete** from the Actions menu.
1. Select **Delete** again to confirm deletion.

## Creating a route from the CLI
{: #create-route-cli-s2s}
{: cli}

To create a VPN gateway route from the CLI, enter the following command:

```sh
ibmcloud is vpn-gateway-route-create VPN_GATEWAY_ID --destination DESTINATION_CIDR [--action translate | deliver | drop] [--name NAME] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPN_GATEWAY_ID**: is the ID of the VPN gateway.
- **--action**: is the action to perform with a packet that matches the route. One of: **translate**, **deliver**, **drop**. (default: **deliver**).
- **--destination**: is the destination to use for this VPN route in the VPN gateway. Must be unique within the VPN gateway. If an incoming packet does not match any destination, it is dropped.
- **--name**: is the name of the VPN route.
- **--output**: specifies output format, only JSON is supported. One of: **JSON**.
- **-q, --quiet**: suppresses verbose output.

For example:

- `ibmcloud is vpn-gateway-route-create r006-77e21079-7291-44c2-866a-8f1848bc10f0 --name myroute --action deliver --destination 10.0.0.0/24`
{: screen}

- `ibmcloud is vpn-gateway-route-create r006-77e21079-7291-44c2-866a-8f1848bc10f0 --name myroute --action drop --destination 10.0.0.0/24`
{: screen}

## Updating a route from the CLI
{: #update-route-cli-s2s}
{: cli}

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

To update a VPN gateway route from the CLI, enter the following command:

```sh
ibmcloud is vpn-gateway-route-update VPN_GATEWAY_ID ROUTE_ID [--name NAME] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPN_GATEWAY_ID**: is the ID of the VPN gateway.
- **ROUTE_ID**: is the ID of the VPN route.
- **--name**: is the new name of the VPN route.
- **--output**: specifies output format, only JSON is supported. One of: **JSON**.
- **-q, --quiet**: suppresses verbose output.

For example:

`ibmcloud is vpn-gateway-route-update r006-77e21079-7291-44c2-866a-8f1848bc10f0 1233a60b-fc95-4dbc-96ab-a976b723bfb0 --name myroute`
{: screen}

## Viewing VPN route details from the CLI
{: #vpn-gateway-route-s2s}
{: cli}

To view details of a VPN route, enter the following command:

```sh
ibmcloud is vpn-gateway-route VPN_GATEWAY_ID ROUTE_ID [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPN_GATEWAY_ID**: is the ID of the VPN gateway.
- **ROUTE_ID**: is the ID of the VPN route.
- **--output**: specifies output format, only JSON is supported. One of: **JSON**.
- **-q, --quiet**: suppresses verbose output.

## Viewing all routes from the CLI
{: #view-routes-cli-s2s}
{: cli}

To view a list of VPN gateway routes for a VPN gateway from the CLI, enter the following command:

```sh
ibmcloud is vpn-gateway-routes VPN_GATEWAY_ID [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPN_GATEWAY_ID**: is the ID of the VPN gateway.
- **--resource-group-id**: is the ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
- **--resource-group-name**: is the name of the resource group. This option is mutually exclusive with **--resource-group-id**.
- **--all-resource-groups**: queries all resource groups.
- **--output**: specifies output format, only JSON is supported. One of: **JSON**.
- **-q, --quiet**: suppresses verbose output.

## Deleting a route from the CLI
{: #delete-route-cli-s2s}
{: cli}

To delete a VPN gateway route from the CLI, enter the following command:

```sh
ibmcloud is vpn-gateway-route-delete VPN_GATEWAY_ID (ROUTE_ID1 ROUTE_ID2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

Where:

- **VPN_GATEWAY_ID**: is the ID of the VPN gateway.
- **ROUTE_ID1**: is the ID of the VPN route.
- **ROUTE_ID2**: is the ID of the VPN route.
- **--output**: specifies output format, only JSON is supported. One of: **JSON**.
- **--force, -f**: forces the operation without confirmation.
- **-q, --quiet**: suppresses verbose output.

## Creating a route with the API
{: #create-route-api-s2s}
{: api}

To create a VPN route on the VPN gateway with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Perform a POST to `/vpn_gateways/{vpn_gateway_id}/routes`.

   ```curl
      curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/routes?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
           "name":"my-route-1",
            "destination": "10.10.10.0/24",
            "action": "translate"
         }'
   ```
   {: codeblock}

## Updating a route with the API
{: #update-route-api-s2s}
{: api}

To update a route on the VPN gateway with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Perform a PATCH on `/vpn_gateways/{vpn_gateway_id}/routes/{id}`

   ```curl
      curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/routes/$route_id?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
           "name":"new-route-name"
         }'
   ```
   {: codeblock}

## Viewing a route with the API
{: #view-routes-api-s2s}
{: api}

To view a route on a VPN gateway with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. When all variables are initiated, list your routes:

   ```curl
   curl -sS -X GET \
   -H "Authorization: $iam_token" \
   "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/routes?version=$api_version&generation=2" | jq
   ```
   {: codeblock}

1. Perform a GET on `/vpn_gateways/{vpn_gateway_id}/routes/{id}`. For details, see [`list_vpn-gateway_routes`](/apidocs/vpc/latest#list_vpn_gateway_routes).

   ```curl
   curl -sS -X GET \
   -H "Authorization: $iam_token" \
   "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/routes/$route_id?version=$api_version&generation=2" | jq
   ```
   {: codeblock}

## Deleting a route with the API
{: #delete-route-api-s2s}
{: api}

To delete a route on a VPN gateway with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Perform a DELETE on `/vpn_gateways/{vpn_gateway_id}/routes/{id}`.

   ```curl
   curl -sS -X DELETE \
   -H "Authorization: $iam_token" \
   "$vpc_api_endpoint/v1/vpn_gateways/$vpn_gateway_id/routes/$route_id?version=$api_version&generation=2"
   ```
   {: codeblock}
