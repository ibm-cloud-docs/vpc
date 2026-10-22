---

copyright:
  years: 2026
lastupdated: "2026-10-22"

keywords: dynamic route server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a peer group policy
{: #drs-create-peer-group-policy}

Dynamic Route Server for VPC is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

A peer group policy is a rule that controls how a dynamic route server handles routes for a peer group. Policies define whether the dynamic route server reflects learned routes to other peer groups, advertises VPC address prefixes or custom static routes to peers, or adds learned routes to VPC routing tables with a specified next hop. You can create multiple policy types for a peer group to build the routing behavior that your topology requires.
{: shortdesc}

## Before you begin
{: #create-peer-group-policy-prerequisites}

Before you create a peer group policy:

- Review [Planning considerations for dynamic route servers](/docs/vpc?topic=vpc-planning-drs).
- Review [Known issues for dynamic route servers](/docs/vpc?topic=vpc-known-issues).
- Make sure that you have [created a peer group](/docs/vpc?topic=vpc-drs-create-peer-group) and [added at least one peer](/docs/vpc?topic=vpc-drs-attach-peer-to-group).

## Creating a peer group policy in the console
{: #drs-create-peer-group-policy-ui}
{: ui}

You can create peer group policies only for peers of the `Router` type because only these peers participate in route exchange and routing decisions.
{: note}

You can add only one policy of each type to a peer group. To add a different policy of the same type, first remove the existing policy.
{: note}

To create a peer group policy in the console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), and then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Dynamic route servers**.
1. In the list, select the dynamic route server that you want to work with, and then click the **Peer groups** tab.
1. Select the name of the peer group.
1. In the **Peer policy** section, click **Create**.
1. In the **Create policy** panel, enter a **Policy name**, such as `my-peer-policy`.
1. Select one of the following **Policy type** options and enter the required information:
   - **Custom routes** - Defines routes that are managed by the dynamic route server. Use this policy when a route is not learned through BGP and must be explicitly defined. Use it to steer traffic toward specific next-hop peers, such as firewalls. In the **Additional details** section, enter one or more destination CIDRs in **Custom route destination** to which you want to route traffic. Separate multiple CIDRs with commas.
   - **VPC routing table** - Adds routes to selected ingress or egress VPC routing tables. Use this policy to steer VPC traffic by using learned or custom routes, usually through a specific next hop with failover support. In the **Additional details** section, enter the following information:
      - **Next hop** - Specifies the peer group whose members are used as next-hop devices for routes added to the VPC routing table. Traffic that matches the route is forwarded to one of the available peers in the selected peer group.
      - **Route delete delay** - Specifies how long the system waits before removing a route after a peer becomes unhealthy.
      - **Ingress routing table** - Specifies the routing table that receives routes learned from peers of the configured peer group.
      - **Egress routing table** - Specifies the routing table that is used to advertise routes to peers of the configured peer group.
      - **Prefix exclusion** - Specifies one or more prefixes that are excluded from route addition or advertisement by this policy. Use prefix exclusions to prevent specific networks from being added to VPC routing tables or propagated to peers, even when they match other policy criteria.

       For prefix exclusions, you can exclude only prefixes that match or are subnets of monitored prefixes that are configured on the peer groups referenced by a policy. For example, if a referenced peer group in the policy contains a monitored prefix of `10.0.0.0/8`, you can exclude `10.0.0.0/8`, `10.1.0.0/16`, or `10.1.1.0/24` because each prefix matches or is a subnet of the monitored prefix. However, you cannot exclude `172.16.0.0/16` because it is outside the monitored prefix range.
       {: note}

   - **Learned routes** - Reflects routes that are learned from one peer group to other router peer groups. Use this policy when the dynamic route server acts as a route reflector between different routing domains. In the **Additional details** section, enter the following information:
      - **Peer groups** - Specifies the peer groups from which routes are learned.
      - **Prefix exclusion** - Specifies prefixes that are excluded from route redistribution. Routes that match an excluded prefix are learned from peers but are not advertised to other peer groups through this policy. Use prefix exclusions to control route propagation and prevent specific networks from being shared across routing domains.
   - **VPC address prefixes** - Advertises the address prefixes of the VPC to router peers. Use this policy when external routers or on-premises devices must learn how to reach VPC workloads.
1. Make sure that **Policy state** is turned on to apply the configured policy rules.
1. Click **Save**.

## Creating a peer group policy from the CLI
{: #drs-create-peer-group-policy-cli}
{: cli}

To create a policy for a peer group from the CLI, follow these steps:

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
   ibmcloud is dynamic-route-server-peer-group-policy-create DYNAMIC_ROUTE_SERVER --peer-group PEER_GROUP (--type custom_routes | learned_routes | vpc_address_prefixes | vpc_routing_tables) [--name NAME] [--state enabled | disabled] [--custom-routes CUSTOM_ROUTES_JSON | @CUSTOM_ROUTES_JSON_FILE] [--peer-groups PEER_GROUPS_JSON | @PEER_GROUPS_JSON_FILE] [--next-hops NEXT_HOP_JSON | @NEXT_HOP_JSON_FILE] [--routing-tables ROUTING_TABLES_JSON | @ROUTING_TABLES_JSON_FILE] [--excluded-prefixes EXCLUDED_PREFIXES_JSON | @EXCLUDED_PREFIXES_JSON_FILE] [--route-delete-delay ROUTE_DELETE_DELAY] [--output JSON] [-q, --quiet]
   ```
   {: codeblock}

Where:

`DYNAMIC_ROUTE_SERVER`
    : The dynamic route server identifier.

`--peer-group`
    : The peer group identifier.

`--type`
    : The policy type. One of: `custom_routes`, `learned_routes`, `vpc_address_prefixes`, or `vpc_routing_tables`.

`--name`
    : The name of the policy.

`--state`
    : The policy state. One of: `enabled` or `disabled`. The default is `enabled`.

`--custom-routes (JSON)`
    : A JSON payload that defines custom route destinations.

`@CUSTOM_ROUTES_JSON_FILE`
    : A file that contains the custom route destinations JSON payload.

`--peer-groups (JSON)`
    : A JSON payload that defines the peer groups for the policy.

`@PEER_GROUPS_JSON_FILE`
    : A file that contains the peer groups JSON payload.

`--next-hops (JSON)`
    : A JSON array that defines the next hop identity. You can specify only one next hop.

`@NEXT_HOP_JSON_FILE`
    : A file that contains the next hop JSON payload.

`--routing-tables (JSON)`
    : A JSON payload that defines routing tables.

`@ROUTING_TABLES_JSON_FILE`
    : A file that contains the routing tables JSON payload.

`--excluded-prefixes (JSON)`
    : A JSON payload that defines the list of prefixes to exclude from the learned routes or VPC routing tables that are selected by this policy. Specify this option only for policies of type `learned_routes` or `vpc_routing_tables`.

`@EXCLUDED_PREFIXES_JSON_FILE`
    : A file that contains the excluded prefixes JSON payload.

`--route-delete-delay`
    : The delay, in seconds, before routes are deleted when they are no longer advertised. The value must be 0 - 60. Applies to policies of type `vpc_routing_tables`.

`--output`
    : Output in JSON format.

`-q, --quiet`
    : Suppresses verbose output.

### Command examples
{: #cli-cmd-examples-drs-create-peer-group-policy}

- To create a peer group policy, run the following command:

   ```sh
   ibmcloud is dynamic-route-server-peer-group-policy-create 0717-drs-12345678 --peer-group my-peer-group --type custom_routes --name my-peer-policy --state enabled --custom-routes '[{"destination":"10.20.0.0/16"}]' --output JSON
   ```
   {: pre}

- To create a peer group policy of type `learned_routes` that excludes a prefix, run the following command:

   ```sh
   ibmcloud is dynamic-route-server-peer-group-policy-create 0717-drs-12345678 --peer-group my-peer-group --type learned_routes --name my-learned-routes-policy --peer-groups '[{"id":"0717-pg-12345678"}]' --excluded-prefixes '[{"prefix":"10.20.1.0/24"}]' --output JSON
   ```
   {: pre}

   For prefix exclusions, you can exclude only prefixes that match or are subnets of monitored prefixes that are configured on the peer groups referenced by a policy. For example, if a referenced peer group in the policy contains a monitored prefix of `10.0.0.0/8`, you can exclude `10.0.0.0/8`, `10.1.0.0/16`, or `10.1.1.0/24` because each prefix matches or is a subnet of the monitored prefix. However, you cannot exclude `172.16.0.0/16` because it is outside the monitored prefix range.
   {: note}

- To create a peer group policy of type `vpc_routing_tables`, run the following command:

   ```sh
   ibmcloud is dynamic-route-server-peer-group-policy-create 0717-drs-12345678 --peer-group my-peer-group --type vpc_routing_tables --name my-vpc-rt-policy --next-hops '[{"id":"0717-pg-12345678"}]' --routing-tables '[{"vpc_routing_table":{"id":"0717-rt-12345678"},"advertise":true}]' --route-delete-delay 5 --output JSON
   ```
   {: pre}

## Creating a peer group policy with the API
{: #drs-create-peer-group-policy-api}
{: api}

To create a peer group policy with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the required variables.
1. Store any additional variables to use in the API commands. For example:

   - `ResourceGroupId` - Find the resource group ID by using the `get resource groups` command and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

1. When all variables are set, create the policy for the peer group:

   ```sh
   curl -X POST \
     "$vpc_api_endpoint/v1/dynamic_route_servers/DRS_ID/peer_groups/PEER_GROUP_ID/policies?version=$api_version&generation=2" \
     -H "Authorization: Bearer $iam_token" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "learned-routes-policy",
       "type": "learned_routes",
       "state": "enabled",
       "excluded_prefixes": [
         {
           "prefix": "10.0.0.0/24"
         }
       ]
     }'
   ```
   {: codeblock}

   For prefix exclusions, you can exclude only prefixes that match or are subnets of monitored prefixes that are configured on the peer groups referenced by a policy. For example, if a referenced peer group in the policy contains a monitored prefix of `10.0.0.0/8`, you can exclude `10.0.0.0/8`, `10.1.0.0/16`, or `10.1.1.0/24` because each prefix matches or is a subnet of the monitored prefix. However, you cannot exclude `172.16.0.0/16` because it is outside the monitored prefix range.
   {: note}
