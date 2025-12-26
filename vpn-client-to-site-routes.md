---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-26"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing VPN routes
{: #vpn-client-to-site-routes}

Each client-to-site VPN uses a routing table to list destination network routes, where network traffic is directed. To specify which clients have access to the destination network, configure authorization rules for each client-to-site VPN route.

Private IPs are not static and can change at any time. Do not create routes and try to use the private IP addresses.
{: important}

## Using split-tunnel on client-to-site VPNs
{: #vpn-client-to-site-split-tunnel}

When the VPN is established in split-tunnel configuration, all routes in the client-to-site VPN server routing tables are added to the client routing table. You must reset the connection if you add routes later, to ensure that the new route is delivered to the client.

To avoid running out of routes, make note of the client device's route capacity before the client-to-site VPN server routing table is modified.
{: tip}

## Creating a route in the console
{: #create-route-ui-c2s}
{: ui}

Specify how destination network traffic is directed by creating a route. To create a route, the VPN client server must have a health status of "Healthy" and a server status of "Stable".

Add `0.0.0.0/0` as a route to enable clients access to the internet.
{: tip}

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPNs**.
1. Select the **Client-to-site servers** tab.
1. Select the VPN server where you want to add a route. Then, select the **VPN server routes** tab.
1. Select **Create +**.
1. Give the route a name.
1. Specify a destination CIDR range for the destination network. For example:
   * For internet access, enter `0.0.0.0/0`.
   * For an on-premises network, enter the IPv4 CIDR range of the site-to-site gateway connection.
   * For the VPC subnet, enter the VPC subnet CIDR.
1. Choose an action:
   * **Deliver** - Use when the route destination is in the VPC, or an on-premises private subnet connected by using VPN gateway. When VPN routes use the Deliver action, the client IP is preserved.
   * **Drop** - Use when you want to block traffic from the client to forward unwanted or undesirable network traffic to a null or "black hole" route.
   * **Translate** - Use to translate the source IP to the VPN server private IP before it is sent out from the VPN server, making your VPN client IP invisible to the destination devices.
1. Click **Create**.

You can select **Edit** from the Actions menu of the VPN server route to change the name of your route.
{: note}

## Deleting a route in the console
{: #delete-route-ui-c2s}
{: ui}

To delete a route by using the UI, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPNs**.
1. Select the **Client-to-site servers** tab.
1. Select the VPN server where you want to delete a route. Then, from the Actions menu, select **Delete**.
1. Select **Delete** again to confirm deletion.

## Creating a route from the CLI
{: #create-route-cli-c2s}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a VPN server route from the CLI, enter the following command:

```sh
ibmcloud is vpn-server-route-create VPN_SERVER_ID --destination DESTINATION_CIDR [--action translate | deliver | drop] [--name NAME] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPN_SERVER_ID**: The ID of the VPN server.
- **--action**: The action to perform with a packet that matches the route. One of: **translate**, **deliver**, **drop**. (default: **deliver**).
- **--destination**: is the destination to use for this VPN route in the VPN server. Must be unique within the VPN server. If an incoming packet does not match any destination, it is dropped.
- **--name**: The name of the VPN route.
- **--output**:  Specifies the output format, only JSON is supported. One of: **JSON**.
- **-q, --quiet**: Suppresses verbose output.



For example:

- `ibmcloud is vpn-server-route-create r006-77e21079-7291-44c2-866a-8f1848bc10f0 --name myroute --action deliver --destination 10.0.0.0/24`
{: screen}

- `ibmcloud is vpn-server-route-create r006-77e21079-7291-44c2-866a-8f1848bc10f0 --name myroute --action drop --destination 10.0.0.0/24`
{: screen}

## Updating a route from the CLI
{: #update-route-cli-c2s}
{: cli}

To update a VPN server route from the CLI, enter the following command:

```sh
ibmcloud is vpn-server-route-update VPN_SERVER_ID ROUTE_ID [--name NAME] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPN_SERVER_ID**: The ID of the VPN server.
- **ROUTE_ID**: The ID of the VPN route.
- **--name**: The new name of the VPN route.
- **--output**: Specifies the output format, only JSON is supported. One of: **JSON**.
- **-q, --quiet**: Suppresses verbose output.

For example:

`ibmcloud is vpn-server-route-update r006-77e21079-7291-44c2-866a-8f1848bc10f0 1233a60b-fc95-4dbc-96ab-a976b723bfb0 --name myroute`
{: screen}

## Viewing VPN route details from the CLI
{: #vpn-server-route}
{: cli}

To view details of a VPN route, enter the following command:

```sh
ibmcloud is vpn-server-route VPN_SERVER_ID ROUTE_ID [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPN_SERVER_ID**: The ID of the VPN server.
- **ROUTE_ID**: The ID of the VPN route.
- **--output**: Specifies the output format, only JSON is supported. One of: **JSON**.
- **-q, --quiet**: Suppresses verbose output.



## Viewing all routes by using the CLI
{: #view-routes-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To view a list of VPN server routes for a VPN server from the CLI, enter the following command:

```sh
ibmcloud is vpn-server-routes VPN_SERVER_ID [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPN_SERVER_ID**: The ID of the VPN server.
- **--resource-group-id**: The ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
- **--resource-group-name**: The name of the resource group. This option is mutually exclusive with **--resource-group-id**.
- **--all-resource-groups**: Queries all resource groups.
- **--output**: Specifies the output format, only JSON is supported. One of: **JSON**.
- **-q, --quiet**: Suppresses verbose output.



## Deleting a route by using the CLI
{: #delete-route-cli-c2s}
{: cli}

To delete a VPN server route from the CLI, enter the following command:

```sh
ibmcloud is vpn-server-route-delete VPN_SERVER_ID (ROUTE_ID1 ROUTE_ID2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

Where:

- **VPN_SERVER_ID**: The ID of the VPN server.
- **ROUTE_ID1**: The ID of the VPN route.
- **ROUTE_ID2**: The ID of the VPN route.
- **--output**: Specifies the output format, only JSON is supported. One of: **JSON**.
- **--force, -f**: Forces the operation without confirmation.
- **-q, --quiet**: Suppresses verbose output.



## Creating a route by using the API
{: #create-route-api-c2s}
{: api}

To create a VPN route on the VPN server with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Perform a POST to `/vpn_servers/{vpn_server_id}/routes`.

   ```sh
      curl -X POST "$vpc_api_endpoint/v1/vpn_servers/$vpn_server_id/routes?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
           "name":"my-route-1",
            "destination": "10.10.10.0/24",
            "action": "translate"
         }'
   ```
   {: codeblock}

## Updating a route by using the API
{: #update-route-api-c2s}
{: api}

To update a route on the VPN server with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Perform a PATCH on `/vpn_servers/{vpn_server_id}/routes/{id}`

   ```sh
      curl -X PUT "$vpc_api_endpoint/v1/vpn_servers/$vpn_server_id/routes/$route_id?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
           "name":"new-route-name"
         }'
   ```
   {: codeblock}

## Viewing a route by using the API
{: #view-routes-api-c2s}
{: api}

To view a route on a VPN server with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. When all variables are initiated, list your routes:

   ```sh
   curl -sS -X GET \
   -H "Authorization: $iam_token" \
   "$vpc_api_endpoint/v1/vpn_servers/$vpn_server_id/routes?version=$api_version&generation=2" | jq
   ```
   {: codeblock}

   The example uses `jq` as a parser, a third-party tool licensed under the [MIT license](https://stedolan.github.io/jq/download/). `jq` might not come preinstalled on all VPC images available when you create an instance. You might need to install `jq` before use or use another parser of your choice.
   {: note}

1. Perform a GET on `/vpn_servers/{vpn_server_id}/routes/{id}`. For details, see [`list_vpn-server_routes`](/apidocs/vpc/latest#list-vpn-server-routes).

   ```sh
   curl -sS -X GET \
   -H "Authorization: $iam_token" \
   "$vpc_api_endpoint/v1/vpn_servers/$vpn_server_id/routes/$route_id?version=$api_version&generation=2" | jq
   ```
   {: codeblock}

## Deleting a route by using the API
{: #delete-route-api}
{: api}

To delete a route on a VPN server with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Perform a DELETE on `/vpn_servers/{vpn_server_id}/routes/{id}`.

   ```sh
   curl -sS -X DELETE \
   -H "Authorization: $iam_token" \
   "$vpc_api_endpoint/v1/vpn_servers/$vpn_server_id/routes/$route_id?version=$api_version&generation=2"
   ```
   {: codeblock}
