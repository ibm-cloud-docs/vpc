---

copyright:
  years: 2019, 2025
lastupdated: "2025-11-19"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to your on-premises network
{: #vpn-onprem-example}

You can use IBM Cloud VPN for VPC to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your VPN gateway to connect to your on-premises network.
{: shortdesc}

Create a VPN gateway in your VPC and create a VPN connection between the VPC and the peer gateway of the on-prem network by specifying the following information.

* **Connection name** - Enter a name for the connection, such as `onprem-connection`.
* **Peer gateway address** - Specify the IP address of the VPN gateway for the on-prem network.
* **Preshared key** - Specify the authentication key of the VPN gateway for the on-prem network.
* **Local subnets (Policy-based VPN only)** - Specify one or more subnets in the VPC that you want to connect through the VPN tunnel.
* **Peer subnets (Policy-based VPN only)** - Specify one or more subnets in the on-premises network that you want to connect through the VPN tunnel.
* **Peer ASN (Dynamic route-based VPN connection only)** - Specify the peer ASN which identifies the remote peer network with which the VPN exchanges routes.
* **Tunnel interface IP (Dynamic route-based VPN connection only)** - Specify the IP address that is assigned to the VPN gateway side of the VPN tunnel.
* **Peer interface IP (Dynamic route-based VPN connection only)** - Specify the IP address that is assigned to the remote network side of the VPN tunnel. This address is your on-premises device or peer VPN gateway.

For the Internet Key Exchange (IKE) and IPsec security parameters, select **Auto** so that the cloud gateway uses auto-negotiation to automatically establish the connection with the on-premises gateway.

The gateway status appears as **Pending** while the VPN gateway is being created, and the status changes to **Available** after it is created.
{: tip}

## Configuring the on-premises VPN gateway
{: #configuring-onprem-gateway}

The next step is configuring your on-premises VPN gateway peer to connect to your IBM Cloud VPN for VPC. The configuration depends on the type of VPN gateway. See the following topics for details.

* [Connecting to an AWS peer](/docs/vpc?topic=vpc-aws-config)
* [Connecting to a Check Point Security Gateway peer](/docs/vpc?topic=vpc-check-point-config)
* [Connecting to a Cisco ASAv peer](/docs/vpc?topic=vpc-cisco-asav-config)
* [Connecting to a FortiGate peer](/docs/vpc?topic=vpc-fortigate-config)
* [Connecting to a Juniper vSRX peer](/docs/vpc?topic=vpc-juniper-vsrx-config)
* [Connecting to a strongSwan peer](/docs/vpc?topic=vpc-strongswan-config)
* [Connecting to a Vyatta peer](/docs/vpc?topic=vpc-vyatta-config)

These configurations are fully tested and supported by IBM. If you plan to use an on-premises VPN gateway peer other than those listed, IBM Support can help troubleshoot your configuration, but cannot guarantee a resolution.
{: note}

## Checking the status of the secure connection
{: #check-connection-status}

You can check the status of your connection in the {{site.data.keyword.cloud_notm}} console. On the {{site.data.keyword.vpn_vpc_short}} page, select your VPN gateway and click **Connections** from the navigation pane on the left of the page.

You can also test the connection by doing a ping from a virtual server instance in your VPC to a server in the on-premises network.
