---

copyright:
  years: 2019, 2023
lastupdated: "2023-12-18"

keywords:
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a VPN gateway
{: #vpn-create-gateway}

You can create an IBM Cloud VPN for VPC to securely connect your VPC to another private network, such as an on-premises network or another VPC.
{: shortdesc}

## Planning considerations for VPN gateways
{: #planning-considerations-vpn}

Review the following considerations before creating a VPN gateway:

* The VPN gateway is created in the zone that is associated with the subnet that you select. Because the VPN gateway can connect to virtual server instances in this zone only, instances in other zones can't use this VPN gateway to communicate with the other network. For zone fault tolerance, deploy one VPN gateway per zone.
* Make sure that there's enough space in the subnet for the gateway. To ensure VPN management and fail-over functions are able to function properly, create the VPN gateway in a subnet without any other VPC resources to guarantee that there are enough available private IPs for the gateway. A VPN gateway needs four private IP addresses to accommodate high availability and rolling upgrades. Since up to five private IPs in a subnet are reserved, the minimum subnet size that can be used to host a VPN gateway is 16 IPs (prefix `/28` or netmask `255.255.255.240`).
* If you plan to set a default route (`0.0.0.0/0`) in a VPC routing table to let egress traffic from your VPC resources pass through a VPN gateway, and you plan to use a route-based VPN, create your VPN gateway in a subnet different from the one associated with the routing table. Otherwise, this default route causes a routing conflict for the VPN gateway and brings the VPN connection down.
* By default, PFS (Perfect Forward Secrecy) is disabled for IBM Cloud VPN for VPC. Some vendors require PFS enablement for Phase 2. Check your vendor instruction and use custom policies if PFS is required.
* IBM Cloud VPN for VPC supports only one route-based VPN per zone per VPC.

    The IBM VPN gateway uses its public IP address as the IKE local identify and designates the peer's public IP address as the IKE peer identify. In cases where the peer VPN gateway is located behind a NAT firewall, and the peer's public IP address is not associated with the peer VPN gateway interface, it is imperative to adjust the configuration of the peer VPN gateway to ensure that the peer's public IP address is used as the IKE identify.
    {: note}

## Creating a VPN gateway in the UI
{: #vpn-create-ui}
{: ui}

To create a VPN gateway using the UI:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** icon![menu icon](../../icons/icon_hamburger.svg), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>VPNs** in the Network section.

    If starting from the VPC Infrastructure menu, click **VPNs** in the Network section to open the {{site.data.keyword.vpn_vpc_short}} page.
    {: tip}

1. Ensure that the **Site-to-site gateways > VPN gateways** tabs are selected.
1. On the VPNs for VPC page, click **Create** and then select the **Site-to-site** gateway tile.
1. Specify the following information:
   * **VPN gateway name** - Enter a name for the VPN gateway, such as `my-vpn-gateway`.
   * **Resource group** - Select a resource group for the VPN gateway.
   * **Tags** - Optionally, add tags to identify this VPN gateway.
   * **Access management tags** - Optionally, add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Region** - Shows the region where the VPC is located and where the VPN gateway will be provisioned.
   * **Virtual Private Cloud** - Select the VPC for the VPN gateway.
   * **Subnet** - Select the subnet in which to create the VPN gateway. See [Planning considerations](/docs/vpc?topic=vpc-vpn-create-gateway#planning-considerations-vpn) for important subnet information.
   * **Mode** - Select to create a policy-based or a route-based VPN. For more information about VPN types, see [VPN features](/docs/vpc?topic=vpc-using-vpn#vpn-features).
1. In the **New VPN connection for VPC** section, you can define a connection between this gateway and a network outside your VPC by specifying the following information. You can create the VPN connection now, or after your VPN gateway is provisioned.

    * **VPN connection name** - Enter a name for the connection, such as `my-connection`.
    * **Peer gateway address** - Specify the IP address of the VPN gateway for the network outside your VPC.
    * **Preshared key** - Specify the authentication key of the VPN gateway for the network outside your VPC. The preshared key is a string of hexadecimal digits, or a passphrase of printable ASCII characters. To be compatible with most peer gateway types, this string must follow these rules:
        * Can be a combination of digits, lower or upper case characters, or the following special characters: `- + & ! @ # $ % ^ * ( ) . , :`
        * The length of the string must be 6 - 128 characters.
        * Cannot start with `0x` or `0s`.

    * **Local subnets (Policy-based VPN only)** - Specify one or more subnets in the VPC that you want to connect through the VPN tunnel.
    * **Peer subnets (Policy-based VPN only)** - Specify one or more subnets in the other network that you want to connect through the VPN tunnel.

        Subnet range overlap between local and peer subnets is not allowed.
        {: important}

1. In the Dead peer detection section, configure how the VPN gateway sends messages to check that the peer gateway is active. Specify the following information:

    * **Action** - The dead peer detection action to take if a peer gateway stops responding. For example, select **Restart** if you want the gateway to immediately renegotiate the connection.
    * **Interval (sec)** - How often to check that the peer gateway is active. By default, messages are sent every 2 seconds.
    * **Timeout (sec)** - How long to wait for a response from the peer gateway. By default, a peer gateway is no longer considered active if a response isn't received within 10 seconds.
1. In the **Policies** section, specify the Internet Key Exchange (IKE) and Internet Protocol Security (IPsec) options to use for Phase 1 and Phase 2 negotiation of the connection.
    * Select **Auto** if you want the gateway to try to automatically establish the connection.
    * Select or create custom policies if you need to enforce particular security requirements, or if the VPN gateway for the other network doesn't support the security proposals that are tried by auto-negotiation.

   The IKE and IPsec security options that you specify for the connection must be the same options that are set on the peer gateway for the network outside your VPC.
   {: important}

## Creating a VPN gateway from the CLI
{: #vpn-create-cli}
{: cli}

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

To create a VPN gateway from the CLI, enter the following command:

```sh
ibmcloud is vpn-gateway-create VPN_GATEWAY_NAME SUBNET
    [--mode policy | route]
    [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
    [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- **VPN_GATEWAY_NAME** - Name of the VPN gateway.
- **SUBNET** - ID of the subnet.
- **--mode** - The mode of the VPN gateway. One of: **policy**, **route**.
- **--resource-group-id** - ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
- **--resource-group-name** - Name of the resource group. This option is mutually exclusive with **--resource-group-id**.
- **--output** - Specify output in JSON format.
- **-q, --quiet** - Suppress verbose output.

### Command examples
{: #cli-cmd-examples-vpn-gateway-create}

- Create a route-based VPN gateway with a specific subnet ID:
   `ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345 --mode route`
- Create a policy-based VPN gateway using the Default resource group:
   `ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345 --mode policy --resource-group-name Default`
- Create a route-based VPN gateway, using a specific resource group ID with output in JSON format:
   `ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345 --mode route --resource-group-id fee82deba12e4c0fb69c3b09d1f12345 --output JSON`

## Creating a VPN gateway with the API
{: #vpn-create-api}
{: api}

To create a policy-based IBM Cloud VPN for VPC with the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.
1. Store any additional variables to be used in the API commands; for example:

   * `ResourceGroupId` - Find the resource group ID by using the **get resource groups** command and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

   * `SubnetId` - Find the subnet ID by using the **get subnet** command and then populate the variable:

    ```sh
    export SubnetId=<your_subnet_id>
    ```
    {: pre}

1. When all variables are initiated, create the VPN gateway:

   ```sh
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

See the [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_vpn_gateway){: external} for more information.

## Next steps
{: #vpn-create-next-steps}

After you create a VPN gateway, you can:

* [Create an IKE policy](/docs/vpc?topic=vpc-creating-ike-policy) if you decide to use custom IKE policy instead of auto-negotiation.
* [Create an IPsec policy](/docs/vpc?topic=vpc-creating-ipsec-policy) if you decide to use custom IPsec policy instead of auto-negotiation.
* Create a VPN connection if you have not already done so when creating your VPN gateway. If you did not create the VPN connection, you can do so after the VPN gateway is provisioned. For more information, see [Adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections).
* For a route-based VPN, select or [create a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table). Then, [create a route using the VPN connection type](/docs/vpc?topic=vpc-create-vpc-route).
