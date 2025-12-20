---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding Internet Communication Protocols
{: #understanding-icp}

Generally speaking, a _communication protocol_ is a system of rules that allow two or more entities of a communications system to transmit information. The internet has a large suite of protocols to cover many situations. In creating web-based applications and programming interfaces, software developers commonly use three of these communication protocols to describe the state of the network and the ways that data packets are moved across the network:

* ICMP, Internet Control Message Protocol, part of the internet protocol suite defined in RFC 792.
* TCP, Transmission Control Protocol
* UDP, User Datagram Protocol

The protocols that are used for a particular implementation of, say, an API call, can influence the overall behavior of your network. So it is worthwhile to understand the basic differences between them. If you need more information, many good articles are available on the internet with detailed descriptions of the protocols.

## ICMP
{: #network-infrastructure-icmp}

ICMP is a _control protocol_, meaning that it is designed to carry information about the status of the network itself. It is essentially a _network layer_ (OSI layer 3) error-reporting and error-control protocol for the network. The best-known examples of ICMP in practice are the `ping` and `traceroute` utilities. The `ping` utility uses ICMP to probe remote hosts for responsiveness and overall round-trip time of the probe messages. The `traceroute` utility uses ICMP to discover and trace network routes that the ICMP packets take when they travel to their destination.

What developers need to know is that ICMP packets have no TCP or UDP port numbers that are associated with them because port numbers are a layer 4 (_transport layer_) construct.

## TCP and UDP
{: #network-infrastructure-tcp-udp}

Both Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) are OSI layer 4 transport protocols. These protocols are used to pass the actual data. The main difference between TCP and UDP, from a developer's perspective, is how they handle packet order.

TCP is a connection-oriented protocol, it guarantees that all sent packets reach the destination in the correct order.

Alternatively, UDP is a connection-less protocol. Communication is datagram-oriented, so the integrity is guaranteed only on the single datagram. Datagrams reach a destination and can arrive out of order, or possibly they don't arrive at all.

Typically, UDP is used for real-time communication, where a little percentage of the packet loss rate is preferable to the overhead of a TCP connection.

## ANY
{: #network-infrastructure-any-ipv4}

You can now apply any [IPv4 protocol listed in IANA](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml) to Network Access Control list rules and Security Group rules. This allows you to bring custom topologies and use cases from on-prem, multi-cloud and Classic infrastructure that leverage protocols outside of TCP, UDP, and ICMP.
