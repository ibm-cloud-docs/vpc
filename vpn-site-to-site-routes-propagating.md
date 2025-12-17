---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-17"

keywords: network, VPN, VPN gateways, encryption

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring route propagation for VPN gateways
{: #advertise-routes-s2s}

The VPC routing table attribute ***Accepts routes from*** controls whether a routing table accepts routes that are added by a VPN gateway. Additionally, if you select **VPN gateway** as the value for this attribute, the VPN gateway propagates routes to this routing table. The **Advertise to** switch controls route advertisement to a transit gateway.
{: shortdesc}

The following sections explain how to configure route propagation for VPN gateways in IBM Cloud VPC routing tables.

When you configure route propagation, select the **VPN gateway** option in the following cases:

1. You want the routing table that is associated with a subnet to accept routes from the VPN gateway.

1. You are using policy-based VPN and want the routes to be propagated to the routing table.

VPN gateway route propagation supports both the default routing table and custom routing tables. For the default routing table, the default selection is **VPN gateway**. For the custom routing table, you must manually select **VPN gateway** if you want the routes to be propagated to the routing table.

## VPN gateway modes and route propagation
{: #vpn-gateway-modes-and-routes}

Understand how route propagation works across VPN gateways:

* **Policy-based VPN** - In a policy-based VPN, the routes are manually defined by using address prefixes and route propagation must be explicitly configured in the routing table. This mode supports manual route advertisement with Transit Gateway. See [setting up a transit gateway with policy-based VPN](/docs/vpc?topic=vpc-advertise-routes-s2s&interface=ui#setup-tg-with-vpn-vpc).

* **Route-based VPN** - Route-based VPN supports two connection types.

   * In a *static route-based connection*, the routes are defined by the VPN tunnel interface and route propagation must be configured in the routing table. This connection type doesn't support route advertisement with Transit Gateway.

   * In a *dynamic route-based connection*, the routes are automatically learned and propagated by using BGP. This connection type doesn't require manual route configuration in the routing table but it supports automatic route advertisement with Transit Gateway. See [planning considerations](/docs/vpc?topic=vpc-planning-considerations-vpn&interface=ui#dynamic-route-based-connection-considerations) for dynamic route-based VPN connection.

## Setting up a transit gateway with policy-based VPN for VPC
{: #setup-tg-with-vpn-vpc}

This configuration is applicable only for a policy-based VPN. A dynamic route-based VPN automatically propagates routes and doesn't require manual configuration with Transit Gateway.
{: note}

Follow the steps to set up a transit gateway with policy-based VPN for VPC:

1. When you [create a transit gateway](/docs/transit-gateway?topic=transit-gateway-ordering-transit-gateway&interface=ui) to connect VPCs, [create an ingress routing table](/docs/vpc?topic=vpc-create-vpc-routing-table&interface=ui) in the VPC where the VPN gateway exists.
1. Select **VPN gateway** if you want VPN gateway routes to be propagated to the routing table.
1. Turn the **Advertise to** switch to **On** when you select **Transit gateway** so that the on-premises routes that are learned from the VPN gateway are advertised to the Transit Gateway.

After the connection with the transit gateway is established, the route displays **On** in the Advertise column of the routing table. The on-premises routes are automatically advertised through the transit gateway after the ingress routing table accepts routes from the VPN gateway.

Both the VPN gateway subnet and the virtual server instance subnet are set as remote CIDRs if they are configured as local CIDRs.

For troubleshooting purposes, you can select or deselect **VPN gateway** to check routes that were added by the VPN gateway.
{: tip}

## Migrating to advertised routes in policy-based VPN for VPC
{: #migrate-to-advertise-routes-s2s}

For a policy-based VPN, you can migrate from manually defined routes (address prefixes) to automatically *advertised routes*. With Transit Gateway route advertisement support beginning 3 May 2024, existing policy-based VPN gateway customers who use address prefixes can choose from the following options:

* Keep using address prefixes in your VPC. If you continue using address prefixes, no further action is needed.
* Migrate to advertise routes by following these steps:
   1. Go to the ingress routing table in VPC where the policy-based VPN gateway exists.
   1. In the **Traffic source** section, switch **Advertise to** to **On**.
   1. Wait for the route's **Advertise** status to indicate **On** in the table.
   1. Delete the address prefix in the VPC.

   A brief traffic disruption is expected during the migration process. To minimize interruptions, plan the migration with a designated maintenance window.
   {: important}

## Related links
{: #propagating-routes-s2s-related}

* [Use case 4: Integrating with a site-to-site VPN gateway](/docs/vpc?topic=vpc-vpn-client-to-site-overview#integrating-with-site-to-site-vpn-gateway)
* [Why did the advertised route creation fail in my VPN gateway?](/docs/vpc?topic=vpc-troubleshoot-s2s-advertise-routes-over-quota)
* [Why isn't route advertisement to ingress sources working?](/docs/vpc?topic=vpc-troubleshoot-advertise-route-does-not-work-s2s)
* [Why can't I privately access my classic virtual server instance?](/docs/vpc?topic=vpc-troubleshoot-s2s-cannot-access-classic-vsi)
