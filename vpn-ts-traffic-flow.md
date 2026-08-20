---

copyright:
  years: 2018, 2026
lastupdated: "2026-08-20"

keywords: virtual private network, VPN, VPN gateway, troubleshooting, traffic, packets, ACL, security groups, flow

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why isn't traffic flowing through my established VPN connection?
{: #troubleshoot-vpn-traffic-flow}
{: troubleshoot}
{: support}

When your VPN connection shows as active but application traffic fails to flow, the issue typically involves access control configurations, routing problems, or traffic selector mismatches. The following sections help you diagnose and resolve traffic flow issues.
{: shortdesc}

You might experience one or more of the following traffic flow problems:
{: tsSymptoms}

* VPN connection status shows as `Up` but no traffic passes through.
* Ping or connection attempts result in _Destination host unreachable_ errors.
* Traffic stops working after updating Network ACL rules.
* For policy-based VPN, some IP addresses can communicate while others cannot (with multiple CIDRs configured).
* For a route-based VPN, IPsec and BGP sessions are established but packets don't flow.
* Traffic works in one direction but not the other (asymmetric traffic).

Traffic flow failures can occur when the VPN tunnel is established but security controls, routing configurations, or protocol limitations prevent packets from traversing the connection:
{: tsCauses}

**Access control restrictions**
:   Network ACLs or security groups block required VPN traffic or application protocols.

**Routing misconfigurations**
:   Missing or incorrect routes prevent traffic from reaching the VPN gateway or destination networks.

**NAT and network path issues**
:   Disabled NAT-Traversal, asymmetric routing, or network path issues block traffic flow.

**Traffic selector limitations**
:   In policy-based and route-based VPNs, the peer device negotiates only a subset of configured CIDRs.

**Transit Gateway issues**
:   Missing Transit Gateway connections or routes prevent packet forwarding between interconnected VPCs.

Follow these steps to diagnose and resolve traffic flow issues:
{: tsResolve}

## Diagnosing traffic flow issues
{: #diagnose-traffic-flow}

Follow this systematic approach to identify where traffic is being blocked:

1. **Verify VPN tunnel establishment**:

   1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
   1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPNs**.
   1. Verify that the Connection status shows as `Up`.
   1. For route-based VPNs, verify whether the BGP state shows as `Established`.
   1. Check for error messages in the connection health details.

   For more information, see [VPN gateway states and descriptions](/docs/vpc?topic=vpc-vpn-states), and [Diagnosing VPN gateway connection health](/docs/vpc?topic=vpc-vpn-health#vpn-connection-health).

1. **Check NAT-Traversal configuration**: NAT-Traversal (NAT-T) enables VPN IPsec traffic to pass through devices that use Network Address Translation (NAT) by encapsulating IPsec packets inside UDP packets. This configuration is essential for proper VPN operation.

   * Enable NAT-T on your peer device if it's a configurable option. For more information, see [Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations).

   * Verify that UDP ports `500` and `4500` are open on the peer device firewall, any intermediate firewalls, VPC Network ACLs, and security groups.

   * From VPN logs, look for logs of communication over port `4500`. For example:

   ```sh
   received packet: from x.x.x.x[4500] to y.y.y.y[4500] (80 bytes)
   sending packet: from y.y.y.y[4500] to x.x.x.x[4500] (80 bytes)
   ```
   {: screen}


   When NAT-T is enabled, phase 2 communication happens through UDP `4500`. If NAT-T is disabled and a NAT device exists between peers, traffic can fail even if the tunnel appears up.
   {: note}

1. Check VPN logs on both sides for:
   * Successful completion of IKE Phase 1 and Phase 2 negotiation
   * Active IPsec Security Associations (SAs)
   * No rekey failures or timeout messages

   For more information on how to check IPsec logs, see [How do I check IPsec logs?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui#faq-vpn-19). If you have access to peer device's logs, verify whether traffic fails in both directions or only one direction to check for asymmetric failure.

1. **Test basic connectivity**:

   1. From a virtual server instance in your VPC, attempt to ping or connect to an on-premises resource:

      ```sh
      ping <on-premises-ip>
      ```
      {: codeblock}

   1. From your on-premises network, attempt to ping or connect to a VPC resource:

      ```sh
      ping <vpc-instance-ip>
      ```
      {: codeblock}

   1. Use `traceroute` to identify where packets stop:

      ```sh
      traceroute <destination-ip>
      ```
      {: codeblock}

1. If the VPN gateway's private IP appears as a next hop in the routing table, traffic is correctly reaching the gateway. If not, the routing is misconfigured.

   * For policy based VPN, verify if the VPN's private IP is the next hop in the routing table. If it isn't, refresh routes through one of the two ways:
    - Remove VPN Gateway from routing table's **Accept routes from list**, then add it back.
    - Disable and re-enable the VPN connection.
   * For route based VPN, verify that next hop is the correct VPN connection for the desired traffic.

1. Test multiple traffic types. If `ping` (ICMP) fails but the VPN tunnel remains `Up`, test TCP or UDP connectivity with tools such as `telnet`, `nc`, or application-specific clients. Some firewalls or ACLs block ICMP traffic while allowing application traffic.

## Resolving access control issues
{: #resolve-access-control}

Network ACLs and security groups are the most common cause of traffic flow failures in established VPN connections.

### Check Network ACLs configuration
{: #configure-acls}

Network ACLs are stateless and require explicit rules for both inbound and outbound traffic.

1. Identify the ACL attached to the subnet that is hosting your virtual server instances and the ACL attached to the subnet that is hosting the VPN gateway.

1. Ensure that both ACLs allow UDP traffic on ports `500` and `4500` for VPN negotiation and encapsulation. The following table shows the ACL configuration for correct traffic flow:

   | Direction | Protocol | Source | Destination | Source Port | Dest Port | Action |
   | --------- | -------- | ------ | ----------- | ----------- | --------- | ------ |
   | Inbound | UDP | Any | Any | Any | 500 | Allow |
   | Inbound | UDP | Any | Any | Any | 4500 | Allow |
   | Outbound | UDP | Any | Any | 500 | Any | Allow |
   | Outbound | UDP | Any | Any | 4500 | Any | Allow |
   {: caption="Required ACL rules for VPN traffic" caption-side="bottom"}

1. Adjust the rules to match your networks. For example, for traffic to flow from VPC subnet `10.0.0.0/16` to on-premises network `192.168.0.0/16`, configure the following rules on the VPC subnet ACL:

   | Direction | Protocol | Source | Destination | Action |
   | --------- | -------- | ------ | ----------- | ------ |
   | Outbound | All or specific | `10.0.0.0/16` | `192.168.0.0/16` | Allow |
   | Inbound | All or specific | `192.168.0.0/16` | `10.0.0.0/16` | Allow |
   {: caption="ACL rules for application traffic" caption-side="bottom"}

1. Ensure that ACLs allow the specific protocols that are required by your applications. For example, ICMP traffic for ping tests might be blocked even when TCP or UDP application traffic is allowed.

1. If you replace broad "allow any-to-any" rules with restrictive TCP-only rules, you might inadvertently block required VPN flows. When you implement restrictive rules, consider the following conditions:

   * Allow all protocols between VPN gateway subnet and workload subnets.
   * Test the connection thoroughly after each ACL change and verify that no implicit deny rules exist that can block traffic.

   To confirm ACLs are the issue, temporarily add an allow-all rule and test connectivity. If traffic flows, the problem is ACL configuration. For more information on ACL configuration guidance, see [About network ACLs](/docs/vpc?topic=vpc-using-acls), and [Configuring network ACLs for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn).
   {: tip}

### Check security groups configuration
{: #configure-security-groups}

Security groups are stateful but still require proper configuration for VPN traffic.

1. **For virtual server instances**, ensure that security groups allow:
   * Inbound traffic from on-premises CIDR ranges
   * Outbound traffic to on-premises CIDR ranges
   * Required application protocols and ports

1. Verify that no security group is blocking traffic at the subnet level to the other resources.

## Resolving routing issues
{: #resolve-routing-issues}

Correct routing is required on both the IBM Cloud and on-premises networks. Missing or incorrect routes can prevent traffic from reaching its destination even when the VPN connection is up.

### Verify VPC routing configuration
{: #verify-vpc-routing}

1. **Check the routing table** associated with your workload subnet.

   * Go to **Infrastructure > Network > Routing tables**.
   * Select your VPC and identify the routing table for the subnet.
   * Verify that a route exists for the on-premises CIDR pointing to the VPN connection.

1. **Verify policy-based VPN routing configuration**.

   * Go to **Infrastructure > Network > Routing tables**.
   * Check whether the routing table has **Accept routes from VPN gateway** turned on, which allows the VPN gateway to automatically create routes in the routing table for peer CIDRs. For more information, see [Creating a route](/docs/vpc?topic=vpc-create-vpc-route&interface=ui).

   For policy-based VPN, don't create any routes manually to on-premises networks that overlap with VPN-managed routes. This action can cause routing conflicts.
   {: note}

1. **Avoid VPN gateway and workload in the same subnet**: If the VPN gateway and virtual server instances are in the same subnet, routing issues can occur. Move workloads to a separate, nonoverlapping subnet.

1. **Verify route-based VPN routing configuration**.

   * Verify that the routes in the routing table point to the VPN gateway and not to private IP addresses. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
   * Check that routes don't create loops or conflicts.
   * Ensure that the `distribute_traffic` property is set when the VPN uses dual tunnels. For more information, see [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn&interface=ui#use-case-4-vpn).

   For more information on resolving routing and reachability issues, see [Why do routing and reachability issues exist with my site-to-site VPN?](/docs/vpc?topic=vpc-troubleshoot-vpn-routing-reachability).

1. **Verify route avertisement to Transit Gateway**

   * When your architecture integrates Transit Gateway for forwarding traffic from another environment to the VPN gateway VPC, check whether the **Advertise to** toggle is turned on. This option advertises the learned routes to other VPCs through the Transit Gateway. For more information, see [Configuring route propagation for VPN gateways](/docs/vpc?topic=vpc-advertise-routes-s2s).

### Verify on-premises routing configuration
{: #verify-onprem-routing}

Traffic must be routed correctly on the on-premises side:

1. Confirm that routes exist for VPC CIDRs on your on-premises network.
1. Verify that the return traffic is directed back through the VPN tunnel, not through other paths.
1. Check for asymmetric routing where traffic goes through the VPN in one direction but returns through a different path.

### Resolve Transit Gateway routing issues
{: #resolve-transit-gateway-routing}

In dynamic route-based VPNs that use Transit Gateway, route advertisement, route conflicts, or Transit Gateway configuration issues can prevent traffic from reaching its destination even when the IPsec tunnel is established.

1. **Verify Transit Gateway connection**: Confirm that the VPN is connected to the Transit Gateway and check that the service connection status shows as `Up`. Dynamic routing requires this connection even if IPsec is up.

1. **Review the Transit Gateway route report**,

   * Go to **Interconnectivity > Transit Gateway**.
   * Select your Transit Gateway and click **Generate report** in the Routes section.
   * Verify that both source and destination IP addresses appear in the report.
   * The on-premises IP addresses must be advertised from the VPN connection.
   * IBM Cloud IP addresses must be advertised from VPC or other connections.

1. **Check for route conflicts**: Look for overlapping routes in the report and resolve conflicts by adjusting VPC address prefixes. Ensure that advertised routes don't exceed quota limits. For more information, see [How to resolve VPN quota limit issues?](/docs/vpc?topic=vpc-troubleshoot-vpn-quota-issues&interface=ui).

1. **Verify ACLs on destination VPCs**: Check ACLs on both the VPN VPC and destination VPCs and make sure that appropriate _allow rules_ exist for cross-VPC traffic with Transit Gateway.

## Resolving traffic selector issues
{: #resolve-traffic-selector-issues}

In VPNs with multiple CIDRs, traffic selector narrowing causes some traffic to fail while other traffic succeeds. Policy‑based VPN relies on fixed traffic‑matching rules to decide which tunnel must carry a packet. When multiple tunnels advertise the same local and remote CIDRs, the VPN gateway cannot consistently select the correct tunnel.

### Review traffic selector narrowing
{: #review-traffic-narrowing}

When you configure multiple local and remote CIDRs on an IBM Cloud VPN connection, the VPN gateway sends all CIDRs in its traffic selector proposal. However, some peer devices respond with only one CIDR pair during IKE negotiation, a behavior called "traffic selector narrowing."

**Example scenario:**

* IBM VPN configuration:
   * Local CIDRs: `192.168.1.0/24`, `192.168.2.0/24`
   * Remote CIDRs: `10.0.1.0/24`, `10.0.2.0/24`

* Peer device negotiates between `192.168.1.0/24` and `10.0.1.0/24`

* Result:
   * Traffic from `192.168.1.5` to `10.0.1.1` works.
   * Traffic from `192.168.2.10` to `10.0.1.1` fails.

### Resolve multi-CIDR traffic issues
{: #resolve-multi-cidr}

1. **Review logs**: Review IKE and IPsec logs to identify which CIDR pairs were successfully negotiated. If a specific IP address is not present, the tunnel does not exist for that IP. If only one CIDR pair is negotiated but multiple pairs are configured, your peer device is narrowing traffic selectors.

   ```sh
   selecting traffic selectors for us:
   config: 192.168.1.0/24, received: 192.168.1.5/32 => match: 192.168.1.5/32
   config: 192.168.2.0/24, received: 192.168.1.5/32 => no match

   selecting traffic selectors for other:
   config: 10.0.1.0/24, received: 10.0.1.1/32 => match
   config: 10.0.2.0/24, received: 10.0.1.1/32 => no match
   config: 10.0.3.0/24, received: 10.0.1.1/32 => no match

   CHILD_SA peer established with TS 192.168.1.5/32 === 10.0.1.1/32
   ```
   {: screen}

   In route-based VPNs, the peer gateway can narrow the proposed traffic selectors, which may result in the tunnel covering only a subset of the intended CIDR ranges. For example, if the local side proposes `0.0.0.0/0` and the peer responds with `192.168.0.0/16`, the resulting tunnel covers only `192.168.0.0/16`. If traffic for certain networks is failing, review the VPN negotiation logs and ensure that the peer gateway configuration includes all required CIDR ranges and that the negotiated selectors match the intended tunnel coverage.
   {: note}

1. **Check peer device capabilities**: Consult peer documentation for multi-CIDR support and verify whether the peer supports multiple traffic selectors per tunnel. Check for configuration options that are related to tunnel sharing or CIDR limits. For more information, see [Connecting to your on-premises network](/docs/vpc?topic=vpc-vpn-onprem-example&interface=ui).

1. **Configure CIDR handling (for policy-based VPN)**:

   1. **Split into separate connections** (recommended): Create individual VPN connections for each CIDR pair. This setup enables each connection to negotiate its own tunnel and provides clear separation for easier troubleshooting.

   1. **Configure peer to accept multiple CIDRs**: If supported by your peer device, modify the peer configuration to allow multiple CIDRs per tunnel. After you make changes, test the setup thoroughly to ensure that all CIDR pairs are negotiated.

   1. **Consolidate CIDRs**: Use larger network ranges instead of multiple smaller ones. For example, use `10.243.0.0/24` instead of 8 separate `/32` addresses. Remove unnecessary or overlapping CIDRs that can cause traffic selector mismatches or partial connectivity failures.

1. **Generate traffic from the peer side**: Some peer devices establish IPsec tunnels only when traffic originates from their side. If traffic must originate from on-premises, generate initial traffic from the peer toward the VPN gateway, which triggers tunnel establishment for all configured CIDR pairs. After the tunnel is established, bidirectional traffic must work.

## Troubleshooting peer CIDR update issues
{: #peer-cidr-update-disruption}

When you update peer CIDRs (for example, by consolidating multiple smaller subnets into a larger CIDR block), you might notice that traffic to those networks briefly stops working. During this time, connections to the affected subnets can fail until the new routing information is applied. This condition occurs because routing entries are briefly removed before the updated routes are applied.

### Planing peer CIDR updates
{: #planning-peer-cidr-updates}

Before you update peer CIDRs, keep the following considerations in mind:

* Expect a brief traffic interruption while existing routes are removed and new routes are applied.

* Schedule CIDR updates during maintenance windows or other low-traffic periods to minimize user impact.

* Review the existing peer CIDRs and identify opportunities to consolidate smaller CIDR blocks into larger summarized ranges (for example, replacing multiple `/24` subnets with a `/23` subnet).

During the update, follow these steps:

* Remove the existing peer CIDRs and add the new consolidated CIDR block.

* Verify that the updated routes appear in the routing table.

* Test connectivity and monitor traffic to confirm that service is restored as expected.

* Communicate the expected interruption to affected users before making the change.

### Reducing the impact of configuration changes
{: #minimize-config-change-impact}

Use the following practices to minimize the impact of VPN configuration changes:

* Use route-based VPNs when possible, as they generally handle routing changes more flexibly than policy-based VPNs.

* Test configuration changes in a nonproduction environment before applying them to production workloads.

* Make changes incrementally to simplify validation and troubleshooting.

* Monitor connection status and traffic flow during and after configuration updates.

* Maintain a rollback plan by documenting the current configuration before making changes.

## Verifying and testing the connection
{: #verify-traffic-flow}

After you apply the required fixes, verify traffic flow with the following steps:

1. Test basic connectivity by sending a ping command to the peer network.

   ```sh
   ping <destination-ip>
   ```

1. Test application protocols such as SSH, RDP, HTTP, or other required protocols and test connectivity from both directions (VPC to on-premises and from on-premises to VPC).

1. Verify all CIDR pairs for multi-CIDR configurations and test connectivity from each local CIDR to each remote CIDR. Confirm that all expected traffic flows work.

1. Observe traffic flow over several hours and ensure stability through rekey events. Check that the idle connections resume.

1. Review logs and check for any warnings or errors on both sides. Verify that no packets are being dropped.

For more information on improving VPN performance, see [Improving site-to-site VPN throughput and performance](/docs/vpc?topic=vpc-vpn-performance).
{: tip}

## Related links
{: #vpn-traffic-flow-related}

* [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes)
* [Configuring ACLs and security groups for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn)
* [Using Transit Gateway with VPN](/docs/transit-gateway?topic=transit-gateway-about)
