---

copyright:
  years: 2026
lastupdated: "2026-10-21"

keywords: dynamic route server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a peer group
{: #drs-create-peer-group}

Dynamic Route Server for VPC is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You can create a peer group to logically group related peers, define their routing roles, configure health monitoring, and control, which routes the dynamic route server processes from those peers. Policies are applied at the peer group level.
{: shortdesc}

## Before you begin
{: #create-peer-group-prerequisites}

Before you create a peer group:

- Review [Planning considerations for dynamic route servers](/docs/vpc?topic=vpc-planning-drs).
- Review [Known issues for dynamic route servers](/docs/vpc?topic=vpc-known-issues).
- Make sure that you have a dynamic route server that is provisioned with at least one member. For more information, see [Creating a dynamic route server](/docs/vpc?topic=vpc-drs-create).

## Creating a peer group in the console
{: #drs-create-peer-group-ui}
{: ui}

To create a peer group in the console, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), and then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Dynamic route servers**.
1. In the list, select the row of the dynamic route server that you want to work with, and then click **Create peer group +**.
1. Enter a name for the peer group, such as `my-peer-group`.
1. Select one of the following peer roles:
   - **Router** - Use this role when the peer exchanges routes with the dynamic route server by using BGP. Suitable for routers, firewalls, SD-WAN devices that advertise and learn routes.
   - **Next hop** - Use this role when the peer is only a forwarding target, not a route source. Suitable for firewalls or appliances through which traffic must flow, but that don't need to advertise routes.

      Transit gateway peer groups can be set up only from the Transit Gateway service. For more information, see [Adding a connection](/docs/transit-gateway?topic=transit-gateway-adding-connections&interface=ui).
      {: note}

1. Select one of the following health monitor modes:
   - **BGP** - Uses BGP session state to determine peer health. When the peer goes down, it automatically removes routes that use this peer as the next hop.
   - **BFD over BGP** - Uses Bidirectional Forwarding Detection (BFD) with BGP to monitor peer connectivity. Provides faster failure detection and quicker route withdrawal, more suitable for next-hop devices.
   - **None** (Next-hop only) - Disables peer health monitoring. Connectivity failures aren't automatically detected.
1. Enable the peer group state to establish a BGP session with peers in the peer group.

### Creating a prefix watcher
{: #drs-create-prefix-watcher}
{: ui}

A prefix watcher is a rule that controls which routes from the peer group are allowed or denied by the dynamic route server before any policies are applied. It is an optional configuration. Use a prefix watcher to limit route intake, block unwanted prefixes, and reduce routing noise.

To create a prefix watcher in the console, follow these steps:

1. In the **Prefix watcher (optional)** section, click **Create**.
1. Select the **Prefix type** based on your requirements.
   - **Monitored prefixes** - Only routes that match these prefixes are eligible for use.
   - **Excluded prefixes** - Routes that match these prefixes are always ignored, even if monitored.
1. Enter the **Prefix**, which is the network prefix along with its subnet mask (for example, `10.0.0.0/8`), that you want to monitor or exclude.
1. Optional: Enter values for **GE** (greater than or equal to) and **LE** (less than or equal to) to define the range of prefix lengths that match the rule.
   - **GE** specifies the minimum prefix length that is allowed to match.
   - **LE** specifies the maximum prefix length that is allowed to match.
   - For example, for a prefix of `10.0.0.0/8`, setting GE = `16` and LE = `24` matches routes from `/16` through `/24` within the `10.0.0.0/8` address space. If you specify only GE, no upper limit exists on the prefix length. If you both GE and LE are blank, no prefix length constraint is applied, and the rule matches all routes within the prefix.
1. Click **Add to list** to add the filter to the prefix watcher list. The prefix watcher is an ordered list of filters.

In the **Peers** section, click **Attach** to attach a peer. In the **Policies** section, turn on **Create policy** to create a peer group policy. Then, click **Create peer group**. For more information, see [Attaching a peer to a peer group](/docs/vpc?topic=vpc-drs-attach-peer-to-group&interface=ui) and [Creating a peer group policy](/docs/vpc?topic=vpc-drs-create-peer-group-policy&interface=ui).


## Creating a peer group from the CLI
{: #drs-create-peer-group-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a peer group for the dynamic route server from the CLI, run the following command:

```sh
ibmcloud is dynamic-route-server-peer-group-create DYNAMIC_ROUTE_SERVER (--roles next_hop | router | next_hop,router) (--peers PEERS_JSON | @PEERS_JSON_FILE) (--health-monitor-roles next_hop | router) [--health-monitor-mode bfd | bgp | none] [--name NAME] [--state disabled | enabled] [--excluded-prefixes EXCLUDED_PREFIXES_JSON | @EXCLUDED_PREFIXES_JSON_FILE] [--monitored-prefixes MONITORED_PREFIXES_JSON | @MONITORED_PREFIXES_JSON_FILE] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

`DYNAMIC_ROUTE_SERVER`
    : The dynamic route server identifier.

`--roles`
    : A comma-separated list of peer group roles. One of: `next_hop`, `router`, or `next_hop,router`.

`--peers`
    : A JSON array that defines the peers. You can specify a minimum of one and a maximum of six peers. You can also add peers separately. For more information, see [Attaching a peer to a peer group](/docs/vpc?topic=vpc-drs-attach-peer-to-group&interface=cli).

`@PEERS_JSON_FILE`
    : A file that contains the peers JSON payload.

`--health-monitor-roles`
    : A comma-separated list of roles for the health monitor configuration. One of: `next_hop` or `router`.

`--health-monitor-mode`
    : The health monitor mode. For the `next_hop` role, the options are `bfd` (default), `bgp`, or `none`. For the `router` role, the options are `bfd` or `bgp` (default).

`--name`
    : The name of the peer group.

`--state`
    : The peer group state. One of: `disabled` or `enabled`. The default is `enabled`.

`--excluded-prefixes`
    : A JSON array of CIDR prefixes to exclude from monitoring.

`@EXCLUDED_PREFIXES_JSON_FILE`
    : A file that contains the excluded prefixes JSON payload.

`--monitored-prefixes`
    : A JSON array of CIDR prefixes to monitor.

`@MONITORED_PREFIXES_JSON_FILE`
    : A file that contains the monitored prefixes JSON payload.

`--output`
    : Output in JSON format.

`-q, --quiet`
    : Suppresses verbose output.

After you create a peer group, attach a peer and create a peer group policy. For more information, see [Attaching a peer to a peer group](/docs/vpc?topic=vpc-drs-attach-peer-to-group&interface=cli) and [Creating a peer group policy](/docs/vpc?topic=vpc-drs-create-peer-group-policy&interface=cli).

## Creating a peer group with the API
{: #drs-create-peer-group-api}
{: api}

To create a peer group with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the required variables.
1. Store any additional variables to use in the API commands. For example:

   * `ResourceGroupId` - Find the resource group ID by using the `get resource groups` command and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

1. When all variables are set, create the peer group.

   * To create a peer group with one to six peers, run the following command:

   ```sh
   curl -X POST \
     "$vpc_api_endpoint/v1/dynamic_route_servers/DRS_ID/peer_groups?version=$api_version&generation=2" \
     -H "Authorization: Bearer $iam_token" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "my-peer-group",
       "state": "enabled",
       "roles": ["next_hop", "router"],
       "health_monitor": {
         "mode": "bfd",
         "roles": ["next_hop"]
       },
       "peers": [
         {
           "name": "my-peer",
           "asn": 64520,
           "priority": 1,
           "endpoint": {
             "address": "10.40.38.2",
             "gateway": {
               "address": "10.30.40.5"
             }
           }
         }
       ]
     }'
   ```
   {: codeblock}

After you create a peer group, attach a peer and create a peer group policy. For more information, see [Attaching a peer to a peer group](/docs/vpc?topic=vpc-drs-attach-peer-to-group&interface=api) and [Creating a peer group policy](/docs/vpc?topic=vpc-drs-create-peer-group-policy&interface=api).



## Next steps
{: #drs-peer-group-next-steps}

Now that your peer group is created, consider these next steps:

- [Attach a peer to a peer group](/docs/vpc?topic=vpc-drs-attach-peer-to-group).
- [Create a peer group policy](/docs/vpc?topic=vpc-drs-create-peer-group-policy).
