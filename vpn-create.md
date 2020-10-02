---

copyright:
  years: 2019, 2020
lastupdated: "2020-10-02"

keywords: vpn for vpc, vpn, connection, on premise connection, on-premises connection, create

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:important: .important}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}
{:external: target="_blank" .external}

# Creating a VPN gateway
{: #vpn-create-gateway}

You can create a virtual private network (VPN) gateway to securely connect your VPC to another private network, such as an on-premises network or another VPC.
{: shortdesc}

## Creating a VPN gateway using the UI
{: #vpn-create-ui}

To create a VPN gateway using the UI:
1. Open [{{site.data.keyword.cloud_notm}} console](https://{DomainName}){: external}.
1. Navigate to the VPN for VPC page by clicking **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > VPNs** under the Network section.

    If starting from the VPC Infrastructure menu, click on **VPN gateways** under the Network section to open the VPN for VPC page.
    {: tip}
1. On the VPN for VPC page, click **Create** and specify the following information:
    * **Name**: Enter a name for the VPN gateway, such as `my-vpn-gateway`.
    * **Virtual private cloud**: Select the VPC.
    * **Resource group**: Select a resource group for the VPN gateway.
    * **Subnet**: Select the subnet in which to create the VPN gateway.

      To ensure VPN management and fail-over functions are able to function properly, create the VPN gateway in a subnet without any other VPC resources to guarantee that there are enough available private IPs for the gateway. A VPN gateway needs 4 private IP addresses to accommodate high availability and rolling upgrades. Since up to 5 private IPs in a subnet are reserved, the minimum subnet size that can be used to host a VPN gateway is 16 IPs (prefix /28` or netmask `255.255.255.240`).

      The VPN gateway is created in the zone that is associated with the subnet you select. Because the VPN gateway can connect to virtual server instances in this zone only, instances in other zones can't use this VPN gateway to communicate with the other network. For zone fault tolerance, deploy one VPN gateway per zone.
      {: important}

1. Optionally, in the **New VPN connection for VPC** section, define a connection between this gateway and a network outside your VPC by specifying the following information.
    * **Connection name**: Enter a name for the connection, such as `my-connection`.
    * **Peer gateway address**: Specify the IP address of the VPN gateway for the network outside your VPC.
    * **Preshared key**: Specify the authentication key of the VPN gateway for the network outside your VPC.
    * **Local subnets**: Specify one or more subnets in the VPC you want to connect through the VPN tunnel.
    * **Peer subnets**: Specify one or more subnets in the other network you want to connect through the VPN tunnel.
1. To configure how the VPN gateway sends messages to check that the peer gateway is active, specify the following information in the **Dead peer detection** section.
    * **Dead peer detection action**: The action to take if a peer gateway stops responding. For example, select **Restart** if you want the gateway to immediately renegotiate the connection.
    * **Interval**: How often to check that the peer gateway is active. By default, messages are sent every 2 seconds.
    * **Timeout**: How long to wait for a response from the peer gateway. By default, a peer gateway is no longer considered active if a response isn't received within 10 seconds.
1. In the **Policies** section, specify the Internet Key Exchange (IKE) and Internet Protocol Security (IPSec) options to use for phase 1 and phase 2 negotiation of the connection.
    * Select **Auto** if you want the gateway to try to automatically establish the connection.
    * Select or create custom policies if you need to enforce particular security requirements, or the VPN gateway for the other network doesn't support the security proposals that are tried by auto-negotiation.

  The IKE and IPsec security options that you specify for the connection must be the same options that are set on the peer gateway for the network outside your VPC.
  {: important}

## Creating a VPN gateway using the CLI
{: #vpn-create-cli}

```
ibmcloud is vpn-gateway-create VPN_GATEWAY_NAME SUBNET \
[--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] \
[--output JSON] [-q, --quiet]
```
{: pre}

### Command examples
{: #command-examples-vpn-gateway-create}

- `ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345`
- `ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345 --resource-group-name Default`
- `ibmcloud is vpn-gateway-create my-vpc-gateway fee82deba12e4c0fb69c3b09d1f12345 --resource-group-id fee82deba12e4c0fb69c3b09d1f12345 --output JSON`

### Command options
{: #command-options-vpn-gateway-create}

- **VPN_GATEWAY_NAME**: Name of the VPN gateway.
- **SUBNET**: ID of the subnet.
- **--resource-group-id**: ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
- **--resource-group-name**: Name of the resource group. This option is mutually exclusive with **--resource-group-id**.
- **--output**: Specify output format, only JSON is supported now. One of: **JSON**.
- **-q, --quiet**: Suppress verbose output.
