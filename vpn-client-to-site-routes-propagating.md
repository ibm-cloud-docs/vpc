---

copyright:
  years: 2022, 2024
lastupdated: "2024-05-03"

keywords: network, encryption, client VPN, server VPN

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring route propagation for VPN servers
{: #vpn-client-to-site-route-propagation}

A client IPv4 address pool that is configured for a VPN server is split into multiple subset CIDR ranges, then added to VPC routing tables as the destination. The VPC routing table field **Accepts routes from** controls whether a routing table accepts routes that were added by the VPN server. You configure the VPN server to propagate routes to this routing table.
{: shortdesc}

When you [create a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table&interface=ui), select **VPN server** on the field **Accepts routes from** so that the VPN server routes propagate to it. For the default routing table, **VPN server** is selected by default. You should do this in the following cases:

1. If you want to access a virtual server instance in a subnet from a VPN client, then select **VPN server** for the routing table that is associated with this subnet.
1. If you want to configure ingress routing tables to accept routes from a VPN server so that your VPN clients can access the VPCs that are connected to ingress traffic sources, select **VPN server** and do the following:

   * Toggle the **Advertise to** switch to **On** when selecting an ingress traffic source, such as a Direct Link or Transit Gateway.
   * When you [create a VPN route](/docs/vpc?topic=vpc-vpn-client-to-site-routes&interface=ui#create-route-ui-c2s), you can use either the **Deliver** or **Translate** action for it.

For troubleshooting purposes, you can select or deselect the **VPN server** switch to check routes that were added by the VPN server.
{: tip}

## Client VPN for VPC migration to advertise route
{: #advertise-routes-c2s}

When you want to connect from your VPN client to multiple VPCs connected through Transit Gateway, you can set a VPN server route to the destination VPC. Then, you can use the `translate` action to translate the source IP address to the VPN server's private IP, making the VPN client IP address invisible. However, this action can introduce a security concern if you want to monitor or audit all client IP addresses that get connected.

Starting 3 May 2024, this issue is resolved with the use of route advertisement for VPN servers.
{: note}

Existing VPN server customers who use a `translate` VPN server route can choose from the following options:

* Keep using the `translate` action for your VPN server route. There is nothing more that you need to do.
* Use the `deliver` action for your VPN server route.
   After you enable the **Advertise to** switch for your VPN server, follow these steps:

   1. Go to the ingress routing table in the VPC where the VPN server exists.
   1. In the **Traffic source** section, switch **Advertise to** to **On**.
   1. Delete the VPN server route with the `translate` action.
   1. Create a VPN server route with the `deliver` action.
   1. Wait for the route's **Advertise** status to indicate **On** in the table.

   There is a brief disruption in traffic during this process. As a result, you should coordinate the migration with a maintenance window to minimize interruptions.
   {: important}

## Related links
{: #propagating-routes-c2s}

* [Use case 3: Integrating with a transit gateway](/docs/vpc?topic=vpc-vpn-client-to-site-overview#vpn-client-to-site-use-cases)
* [Why did the advertised route creation fail in my Client VPN for VPC?](/docs/vpc?topic=vpc-troubleshoot-c2s-advertise-routes-over-quota)
* [Why isn't route advertisement to ingress sources working?](/docs/vpc?topic=vpc-troubleshoot-advertise-route-not-work-c2s)
