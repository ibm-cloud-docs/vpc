---

copyright:
  years: 2018, 2026
lastupdated: "2026-08-20"

keywords: virtual private network, VPN, VPN gateway, troubleshooting, connection, tunnel, negotiation, IKE, IPsec, BGP, suspended

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I establish or maintain a VPN connection?
{: #troubleshoot-vpn-connection-establishment}
{: troubleshoot}
{: support}

VPN connection issues can prevent tunnel establishment or cause established connections to become unstable. These issues commonly arise from configuration mismatches, protocol negotiation failures, network problems, or resource state issues.
{: shortdesc}

You might experience one or more of the following issues with your VPN connection:
{: tsSymptoms}

* The VPN tunnel fails to establish initially.
* The connection establishes but frequently disconnects and reconnects.
* The connection status shows `Down` even though the gateway appears healthy.
* The tunnel tears down after periods of inactivity.
* Existing connections fail when you add new connections.

VPN connection failures can occur due to several interconnected factors:
{: tsCauses}

**Firewall restrictions**
:   A firewall blocks the required ports on your peer network, which can prevent VPN traffic from passing through.

**Configuration mismatches**
:   Occur due to misconfigured IKE and IPsec parameters, CIDR ranges, or authentication settings between IBM Cloud and your peer device.

**Protocol negotiation failures**
:   Occur due to problems with IKE security association (SA) negotiation, traffic selector narrowing, or duplicate security association detection.

**Network and timing issues**
:   Caused due to disabled NAT-Traversal, idle timeouts, or aggressive rekey behavior between devices.

Follow these steps to diagnose and resolve VPN connection establishment issues:
{: tsResolve}

## Diagnosing VPN connection issues
{: #diagnose-vpn-connection-issues}

Before you modify the VPN configuration, determine where the failure occurs.

1. **Check VPN gateway and connection status**:
   1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
   1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **VPNs**.
   1. Check the VPN gateway lifecycle state (active, suspended, failed) and review the connection status (`Up`, `Down`). Confirm that the gateway is in an active state and that the connection isn't degraded or faulty.

   For more information, see [VPN gateway states and descriptions](/docs/vpc?topic=vpc-vpn-states), and [Diagnosing VPN gateway health](/docs/vpc?topic=vpc-vpn-health).

1. **Enable NAT-Traversal and open required ports**: NAT-Traversal (NAT-T) enables VPN IPsec traffic to pass through devices that use Network Address Translation (NAT) by encapsulating IPsec packets inside UDP packets. This configuration is essential for VPN connections, especially when NAT devices exist between peers.

   * Enable NAT-T on your peer device if it's a configurable option. For more information, see [Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations).

   * Verify that UDP ports `500` and `4500` are allowed for traffic flow on the peer device firewall, any intermediate firewalls, VPC Network ACLs, and security groups. For more information, see [Configuring network ACLs for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn&interface=ui).

   If NAT-T is disabled and a NAT device exists in the network path, tunnel establishment or bidirectional traffic can fail.
   {: note}

1. **Verify network connectivity**: Ensure that network traffic that is required for VPN negotiation is allowed between peers and that intermediate network devices don't block VPN traffic.

1. **Review VPN logs**: Review logs on both IBM Cloud VPN and your peer device for the following indicators:
   * IKE negotiation failures, `TEMPORARY_FAILURE`, or duplicate `IKE_SA` messages
   * Rekey failures or timeout messages
   * Traffic selector negotiation results

   These messages often identify whether the issue occurs during Phase 1 negotiation, Phase 2 negotiation, or route establishment. For more information on how to check IPsec logs, see [How do I check IPsec logs?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui#faq-vpn-19)

1. Check for overlapping CIDRs to the same peer, conflicting routes, or mismatched IKE and IPsec policies. Rectify the configuration mismatches.

## Resolving configuration and negotiation issues
{: #resolve-configuration-issues}

VPN connection failures often result from configuration mismatches or negotiation issues between IBM Cloud and the peer device.

### Match IKE and IPsec parameters
{: #match-parameters}

IKE and IPsec algorithm configuration mismatches are the most common cause of connection failures.

1. Verify that the following Phase 1 (IKE) and Phase 2 (IPsec) settings match exactly on both sides:
   * Encryption algorithm (`AES-128`, `AES-256`)
   * Authentication algorithm (`SHA-256`, `SHA-384`)
   * Diffie-Hellman group
   * IKE version (`IKEv1` or `IKEv2`)

   When you use multiple IKE and IPsec algorithms with array-based properties, consider the update behavior to prevent negotiation issues. For more information, see [Updating to multiple IKE and IPsec algorithms](/docs/vpc?topic=vpc-vpn-update-multiple-algorithms&interface=api).
   {: note}

1. Check Perfect Forward Secrecy (PFS) settings. A mismatch in PFS settings between VPN peers can prevent successful Phase 2 IPsec negotiation.

   When you create an IPsec policy, PFS generates a unique encryption key for each IPsec session, which prevents compromise of one key from affecting past or future sessions. For more information, see [Creating an IPsec policy](/docs/vpc?topic=vpc-creating-ipsec-policy&interface=ui). PFS is disabled by default in {{site.data.keyword.vpn_vpc_short}} Phase 2. If PFS is enabled on your peer, create a custom IPsec policy with PFS enabled.
   {: important}

1. Ensure that the IKE ID of the peer device is its public IP address.

1. When the configured peer identity type doesn't match what the remote peer sends, you might see the following error:

   ```sh
   constraint check failed: identity required
   AUTH_FAILED
   ```
   {: screen}

   * In this case, check the identity of the remote peer (from VPN logs or remote device configuration).
   * If the configured peer identity and received identity don't match, create a new VPN connection and set the peer identity to IP address if the remote peer uses IP-based identity. For more information, see [Adding connections to a VPN gateway](/docs/vpc?topic=vpc-vpn-adding-connections&interface=ui).

### Configure correct traffic selectors
{: #configure-traffic-selectors}

A traffic selector is the local and remote IP subnet (CIDR) pair that defines which network traffic is allowed to pass through a VPN tunnel. Traffic selector issues can cause connection failures in policy-based VPNs, especially with multiple CIDRs.

**For policy-based VPNs:**

1. Ensure that each VPN connection has unique local and remote CIDR pairs. Do not configure multiple VPN connections with identical traffic selectors to the same peer IP.

1. In case multiple CIDR pairs don't work, create separate VPN connections, one for each CIDR pair to match the peer behavior. Alternatively, use route-based VPN, which handles multiple CIDRs more effectively. For more information, see [Why isn't traffic flowing through my established VPN connection?](/docs/vpc?topic=vpc-troubleshoot-vpn-traffic-flow)

1. Verify that your peer device supports multiple CIDRs per tunnel.

   Some peer devices support only one negotiated CIDR pair and narrow traffic selectors. This behavior causes traffic from other CIDRs to fail.
   {: note}

### Prevent duplicate IKE security associations
{: #prevent-duplicate-sa}

When you add new connections to an existing VPN gateway, duplicate SA detection can tear down active tunnels. This behavior is observed with certain peer vendor devices due to peer timeout events.

1. Before you create a new connection, verify that it doesn't have identical or overlapping `local_cidrs` or `peer_cidrs`.

1. If multiple connections must share a peer IP, ensure that each connection has unique traffic selectors (CIDRs). Use the same Dead Peer Detection (DPD) values across all connections and configure unique policies on the peer firewall for each tunnel.

1. If the VPN logs contain messages such as `Got a response on a duplicate IKE_SA`, review all connections for overlapping traffic selectors, duplicate peer definitions, or simultaneous tunnels to the same peer. If necessary, distribute connections across multiple VPN gateways.

## Resolving network and timing issues
{: #resolve-network-timing}

Rekey behavior, and idle timeout configuration can cause network and timing‑related VPN issues.

### Optimize rekey and negotiation behavior between peers
{: #optimize-rekey-peers}

Simultaneous rekeying from both sides can cause connection instability.

1. Configure rekey timers so that only one side initiates the rekey.

   Configure the peer device IKE SA (Phase 1) and IPsec SA (Phase 2) lifetimes to be slightly shorter than the IBM Cloud gateway lifetimes. This configuration helps ensure that the peer initiates rekeying before the gateway.
   {: tip}

1. Monitor logs during rekey events to confirm successful negotiation without errors.

### Optimize rekey settings for distributed traffic
{: #optimize-rekey-distributed-traffic}
 
When you use a route-based VPN with Distributed Traffic enabled, traffic can be distributed across multiple active tunnels. During IKE or IPsec rekey operations, a tunnel is briefly torn down and reestablished. If multiple tunnels rekey simultaneously, traffic can be temporarily interrupted while the tunnels are reestablished.
 
To reduce the likelihood of traffic disruption, follow these steps:
 
1. Configure different IKE SA (Phase 1) and IPsec SA (Phase 2) lifetime values for each tunnel.
1. Ensure that the lifetime values are staggered so that rekey events occur at different times.

   For example:
 
   Tunnel 1:
   - IKE SA lifetime: `24 hours`
   - IPsec SA lifetime: `8 hours`

   Tunnel 2:
   - IKE SA lifetime: `16 hours`
   - IPsec SA lifetime: `6 hours`

   Using different lifetime values helps prevent multiple tunnels from rekeying simultaneously and reduces the possibility of temporary traffic loss during tunnel reestablishment.
   {: tip}

### Handle idle timeouts
{: #handle-idle-timeouts}

Tunnels that tear down after inactivity indicate peer-side idle timeout issues.

1. Increase the idle timeout on your peer device from 30 minutes to several hours, or disable it entirely if your organization's security policy allows it.

1. If you can't change peer timeout settings, configure IKE keepalive or DPD on the peer device. If native keepalive mechanisms are unavailable or ineffective, generate periodic traffic such as ICMP pings at an interval shorter than the configured idle timeout. This approach keeps the security association active without requiring long timers.

1. Review Dead Peer Detection (DPD) timers and retry settings to balance responsiveness and stability. Excessively aggressive DPD settings can cause peers to declare a tunnel unavailable during periods of packet loss, latency, or transient network congestion.

1. For applications, implement an automatic retry logic for the first request after idle periods.

## Troubleshooting VPN status inconsistencies and performance issues
{: #vpn-performance-issues}

VPN status inconsistencies and performance issues can occur even when the tunnel configuration is correct. These issues are often caused by traffic flow patterns, peer device behavior, network instability, or incorrect MTU and MSS settings.

### Troubleshooting VPN status inconsistencies
{: #vpn-status-inconsistencies}

VPN status can sometimes appear inconsistent between the cloud side and the peer side, or the reported status might not match the actual tunnel state.

1. Verify that Phase 1 (IKE) and Phase 2 (IPsec) negotiations between the VPN gateway and peer device complete successfully, and review VPN logs for tunnel establishment or negotiation failures.

1. Check whether you receive traffic from the peer network. Some peer devices establish or maintain tunnels only when traffic originates from their side. Generate test traffic, such as a `ping`, from the peer network and observe whether the reported tunnel status changes.

1. If the status remains inconsistent, review logs for rekey events, or other transient conditions that might temporarily affect tunnel state reporting.

### Troubleshooting high latency and packet loss
{: #high-latency-packet-loss}

High latency and packet loss over a VPN connection can occur when the underlying network device (for example, a firewall or gateway) becomes unstable or stops processing traffic correctly.

1. Use `ping`, `traceroute`, or similar tools to verify packet loss and latency between VPN endpoints and identify where network degradation occurs.

1. Check the health of firewalls and VPN gateways. Review peer gateway device logs for routing, interface, or resource-related errors. If IBM VPN gateway doesn't look healthy, contact [IBM Cloud support](/unifiedsupport/cases/form) by opening a support case.

1. If the affected peer gateway device appears unhealthy, restart it or trigger a failover in highly available deployments, then retest connectivity to confirm that latency and packet loss are resolved.

### Troubleshooting intermittent outages and slow data transfer
{: #intermittent-outages-slow-transfer}

Intermittent VPN outages can occur when the peer device temporarily stops responding to VPN keepalive messages, which causes the tunnel to go down and reconnect. Slow data transfer can be caused by MTU or MSS misconfiguration, packet fragmentation, packet loss, or other network performance issues.

1. Verify connectivity stability between VPN endpoints by running continuous `ping` or MTR tests and review peer logs for dropped IKE or IPsec packets, device restarts, or missed keepalive messages.

1. Configure TCP MSS to `1360` and enable MSS clamping on the peer device if supported, then retest application traffic. If performance issues persist, collect packet captures on both ends of the tunnel for further analysis. For more information, see [Optimizing MTU and MSS to prevent packet fragmentation](/docs/vpc?topic=vpc-vpn-performance&interface=ui#optimize-mtu-mss), and [Improving site-to-site VPN throughput and performance](/docs/vpc?topic=vpc-vpn-performance&interface=api).

## Verifying the VPN connection
{: #verify-vpn-connection}

After you apply configuration changes, verify that the VPN connection operates correctly.

1. Confirm that the VPN connection status is reported as `Up` and that test traffic can successfully pass through the tunnel.

1. Monitor the connection through one or more rekey intervals and review logs for warnings, retransmissions, or negotiation failures that might indicate instability.

## Related links
{: #vpn-connection-establishment-related}

* [About VPN gateways](/docs/vpc?topic=vpc-using-vpn)
* [Creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway)
* [Why do routing and reachability issues exist with my site-to-site VPN?](/docs/vpc?topic=vpc-troubleshoot-vpn-routing-reachability)
* [Configuring ACLs and security groups for use with VPN](/docs/vpc?topic=vpc-configuring-acls-vpn&interface=ui)
