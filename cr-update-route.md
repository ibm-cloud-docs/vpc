---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-18"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Updating a route
{: #update-vpc-route}

You can update a route for an IBM Cloud service by using the UI, CLI, or API.
{: shortdesc}

## Updating a route in the UI
{: #cr-route-update-the-ui}
{: ui}

To update a route in the UI, follow these steps:

1. Make sure to review [Limitations and guidelines](/docs/vpc?topic=vpc-about-custom-routes&interface=ui#limitations-custom-routes).
1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the Navigation Menu ![Navigation Menu](/images/menu_icon.png), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>Routing tables** in the Network section. The Routing tables for VPC page appears.
1. Locate the routing table with the routes that you want to update, and click its name in the table.  
1. In the Routes section, locate the route that you want to update in the table. Then, click the Actions menu ![Actions menu](images/overflow.png) to edit or delete the route. You can change the name, priority, and route type information.
1. Click **Save** to save your updates.

## Updating a route from the CLI
{: #cr-update-using-the-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To update a VPC route from the CLI, run the following command: 

```sh
ibmcloud is vpc-routing-table-route-update VPC ROUTING_TABLE ROUTE --name NEW_NAME [--priority PRIORITY] [--next-hop NEXT_HOP [--vpngw VPNGW]] [--output JSON] [-q, --quiet] 
```
{: pre}

Where:

* **VPC** is the ID or name of the VPC.
* **ROUTING_TABLE** is the ID or name of the VPC routing table.
* **ROUTE** is the ID or name of the VPC route.
* **--name** is the new name of the route.
* **--priority** is the route's priority. Smaller values have higher priority. If a custom routing table contains routes with the same destination, the route with the highest priority (smallest value) is selected.
* **-next-hop** If the action is **deliver**, this is the IP address or VPN connection ID or name of the next hop to which to route packets.
* **--output** formats the output in JSON.
* **--q, quiet** causes the command to run silently and does not generate any output.

### CLI examples
{: #examples-cli-route-update}
{: cli}

```sh
ibmcloud is vpc-routing-table-route-update 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 72b27b5c-f4b0-48bb-b954-5becc7c1d456 72b27b5c-f4b0-48bb-b954-5becc7c1d4ef --name my-vpc-route --priority 1
```

```sh
ibmcloud is vpc-routing-table-route-update 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 72b27b5c-f4b0-48bb-b954-5becc7c1d456 72b27b5c-f4b0-48bb-b954-5becc7c1d4ef --name my-vpc-route --next-hop 10.0.0.2
```

```sh
ibmcloud is vpc-routing-table-update 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3 72b27b5c-f4b0-48bb-b954-5becc7d1d2d3 --direct-link-ingress true -—output JSON
```

```sh
ibmcloud is vpc-routing-table-update my-vpc my-vpc-routing-table -—transit-gateway-ingress true --output JSON
```

```sh
ibmcloud is vpc-routing-table-update my-vpc my-vpc-routing-table --direct-link-ingress true —transit-gateway-ingress true -—output JSON
```

```sh
ibmcloud is vpc-routing-table-update my-vpc my-vpc-routing-table --transit-gateway-ingress true --output JSON
```
 
## Updating a route with the API
{: #cr-route-update-the-api}
{: api}

To update a destination route with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
2. Store values for the following variables to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    export RoutingTableId=<your_routing_table_id>
    ```
    {: codeblock}

3. Create a route:

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
