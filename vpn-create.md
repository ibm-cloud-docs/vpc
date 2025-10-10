---

copyright:
  years: 2019, 2025
lastupdated: "2025-10-10"

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
   * **VPN gateway name** - Enter a name for the VPN gateway, such as `my-vpn-gateway`.
   * **Resource group** - Select a resource group for the VPN gateway.
   * **Tags** - Optionally, add tags to identify this VPN gateway.
   * **Access management tags** - Optionally, add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Region** - Shows the region where the VPC is located and where the VPN gateway is going to be provisioned.
   * **Virtual Private Cloud** - Select the VPC for the VPN gateway.
   * **Subnet** - Select the subnet where you want to create the VPN gateway. See [Planning considerations](/docs/vpc?topic=vpc-planning-considerations-vpn) for important subnet information.

   
   * **Mode** - Select either a policy-based or route-based VPN. For more information about VPN types, see [VPN features](/docs/vpc?topic=vpc-using-vpn#vpn-features).
1. In the **VPN connection for VPC** section, toggle the switch on to establish connectivity between this gateway and the network outside your VPN. You can also add a VPN connection after you provision the gateway. In the Connection details section, specify the following information:

    * **VPN connection name** - Enter a name for the connection, such as `my-connection`.
    * **Peer gateway address** - Specify the peer device through a public IP address or FQDN of the VPN gateway for the network outside your VPC.
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
    * **Peer subnets (Policy-based VPN only)** - Specify one or more subnets in the other network that you want to connect through the VPN tunnel.

        Subnet range overlap between local and peer subnets is not allowed.
        {: important}



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
