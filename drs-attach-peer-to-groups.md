---

copyright:
  years: 2026
lastupdated: "2026-10-22"

keywords: dynamic route server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching a peer to a peer group
{: #drs-attach-peer-to-group}

Dynamic Route Server for VPC is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You can attach a peer, such as firewalls, routers, SD-WAN devices, or a transit gateway, to a peer group to establish a BGP session between the dynamic route server and peer routing device.
{: shortdesc}

## Before you begin
{: #attach-peer-prerequisites}

Before you attach a peer to a peer group:

- Review [Planning considerations for dynamic route servers](/docs/vpc?topic=vpc-planning-drs).
- Review [Known issues for dynamic route servers](/docs/vpc?topic=vpc-known-issues).
- Make sure that you have a peer group created. For more information, see [Creating a peer group](/docs/vpc?topic=vpc-drs-create-peer-group).
- Make sure that the peer endpoint isn't used by another peer in any peer group of the dynamic route server.

## Attaching a peer to a peer group in the console
{: #drs-attach-peer-to-group-ui}
{: ui}

To attach a peer to a peer group in the console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), and then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Dynamic route servers**.
1. In the list, select the dynamic route server that you want to work with, and then click the **Peer groups** tab.
1. Select the name of the peer group.
1. In the **Peer members** section, click **Create**.
1. In the **Attach a peer** panel, enter a name for the peer, such as `my-peer`.
1. Enter the **Peer ASN**, which identifies the peer in the network.
1. Select the **Peer priority**. A lower value indicates a higher priority. The default is 2.
1. Turn on **Peer state** to establish a BGP session with the peer.
1. Define how the dynamic route server identifies and connects to the peer. Click **Select peer network** to use an existing network, and then select the **Peer VPC**, **Subnet**, and **Reserved IP**. Alternatively, click **Add peer network** and specify the IP address and the gateway IP to add a new one.
1. Click **Save**.

   A peer group must contain at least one peer. To remove the last peer, delete the peer group. Removing a peer also deletes all routes learned from it, and this action cannot be undone.
   {: note}

## Attaching a peer to a peer group from the CLI
{: #drs-attach-peer-group-cli}
{: cli}

To attach a peer to a peer group from the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your CLI environment. After you enter the password, the system prompts which account and region that you want to use:

   ```sh
   ibmcloud login --sso
   ```
   {: pre}

1. Enable the following feature flag:

   ```sh
   export IBMCLOUD_IS_FEATURE_DYNAMIC_ROUTE_SERVER=true
   ```
   {: pre}

1. Run the following command:

   ```sh
   ibmcloud is dynamic-route-server-peer-group-peer-create DYNAMIC_ROUTE_SERVER --peer-group PEER_GROUP --asn ASN --endpoint-rip ENDPOINT_RIP [--endpoint-rip-subnet ENDPOINT_RIP_SUBNET] [--vpc VPC] | --endpoint-address IP_ADDRESS [--gateway-address IP_ADDRESS] [--name NAME] [--state enabled | disabled] [--priority PRIORITY] [--output JSON] [-q, --quiet]
   ```
   {: codeblock}

Where:

`DYNAMIC_ROUTE_SERVER`
    : The dynamic route server identifier.

`--peer-group`
    : The peer group identifier.

`--asn`
    : The autonomous system number (ASN) for the peer.

`--endpoint-rip`
    : The route interface port identifier.

`--endpoint-rip-subnet`
    : The subnet identifier associated with the endpoint route interface port.

`--vpc`
    : The VPC identifier associated with the endpoint.

`--endpoint-address`
    : The peer IP address.

`--gateway-address`
    : The gateway IP address.

`--name`
    : The name of the peer.

`--state`
    : The peer state. One of: `enabled` or `disabled`. The default is `enabled`.

`--priority`
    : The peer priority, from 0 to 4. Lower values have higher priority. The default is 2.

`--output`
    : Output in JSON format.

`-q, --quiet`
    : Suppresses verbose output.

### Command examples
{: #cli-cmd-examples-drs-attach-peer-to-group}

- To attach a peer to a peer group, run the following command:

   ```sh
   ibmcloud is dynamic-route-server-peer-group-peer-create 0717-drs-12345678 --peer-group my-peer-group --asn 65001 --endpoint-address 192.168.10.10 --gateway-address 192.168.10.1 --name my-peer-1 --state enabled --priority 1 --output JSON
   ```
   {: pre}

## Attaching a peer to a peer group with the API
{: #drs-attach-peer-group-api}
{: api}

To attach a peer to a peer group with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the required variables.
1. Store any additional variables to use in the API commands. For example:

   - `ResourceGroupId` - Find the resource group ID by using the `get resource groups` command and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

1. When all variables are set, attach the peer to the peer group:

   ```sh
   curl -X POST \
     "$vpc_api_endpoint/v1/dynamic_route_servers/DRS_ID/peer_groups/PEER_GROUP_ID/peers?version=$api_version&generation=2" \
     -H "Authorization: Bearer $iam_token" \
     -H "Content-Type: application/json" \
     -d '{
       "asn": 64520,
       "state": "enabled",
       "priority": 1,
       "endpoint": {
         "address": "10.40.38.2",
         "gateway": {
           "address": "10.30.40.5"
         }
       }
     }'
   ```
   {: codeblock}



## Next steps
{: #drs-attach-peer-next-steps}

Now that you have attached a peer to the peer group, consider these next steps:

[Create a peer group policy](/docs/vpc?topic=vpc-drs-create-peer-group-policy).
