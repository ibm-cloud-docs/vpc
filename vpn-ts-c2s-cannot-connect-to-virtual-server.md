---

copyright:
  years: 2024, 2026
lastupdated: "2026-04-21"

keywords: virtual private network, VPN, VPN gateway, troubleshooting, classic

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can’t I access one virtual server over the client-to-site VPN while I can reach the others?
{: #troubleshoot-c2s-cannot-connect-to-virtual-server}
{: troubleshoot}
{: support}

You can’t establish a connection to a specific virtual server over a client-to-site VPN, while others remain reachable. This issue can occur due to networking inconsistencies that can affect the virtual server, even if the VPN connection is successful.
{: shortdesc}

After you connect to your client‑to‑site VPN successfully, you can access some virtual server instances through the VPN, but connectivity to a specific virtual server fails even though both are expected to be reachable via the same VPN. You might observe one or more of the following conditions:
{: tsSymptoms}

* The VPN connection establishes successfully, which confirms that the client configuration is correct.
* The VPN server is active and reachable.
* One virtual server instance is reachable over the VPN. However, connection attempts to another virtual server fail with timeout or connectivity errors.
* The issue is limited to a specific virtual server and doesn't affect all resources in the VPC.

This issue occurs due to virtual server-specific networking or routing inconsistencies rather than a VPN issue.
{: tsCauses}

Follow the steps to resolve this issue:
{: tsResolve}

1. __Verify and update VPC placement__
   * Check whether both virtual server instances are deployed in the same VPC as the client-to-site VPN server.
   * If the virtual server you are trying to connect is in a different VPC, place the virtual server in the same VPC as the VPN, or use a Transit Gateway to interconnect the VPCs. For route propagation with Transit Gateway, see the following step.
1. __Verify route propagation__
   * Ensure that the VPN server VPC and the target VPC are attached to the same Transit Gateway.
   * When you configure the VPN server, ensure that __Accept routes from__ is enabled for the ingress routing table. This option allows the VPN service to add routes for the VPN client IP pool.
   * Also, make sure that __Advertise to__ is enabled for the Transit Gateway so that the VPN client IP pool CIDR is propagated to other VPCs connected to the Transit Gateway. For more information see, [Configuring route propagation for VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation).
   * Confirm that there are no overlapping CIDR ranges between the VPN client IP pool and any connected VPCs.
1. __Check security group and network ACL rules__
   * Compare the security group rules that are attached to the reachable and unreachable virtual server instances.
   * Ensure that the inbound rules allow protocols and ports required for the applications or data traffic.
   * Make sure that the rules are identical and that no restrictive deny network ACL rules block VPN client subnets.
1. __Retry the connection__ - After you update the configuration, reconnect to the VPN.
