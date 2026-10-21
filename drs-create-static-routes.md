---

copyright:
  years: 2026
lastupdated: "2026-10-21"

keywords: dynamic route server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a static route
{: #drs-create-static-routes}

Dynamic Route Server for VPC is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

A static route on a dynamic route server is a manually defined network path that the dynamic route server manages in VPC routing tables on your behalf. Unlike a traditional VPC static route, a dynamic route server static route is health-aware: it is automatically added to or removed from the specified VPC routing tables based on the health of the peer group that serves as the next hop.
{: shortdesc}

## Before you begin
{: #create-static-route-prerequisites}

Before you create a static route:

- Review [Planning considerations for dynamic route servers](/docs/vpc?topic=vpc-planning-drs).
- Review [Known issues for dynamic route servers](/docs/vpc?topic=vpc-known-issues).
- Make sure that you have a dynamic route server that is provisioned with at least one member. For more information, see [Creating a dynamic route server](/docs/vpc?topic=vpc-drs-create).

## Creating a static route in the console
{: #drs-create-static-route-ui}
{: ui}

To create a static route in the console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), and then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Dynamic route servers**.
1. In the list, select the dynamic route server that you want to work with, and then click the **Routes** tab.
1. Click **Create**.
1. Enter a name for the static route, such as `my-static-route`.
1. Enter the destination CIDR where you want the traffic to be directed.
1. For **Next hop**, select the peer group to use as the next hop.
1. Optional: Enter a **Route delete delay (in sec)**.
1. In the **Ingress routing table** and **Egress routing table** sections, click **Create** to add the VPC routing tables to use. For each routing table, turn on **Advertise routes** if you want to advertise the routes.
1. Click **Add**.

## Creating a static route from the CLI
{: #drs-create-static-route-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a static route from the CLI, run the following command:

```sh
ibmcloud is dynamic-route-server-static-route-create DYNAMIC_ROUTE_SERVER --destination DESTINATION (--next-hops NEXT_HOPS_JSON | @NEXT_HOPS_JSON_FILE) (--routing-tables ROUTING_TABLES_JSON | @ROUTING_TABLES_JSON_FILE) [--name NAME] [--route-delete-delay ROUTE_DELETE_DELAY] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

`DYNAMIC_ROUTE_SERVER`
    : The dynamic route server identifier.

`--destination`
    : The destination CIDR of the static route. The host identifier in the CIDR must be zero.

`--next-hops`
    : A JSON array that defines the peer group to use as the next hop for the static route. You can specify only one next hop.

`@NEXT_HOPS_JSON_FILE`
    : A file that contains the next hops JSON payload.

`--routing-tables`
    : A JSON array that defines the VPC routing tables to add the static route to.

`@ROUTING_TABLES_JSON_FILE`
    : A file that contains the routing tables JSON payload.

`--name`
    : The name of the static route.

`--route-delete-delay`
    : The delay, in seconds, before the routes are deleted when the status of the associated peer is down. The value must be 0 - 60. The default is 0.

`--output`
    : Output in JSON format.

`-q, --quiet`
    : Suppresses verbose output.

### Command examples
{: #cli-cmd-examples-drs-create-static-route}

- To create a static route, run the following command:

   ```sh
   ibmcloud is dynamic-route-server-static-route-create 0717-drs-12345678 --destination 10.50.0.0/16 --next-hops '[{"id":"0717-pg-12345678"}]' --routing-tables '[{"vpc_routing_table":{"id":"0717-rt-12345678"},"advertise":true}]' --name my-static-route-1 --route-delete-delay 5 --output JSON
   ```
   {: pre}

## Creating a static route with the API
{: #drs-create-static-route-api}
{: api}

To create a static route with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the required variables.
1. Store any additional variables to use in the API commands. For example:

   - `ResourceGroupId` - Find the resource group ID by using the `get resource groups` command and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

1. When all variables are set, create the static route:

   ```sh
   curl -X POST \
     "$vpc_api_endpoint/v1/dynamic_route_servers/DRS_ID/static_routes?version=$api_version&generation=2" \
     -H "Authorization: Bearer $iam_token" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "my-static-route",
       "destination": "10.0.0.0/8",
       "next_hops": [
         {
           "id": "PEER_GROUP_ID"
         }
       ],
       "routing_tables": [
         {
           "vpc_routing_table": {
             "id": "ROUTING_TABLE_ID"
           },
           "advertise": true
         }
       ]
     }'
   ```
   {: codeblock}
