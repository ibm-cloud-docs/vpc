---

copyright:
  years: 2022, 2026
lastupdated: "2026-05-25"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot
---

{{site.data.keyword.attribute-definition-list}}

# Why is my site-to-site VPN connection down even though the gateway is healthy?
{: #troubleshoot-vpn-connection-down-gateway-healthy}
{: troubleshoot}
{: support}

A site‑to‑site policy-based VPN connection can remain down if the VPN gateway fails to add a required route to a VPC routing table due to an existing conflicting route with different advertise settings. Removing the conflicting route allows the VPN to re-create the route and establish the tunnel.
{: shortdesc}

The VPN gateway and connection appear healthy and configured, but the VPN tunnel remains down. Refreshing or recovering the VPN gateway doesn't restore connectivity.
{: tsSymptoms}

You might observe one or more of the following behaviors:

* The VPN connection status remains down
* Gateway refresh or recovery completes successfully, but the tunnel doesn't come up
* The VPN connection health details show the status reason `CANNOT_CREATE_VPC_ROUTE`
* Traffic doesn't flow between on‑premises and VPC networks

This issue occurs when the VPN gateway attempts to automatically add a route to a VPC routing table, but a route for the same destination prefix exists with different attributes. Because the existing route can't be updated by the VPN service, the route creation fails, preventing the VPN from completing its setup and establishing the tunnel.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Identify the VPC routing table that is associated with the subnet and is used by the VPN gateway. For more information see, [Viewing details of a routing table](/docs/vpc?topic=vpc-view-details-routing-table) and [Configuring route propagation for VPN gateways](/docs/vpc?topic=vpc-advertise-routes-s2s).
1. Review the existing routes in the routing table and look for routes that overlap with the VPN local or peer CIDR ranges.
1. Check whether any overlapping route has attributes that differ from what the VPN requires, such as a different advertise setting.
1. Refresh the VPN connection.
1. Verify that the connection status changes to UP and that traffic flows successfully between networks.
