---

copyright:
  years: 2018, 2026
lastupdated: "2026-08-20"

keywords: virtual private network, VPN, VPN gateway, troubleshooting, routing, routes, reachability, advertise, propagation

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do routing and reachability issues exist with my site-to-site VPN?
{: #troubleshoot-vpn-routing-reachability}
{: troubleshoot}
{: support}

Routing issues in site-to-site VPN stem from incorrect route configurations, route conflicts, or issues with route advertisement and propagation. They can prevent proper communication between VPC resources and on-premises networks even when VPN tunnels are established.
{: shortdesc}

You experience one or more of the following routing and reachability problems:
{: tsSymptoms}

* VPN gateway can't establish connection with the on-premises VPN (route loop).
* Route advertisement to Transit Gateway doesn't work as expected.
* You see _Destination host unreachable_ errors despite an active VPN connection.
* Traffic works initially but fails after routing table changes.
* Virtual server instances in certain zones can't reach your peer network.
* Virtual server instances in different subnets can't communicate with each other after route addition.
* You can't access classic virtual servers through VPN.

Routing and reachability failures occur when routes are missing, incorrect, conflicting, or not properly propagated.
{: tsCauses}

**Route configuration issues**
:   Missing routes, incorrect next hops, or routes pointing to wrong destinations prevent traffic from reaching the VPN gateway or destination networks.

**Route propagation issues**
:   VPN routes are not propagated to routing tables. Route advertisement to Transit Gateway or Direct Link fails due to conflicts.

**Default route issues**
:   Using `0.0.0.0/0` as peer CIDR creates route loops and prevents inter-subnet communication and connection establishment with VPN gateway.

**Route conflicts and overlaps**
:   Conflicting routes with different attributes or overlapping address ranges cause routing ambiguity and traffic failures.

**Classic infrastructure routing issues**
:   Classic virtual server instances that are missing routes to private on-premises network.

Follow these steps to diagnose and resolve routing and reachability issues:
{: tsResolve}

## Diagnosing routing issues
{: #diagnose-routing-issues}

Follow this systematic approach to identify routing problems:

1. **Check VPN gateway and connection status**:

   1. Verify that the VPN connection is established and has an `Up` status.
   1. For dynamic route-based VPNs, verify whether your peer device supports BGP and check if BGP configuration on both sides matches.
   1. Additionally, check that tunnel IP addresses are properly configured for BGP peering and that the BGP state is "Established". The `tunnel_interface_ip` on IBM VPN must match the remote tunnel IP on your peer device and peer’s tunnel IP must be configured as the remote IP on IBM VPN.
   1. Check VPN connection health details.

   For more information, see [VPN gateway states and descriptions](/docs/vpc?topic=vpc-vpn-states), and [Diagnosing VPN gateway connection health](/docs/vpc?topic=vpc-vpn-health#vpn-connection-health).

1. **Enable NAT-Traversal and open required ports**: NAT-Traversal (NAT-T) enables VPN IPsec traffic to pass through devices that use Network Address Translation (NAT) by encapsulating IPsec packets inside UDP packets. This configuration is essential for VPN connections, especially when NAT devices exist between peers.

   * Enable NAT-T on your peer device if it's a configurable option. For more information, see [Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations).

   * Verify that UDP ports `500` and `4500` are allowed for traffic flow on the peer device firewall, any intermediate firewalls, VPC Network ACLs, and security groups. For more information, see [Configuring network ACLs for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn&interface=ui).

   If NAT-T is disabled and a NAT device exists in the network path, tunnel establishment or bidirectional traffic can fail.
   {: note}

1. **Review VPN logs**: Review logs on both IBM Cloud VPN and your peer device for any errors. For more information on how to check IPsec logs, see [How do I check IPsec logs?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui#faq-vpn-19).

1. **Verify routing table configuration**:

   1. **Identify the relevant routing tables**

      * In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Routing tables**.
      * Locate the routing table for both the workload subnet and the VPN gateway subnet.

   1. **Validate routing configuration**

      * Ensure that the routes for on-premises CIDRs exist and point to the correct VPN connection and gateway.
      * Verify that the routes in the routing table have the correct next-hop information.

   1. **Verify route propagation and advertisement**

      * Additionally, for policy-based VPN gateway, check whether **Accept routes from VPN gateway** is enabled in the routing table. This setting allows the VPN gateway to automatically insert routes into the routing table for on-premises peer CIDRs.
      * Verify that the **Advertise to** settings for Transit Gateway is turned on. This setting advertises on-premises routes to other VPCs through the transit gateway. For more information, see [Configuring route propagation for VPN gateways](/docs/vpc?topic=vpc-advertise-routes-s2s).

1. **Test network reachability**: Use `ping` to test bidirectional connection from VPC to on-premises and from on-premises to VPC.

   ```sh
   ping <destination-ip>
   ```
   {: codeblock}

   Use packet capture with `traceroute` to identify where packets stop. Asymmetric failures indicate routing issues on one side.

      ```sh
   traceroute <destination-ip>
   ```
   {: codeblock}

   If routing is correct, the VPN gateway's private IP appears as a hop.

1. **Verify Network ACL and security groups configuration**: Confirm that network ACLs and security groups on both sides of the VPN allow the required inbound and outbound traffic. Misconfigured ACL or security group rules can prevent traffic from traversing the VPN tunnel even when the tunnel status is up. For more information, see [About network ACLs](/docs/vpc?topic=vpc-using-acls&interface=ui), and [About security groups](/docs/vpc?topic=vpc-using-security-groups).

## Resolving route configuration issues
{: #resolve-route-configuration}

Follow these steps to resolve route configuration issues:

### Fix missing or incorrect routes
{: #fix-missing-routes}

**For policy-based VPNs:**

1. **Verify that service routes are created**.

   * In the routing table associated with your workload subnet, check for routes with names prefixed by `ibm-vpn-gateway-`.
   * Verify that the next hop for the route is the private IP address of the active VPN gateway member.
   * Verify that the destination CIDR matches the peer CIDR ranges and that no overlap exists. If conflicting routes exists, make sure that the destination CIDR has the highest priority.

1. **Enable automatic route creation**.

   * In the routing table that is associated with your workload subnet, click **Edit**.
   * In **Accept routes from**, enable **VPN gateway**.
   * Click **Save**.
   * After you enable this setting, the VPN service automatically inserts peer CIDR routes into the routing table and creates the corresponding service routes.

**For route-based VPNs:**

1. **Create manual routes** if needed.

   * Go to **Infrastructure > Network > Routing tables**.
   * Select the routing table and click **Create** to create routes.
   * Specify the on-premises CIDR as the destination.
   * Set the next hop type to **VPN connection**.
   * Select your VPN gateway and VPN connection.
   * Click **Create**.

   For route-based VPN, verify that the routes point to the VPN gateway and not to the private IP addresses.
   {: note}

1. **For dual-tunnel configurations**: Select the `distribute_traffic` connection property and disable it if on-premises doesn't support asymmetric routing. Always establish a connection to the smaller public IP address. For more information, see [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn&interface=ui#use-case-4-vpn).

1. **Verify routes in all zones**: If a virtual server instance in one zone can communicate with your peer network but an instance in another zone cannot, the affected zone is likely missing the required routes. To allow workloads in all zones to communicate with your peer network, add the corresponding routes to the routing table in each zone.

### Resolve route loops with default routes
{: #resolve-route-loops}

When the peer CIDR is configured as `0.0.0.0/0`, the VPN service adds a default route that can cause route loop. The virtual server instances in different subnets cannot communicate with each other and VPN gateway cannot communicate with on-premises gateway.

1. **Resolve inter-subnet communication issues**.

   * Create a **Delegate** route for each subnet to ensure that local traffic bypasses the VPN default route.
   * Configure the route with the following parameters:
      * **Destination**: Subnet CIDR
      * **Action**: Delegate
      * **Next hop**: Not applicable

1. **Prevent VPN gateway route loops**.

   * Create a dedicated subnet for the VPN gateway.
   * Create a dedicated custom routing table and associate it with the VPN gateway subnet.
   * This configuration isolates VPN gateway traffic from the effects of the default route.

1. **Use specific peer CIDR ranges**.

   * Avoid using `0.0.0.0/0` as a peer CIDR.
   * Verify that `0.0.0.0/0` isn't configured in the routing table that is attached to the subnet hosting the VPN gateway.
   * Specify only the required on-premises network CIDR ranges to prevent default route conflicts.

## Resolving route conflicts and overlaps
{: #resolve-route-conflicts}

### Resolve VPC routing table conflicts
{: #resolve-vpc-conflicts}

Route conflicts in the VPC routing table can prevent the VPN service from creating the routes that are required for connectivity.

1. **Identify conflicting routes**.

   * Review all routes in the routing table.
   * Look for routes that use the same destination prefixes as the peer VPN CIDRs or overlap with VPN local or peer CIDR ranges.
   * Check for routes with conflicting attributes, such as next-hop type or route advertisement settings.

1. **Resolve route conflicts**.

   * Delete or modify the conflicting route and refresh the VPN connection.
   * Ensure that only one route exists for each destination prefix.
   * If the existing route is required, update the VPN peer CIDRs to eliminate overlapping prefixes.

1. **Verify route creation and connectivity**.

   * Confirm that the VPN service creates the required routes after the conflict is resolved.
   * Toggle the VPN connection from `Down` to `Up`.
   * Test traffic flow between the connected networks.

### Resolve Transit Gateway route conflicts
{: #resolve-tg-conflicts}

Route conflicts in Transit Gateway can prevent routes from being advertised between connected networks.

1. **Generate Transit Gateway route report**.

   * Go to **Interconnectivity > Transit Gateway**
   * Select your Transit Gateway
   * In the Routes section, click **Generate report**

1. **Identify and resolve route conflicts**.

   * Check for routes with the same prefix but different sources.
   * Identify and resolve any overlapping routes that prevent route advertisement.
   * Ensure that each route has a unique, nonoverlapping destination.

1. **Verify route advertisement**.

   * After resolving conflicts, routes must advertise successfully.
   * Check that on-premises routes appear in Transit Gateway.

### Resolve route advertisement to Transit Gateway
{: #resolve-tg-advertisement}

Route advertisement enables the VPN gateway to dynamically advertise routes to Transit Gateway, which allows on-premises networks to communicate with other connected IBM Cloud resources in different VPCs. If route advertisement isn't working as expected, verify the route advertisement settings and confirm that the Transit Gateway connection is healthy.

1. **Enable route advertisement to Transit Gateway**.

   * Go to **Infrastructure > Network > Routing tables**.
   * Select the ingress routing table for the VPC.
   * In the Traffic source section, set `Advertise to` for Transit Gateway to `On`.

1. **Resolve route conflicts**.

    * Generate a Transit Gateway route report.
    * Identify and resolve any overlapping routes that prevent route advertisement.

1. **Verify VPN connection to Transit Gateway**.

   * Ensure that the VPN gateway is connected to the transit gateway and that the service connection status shows as `Up`.
   * Check and confirm Transit Gateway attachment status.

1. **Fix Transit Gateway connection issues**

   For dynamic route-based VPNs that use Transit Gateway:

   1. Verify that the VPN is connected to Transit Gateway and the service connection status shows as `Up`.
   1. Check the Transit Gateway route report to ensure that the on-premises IP addresses are advertised from the VPN connection and the IBM Cloud IP addresses are advertised from VPC or other connections.
   1. Verify that the VPN gateway lifecycle state is stable and healthy.
   1. If the service connection is in a failed state, wait for the VPN gateway to stabilize and re-create the connection. If the issue persists, contact IBM support. For more information, see [Resolving Transit Gateway routing issues](/docs/vpc?topic=vpc-troubleshoot-vpn-traffic-flow#resolve-transit-gateway-routing).

### Resolve route quota issues
{: #resolve-quota-issues}

When the VPC routing table reaches its route quota, the VPN gateway can't create additional routes. Any attempt to create or advertise new routes fails with a quota exceeded error.

Follow these steps to resolve route quota issues:

1. **Reduce the number of routes**.

   * In routing tables, set **Advertise to** to **Off** where route advertisement is not required.
   * In **Accept routes from**, set **VPN gateway** to **Off** where automatic route learning is not required.
   * Aggregate VPC subnets where possible to reduce the number of advertised routes.
   * Review egress custom routes and consolidate prefixes to stay within routing table limits.

1. **Use VPC address prefixes**: Advertise broader VPC address prefixes instead of individual subnet routes, which reduces the number of advertised routes. For more information, see [How to resolve VPN quota limit issues?](/docs/vpc?topic=vpc-troubleshoot-vpn-quota-issues).

1. **Request a quota increase**

   * [Open an IBM support case](/unifiedsupport/cases/form){: external}.
   * Request an increase to the route quota.
   * Provide details about your routing requirements and the reason for the increase.

   Each routing table supports a maximum of 14 unique route prefixes, which is hard limit. This limit can't be increased through a support request. If your environment exceeds this limit, consider consolidating or redesigning the routing configuration.
   {: note}

## Resolving classic infrastructure access issues
{: #resolve-classic-access}

Accessing classic infrastructure virtual server instances through VPN requires additional routing configuration. By default, classic virtual server instances route traffic through their public interfaces and do not have routes to private on-premises networks.

1. **Identify the private gateway for the classic virtual server instance**

   * Go to **Classic Infrastructure > Devices** and select the virtual server instance.
   * In the Network details table, hover over the information icon next to the private IP address.
   * Note the gateway IP address.
   * Add a route to specify the destination CIDR and the gateway IP. As an example, in the following command for Linux, `10.240.5.0/24` is the CIDR of your network on-premises and `10.188.170.65` is the gateway of the private IP address.

   ```sh
   ip route add 10.240.5.0/24 via 10.188.170.65
   ```
   {: codeblock}

1. **Verify connectivity**

   * From the on-premises network, test connectivity to the classic instance's private IP address.
   * From the classic instance, test connectivity to on-premises resources.
   * Verify bidirectional traffic flow.

For detailed instructions on adding routes for different operating systems, see [How do I add the new routing for an operating system?](/docs/virtual-servers?topic=virtual-servers-faqs-servers-general-#how-to-add-the-new-routing-for-various-oses).
{: tip}

## Resolving destination unreachable errors
{: #resolve-destination-unreachable}

The "Destination host unreachable" error indicates a routing misconfiguration between the VPC and on-premises network. Even if the VPN tunnel is active, traffic fails when routes are missing or incorrect, on either side of the VPN connection.

1. **Use traceroute to identify the failure point**.

   ```sh
   traceroute <destination-ip>
   ```

   * Determine whether traffic reaches the VPN gateway.
   * If the VPN gateway private IP appears in the traceroute output, routing to the gateway is working correctly.
   * If the VPN gateway does not appear, verify the routing configuration.
   * If the traceroute stops at the VPN gateway, investigate connectivity beyond the gateway.

1. **Verify VPC routing**.

   * Review the routing table associated with the workload subnet.
   * Verify that a route exists for the on-premises CIDR range and that the route points to the correct VPN connection or gateway private IP.
   * If the workload and VPN gateway are in the same subnet, move the workload to a separate subnet.

1. **Verify route propagation**: Ensure that the **Accept routes from VPN gateway** option is enabled in the ingress and egress routing table for policy-based VPN.

1. **Verify on-premises routing**

   * Verify that routes to the VPC CIDR ranges exist on the on-premises network.
   * Ensure that return traffic is routed through the VPN tunnel.
   * Check for asymmetric routing or alternate preferred paths that bypass the VPN.

## Verifying and testing the connection
{: #verify-routing}

After resolving routing issues, follow these steps to verify proper operation:

1. Check routing tables and verify that all required routes are present. Confirm that the next hops are correct and that no conflicting routes exist.

1. Test connectivity from multiple sources such as different subnets in VPC, and different networks on-premises. Verify that all expected paths work.

1. Use `traceroute` to confirm paths and verify traffic that traverses the VPN gateway. Check that routes are optimal and ensure that no unexpected hops are present.

1. Monitor route advertisements for Transit Gateway and verify that routes appear in the route report. Check that routes are advertised correctly and no conflicts or quota issues exist.

1. Test the connection after changes and verify that the routing remains stable after VPN rekey. Test after adding new subnets or connections and confirm that routes persist after gateway recovery.

## Related links
{: #vpn-routing-reachability-related}

* [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes)
* [Creating a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table)
* [Configuring route propagation for VPN gateways](/docs/vpc?topic=vpc-advertise-routes-s2s)
* [About IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about)
* [Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations)
