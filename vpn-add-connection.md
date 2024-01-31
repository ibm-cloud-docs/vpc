---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-07"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adding connections to a VPN gateway
{: #vpn-adding-connections}

You can add connections when you create an IBM Cloud VPN for VPC, or after you provision a VPN gateway. When you configure a VPN connection, you can choose to connect with auto-negotiation or use a pre-defined custom IKE or IPsec policy. For more information, see [About policy negotiation](/docs/vpc?topic=vpc-using-vpn#policy-negotiation).
{: shortdesc}

The IKE Phase 1 and Phase 2 (IPsec) security options that you specify for the connection must be the same options that are set on the peer gateway for the network outside your VPC.
{: important}

## Adding a connection in the UI
{: #vpn-using-ui-add-connection}
{: ui}

To add a VPN connection to an existing VPN gateway, follow these steps:

1. Highlight the row of the gateway in the VPN gateways table, then click **New connection** from the Actions menu ![Actions menu](images/overflow.png).

   Alternatively, on the gateway's details page, you can click **Create +** in the VPN connections section.
   {: note}

1. Define a connection between this gateway and a network outside your VPC by specifying the following information:
   * **VPN connection name** - Enter a name for the connection, such as `my-connection`.
   * **Peer gateway address** - Specify the IP address of the VPN gateway for the network outside your VPC.
   * **Preshared key** - Specify the authentication key of the VPN gateway for the network outside your VPC. The preshared key is a string of hexadecimal digits, or a passphrase of printable ASCII characters. To be compatible with most peer gateway types, this string must follow these rules:
      * Can be a combination of digits, lower or upper case characters, or the following special characters: `- + & ! @ # $ % ^ * ( ) . , :`
      * The length of the string must be 6 - 128 characters.
      * Cannot start with `0x` or `0s`.
   * **Local IBM CIDRs (Policy-based VPN only)** - Specify one or more CIDRs in the VPC you want to connect through the VPN tunnel.
   * **Peer CIDRs (Policy-based VPN only)** - Specify one or more CIDRs in the other network you want to connect through the VPN tunnel. Subnet range overlap between local and peer subnets is NOT allowed.
1. To configure how the VPN gateway sends messages to check that the peer gateway is active, specify the following information in the **Dead peer detection** section.
   * **Action** - The action to take if a peer gateway stops responding. For example, select **Restart** if you want the gateway to immediately renegotiate the connection.
   * **Interval (secs)** - How often to check that the peer gateway is active. By default, messages are sent every 2 seconds.
   * **Timeout (secs)** - How long to wait for a response from the peer gateway. By default, a peer gateway is considered inactive if a response isn't received within 10 seconds.
1. In the **Policies** section, specify the Internet Key Exchange (IKE) and Internet Protocol Security (IPsec) options to use for Phase 1 and Phase 2 negotiation of the connection.
   * Select **Auto** if you want the gateway to try to automatically establish the connection.
   * Select or create custom policies if you need to enforce particular security requirements, or if the VPN gateway for the other network doesn't support the security proposals that are tried by auto-negotiation.

## Adding a connection from the CLI
{: #vpn-using-cli-add-connection}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a VPN connection from the CLI, enter the following command:

```sh
ibmcloud is vpn-gateway-connection-create CONNECTION_NAME VPN_GATEWAY_ID PEER_ADDRESS PRESHARED_KEY
     --local-cidr CIDR1 --local-cidr CIDR2 ...
     --peer-cidr CIDR1 --peer-cidr CIDR2 ...
     [--admin-state-up true | false] [--dead-peer-detection-action restart | clear | hold | none]
     [--dead-peer-detection-interval INTERVAL] [--dead-peer-detection-timeout TIMEOUT]
     [--ike-policy IKE_POLICY_ID] [--ipsec-policy IPSEC_POLICY_ID] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

- **CONNECTION_NAME** - Name of the connection.
- **VPN_GATEWAY_ID** - ID of the VPN gateway.
- **PEER_ADDRESS** - The IP address of the peer VPN gateway.
- **PRESHARED_KEY** - The preshared key.
- **--admin-state-up value** - If set to `false`, the VPN gateway connection is shut down. One of: `true`, `false`. The default is `true`.
- **--dead-peer-detection-action value** - Dead Peer Detection actions. One of: `restart`, `clear`, `hold`, `none`. The default is `restart`.
- **--dead-peer-detection-interval value** - Dead Peer Detection interval in seconds. The default is `2`.
- **--dead-peer-detection-timeout value** - Dead Peer Detection timeout in seconds. The default is `10`.
- **--ike-policy value** - ID of the IKE policy.
- **--ipsec-policy value** - ID of the IPsec policy.
- **--local-cidr value** - Local CIDR for the resource.
- **--peer-cidr value** - Peer CIDRs for the resource.
- **--output value** - Specify the output in JSON format.
- **-q, --quiet** - Suppress verbose output.

### Command examples
{: #command-examples-vpn-connection-create}

- Create a VPN connection for a specific gateway ID with required configuration values:
   `ibmcloud is vpn-gateway-connection-create my-connection fee82deba12e4c0fb69c3b09d1f12345 169.21.50.5 lkj14b1oi0alcniejkso --local-cidr 10.240.0.0/24 --peer-cidr 192.168.1.0/24`
- Create a VPN connection with the same core parameters and specified DPD configurations:
   `ibmcloud is vpn-gateway-connection-create my-connection fee82deba12e4c0fb69c3b09d1f12345 169.21.50.5 lkj14b1oi0alcniejkso --local-cidr 10.240.0.0/24 --peer-cidr 192.168.1.0/24 --dead-peer-detection-action clear --dead-peer-detection-interval 33 --dead-peer-detection-timeout 100`
- Create a VPN connection with the same core parameters and custom policies with specified IDs:
   `ibmcloud is vpn-gateway-connection-create my-connection fee82deba12e4c0fb69c3b09d1f12345 169.21.50.5 lkj14b1oi0alcniejkso --local-cidr 10.240.0.0/24 --peer-cidr 192.168.1.0/24 --ipsec-policy 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --ike-policy 72251a2e-d6c5-42b4-97b0-b5f8e8d1f480`


## Adding a local CIDR to a VPN gateway connection from the CLI
{: #vpn-using-cli-vpn-gateway-connection-local-cidr-add}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To add a local CIDR to a VPN gateway connection from the CLI, enter the following command:

This command is supported only by policy mode VPN gateway.
   {: note}

```sh
bx is vpn-gateway-connection-local-cidr-add VPN_GATEWAY CONNECTION PREFIX_ADDRESS PREFIX_LENGTH [--vpc VPC] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

- **VPN_GATEWAY** - ID of the VPN gateway.
- **CONNECTION** - ID or name of the VPN connection.
- **PREFIX_ADDRESS** - The prefix address part of the CIDR.
- **PREFIX_LENGTH** - The prefix length part of the CIDR.
- **--output value** - Specify the output in JSON format.
- **-q, --quiet** - Suppress verbose output.

### Command examples
{: #command-examples-vpn-gateway-connection-local-cidr-add}

- Add a local CIDR for a specific connection name with required configuration values:
   `ibmcloud is vpn-gateway-connection-local-cidr-add my-vpn-gateway my-connection 3.3.3.0 24`

## Adding a peer CIDR to a VPN gateway connection from the CLI
{: #vpn-using-cli-vpn-gateway-connection-peer-cidr-add}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To add a peer CIDR to a VPN gateway connection from the CLI, enter the following command:

This command is supported only by policy mode VPN gateway.
   {: note}

```sh
bx is vpn-gateway-connection-peer-cidr-add VPN_GATEWAY CONNECTION PREFIX_ADDRESS PREFIX_LENGTH [--vpc VPC] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

- **VPN_GATEWAY** - ID of the VPN gateway.
- **CONNECTION** - ID or name of the VPN connection.
- **PREFIX_ADDRESS** - The prefix address part of the CIDR.
- **PREFIX_LENGTH** - The prefix length part of the CIDR.
- **--output value** - Specify the output in JSON format.
- **-q, --quiet** - Suppress verbose output.

### Command examples
{: #command-examples-vpn-gateway-connection-peer-cidr-add}

- Add a peer CIDR for a specific connection name with required configuration values:
   `ibmcloud is vpn-gateway-connection-peer-cidr-add my-vpn-gateway my-connection 4.4.4.0 24`

## Adding a connection with the API
{: #vpn-using-api-add-connection}
{: api}

To create a VPN connection with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   * `vpnGatewayId` - The VPN gateway identifier

       ```sh
       export vpnGatewayId=<your_vpn_gateway_id>
       ```
       {: pre}

   * `ikePolicyId` - The unique identifier for this IKE policy

      ```sh
      export ikePolicyId=<your_ike_policy_id>
      ```
      {: pre}

   * `ipsecPolicyId` - The unique identifier for this IPsec policy

       ```sh
       export ipsecPolicyId=<your_ipsec_policy_id>
       ```
       {: pre}

1. When all variables are initiated, create the VPN gateway connection. For example:

   ```sh
      curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$vpnGatewayId/connections?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
            "local_cidrs": [
                "'$localCidrs'"
            ],
            "name": "my-vpn-connection",
            "peer_address": "7.8.9.10",
            "peer_cidrs": [
                "'$remoteCidrs'"
            ],
            "psk": "'$psk'",
            "dead_peer_detection": {
                "action": "restart",
                "interval": 2,
                "timeout": 10
            },
            "ike_policy": {
                "id": "'$ikePolicyId'"
            },
            "ipsec_policy": {
                "id": "'$ipsecPolicyId'"
            }
        }'
   ```
   {: codeblock}

## Adding a local CIDR to a VPN gateway connection with the API
{: #vpn-using-api-vpn-gateway-connection-local-cidr-add}
{: api}

To add a local CIDR to a VPN gateway connection with the API, follow these steps:

This API is supported only by policy mode VPN gateway.
   {: note}

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   * `vpnGatewayId` - The VPN gateway identifier

       ```sh
       export vpnGatewayId=<your_vpn_gateway_id>
       ```
       {: pre}

   * `connectionId` - The unique identifier for this VPN connection

      ```sh
      export connectionId=<your_connection_id>
      ```
      {: pre}

   * `cidr_prefix` - The prefix address part of the CIDR

      ```sh
      export cidr_prefix=<your_cidr_prefix>
      ```
      {: pre}

   * `prefix_length` - The prefix length part of the CIDR.

      ```sh
      export prefix_length=<your_prefix_length>
      ```
      {: pre}


1. When all variables are initiated, add a local CIDR to a VPN gateway connection. For example:

   ```sh
      curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpnGatewayId/connections/$connectionId/local_cidrs/${cidr_prefix}/${prefix_length}?version=$api_version&generation=2" \
        -H "Authorization: $iam_token"
   ```
   {: codeblock}

## Adding a peer CIDR to a VPN gateway connection with the API
{: #vpn-using-api-vpn-gateway-connection-peer-cidr-add}
{: api}

To add a peer CIDR to a VPN gateway connection with the API, follow these steps:

This API is supported only by policy mode VPN gateway.
   {: note}

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   * `vpnGatewayId` - The VPN gateway identifier

       ```sh
       export vpnGatewayId=<your_vpn_gateway_id>
       ```
       {: pre}

   * `connectionId` - The unique identifier for this VPN connection

      ```sh
      export connectionId=<your_connection_id>
      ```
      {: pre}

   * `cidr_prefix` - The prefix address part of the CIDR

      ```sh
      export cidr_prefix=<your_cidr_prefix>
      ```
      {: pre}

   * `prefix_length` - The prefix length part of the CIDR.

      ```sh
      export prefix_length=<your_prefix_length>
      ```
      {: pre}


1. When all variables are initiated, add a peer CIDR to a VPN gateway connection. For example:

   ```sh
      curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpnGatewayId/connections/$connectionId/peer_cidrs/${cidr_prefix}/${prefix_length}?version=$api_version&generation=2" \
        -H "Authorization: $iam_token"
   ```
   {: codeblock}

## Adding a connection by using Terraform
{: #vpn-using-cli-add-terraform}
{: terraform}

The following example creates a VPN gateway connection.

```terraform
   resource "ibm_is_vpn_gateway_connection" "is_vpn_gateway_connection" {
     name                 = "my-vpn-gateway-connection"
     vpn_gateway    = ibm_is_vpn_gateway.is_vpn_gateway.id
     peer_address   =  "7.8.9.10"
     preshared_key = var.presharedkey
     local_cidrs        = [var.localCIDR]
     peer_cidrs        = [var.peerCIDR]
   }
```
{: codeblock}

See the [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpn_gateway_connection){: external} for more information.

## Next steps
{: #vpn-add-connection-next-steps}

For a route-based VPN, select or [create a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table). Then, [create a route using the VPN connection type](/docs/vpc?topic=vpc-create-vpc-route).
