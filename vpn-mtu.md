---

copyright:
  years: 2023
lastupdated: "2023-09-15"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# IBM site-to-site VPN Maximum Transmission Unit (MTU) clamping
{: #about-mtu}

When you configure and optimize site-to-site IPsec VPN connections, you might encounter network performance issues, one of which is related to Maximum Transmission Unit (MTU) and Maximum Segment Size (MSS) clamping. This topic delves into these concepts to help you better understand and optimize your IPsec VPN connections.

## What is MTU?
{: #what-is-mtu}

MTU refers to the maximum size of a data packet that can be transmitted in a computer network in a single frame. Typically, MTU values are fixed and measured in bytes. In Ethernet networks, the standard MTU size is 1500 bytes. When a data packet exceeds the MTU, it is fragmented into smaller segments for transmission. In the IBM Cloud VPC network, you can enable [jumbo frames](/docs/virtual-servers?topic=virtual-servers-configuring-network-performance#configuring-jumbo-frames) with an MTU of 9000 bytes to increase virtual machine performance. However, the MTU is fixed at 1500 bytes on the IBM site-to-site VPN gateway because VPN traffic must travel over the internet.

## What is MSS?
{: #what-is-mss}

MSS is a parameter specific to the Transmission Control Protocol (TCP) layer within the TCP/IP networking stack. The MSS represents the largest amount of data that can be included within a single TCP segment within an IP packet. Unlike MTU, which is determined by the network infrastructure, the MSS value is negotiated during the TCP handshake between two communicating devices. It is used to make sure that TCP segments fit within the network's MTU, thus avoiding fragmentation and reassembly.

The relationship between MTU and MSS is based on ensuring efficient data transmission within the constraints of the network's MTU. To align MSS with the network's MTU, you can use the following arithmetic relationship:

`MSS = MTU - (IP Header Size + TCP Header Size)`

Where:

* `IP Header Size` is the size of the IP header (usually 20 bytes).
* `TCP Header Size` is the size of the TCP header (usually 20 bytes). So, the MSS is 1460 on the VPN gateway.

## MSS clamping
{: #mss-clamping}

The VPN gateway's MSS is 1460 when the packet is not yet encrypted by IPsec. When a VPC virtual machine sends traffic into VPN connections through an IBM VPN gateway, issues can arise due to the encapsulation of IP packets within additional headers for encryption and authentication. This encapsulation increases the size of the packets, exceeding the MTU 1500 bytes. When this happens, problems like packet fragmentation and reassembly can occur, leading to degraded performance and increased latency.

To address the MTU-related problems in IBM site-to-site IPsec VPNs, the VPN gateway uses _MSS clamping_, which is an approach that is used to limit the MSS of TCP packets to a value that ensures they fit within the network's MTU. By doing this, it prevents packet fragmentation and the associated performance issues.

The increased size of the packets due to IPsec encapsulation varies with the encryption cipher that is used by the IPsec VPN. Generally, an MSS of 1360 is a safe value for all kinds of encryption ciphers for the MTU 1500. So, when the traffic passes through the IBM site-to-site VPN gateway, the MSS in the TCP packet is automatically reduced to 1360.

If your network or ISP provider network MTU is less than 1500 bytes, you must enable MSS clamping and reduce the MSS on your on-premises VPN gateway.
