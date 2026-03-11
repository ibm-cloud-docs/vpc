---

copyright:
  years: 2025, 2026
lastupdated: "2026-03-11"

keywords: VPN, vpn gateways, performance, best practices

subcollection: vpc

content-type: tutorial
services: vpc
account-plan: paid

---

{{site.data.keyword.attribute-definition-list}}

# Improving site-to-site VPN throughput and performance
{: #vpn-performance}
{: toc-content-type="tutorial"}
{: toc-services="vpc"}

Follow these recommended best practices to optimize site-to-site VPN throughput while maintaining stable connectivity. You can maximize performance by configuring the VPN gateway, cryptographic settings (using IKEv2 and AES-GCM), routing, and network parameters (MTU/MSS). When deployed in active-active mode with distributed traffic enabled, a route-based VPN can support up to a maximum of 2 Gbps aggregate throughput under optimal conditions.
{: shortdesc}

Throughput might be lower in the following scenarios:

* A policy-based VPN is used.
* Distributed traffic is not enabled.
* Only a single traffic flow is active.
* The peer VPN device is CPU constrained.
* Network fragmentation occurs.

Actual throughput depends on peer device capacity, available ISP bandwidth, routing configuration, packet size, traffic patterns, and other environmental factors. The following guidance covers mode selection, cryptographic tuning, network configuration, and operational best practices to help you approach optimal performance.

## Before you begin
{: #before-vpn-performance}

To achieve optimal results, follow these recommendations:

* Test performance in your own test environment before deploying to production.
* Ensure that your on-premises VPN device meets your throughput requirements.
* Confirm that your ISP bandwidth aligns with your expected VPN capacity.

Complete the following prerequisites before you proceed:

* Make sure that your site‑to‑site VPN gateway is provisioned and that both VPN tunnels are active.
* Verify that you have permission to modify configuration settings on the on-premises peer device.
* Confirm that the gateway and peer device support IKEv2 and `AES‑GCM`.
* Verify that you have the permissions to modify MTU and MSS settings on your on‑premises device.
* Install `iperf` on both an IBM Cloud virtual server instance and your on‑premises host to validate performance.

## Select VPN mode for maximum throughput
{: #reviewing-vpn-throughput}
{: step}

IBM Cloud supports both route-based and policy-based site-to-site VPN modes. Your choice of mode directly affects achievable throughput.

### Route-based VPN (recommended)
{: #route-based-vpn}

Route-based VPN provides the highest throughput potential. When deployed in active-active mode with **Distribute traffic** enabled, traffic can flow across both tunnels simultaneously.

To maximize throughput:

1. Use route-based VPN.
1. Configure tunnels to both the route-based VPN public IP addresses.
1. Enable **Distribute traffic** so that both tunnels actively forward traffic.
1. Generate multiple concurrent traffic flows when testing to fully use available bandwidth.

Observed performance under controlled testing shows that a single tunnel can handle approximately 1.6 Gbps, with an aggregate of up to 2 Gbps across both tunnels when configured with recommended architectural and cryptographic settings. These figures are examples from testing and are not guaranteed in your environment.

If distributed traffic is disabled, only one tunnel carries traffic at a time, typically resulting in lower overall throughput.

### Policy-based VPN
{: #policy-based-vpn}

Policy-based VPN supports only a single active tunnel at a time. The secondary tunnel becomes active only during failover.

Because traffic can't be distributed across tunnels, throughput is typically lower than route-based VPN. Use policy-based VPN only if required by your network topology or peer device limitations.

### Encryption algorithms: AES vs AES-GCM
{: #encryption-algorithms}

VPN traffic is encrypted with IPsec, and the choice of encryption algorithm affects both security and performance:

* **AES (Advanced Encryption Standard)** – A widely used encryption standard that provides strong security. When used with separate hashing for integrity (AES-CBC), it incurs slightly higher overhead.

* **AES-GCM (Galois/Counter Mode)** – A modern variant of AES that combines encryption and integrity checking in a single operation. AES-GCM supports parallel processing and generally provides higher throughput than standard AES-CBC.

Use AES-GCM whenever supported by both VPN peers to maximize throughput without compromising security.

The following table summarizes minimum example benchmark throughput values that are tested within IBM’s internal network. These figures represent what was observed under controlled test conditions and are "not" a guarantee of actual performance in your environment. Actual throughput depends on peer device capacity, available ISP bandwidth, routing configuration, packet size, traffic patterns, compute capacity, and other network conditions.

|VPN mode                     | AES only         | AES-GCM          |
|-----------------------------|------------------|------------------|
| Route-based distributed     | ~1.6 Gbps        | ~1.6 Gbps        |
| Route-based non-distributed | ~674 Mbps        | ~1.11 Gbps       |
| Policy-based                | ~598 Mbps        | ~1.11 Gbps       |
{: caption="Example benchmark throughput for route-based and policy-based VPN with AES and GCM cipher suites." caption-side="bottom"}

## Configure cryptographic settings for performance
{: #configure-cryptographic-settings}
{: step}

In a site-to-site VPN between an IBM Cloud VPC and your on-premises network, traffic is encrypted using the IPsec protocol. Before encryption begins, a secure key exchange is performed by using the Internet Key Exchange (IKE) protocol, which consists of two phases:

* Phase 1: Establishes a secure and authenticated communication channel between VPN peers.
* Phase 2: Negotiates the IPsec Security Associations (SAs) that are used to encrypt actual data traffic.

Cryptographic choices directly affect VPN performance because encryption, hashing, and key exchange operations consume CPU resources on both peers. Selecting efficient and secure algorithms helps maximize throughput and maintain stable connectivity.

### Select the IKE protocol version
{: #ike-version}

IKE establishes the secure, authenticated communication channel between VPN devices. It negotiates security parameters, exchanges keys, and sets up the encrypted tunnel.

IBM Cloud supports both IKEv1 and IKEv2. However, IKEv2 is recommended because it:

* Provides faster negotiation
* Reduces overhead
* Improves stability
* Minimizes rekeying issues

   For more information, see [What is a rekey collision in site-to-site VPNs?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui&loginMethod=federated#faq-vpn-14).
   {: note}

Use IKEv2 whenever supported by your peer device.

#### Phase 1 (IKE) cryptographic settings
{: #ike-phase1}

During Phase 1, VPN peers authenticate each other and establish a secure channel for further negotiation. Hashing algorithms help ensure that the data that is sent over the VPN tunnel isn't tampered with. They create a unique fingerprint (hash) of the data, which is verified at the receiving end. For more information, see [supported algorithms in Phase 1](/docs/vpc?topic=vpc-using-vpn&interface=api#ike-auto-negotiation-phase-1).

Configure the following values for Phase 1:

* Authentication: `SHA‑256` or `SHA‑384`
* Diffie-Hellman (DH) group: `14` or `19`
* Lifetime: Default unless your environment requires shorter intervals

Ensure that the selected hashing algorithm is supported by your peer network. Otherwise, use `SHA-256` or `SHA-384` unless stronger algorithms are required for compliance.
{: note}

#### Phase 2 (IPsec) cryptographic settings
{: #ipsec-phase2}

During Phase 2, VPN peers negotiate IPsec SAs that define how actual data traffic is encrypted. Encryption ciphers scramble the data and protect the confidentiality of your traffic so that only authorized parties can read it. For more information, see [supported algorithms in Phase 2](/docs/vpc?topic=vpc-using-vpn&interface=api#ipsec-auto-negotiation-phase-2).

Configure the following values for Phase 2:

* Encryption: `AES‑GCM` (recommended)
* Lifetime: Default for stability

`AES-GCM` typically delivers higher throughput than `AES-CBC`. It supports parallel processing and provides built-in integrity protection, eliminating the need for separate hashing.
{: tip}

## Optimize MTU and MSS to prevent packet fragmentation
{: #optimize-mtu-mss}
{: step}

Network parameters such as MTU (Maximum Transmission Unit) and MSS (Maximum Segment Size) can directly affect VPN throughput. Fragmentation or packet loss reduces performance.

To optimize MTU and MSS, follow these steps:

1. Set the MSS to `1360` bytes on your on‑premises device. This value accounts for IPsec overhead and avoids fragmentation in TCP packets. For more information, see [MSS clamping](/docs/vpc?topic=vpc-about-mtu&interface=ui#mss-clamping) to limit the MSS of TCP packets.

1. Validate the MTU by using **ping** tests, which check the size of the packet.

   ```sh
   ping -s 1472 -M do DESTINATION
   ```
   {: pre}

   Where:

   `-s 1472`
      : Sends a packet with `1472` bytes of payload.

   `-M do`
      : Sets the "Don't Fragment" (DF) flag to prevent fragmentation.

   `DESTINATION`
      : IP address or hostname you want to test.

   On Windows:

   ```sh
   ping www.example.com -f -l 1472
   ```
   {: pre}

   Where:

   `-f`
      : Sets the "Don't Fragment" flag in the packet, which helps you test the maximum size that can be sent without fragmentation.

   `-l`
      : Specifies the payload size in bytes excluding headers.

   For IBM VPN for VPC, the MTU is `1500` bytes and the recommended MSS is `1360` bytes. If fragmentation occurs, reduce MTU to `1490` bytes.

Optimizing these parameters can help ensure full utilization of available VPN throughput.

## Configure the VPN gateway
{: #configure-the-vpn-gateway}
{: step}

Optimizing the VPN gateway ensures that traffic can flow efficiently and both tunnels are used when supported.

To optimize the VPN gateway settings, follow these steps:

1. Enable a route‑based VPN mode with distributed traffic.

   * **Route-based VPN** – Traffic flows through both VPN tunnels simultaneously when the "Distribute traffic" option is enabled. Also, connecting more peers to the gateway enables better distribution of traffic loads, which can improve the overall throughput. For more information, see [Distributing traffic for a route-based VPN](/docs/vpc?topic=vpc-using-vpn&interface=ui#use-case-4-vpn).

      In a static route-based VPN connection without distributed traffic, only one tunnel is used, which reduces overall throughput.
      {: note}

   * **Policy-based VPN** – Only one VPN tunnel is active at a time. The secondary tunnel becomes active only if the primary fails, which lowers throughput. "Distribute traffic" isn't available in this mode.

1. Make sure that the VPN gateway is in the same availability zone as your VPC subnets to avoid cross‑zone latency. For more information, see [When isn't traffic routed through a route-based VPN gateway?](/docs/vpc?topic=vpc-faqs-vpn&interface=ui&loginMethod=federated#vpn-gateway-route).

1. Move high‑traffic tunnels to a dedicated VPN gateway because the overall throughput depends on the gateway total capacity. If you have created multiple connections on a single gateway, consider moving them to a separate gateway, which helps reduce load on shared appliances and improves throughput stability.

1. Disable unused tunnels because these can increase the IKE and rekeying overhead, all of which must be encrypted and decrypted.

1. Ensure that routes are properly propagated to the VPC routing tables. The source IP addresses must match the configured VPN local subnet ranges to prevent traffic bypass. Also, make sure that no overlapping CIDRs exist.

1. Enable NAT‑T. If your on-premises VPN device is behind NAT, or ESP (Encapsulating Security Payload) traffic is blocked by intermediate devices. NAT-T encapsulates IPsec packets in UDP, allowing traffic to traverse NAT devices.

## Review operational considerations
{: #review-operational-considerations}
{: step}

Other operational factors can affect VPN performance:

* Make sure that firewalls in your peer network allow IBM Cloud VPC CIDRs to avoid throttling and rate-limiting errors.
* Verify that your ISP does not limit network traffic.
* When connecting other environments or workloads through Transit Gateway, use policy-based VPN only (route-based is not supported in this topology). For more information, see [Configuring route propagation for VPN gateways](/docs/vpc?topic=vpc-advertise-routes-s2s).

## Validate throughput
{: #validate-throughput}
{: step}

Regularly test VPN throughput by using network performance tools such as `iperf` so that performance remains within expected ranges. You must install `iperf` on both the server and client systems. To use `iperf`, follow these steps:

1. On the server (IBM Cloud or on-premises host), start the `iperf` server that listens for incoming connections:

   ```sh
   iperf -s
   ```
   {: pre}

1. On the client, initiate a test from the client to the server:

   ```sh
   iperf -c SERVER_IP_ADDRESS
   ```
   {: pre}

1. Review output for bandwidth, transfer speed, and other metrics.

By following these best practices, you can significantly improve site-to-site VPN throughput while maintaining secure and stable connectivity.

After you complete these steps, measure your VPN throughput again and compare the results with the baseline values that you captured before making changes. This comparison helps confirm that the optimizations are effective and allows you to identify any remaining bottlenecks, such as peer device CPU limits, ISP constraints, or traffic pattern inefficiencies.

If throughput still does not meet your requirements, review each configuration area methodically and validate performance after every adjustment. Continuous testing and tuning helps ensure that your VPN deployment operates as efficiently and reliably as possible within your environment.

## Related links
{: #related-link-vpn-gateways}

* [Known issues for VPN gateways](/docs/vpc?topic=vpc-vpn-limitations)
* [FAQs for VPN gateways](/docs/vpc?topic=vpc-faqs-vpn)
* [Troubleshooting VPN gateways](/docs/vpc?group=tbs-site-to-site-vpn-gateways)
