---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot
---

{{site.data.keyword.attribute-definition-list}}

# Why can't I access VPC resources when my VPN client is connected?
{: #troubleshoot-not-access-vpc-with-active-client}
{: troubleshoot}
{: support}

In the network, the route controls how the traffic is forwarded, and the firewall can block this traffic for security reasons. The VPN traffic cannot reach the destination because either the route or firewall configuration is incorrect.
{: shortdesc}

Cannot access VPC resources from the VPN client.
{: tsSymptoms}

The firewall or route configuration is incorrect. For example, the server routes might not be added to the destination CIDR.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Check the security group that is attached to the VPN server to allow traffic between your VPN client and destination.
1. Check the security group that is attached to the VPC virtual server instance to allow traffic between your VPN client and destination.
1. Check the ACL on the subnet of the VPN server to allow traffic between your VPN client and destination.
1. Check the ACL on the subnet of the VPC virtual server instance to allow traffic between your VPN client and destination.
1. Check the routing table where the VPC virtual server instance's subnet attaches, and make sure that the VPN server is included in the `accept routes from` flag.
1. Check the routing table where the VPC virtual server instance's subnet attaches. Make sure that each zone has one route, and that the destination is a subset of the VPN server's client IP pool. The next hop is the private IP of the VPN server member. A total of six routes exists if the VPN server is in high-availability mode, and a total of three routes if the VPN server is in stand-alone mode. If you see that either the next hop is not the private IP of the VPN server member, or the number of routes is incorrect, you can remove the VPN server from the `accept routes from` flag, wait for 2 minutes, then add it back in.
1. Check that a VPN route exists to allow traffic between your VPN client and destination. If you can connect to the VPN server successfully with an OpenVPN client, but cannot access the cloud resources in VPC, verify that you added the server routes to the destination CIDR.
1. Check the firewall configuration on your VPN client to allow traffic between your VPN client and destination.
