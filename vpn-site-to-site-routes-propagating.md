---

copyright:
  years: 2024
lastupdated: "2024-12-16"

keywords:  network, VPN, VPN gateways, encryption

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring route propagation for VPN gateways
{: #advertise-routes-s2s}

The VPC routing table attribute **Accepts routes from** controls whether a routing table accepts routes that were added by the VPN gateway. If you select **VPN gateway** for this attribute, the VPN gateway propagates routes to this routing table.
{: shortdesc}

Select **VPN gateway** in the following cases:

1. If you want to accept a route from the VPN gateway for the routing table, select **VPN gateway** for the routing table that is associated with this subnet.

1. VPN gateway route propagation supports both the default routing table and custom routing tables. When you create a routing table, make sure to select **VPN gateway** if you want VPN gateway routes propagated to it. For the default routing table, **VPN gateway** is selected by default.

## Setting up a transit gateway with VPN for VPC
{: #setup-tg-with-vpn-vpc}

When you [create a transit gateway](/docs/transit-gateway?topic=transit-gateway-ordering-transit-gateway&interface=ui) to connect VPCs you must [create an ingress routing table](/docs/vpc?topic=vpc-create-vpc-routing-table&interface=ui) in the VPC where the VPN gateway exists. You must also make sure to select **VPN gateway** if you want VPN gateway routes propagated to it. In addition, you must turn the **Advertise to** switch to **On** when selecting **Transit gateway**.

After the connection is established, the route displays **On** in the Advertise column of the routing table. The on-prem routes are automatically advertised through the transit gateway after the ingress routing table accepts routes from the VPN gateway.

Both the VPN gateway subnet and the virtual server instance whose subnet exists in the VPC connected by the transit gateway are set as remote CIDRs if they are set as local CIDRs.

For troubleshooting purposes, you can select or deselect **VPN gateway** to check routes that were added by the VPN gateway.
{: tip}

## VPN for VPC migration to advertise routes
{: #migrate-to-advertise-routes-s2s}

Currently, only policy-based VPNs support Transit Gateway route advertisement. Route-based VPNs do not support Transit Gateway route advertisement at this time.
{: important}

With Transit Gateway route advertisement support beginning 3 May 2024, existing policy-based VPN gateway customers who use address prefixes can choose from the following options:

* Keep your address prefixes in your VPC. If you decide to keep using address prefixes, there is nothing else that you need to do.
* Migrate to advertise routes by following these steps:
   1. Go to the ingress routing table in VPC where the policy-based VPN gateway exists.
   1. In the **Traffic source** section, switch **Advertise to** to **On**.
   1. Wait for the route's **Advertise** status to indicate **On** in the table.
   1. Delete the address prefix in the VPC.

   There will be a brief disruption in traffic during this process. You should coordinate the migration with a maintenance window to minimize interruptions.
   {: important}

## Related links
{: #propagating-routes-s2s-related}

* [Use case 4: Integrating with a site-to-site VPN gateway](/docs/vpc?topic=vpc-vpn-client-to-site-overview#integrating-with-site-to-site-vpn-gateway)
* [Why did the advertised route creation fail in my VPN gateway?](/docs/vpc?topic=vpc-troubleshoot-s2s-advertise-routes-over-quota)
* [Why isn't route advertisement to ingress sources working?](/docs/vpc?topic=vpc-troubleshoot-advertise-route-does-not-work-s2s)
* [Why can't I privately access my classic virtual server instance?](/docs/vpc?topic=vpc-troubleshoot-s2s-cannot-access-classic-vsi)
