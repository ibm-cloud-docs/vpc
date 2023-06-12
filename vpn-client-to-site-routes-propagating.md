---

copyright:
  years: 2022
lastupdated: "2022-07-06"

keywords:  network, encryption, client VPN, server VPN

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring route propagation for VPN servers
{: #vpn-client-to-site-route-propagation}  

A client IPv4 address pool that is configured for a VPN server is split into multiple subset CIDR ranges, and added to VPC routing tables as the destination. The VPC routing table attribute `Accepts routes from` controls whether a routing table accepts routes that were added by the VPN server. If `VPN server` is selected for this attribute, the VPN server propagates routes to this routing table.
{: shortdesc}

Select `VPN server` in the following cases:

1. If you want to access a virtual server instance in a subnet from a VPN client, you must select `VPN server` for the routing table that is associated with this subnet.
1. For troubleshooting purposes, you can select or deselect `VPN server` to check routes that were added by the VPN server.

When you create a routing table, make sure to select `VPN server` if you want VPN server routes propagated to it. For the default routing table, `VPN server` is selected by default.
