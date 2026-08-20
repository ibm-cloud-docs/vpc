---

copyright:
  years: 2021, 2026
lastupdated: "2026-08-20"

keywords: virtual private network, VPN, VPN server, troubleshooting, client-to-site, traffic flow, cannot access, DNS, VPE, virtual server

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do routing and reachability issues exist with my client-to-site VPN connection?
{: #troubleshoot-c2s-traffic-flow}
{: troubleshoot}
{: support}

When your client-to-site VPN connection is established but you cannot access VPC or connected network resources, the issue is related to routing configuration, access controls, VPN routes, or route propagation. The following sections help you diagnose and resolve common traffic flow issues.
{: shortdesc}

You might experience one or more of the following traffic flow issues:
{: tsSymptoms}

- VPN clients can connect successfully to the VPN server but cannot reach resources in the VPC.
- Some workloads are reachable, while others aren't.
- Private virtual endpoints (VPEs) are unreachable when split-tunnel mode is enabled.
- You can't reach resources in connected VPCs or classic infrastructure through Transit Gateway.
- DNS queries continue to use the local DNS server instead of the VPN-configured DNS server.
- Traffic flows to some destinations but fails for specific subnets or applications.
- Traffic stops flowing after changes to routing table configuration.
- Advertised route creation fails with quota-related errors.

Traffic flow issues can occur when the VPN tunnel is established but routing, access controls, or route propagation prevent packets from reaching their destination.
{: tsCauses}

**Access control restrictions**
:   Security groups, network ACLs, or client-side firewalls can block the VPN protocol or port that is used by the VPN server.

**Routing misconfigurations**
:   Missing VPN routes, incorrect route propagation, or invalid next-hop routes prevent traffic forwarding.

**Transit Gateway route advertisement issues**
:   Connected VPCs don't receive routes for the VPN client IP pool and route advertisement to Transit Gateway doesn't work as expected.

**Split-tunnel configuration issues**
:   Required VPC or VPE subnets aren't included in VPN routes.

**Advertised route quota limits**
:   New advertised routes can't be created when the VPC advertised route quota limit is reached.

Follow these steps to diagnose and resolve traffic flow issues:
{: tsResolve}

## Diagnosing traffic flow issues
{: #diagnose-c2s-traffic-flow}

Follow this systematic approach to identify where traffic is being blocked:

1. **Verify VPN connection establishment**:

   1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
   1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPN servers**.
   1. Verify that the VPN server health status is `Healthy`.
   1. Additionally, go to the **Clients** tab to check whether your VPN connections are present in the list.

   For more information, see [VPN server states and descriptions](/docs/vpc?topic=vpc-vpn-server-states&interface=ui), and [Diagnosing VPN server health](/docs/vpc?topic=vpc-vpn-server-health&interface=ui).

1. **Verify client connectivity**:

   - Confirm that the VPN client receives an IP address from the configured client IP pool.
   - Test connectivity to a known VPC resource with the following command:

   ```sh
   ping <destination-ip>
   ```
   {: codeblock}

   - Use `traceroute` to identify where packets stop:

   ```sh
   traceroute <destination-ip>
   ```
   {: codeblock}

1. **Check VPN routes**:

   - Review the VPN routes configured on the VPN server.
   - Verify that the destination subnet is included in the advertised VPN routes.
   - In split-tunnel mode, ensure all required destination CIDRs are explicitly configured.
   - In full-tunnel mode, ensure that the `0.0.0.0/0` route is not added automatically, and verify that the default route is present.
   - Make sure that the route action is either `Accept` or `Translate` and not `Drop`.

1. **Review security controls**:

   - Verify the security groups that are attached to the VPN server.
   - Verify the security groups that are attached to the destination resources.
   - Verify the network ACLs on the VPN server subnet and workload subnets.

1. **Verify route advertisement configuration**:

   * If Transit Gateway or Direct Link connectivity is required, verify that route advertisement is enabled on the ingress routing table. For more information, see [Creating a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table).
   * Check for route conflicts that might prevent route propagation. For more information, see [Resolving routing issues](/docs/vpc?topic=vpc-troubleshoot-c2s-traffic-flow#resolve-c2s-routing).

## Resolving access control issues
{: #resolve-c2s-access-controls}

Security groups and network ACLs are the most common cause of client-to-site VPN traffic flow failures.

### Check security group configuration
{: #c2s-check-security-groups}

You might see one or more of the following messages:

   ```text
   TCP connection timeout

   TLS handshake failed

   Connection timed out
   ```
   {: screen}

1. Verify the security group that is attached to the VPN server to allow traffic between your VPN client and destination.

1. Verify the security group that is attached to the VPC virtual server instance.

1. Ensure that inbound and outbound rules allow traffic between:

   - VPN client IP pool CIDR
   - Destination VPC resource CIDRs

1. Confirm that required application ports and protocols are allowed for traffic flow. For more information, see [Configuring security groups and NACLs for use with a VPN server](/docs/vpc?topic=vpc-vpn-client-to-site-security-groups&interface=ui).

1. Update the security group to allow inbound VPN traffic from the appropriate source ranges. For example, allow traffic from `0.0.0.0/0` or from trusted client CIDR blocks as required by your security policy.

   Add security group rules that match the VPN server configuration. Depending on whether the VPN server is configured to use TCP or UDP, allow the corresponding protocol and the configured VPN port in the security group's inbound and outbound rules.
   {: note}

### Check network ACL configuration
{: #c2s-check-acls}

Network ACLs are stateless and require rules for both inbound and outbound traffic.

1. Identify the ACL attached to the VPN server subnet to allow traffic between your VPN client and destination.

1. Identify the ACL attached to the destination workload subnet.

1. Verify that ACL rules allow traffic between:

   - VPN client IP pool
   - Destination resource CIDRs

1. Review any deny rules that might override allow rules.

If connectivity begins working after temporarily allowing all traffic, the issue is likely ACL related. For more information, see [Configuring security groups and NACLs for use with a VPN server](/docs/vpc?topic=vpc-vpn-client-to-site-security-groups&interface=ui).
{: tip}

## Resolving routing issues
{: #resolve-c2s-routing}

Correct routing is required for traffic to reach VPN-connected resources.

### Verify routing table configuration
{: #verify-c2s-routing-table}

1. Go to **Infrastructure > Network > Routing tables** and select your VPC and identify the routing table that is associated with the VPC subnet.

1. Verify that the **Accept routes from** setting includes the VPN server.

1. Confirm that routes are automatically created for the VPN client IP pool.

1. Additionally, verify the following conditions:

   - Each route points to a VPN server member private IP.
   - Routes exist for all availability zones.
   - No routes point to incorrect next hops.

1. Check the routing table where the VPC virtual server instance's subnet attaches. Make sure that each zone has one route, and that the destination is a subset of the VPN server's client IP pool. The next hop is the private IP of the VPN server member. A total of six routes exists if the VPN server is in high-availability mode, and a total of three routes if the VPN server is in stand-alone mode. If either the next hop is not the private IP of the VPN server member, or the routes appear stale or incomplete:

   - Remove the VPN server from **Accept routes from**.
   - Wait approximately two minutes.
   - Re-enable **Accept routes from**.
   - Verify that routes are re-created correctly.

1. Check that a VPN route exists to allow traffic between your VPN client and destination. If you can connect to the VPN server successfully with an OpenVPN client, but can't access the cloud resources in VPC, verify that you added the routes in the VPN server for the destination CIDR.

### Resolve route advertisement issues
{: #resolve-route-advertisement}

Route advertisement enables VPN client routes to be propagated to Transit Gateway, and other ingress sources. Route advertisement can fail when route conflicts or overlapping prefixes exist.

1. **Verify route advertisement settings**.

   1. Open the ingress routing table associated with the VPN server.
   1. Confirm that route advertisement is enabled for the required ingress source. For more information see, [Configuring route propagation for VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation).

1. **Check for route conflicts**.

   * Go to **Interconnectivity** > **Transit Gateway**.
   * Select the Transit Gateway that is connected to the VPC.
   * In the **Routes** section, generate a route report.

1. **Identify overlapping prefixes**.

   * Look for routes with duplicate or overlapping CIDRs.
   * Review VPC address prefixes and propagated routes for conflicts.

1. **Resolve route conflicts**.

   * Modify overlapping CIDRs where possible.
   * Remove unnecessary route advertisements.
   * Regenerate the route report and verify that routes are now advertised successfully.

### Resolve recursive routing issues
{: #resolve-recursive-routing}

A recursive routing issue occurs when the VPN client attempts to reach the VPN server's public endpoint through the VPN tunnel itself. OpenVPN detects this routing loop and drops the packets.

1. **Review VPN server routes**.

   * Verify that none of the pushed routes contain the VPN server's public endpoint or public address range.
   * Verify that the CIDRs defined in the VPN routes do not include the VPN server’s public endpoint or public address range, as these CIDRs determine the routes that are pushed to VPN clients.
   * Remove any routes that cause traffic destined for the VPN server to be routed through the tunnel.

1. **Verify address range configuration**.

   * Ensure that the client IP address pool does not overlap with VPC subnet CIDRs.
   * Verify that the client IP address pool does not overlap with any connected on-premises networks.

1. **Review VPC routing tables**.

   * Check for conflicting or overly broad routes such as `0.0.0.0/0`.
   * Remove or modify routes that interfere with VPN client traffic.

1. **Recreate the VPN server if necessary**.

   * If routing inconsistencies persist, re-create the VPN server.
   * Consider deploying the VPN server in a dedicated subnet to simplify routing and avoid future conflicts.

### Resolve routes stuck Pending or Deleting state
{: #resolve-stuck-vpn-routes}

VPN route creation or deletion can remain in a `Pending` or `Deleting` state when the VPN server is unable to successfully apply configuration updates.
 
1. From the VPN server details page, review the VPN route status.
 
1. Check whether any VPN routes remain in a `Pending` or `Deleting` state for an extended period.

1. If routes are stuck, verify that the certificates associated with the VPN server exist in Secrets Manager and are valid. Additionaly, verify that these certificates are accessible to the VPN server are not expired.
 
1. Renew or replace the expired certificates and retry the route operation.
 
   If the VPN server certificates are expired, invalid, or inaccessible, route creation can remain in a `Pending` state and route deletion can remain in a `Deleting` state because the VPN server cannot successfully apply configuration updates.
   {: note}

### Resolve incorrect VPN server service routes
{: #resolve-service-route-issues}

VPN server service routes are automatically propagated to VPC routing tables that have **VPN server** enabled in the **Accept routes from** setting. These routes use names that are prefixed with `ibm-vpn-server-`.

When these routes contain incorrect next-hop information, traffic can fail even though VPN clients connect successfully.

1. **Identify incorrect service routes**.

   * Open the routing table that is associated with the VPN server.
   * Locate routes with names prefixed with `ibm-vpn-server-`.
   * Verify that the next hop matches the VPN server's private IP address.

   If no `ibm-vpn-server-` service routes are present, do not add them manually. Regenerate the routes by disabling and re-enabling the VPN server option under Accept routes from.
   {: note}

1. **Regenerate service routes**.

   1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
   1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Routing tables**.
   1. Select your VPC from the VPC drop-down menu.
   1. Click the routing table to open its details page, then click **Edit**.
   1. Clear the **VPN server** option under **Accept routes from** section and click **Save**.
   1. Click **Edit** again.
   1. Select the **VPN server** option in the Accepts routes from (optional) section and click **Save**.

   The VPN server re-creates the required service routes.

1. **Verify connectivity**.

   1. Reconnect to the VPN server.
   1. Test access to VPC resources.
   1. Confirm that traffic is successfully routed through the regenerated service routes.

## Resolving connectivity to specific virtual server instances
{: #resolve-specific-vsi-connectivity}

If some virtual server instances are reachable while others aren't, the issue is due to virtual server-specific networking or routing inconsistencies rather than VPN-related.

### Verify VPC placement
{: #verify-vpc-placement}

1. Confirm that the target virtual server instance resides in the same VPC as the VPN server.

1. If the virtual server instance is located in another VPC, attach both VPCs to a Transit Gateway and verify route propagation between connected VPCs.

### Verify route propagation and advertisement
{: #verify-route-propagation}

1. Confirm that the VPN server VPC and destination VPC are connected to the same Transit Gateway.

1. Verify that:

   - **Accept routes from** is enabled on the ingress routing table. This option allows the VPN service to add routes to the routing table for the VPN client IP pool.
   - **Advertise to** is enabled in the ingress routing table for Transit Gateway source so that the VPN client IP pool CIDR is propagated to other VPCs connected to the Transit Gateway. For more information see, [Configuring route propagation for VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation).

1. Confirm that the VPN client IP pool CIDR doesn't overlap with any connected VPC CIDR.

### Compare security policies
{: #compare-security-policies}

1. Compare security group rules between a reachable and unreachable virtual server.

1. Confirm that the required protocols are allowed and that no ACL rules block traffic from the VPN client subnet.

2. Reconnect the VPN client and retest connectivity after making configuration changes.

## Resolving VPE access issues in split-tunnel mode
{: #resolve-vpe-split-tunnel}

When split-tunnel mode is enabled, VPE traffic is routed through the VPN only if appropriate routes and DNS access exist.

### Verify DNS access
{: #verify-vpe-dns}

The VPN client must be able to resolve private VPE hostnames. If port `53` (DNS) isn't allowed in the security group, the request is blocked, and the VPE IP address is never returned.

Ensure that security groups allow DNS traffic:

| Protocol | Port |
| -------- | ---- |
| UDP | 53 |
| TCP | 53 |
{: caption="Required DNS traffic" caption-side="bottom"}

### Verify VPN routes
{: #verify-vpe-routes}

In split-tunnel mode, the VPN server specifies routes for the traffic that must go through the tunnel. If no VPN route exists for the VPC subnet where the VPE is located, your client sends the traffic to the public internet, where private IP addresses aren't accessible.

1. Review VPN server routes.

1. Add one of the following routes in the VPN server routes section:

   - The VPC subnet CIDR that contains the VPE (recommended).

   ```text
   10.x.y.0/24
   ```
   {: codeblock}

   - The VPE private IP address as a `/32` route.

   ```text
   10.x.y.z/32
   ```
   {: codeblock}

3. Disconnect and reconnect the VPN client to receive updated routes.

## Resolving classic infrastructure connectivity issues
{: #resolve-classic-connectivity}

When accessing classic infrastructure through Transit Gateway, classic virtual servers must know how to return traffic to the VPN client subnet. By default, your classic virtual server instance is configured to route through the public interface and doesn't know how to route traffic to the private network on-premises or remote.

### Verify return routing
{: #verify-classic-return-routing}

1. Open **Classic Infrastructure** > **Devices**.

1. Access the target virtual server instance.

1. In the Network details table, identify the private interface gateway, by hovering over the information icon of the IP address.

1. Add a route for the VPN client network.

As an example, in the following command for Linux, `10.240.5.0/24` is the CIDR of your network on-premises and `10.188.170.65` is the gateway of the private IP address.

   ```sh
   ip route add 10.240.5.0/24 via 10.188.170.65
   ```
   {: pre}

For more details about adding routes on different operating systems, see [How do I add the new routing for an operating system?](/docs/virtual-servers?topic=virtual-servers-faqs-servers-general-#how-to-add-the-new-routing-for-various-oses){: external}.

## Resolving DNS issues
{: #resolve-c2s-dns}

If the VPN client uses a local DNS server after connecting, the VPN DNS settings might not be applied by the client software.

### Verify DNS behavior
{: #verify-dns-behavior}

1. Check whether the VPN-assigned DNS server appears in the client network configuration.

1. Verify that DNS requests are sent to the VPN-configured DNS server.

### Resolve DNS configuration issues
{: #resolve-dns-config}

Verify that the VPN server is configured correctly to allow DNS resolution for VPN clients.

* If you configure the IBM Cloud DNS Services private DNS resolvers (`161.26.0.7` and `161.26.0.8`) when you provision the VPN server, add a VPN server route with destination `161.26.0.0/16` and the **Translate** action.
* Without this route, VPN clients might be unable to resolve private DNS names. For more information, see [Existing VPC configuration considerations](/docs/vpc?topic=vpc-client-to-site-vpn-planning#existing-vpc-configuration-considerations), and [About IBM Cloud DNS Services](https://cloud.ibm.com/docs/dns-svcs?topic=dns-svcs-about-dns-services).

## Resolving advertised route quota issues
{: #resolve-c2s-route-quota-issues}

Advertised route creation can fail when the VPC reaches its advertised route quota.

### Reduce advertised route consumption
{: #reduce-c2s-advertised-routes}

A VPC supports a limited number of advertised routes by default. When the quota limit is reached, additional routes can't be created.

Follow these steps to resolve advertised route quota issues:

1. Review routing tables in the VPC.

1. Identify routes that are no longer required.

1. Disable unnecessary route advertisements:

   - Set **Advertise to** to **Off** where route advertisement is not required.
   - Review the **Accept routes from** settings and disable unused route propagation sources.

1. Retry the advertised route creation.

For client-to-site VPN server quota, see [Client VPN for VPC (client-to-site)](/docs/vpc?topic=vpc-quotas#vpn-server-quotas). For routing table quota, see [Routing tables and routes](/docs/vpc?topic=vpc-quotas&interface=api#routing-tables-routes-quotas).

### Request a quota increase
{: #request-c2s-route-quota-increase}

If route consolidation isn't possible, request a quota increase through IBM Support.

1. Gather the following information to help IBM evaluate your requirements:

   - Account ID
   - VPC ID
   - Current advertised route count
   - Requested quota increase
   - Business and technical justification
   - Required implementation timeline

1. Submit an [IBM Support case](/unifiedsupport/cases/form){: external} and request an increase in quota with the details.

    IBM Support reviews each request based on technical feasibility and the justification that is provided. If approved, you will be notified of the outcome and the quota is adjusted accordingly.
   {: note}

1. Continue to use the `translate` action in the VPN server route.

## Related links
{: #c2s-traffic-flow-related}

- [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes)
- [Configuring route propagation for VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation)
- [About IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about)
- [Quotas and service limits](/docs/vpc?topic=vpc-quotas)
- [Configuring route propagation for VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation&interface=ui).
- [Why can't I establish or maintain a connection to my VPN server?](/docs/vpc?topic=vpc-troubleshoot-c2s-connection-establishment)
