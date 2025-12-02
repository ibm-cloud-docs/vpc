---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-02"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adding connections to a VPN gateway
{: #vpn-adding-connections}

You can add connections when [creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway&interface=ui), or after provisioning one. When you configure a VPN connection, you can choose to connect with auto-negotiation or use a pre-defined custom IKE or IPsec policy. For more information, see [About policy negotiation](/docs/vpc?topic=vpc-using-vpn#policy-negotiation).
{: shortdesc}

The IKE and IPsec security options that you specify for the connection must exactly match the ones that are set on the peer gateway for the network outside your VPC.
{: important}

## Adding a connection in the console
{: #vpn-using-ui-add-connection}
{: ui}

To add a VPN connection to an existing VPN gateway, follow these steps:

1. Highlight the row of the gateway that you want to work with in the VPN gateways table, then click **Create connection** from the Actions menu ![Actions menu](../icons/action-menu-icon.svg "Actions").

   Alternatively, on the gateway's details page, you can click **Create** in the VPN connections section.
   {: note}

1. Define a connection between this gateway and a network outside your VPC by specifying the following information:
   * **VPN connection name** - Enter a name for the connection, such as `my-connection`.
   * **Connection type** - Select static or dynamic connection type.
   * **Peer gateway address** - Specify the IP address of the VPN gateway for the network outside your VPC.

      After you provision the VPN connection, you cannot change the peer gateway address type from IP address to FQDN, or from FQDN to IP address.
      {: note}

   * **Peer ASN (Dynamic route-based only)** - If you select **Dynamic** as the connection type, you must specify the peer ASN. This value identifies the external peer network with which the VPN exchanges routes.

   * **Establish mode** - Select either **Bidirectional** or **Peer only**.

       - **Bidirectional** mode initiates IKE protocol negotiations (or rekeying processes) from either side of the VPN gateway.
       - **Peer only** mode allows the peer to initiate IKE protocol negotiations for this VPN gateway connection. The peer is also responsible for initiating the rekeying process after the connection is established.

      If your peer device is behind a NAT device and doesn't have a public IP address, make sure to specify **Peer only**.
      {: important}

   * **Preshared key** - Specify the authentication key of the VPN gateway for the network outside your VPC. The preshared key is a string of hexadecimal digits, or a passphrase of printable ASCII characters. To be compatible with most peer gateway types, this string must follow these rules:
      * Can be a combination of digits, lower or uppercase characters, or the following special characters: `- + & ! @ # $ % ^ * ( ) . , :`
      * The length of the string must be 6 - 128 characters.
      * Cannot start with `0x` or `0s`.

   * **Distribute traffic (Route-based VPN only)** - Enable this option to automatically distribute traffic between the active tunnels of the VPN gateway when a VPC route's next hop is the VPN connection. This option is useful to maximize throughput wherein two VPN tunnels connect to your remote peer network. If this option isn't enabled, the VPN gateway chooses the tunnel with the smaller public IP as the primary path, and switches to the secondary tunnel only if the primary goes down. For more information, see [Use case 4: Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn&interface=ui#use-case-4-vpn).

   * **Local IBM CIDRs (Policy-based VPN only)** - Specify one or more CIDRs in the VPC that you want to connect through the VPN tunnel.
   * **Peer CIDRs (Policy-based VPN only)** - Specify one or more CIDRs in the other network that you want to connect through the VPN tunnel. Subnet range overlap between local and peer subnets is not allowed.

1. In the **Tunnel details** section (Dynamic route-based only), specify the tunnel and peer interface IPs required for dynamic routing with BGP.
    * **Tunnel interface IP** - Specify the IP address that is assigned to the VPN gateway side of the VPN tunnel.
    * **Peer interface IP** - Specify the IP address that is assigned to the remote network side of the VPN tunnel. This address is your on-premises device or peer VPN gateway.

        The tunnel and peer interface IPs must be continuous and belong to the same `/30` subnet. This subnet provides four IP addresses, but the first (network address) and last (broadcast address) are reserved. For example, in the subnet `192.168.0.0/30`, usable IPs are `192.168.0.1` and `192.168.0.2`. If you assign `192.168.0.1` to the tunnel 1 interface, the peer 1 interface must be `192.168.0.2`. Similarly, for Tunnel 2, use a different `/30` subnet, such as `192.168.0.4/30`. In this case, assign `192.168.0.5` to the tunnel 2 interface and `192.168.0.6` to the peer 2 interface.
        {: note}

1. To configure how the VPN gateway sends messages to check that the peer gateway is active, specify the following information in the **Dead peer detection** section.
   * **Action** - The action to take if a peer gateway stops responding. For example, select **Restart** if you want the gateway to immediately renegotiate the connection.
   * **Interval (secs)** - How often to check that the peer gateway is active. By default, messages are sent every 2 seconds.
   * **Timeout (secs)** - How long to wait for a response from the peer gateway. By default, a peer gateway is considered inactive if a response isn't received within 10 seconds.
1. In the **Policies** section, specify the Internet Key Exchange (IKE) and Internet Protocol Security (IPsec) options to use for Phase 1 and Phase 2 negotiation of the connection.
   * Select **Auto** if you want the gateway to try to automatically establish the connection.
   * Select or create custom policies if:
      * You need to enforce particular security requirements.
      * The VPN gateway on the other network doesn't support the security proposals that are automatically negotiated during setup.


1. In the **Advanced options** section, you can customize local and peer IKE identities instead of using the default IKE identity. One peer IKE identity can be specified at most.

   For policy-based VPN gateways, you can configure one local IKE identity at most. For route-based VPN gateways, if you want to configure a local IKE identity, you must provide two. You can provide values for the members or leave the input fields empty.
   {: note}

   * **Local IKE identities** - Select a type for the local IKE identity, then enter its value. For example, you can enter a single 4 octet IPv4 address (`9.168.3.4`), an FQDN (`my-vpn.example.com`), hostname (`my-host`), or base64-encoded key ID (`MTIzNA==`).

      * Static route mode consists of two members in active-active mode, where the first identity applies to the first member and the second identity applies to the second member. If you do not specify local IKE identities, then the type is an IPv4 address, and the value is the public IP address of the member’s VPN connection tunnel.

      * Policy mode consists of two members in active-standby mode. The local IKE identity applies to the active member. If you do not specify a value, then the local IKE identity is the public IP address of the VPN gateway.

   * **Peer IKE identity** - Select a type for the peer IKE identity, then enter its value. For example, you can enter an IPv4 address (`9.168.3.4`), an FQDN (`my-vpn.example.com`), hostname (`my-host`), or base64-encoded key ID (`MTIzNA==`).

      The peer IKE identity applies to the active member. If you do not specify a value, then use the peer gateway's IPv4 address or FQDN.

1. Review the Summary panel, then click **Create VPN connection**.

## Adding a connection from the CLI
{: #vpn-using-cli-add-connection}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a VPN connection from the CLI, enter the following command:

```sh
ibmcloud is vpn-gateway-connection-create CONNECTION_NAME VPN_GATEWAY PEER PRESHARED_KEY
[--vpc VPC] [--admin-state-up true | false]
[--routing-protocol bgp | none]
[--dead-peer-detection-action restart | clear | hold | none]
[--distribute-traffic true | false]
[--dead-peer-detection-interval INTERVAL] [--dead-peer-detection-timeout TIMEOUT] [--ike-policy IKE_POLICY_ID]
[--ipsec-policy IPSEC_POLICY_ID] [--peer-cidr CIDR1 --peer-cidr CIDR2 ... --local-cidr CIDR1 --local-cidr CIDR2 ...]
[[--local-ike-identity-type fqdn | hostname | ipv4_address | key_id --local-ike-identity-value VALUE] |
[--local-ike-identities LISTENER_POLICIES_JSON | @LISTENER_POLICIES_JSON_FILE]]
[--peer-asn]
[--tunnels NEIGHBOR_IP TUNNEL_INTERFACE_IP]
[--peer-ike-identity-type fqdn | hostname | ipv4_address | key_id --peer-ike-identity-value VALUE]
[--establish-mode bidirectional | peer_only] [--output JSON] [-q, --quiet]
ibmcloud is vpn-gateway-connection-create CONNECTION_NAME VPN_GATEWAY PEER PRESHARED_KEY
```
{: codeblock}

Where:

`CONNECTION_NAME`
    : The name of the connection.

`VPN_GATEWAY`
    : The ID of the VPN gateway.

`PEER`
    : The IP address or FQDN of the peer VPN gateway.

`PRESHARED_KEY`
    : The preshared key.

`--vpc`
    : The ID or name of the VPC. This field is only required to specify the unique resource by its name inside this VPC.

`--admin-state-up`
    : If set to `false`, the VPN gateway connection is shut down. This field value can be either `true` or `false`.

`--routing-protocol`
    : Determines if the mode is static or dynamic. This field value can either be `bgp` or `none`. Set the value to `bgp` for dynamic route-based VPN.

`--dead-peer-detection-action`
    : The dead peer detection action. This field value can be either `restart`, `clear`, `hold`, or `none`. (Default: `restart`).

`--dead-peer-detection-interval`
    : The dead peer detection interval in seconds (default: `2`).

`--dead-peer-detection-timeout`
    :  The dead peer detection timeout in seconds (default: `10`).

`--distribute-traffic`
    :  Set to `true` to distribute traffic between the `Up` tunnels of the VPN gateway connection when a VPC route's next hop is the VPN connection. This value can be either `true` or `false`. For more information, see [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn&interface=terraform#use-case-4-vpn).

`--ike-policy`
    : The ID of the IKE policy.

`--ipsec-policy`
    : The ID of the IPsec policy.

`--local-ike-identity-type`
    : The type of local IKE identity. This field value can be either `fqdn`, `hostname`, `ipv4_address`, or `key_id`.

`--local-ike-identity-value`
    : The value of the local IKE identity.

`--local-ike-identities`
    : The ID of the local IKE identity. `LOCAL_IKE_IDENTITIES_JSON  | @LOCAL_IKE_IDENTITIES_JSON_FILE` in JSON or a JSON file.

`--peer-asn`
    : Identifies the external peer network in dynamic routing with which the VPN exchanges routes.

`--tunnels`
    : Specifies the tunnel and peer interface IPs required for dynamic routing with BGP. This field takes the IP address value of the local (tunnel) and remote (peer) interfaces. The tunnel and peer interface IPs must be continuous and belong to the same /30 subnet.

`-peer-cidr`
    : The peer CIDRs for the resource.

`-local-cidr`
    : The local CIDR for the resource.

`-peer-ike-identity-type`
    : The type of peer IKE identity. This field value can either be `ipv4_address`, `fqdn`, `hostname`, or `key_id`.

`--peer-ike-identity-value`
    : The value of the peer IKE identity.

   Policy-based VPN gateways can have only one local IKE identity.

   If a route-based VPN gateway has local IKE identities that are specified, then there must be at least two; the first identity applies to the first member of the VPN gateway, and the second identity applies to the second member.

`--establish-mode`
    : This field can be either `bidirectional` or `peer_only`. Bidirectional mode initiates IKE protocol negotiations (or rekeying processes) from either side of the VPN gateway. Peer only mode allows the peer to initiate IKE protocol negotiations for this VPN gateway connection. The peer is also responsible for initiating the rekeying process after the connection is established. If rekeying doesn't occur, the VPN gateway connection is removed after its lifetime expires.

`-output`
    : Specifies that the output format is JSON.

`-q, --quiet`
    : Suppresses verbose output.


### Command examples
{: #command-examples-vpn-connection-create}

- Create a VPN connection for a specific gateway ID with its required configuration values:

   ```sh
   ibmcloud is vpn-gateway-connection-create my-connection fee82deba12e4c0fb69c3b09d1f12345 169.21.50.5 lkj14b1oi0alcniejkso --local-cidr 10.240.0.0/24 --peer-cidr 192.168.1.0/24
   ```
   {: pre}

- Create a VPN connection for a route-based VPN gateway with the [distribute traffic feature](/docs/vpc?topic=vpc-using-vpn&interface=terraform#use-case-4-vpn) enabled:

   ```sh
   ibmcloud is vpn-gateway-connection-create CONNECTION_NAME VPN_GATEWAY PEER PRESHARED_KEY --distribute-traffic true
   ```
   {: pre}

- Create a VPN connection with the same core parameters and specified DPD configurations:

   ```sh
   ibmcloud is vpn-gateway-connection-create my-connection fee82deba12e4c0fb69c3b09d1f12345 169.21.50.5 lkj14b1oi0alcniejkso --local-cidr 10.240.0.0/24 --peer-cidr 192.168.1.0/24 --dead-peer-detection-action clear --dead-peer-detection-interval 33 --dead-peer-detection-timeout 100
   ```
   {: pre}

- Create a VPN connection with the same core parameters and custom policies with specified IDs:

   ```sh
   ibmcloud is vpn-gateway-connection-create my-connection fee82deba12e4c0fb69c3b09d1f12345 169.21.50.5 lkj14b1oi0alcniejkso --local-cidr 10.240.0.0/24 --peer-cidr 192.168.1.0/24 --ipsec-policy 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --ike-policy 72251a2e-d6c5-42b4-97b0-b5f8e8d1f480
   ```
   {: pre}

- Create a VPN connection with peer FQDN and specify the local and peer IKE identity:

   ```sh
   ibmcloud is vpn-gateway-connection-create my-connection fee82deba12e4c0fb69c3b09d1f12345 on-prem.my-company.com lkj14b1oi0alcniejkso --local-cidr 10.240.0.0/24 --peer-cidr 192.168.1.0/24 --local-ike-identities '[{"type":"key_id","value":"MTIzNA=="}]' --peer-ike-identity-type fqdn --peer-ike-identity-value on-prem.my-company.com --establish-mode peer_only
   ```
   {: pre}

- Create a VPN connection that allows the peer to initiate IKE protocol negotiations for this VPN gateway connection:

   ```sh
   ibmcloud is vpn-gateway-connection-create my-connection fee82deba12e4c0fb69c3b09d1f12345 169.21.50.5 lkj14b1oi0alcniejkso --establish-mode peer_only --local-ike-identities '[{type:ipv4_address,value:2.2.2.2},{type:fqdn,value:sadsadasd.com}]' --peer-ike-identity-type key_id --peer-ike-identity-value MTIzNA==
   ```
   {: pre}


- Create a VPN connection by using advanced configuration options:

   ```sh
   ibmcloud is vpn-gateway-connection-create to-prem ${gateway_id} on-prem.test.com test123 --local-cidr 10.10.20.0/28 --peer-cidr 192.168.0.0/24 --peer-ike-identity-type ipv4_address --peer-ike-identity-value 192.168.0.1 --establish-mode peer_only
   ```
   {: pre}

- Create a dynamic route-based VPN connection with peer ASN and tunnels:

   ```sh
   ibmcloud is vpn-gateway-connection-create my-connection --routing-protocol bgp my-vpc-gateway 169.21.50.5 lkj14b1oi0alcniejkso --distribute-traffic true --local-ike-identities '[{"type":"fqdn","value":"example.com"},{"type":"fqdn","value":"example_1.com"}]' --peer-asn 65534 --tunnels '[{"neighbor_ip":{"address":"192.168.0.2"},"tunnel_interface_ip":{"address":"192.168.0.1"}},{"neighbor_ip":{"address":"192.168.0.6"},"tunnel_interface_ip":{"address":"192.168.0.5"}}]' --peer-ike-identity-type ipv4_address --peer-ike-identity-value 192.168.0.1
   ```
   {: pre}

- Get a list of VPN gateway connections:

   ```sh
   ibmcloud is vpn-gateway-connection  my-vpc-gateway my-connection
   ```
   {: pre}

- List all service connections for a VPN gateway:

   ```sh
   ibmcloud is vpn-gateway-service-connections 0727-bc813695-d777-4ceb-acfb-bd68f4828358
   ```
   {: pre}

- Get the service connection details of a specifc VPN gateway:

   ```sh
   ibmcloud is vpn-gateway-service-connection my-vpc-gateway --service-connection-id 72fd9e00-3117-4b2e-984d-9361a9a97801
   ```
   {: pre}

## Adding a local CIDR to a VPN gateway connection from the CLI
{: #vpn-using-cli-vpn-gateway-connection-local-cidr-add}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To add a local CIDR to a VPN gateway connection from the CLI, enter the following command:

This command is only supported by policy mode VPN gateways.
   {: note}

```sh
ibmcloud is vpn-gateway-connection-local-cidr-add VPN_GATEWAY CONNECTION PREFIX_ADDRESS PREFIX_LENGTH [--vpc VPC] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

`VPN_GATEWAY`
    : The ID of the VPN gateway.

`CONNECTION`
    : The ID or name of the VPN connection.

`PREFIX_ADDRESS`
    : The prefix address part of the CIDR.

`PREFIX_LENGTH`
    : The prefix length part of the CIDR.

`--output value`
    : The output in JSON format.

`-q, --quiet`
    : Suppresses verbose output.



### Command example
{: #command-examples-for-vpn-gateway-connection-local-cidr-add}

Add a local CIDR for a specific connection name with required configuration values:

   ```sh
   ibmcloud is vpn-gateway-connection-local-cidr-add my-vpn-gateway my-connection 3.3.3.0/24
   ```
   {: pre}

## Adding a peer CIDR to a VPN gateway connection from the CLI
{: #vpn-using-cli-vpn-gateway-connection-peer-cidr-add}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To add a peer CIDR to a VPN gateway connection from the CLI, enter the following command:

This command is only supported by policy mode VPN gateway.
{: note}

```sh
ibmcloud is vpn-gateway-connection-peer-cidr-add VPN_GATEWAY CONNECTION PREFIX_ADDRESS PREFIX_LENGTH [--vpc VPC] [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

`VPN_GATEWAY`
    : ID of the VPN gateway.

`CONNECTION`
    : ID or name of the VPN connection.

`PREFIX_ADDRESS`
    : Prefix address part of the CIDR.

`PREFIX_LENGTH`
    : Prefix length part of the CIDR.

`--output value`
    : Output in JSON format.

`-q, --quiet`
    : Suppress verbose output.


### Command example
{: #command-examples-for-vpn-gateway-connection-peer-cidr-add}

Add a peer CIDR for a specific connection name with its required configuration values:

   ```sh
   ibmcloud is vpn-gateway-connection-peer-cidr-add my-vpn-gateway my-connection 4.4.4.0/24
   ```
   {: pre}

## Adding a connection with the API
{: #vpn-using-api-add-connection}
{: api}

To create a VPN connection with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   * `vpnGatewayId` - The VPN gateway identifier.

       ```sh
       export vpnGatewayId=<your_vpn_gateway_id>
       ```
       {: pre}

   * `ikePolicyId` - The unique identifier for this IKE policy.

      ```sh
      export ikePolicyId=<your_ike_policy_id>
      ```
      {: pre}

   * `ipsecPolicyId` - The unique identifier for this IPsec policy.

       ```sh
       export ipsecPolicyId=<your_ipsec_policy_id>
       ```
       {: pre}

1. When all variables are initiated, create the VPN gateway connection. For example,

   ```sh
      # To create a connection for policy-based VPN, use the following command:
      curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$vpnGatewayId/connections?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
            "name": "my-vpn-connection",
            "psk": "'$psk'",
            "dead_peer_detection": {
                "action": "restart",
                "interval": 2,
                "timeout": 10
            },
            "local": {
                "cidrs": "'$localCidrs'"
            },
            "peer": {
                "cidrs": "'$remoteCidrs'",
                "address": "7.8.9.10"
            }
            "ike_policy": {
                "id": "'$ikePolicyId'"
            },
            "ipsec_policy": {
                "id": "'$ipsecPolicyId'"
            }
        }'
   ```
   {: codeblock}

   ```sh
      # For a static route-based VPN connection, use the following command:
      curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$vpnGatewayId/connections?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
            "name": "my-vpn-connection",
            "routing_protocol": "none",
            "psk": "'$psk'",
            "distribute_traffic":true,
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

   ```sh
     # For a dynamic route-based VPN connection, use the following command:
    curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$vpnGatewayId/connections?version=$api_version&generation=2" \
        -H "Authorization: Bearer $iam_token" \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -H "X-Correlation-ID: 0ba2d683-cce9-404f-9b8e-463543888ff9" \
        -d '{
            "name": "vpn-new-connection",
            "psk": "'$psk'",
            "dead_peer_detection": {
                "action": "restart",
                "interval": 2,
                "timeout": 10
            },
            "distribute_traffic": true,
            "establish_mode": "bidirectional",
            "peer": {
                "address": "9.168.3.4",
                "asn": 64543
            },
            "routing_protocol": "bgp",
            "tunnels": [
            {
                "neighbor_ip": { "address": "192.168.0.2" },
                "tunnel_interface_ip": { "address": "192.168.0.1" }
            },
            {
                "neighbor_ip": { "address": "192.168.0.6" },
                "tunnel_interface_ip": { "address": "192.168.0.5" }
            }
            ]
        }'
   ```
   {: codeblock}

1. (Optional) To create a connection by using advanced configuration options:

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/vpn_gateways/$vpnGatewayId/connections?version=$api_version&generation=2"  \
         -H "Authorization: $iam_token"      -d '{  \
         "name": "my-advanced-vpn-connection",
         "establish_mode": "peer_only",
         "psk": "'$psk'",
         "dead_peer_detection": {
             "action": "restart",
             "interval": 2,
             "timeout": 10
         },
         "local": {
             "cidrs": "'$localCidrs'",
             "ike_identities": [
                 {
                     "type": "key_id",
                     "value": "dGVzdGtleQ=="
                 }
             ]
         },
         "peer": {
             "cidrs": "'$remoteCidrs'",
             "ike_identity": {
                 "type": "hostname",
                 "value": "cisco-asa"
             },
             "fqdn": "on-prem.test.com"
         }
         "ike_policy": {
             "id": "'$ikePolicyId'"
         },
         "ipsec_policy": {
             "id": "'$ipsecPolicyId'"
         },
         "distribute_traffic":true
     }'
   ```
   {: codeblock}

## Adding a local CIDR to a VPN gateway connection with the API
{: #vpn-using-api-vpn-gateway-connection-local-cidr-add}
{: api}

To add a local CIDR to a VPN gateway connection with the API, follow these steps:

This API is only supported by policy mode VPN gateways.
{: note}

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

1. Store any additional variables to be used in the API commands, for example:

   * `vpnGatewayId` - The VPN gateway identifier.

       ```sh
       export vpnGatewayId=<your_vpn_gateway_id>
       ```
       {: pre}

   * `connectionId` - The unique identifier for this VPN connection.

      ```sh
      export connectionId=<your_connection_id>
      ```
      {: pre}

   * `cidr_prefix` - The prefix address part of the CIDR.

      ```sh
      export cidr_prefix=<your_cidr_prefix>
      ```
      {: pre}

   * `prefix_length` - The prefix length part of the CIDR.

      ```sh
      export prefix_length=<your_prefix_length>
      ```
      {: pre}


1. When all variables are initiated, add a local CIDR to a VPN gateway connection. For example,

   ```sh
      curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpnGatewayId/connections/$connectionId/local_cidrs/${cidr_prefix}/${prefix_length}?version=$api_version&generation=2" \
        -H "Authorization: $iam_token"
   ```
   {: codeblock}

## Adding a peer CIDR to a VPN gateway connection with the API
{: #vpn-using-api-vpn-gateway-connection-peer-cidr-add}
{: api}

To add a peer CIDR to a VPN gateway connection with the API, follow these steps:

This API is only supported by policy mode VPN gateways.
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


1. When all variables are initiated, add a peer CIDR to a VPN gateway connection. For example,

   ```sh
      curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways/$vpnGatewayId/connections/$connectionId/peer_cidrs/${cidr_prefix}/${prefix_length}?version=$api_version&generation=2" \
        -H "Authorization: $iam_token"
   ```
   {: codeblock}

## Adding a connection by using Terraform
{: #vpn-using-cli-add-terraform}
{: terraform}

To add a connection by using Terraform, run the following command:

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

The following Terraform example creates a VPN gateway connection:

```terraform
resource "ibm_is_vpn_gateway_connection" "is_vpn_gateway_connection" {
  name           = "my-vpn-gateway-connection"
  vpn_gateway    = ibm_is_vpn_gateway.is_vpn_gateway.id
  preshared_key  = "VPNDemoPassword"
  establish_mode = "bidirectional"
  peer {
    cidrs   = [var.peerCIDR]
    address = "7.8.9.10"
  }
  local {
    cidrs = [var.localCIDR]
  }
  ike_policy   = ibm_is_ike_policy.is_ike_policy.id
  ipsec_policy = ibm_is_ipsec_policy.is_ipsec_policy.id
}
```
{: codeblock}

The following Terraform example creates a VPN connection for a route-based VPN gateway with the [distribute traffic feature](/docs/vpc?topic=vpc-using-vpn&interface=terraform#use-case-4-vpn) enabled:

```terraform

resource "ibm_is_vpn_gateway_connection" "test_VPNGatewayConnection1" {
    name = "example-vpn-gateway-connection"
    vpn_gateway = "${ibm_is_vpn_gateway.example.id}"
    peer_address = "${ibm_is_vpn_gateway.example.public_ip_address}"
    preshared_key = "VPNDemoPassword"
    distribute-traffic = true
}
```
{: codeblock}

The following Terraform example creates a VPN connection by using advanced configuration options:

```terraform
resource "ibm_is_vpn_gateway_connection" "is_vpn_gateway_connection" {
  name           = "to-prem"
  vpn_gateway    = ibm_is_vpn_gateway.is_vpn_gateway.id
  preshared_key  = "test123"
  establish_mode = "peer_only"
  peer {
    cidrs   = ["192.168.0.0/24"]
    ike_identity {
      type  = "ipv4_address"
      value = "192.168.0.1"
    }
    fqdn = "on-prem.test.com"
  }
  local {
    cidrs = ["10.10.20.0/28"]
  }
}
```
{: codeblock}




For more information, see the [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpn_gateway_connection){: external}.

## Next steps
{: #vpn-add-connection-next-steps}

To create a route-based VPN, first [create a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table), then [create a route using the VPN connection type](/docs/vpc?topic=vpc-create-vpc-route).
For a dynamic route-based VPN connection, attach a transit gateway to the VPN gateway. See [Creating a transit gateway](/docs/transit-gateway?topic=transit-gateway-ordering-transit-gateway&interface=ui).
