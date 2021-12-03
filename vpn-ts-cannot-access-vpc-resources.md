---

copyright:
  years: 2021
lastupdated: "2021-06-28"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

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

1. Check the security group attached to the VPN server to allow traffic between your VPN client and destination.
1. Check the security group attached to the VPC virtual server instance to allow traffic between your VPN client and destination.
1. Check the ACL on the subnet of the VPN server to allow traffic between your VPN client and destination.
1. Check the ACL on the subnet of the VPC VSI to allow traffic between your VPN client and destination.
1. Check that there is a VPN route to allow traffic between your VPN client and destination. If you can connect to the VPN server successfully with an OpenVPN client, but cannot access the cloud resources in VPC, verify that you have added the server routes to the destination CIDR.
1. Check the firewall configuration on your VPN client to allow traffic between your VPN client and destination.
