---

copyright:
  years: 2019, 2025
lastupdated: "2025-11-19"

keywords:
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a VPN gateway
{: #vpn-create-gateway}

You can create an IBM Cloud VPN for VPC to securely connect your VPC to another private network, such as an on-premises network or another VPC.
{: shortdesc}

Before you begin, review [Planning considerations for VPN gateways](/docs/vpc?topic=vpc-planning-considerations-vpn) and [VPN gateway known limitations, issues, and restrictions](/docs/vpc?topic=vpc-vpn-limitations&interface=ui).
{: important}

## Creating a VPN gateway in the console
{: #vpn-create-ui}
{: ui}

To create a VPN gateway in the console:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPNs**.
1. Ensure that you select the **Site-to-site gateways > VPN gateways** tabs.
1. On the VPNs for VPC page, click **Create** and then select the **Site-to-site** gateway tile.
1. Specify the following information:
   * **Region** - Select the region where the VPN gateway is going to be provisioned.
   * **VPN gateway name** - Enter a name for the VPN gateway, such as `my-vpn-gateway`.
   * **Resource group** - Select a resource group for the VPN gateway.
   * **Tags** - Optionally, add tags to identify this VPN gateway.
   * **Access management tags** - Optionally, add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Virtual Private Cloud** - Select the VPC for the VPN gateway.
   * **Subnet** - Select the subnet where you want to create the VPN gateway. See [Planning considerations](/docs/vpc?topic=vpc-planning-considerations-vpn) for important subnet information.
   * **Mode** - Select either a policy-based or route-based VPN. For more information about VPN types, see [VPN features](/docs/vpc?topic=vpc-using-vpn#vpn-features).
   * **ASN** - It is the numeric identifier for the VPN gateway. This value identifies your local network for BGP peering and is used during the BGP session setup with your on-premises device.
   * **Advertised CIDRs (optional)** - The CIDR range values that advertise the IPv4 network ranges to the remote VPN peers.

      The ASN value is required for dynamic routing. If you don't specify an ASN, the VPN gateway is created with the default ASN of `64520`. You can't change the local ASN value after you connect the VPN gateway to the transit gateway.
      {: note}

1. In the **VPN connection for VPC** section, toggle the switch on to establish connectivity between this gateway and the network outside your VPN. You can also add a VPN connection after you provision the gateway. In the Connection details section, specify the following information:

    * **VPN connection name** - Enter a name for the connection, such as `my-connection`.
    * **Connection type** - Select static or dynamic connection type.
    * **Peer gateway address** - Specify the peer device through a public IP address or FQDN of the VPN gateway for the network outside your VPC.
    * **Peer ASN (Dynamic route-based only)** - If you select **Dynamic** as the connection type, you must specify the peer ASN. This value identifies the remote peer network with which the VPN exchanges routes.

       Certain ASN values are restricted and can't be used as peer ASNs, which include `0`, `13884`, `36351`, `64512`, `64513`, `65100`, `65200–‍65234`, `65402‍–‍65433`, `65500`, or `4201065000‍–‍4201065999`. These values are either reserved or part of private ASN ranges, and can cause routing conflicts.
       {: note}

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
    * **Local subnets (Policy-based VPN only)** - Specify one or more subnets in the VPC that you want to connect through the VPN tunnel.
    * **Peer subnets (Policy-based VPN only)** - Specify one or more subnets in the peer network that you want to connect through the VPN tunnel.

        Subnet range overlap between local and peer subnets is not allowed.
        {: important}

1. In the **Tunnel details** section (Dynamic route-based only), specify the tunnel and peer interface IPs required for dynamic routing with BGP.
    * **Tunnel interface IP** - Specify the IP address that is assigned to the VPN gateway side of the VPN tunnel.
    * **Peer interface IP** - Specify the IP address that is assigned to the remote network side of the VPN tunnel. This address is your on-premises device or peer VPN gateway.

        The tunnel and peer interface IPs must be continuous and belong to the same `/30` subnet. This subnet provides four IP addresses, but the first (network address) and last (broadcast address) are reserved. For example, in the subnet `192.168.0.0/30`, usable IPs are `192.168.0.1` and `192.168.0.2`. If you assign `192.168.0.1` to the tunnel 1 interface, then you must assign `192.168.0.2` to peer 1 interface. Similarly, for Tunnel 2, use a different `/30` subnet, such as `192.168.0.4/30`. In this case, assign `192.168.0.5` to the tunnel 2 interface and `192.168.0.6` to the peer 2 interface. Also, the tunnel IPs used for setting up IPsec connections can't overlap with the CIDR range that is chosen for Transit Gateway connection.
        {: note}

1. In the **Dead peer detection** section, configure how the VPN gateway sends messages to check that the peer gateway is active. Specify the following information:

    * **Action** - The dead peer detection action to take if a peer gateway stops responding. For example, select **Restart** if you want the gateway to immediately renegotiate the connection.
    * **Interval (sec)** - How often to check that the peer gateway is active. By default, messages are sent every 2 seconds.
    * **Timeout (sec)** - How long to wait for a response from the peer gateway. By default, a peer gateway is no longer considered active if a response isn't received within 10 seconds.
1. In the **Policies** section, specify the Internet Key Exchange (IKE) and Internet Protocol Security (IPsec) options to use for Phase 1 and Phase 2 negotiation of the connection.
    * Select **Auto** if you want the gateway to try to automatically establish the connection.
    * Select or create custom policies if:
      * You need to enforce particular security requirements.
      * The VPN gateway on the other network doesn't support the security proposals that are automatically negotiated during setup.

   The IKE and IPsec security options that you specify for the connection must exactly match the ones that are set on the peer gateway for the network outside your VPC.
   {: important} {: #ike-identities}

1. In the Advanced options section, you can customize local and peer IKE identities instead of using the default IKE identity. One peer IKE identity can be specified at most.

   For policy-based VPN gateways, you can configure one local IKE identity at most. For route-based VPN gateways, if you want to configure a local IKE identity, you must provide two. You can provide values for the members or leave the input fields empty.
   {: note}

   * **Local IKE identities** - Select a type for the local IKE identity, then enter its value. For example, you can enter a single 4 octet IPv4 address (`9.168.3.4`), an FQDN (`my-vpn.example.com`), hostname (`my-host`), or base64-encoded key ID (`MTIzNA==`).

      * Static route mode consists of two members in active-active mode, where the first identity applies to the first member and the second identity applies to the second member. If you do not specify local IKE identities, then the type is an IPv4 address, and the value is the public IP address of the member’s VPN connection tunnel.

      * Policy mode consists of two members in active-standby mode. The local IKE identity applies to the active member. If you do not specify a value, then the local IKE identity is the public IP address of the VPN gateway.

   * **Peer IKE identity** - Select a type for the peer IKE identity, then enter its value. For example, you can enter an IPv4 address (`9.168.3.4`), an FQDN (`my-vpn.example.com`), hostname (`my-host`), or base64-encoded key ID (`MTIzNA==`).

      The peer IKE identity applies to the active member. If you do not specify a value, then use the peer gateway's IPv4 address or FQDN.

## Creating a VPN gateway from the CLI
{: #vpn-create-cli}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a VPN gateway from the CLI, enter the following command:

```sh
ibmcloud is vpn-gateway-create VPN_GATEWAY_NAME SUBNET
    [--mode policy | route]
    [--advertised-cidrs IPv4_NETWORK_RANGE]
    [--local-asn ASN_NUMBER]
    [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
    [--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

`VPN_GATEWAY_NAME`
    : The name of the VPN gateway.

`SUBNET`
    : The ID of the subnet.

`--mode`
    : The mode of the VPN gateway. One of: `policy`, `route`.

`--advertised-cidrs`
    : IPv4 network range to advertise the address to the remote VPN peer.

`--local-asn`
    : Identifies your local network for BGP peering.

`--resource-group-id`
    : The ID of the resource group. This option is mutually exclusive with `--resource-group-name`.

`--resource-group-name`
    : The name of the resource group. This option is mutually exclusive with `--resource-group-id`.

`--output`
    : Output in JSON format.

`-q, --quiet`
    : An option that suppresses verbose output.



### Command examples
{: #cli-cmd-examples-vpn-gateway-create}

- Create a route-based VPN gateway with a specific subnet ID:

   ```sh
   ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345 --mode route
   ```
   {: pre}

- Create a policy-based VPN gateway by using the Default resource group:

   ```sh
   ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345 --mode policy --resource-group-name Default
   ```
   {: pre}

- Create a route-based VPN gateway, by using a specific resource group ID with output in JSON format:

   ```sh
   ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345 --mode route --resource-group-id fee82deba12e4c0fb69c3b09d1f12345 --output JSON
   ```
   {: pre}

- Create a route-based VPN gateway for dynamic connection with advertised CIDRs and local ASN:

   ```sh
   ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345 --mode route --advertised-cidrs 192.168.3.0/24 --local-asn 64520
   ```
   {: pre}

- List all VPN gateways:

   ```sh
   ibmcloud is vpn-gateways
   ic vpn-gateways
   ```
   {: pre}

- Get details of a specifc VPN gateway:

   ```sh
   ic vpn-gateway my-vpc-gateway
   ```
   {: pre}

- Update VPN gateway with local ASN:

   ```sh
   ic vpn-gateway-update my-vpc-gateway --name my-vpc-gateway --local-asn 64521
   ```
   

- List advertised CIDRs for a VPN gateway:

   ```sh
   ibmcloud is vpn-gateway-advertised-cidrs my-vpc-gateway
   ```
   {: pre}

- Check if the specified advertised CIDR exists on the VPN gateway:

   ```sh
   ibmcloud is vpn-gateway-advertised-cidr  my-vpc-gateway --cidr 10.45.0.0/26
   ```
   {: pre}

- Add an advertised CIDR to the VPN gateway:

   ```sh
   ibmcloud is vpn-gateway-advertised-cidr-add my-vpc-gateway --cidr 10.45.0.0/26
   ```
   {: pre}

- Delete an advertised CIDR from the VPN gateway:

   ```sh
   ibmcloud is vpn-gateway-advertised-cidr-delete my-vpc-gateway 10.45.0.0/26
   ```
   {: pre}

- List all service connections for a VPN gateway:

   ```sh
   ibmcloud is vpn-gateway-service-connections my-vpc-gateway
   ```
   {: pre}

- Get the service connection details of a specifc VPN gateway:

   ```sh
   ibmcloud is vpn-gateway-service-connection my-vpc-gateway --service-connection-id 72fd9e00-3117-4b2e-984d-9361a9a97801
   ```
   {: pre}

## Creating a VPN gateway with the API
{: #vpn-create-api}
{: api}

To create a VPN gateway with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.
1. Store any additional variables to be used in the API commands. For example,

   * `ResourceGroupId` - Find the resource group ID by using the `get resource groups` command and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

   * `SubnetId` - Find the subnet ID by using the `get subnet` command and then populate the variable:

    ```sh
    export SubnetId=<your_subnet_id>
    ```
    {: pre}

1. When all variables are initiated, create the VPN gateway:

   ```sh
      # To create VPN gateway for policy-based VPN, use the following command:
      curl -X POST "$vpc_api_endpoint/v1/vpn_gateways?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
           "name": "my-new-vpn-gateway",
           "mode": "policy",
           "subnet": {
            "id": "'$SubnetId'"
            },
           "resource_group": {
             "id": "'$ResourceGroupId'"
           }
         }'
   ```
   {: codeblock}

   ```sh
      # To create VPN gateway for static route-based VPN connection, use the following command:
      curl -X POST "$vpc_api_endpoint/v1/vpn_gateways?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{
           "name": "my-new-vpn-gateway",
           "mode": "route",
           "subnet": {
            "id": "'$SubnetId'"
            },
           "resource_group": {
             "id": "'$ResourceGroupId'"
           }
         }'
   ```
   {: codeblock}

   ```sh
      # To create VPN gateway for dynamic route-based VPN connection, use the following command:
      curl -X POST "$vpc_api_endpoint/v1/vpn_gateways?version=$api_version&generation=2" \
        -H "Authorization: Bearer $iam_token" \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -H "X-Correlation-ID: 9852c2ff-c20a-4082-98da-cd689bcd5a11" \
        -d '{
            "name": "my-new-vpn-gateway",
            "subnet": {
            "id": "'"$SubnetId"'"
            },
            "mode": "route",
            "local_asn": 64520,
            "advertised_cidrs": [
            "192.168.0.0/24"
            ],
            "resource_group": {
            "id": "'"$ResourceGroupId"'"
            }
        }'
   ```
   {: codeblock}

   ```sh
      # To list all the advertised CIDRs for a VPN gateway, use the following command:
      curl -X GET "$vpc_api_endpoint/v1/vpn_gateways?version=$api_version&generation=2/$vpn_gateway_id/advertised_cidrs/$cidr?__QUERY__" \
        -H "Authorization: Bearer $iam_token"
   ```
   {: codeblock}

   ```sh
      # To remove an advertised CIDR from a VPN gateway, use the following command:
      curl -X DELETE "$vpc_api_endpoint/v1/vpn_gateways?version=$api_version&generation=2/$vpn_gateway_id/advertised_cidrs/$cidr?__QUERY__" \
        -H "Authorization: Bearer $iam_token" \
   ```
   {: codeblock}

   ```sh
      # To set an advertised CIDR on a VPN gateway, use the following command:
      curl -X PUT "$vpc_api_endpoint/v1/vpn_gateways?version=$api_version&generation=2/$vpn_gateway_id/advertised_cidrs/$cidr?__QUERY__" \
        -H "Authorization: Bearer $iam_token"
   ```
   {: codeblock}

## Creating a VPN gateway with Terraform
{: #vpn-create-terraform}
{: terraform}

The following example creates a VPN gateway by using Terraform:

```terraform
   resource "ibm_is_vpn_gateway" "is_vpn_gateway" {
   name = "my-vpn-gateway"
   subnet = ibm_is_subnet.is_subnet.id
   mode = "route"
   }
```
{: codeblock}

For more information, see the [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpn_gateway){: external}.

## Next steps
{: #vpn-create-next-steps}

After you create a VPN gateway, you can:

* [Create an IKE policy](/docs/vpc?topic=vpc-creating-ike-policy) if you decide to use a custom IKE policy instead of auto-negotiation.
* [Create an IPsec policy](/docs/vpc?topic=vpc-creating-ipsec-policy) if you decide to use a custom IPsec policy instead of auto-negotiation.
* Create a VPN connection if you haven't already created one when provisioning your VPN gateway. For more information, see [Adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections).
* [Configure route-propagation](/docs/vpc?topic=vpc-advertise-routes-s2s) for VPN gateways in policy-based mode.
* To create a route-based VPN, first [create a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table), then [create a route by using the VPN connection type](/docs/vpc?topic=vpc-create-vpc-route).
* For a dynamic route-based VPN connection, attach a transit gateway to the VPN gateway. See [Creating a transit gateway](/docs/transit-gateway?topic=transit-gateway-ordering-transit-gateway&interface=ui).
