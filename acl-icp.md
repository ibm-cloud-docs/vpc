---

copyright:
  years: 2019, 2020

lastupdated: "2020-04-04"

keywords:  

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Understanding Internet Communication Protocols
{:#understanding-icp}

Generally speaking, a _communication protocol_ is a system of rules that allow two or more entities of a communications system to transmit information. The internet has a large suite of protocols to cover many situations. In creating web-based applications and programming interfaces, software developers commonly utilize three of these communication protocols to describe the state of the network and the ways that data packets are moved across the network:

* ICMP, Internet Control Message Protocol, part of the internet protocol suite defined in RFC 792.
* TCP, Transmission Control Protocol
* UDP, User Datagram Protocol

The protocols that are used for a particular implementation of, say, an API call, can influence the overall behavior of your network, so it is worthwhile to understand the basic differences between them. If you need more information, many good articles are available on the internet with detailed descriptions of the protocols.

## ICMP
{:#network-infrastructure-icmp}

ICMP is a _control protocol_, meaning it is designed not to carry application data, but rather information about the status of the network itself. It is essentially a _network layer_ (OSI layer 3) error-reporting and error-control protocol for the network. The best-known examples of ICMP in practice are the `ping` utility, which uses ICMP to probe remote hosts for responsiveness and overall round-trip time of the probe messages, and the `traceroute` utility.

What developers need to know is that ICMP packets have no TCP or UDP port numbers associated with them, because port numbers are a layer 4 (_transport layer_) construct.

## TCP and UDP
{:#network-infrastructure-tcp-udp}

Both Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) are OSI layer 4 _transport protocols_; they are used to pass the actual data. The main difference between TCP and UDP, from a developer's perspective, is how they handle **packet order**.

TCP is a connection-oriented protocol, it guarantees that all sent packets will reach the destination in the correct order.

UDP, on the other hand, is a connection-less protocol. Communication is datagram-oriented, so the integrity is guaranteed only on the single datagram. Datagrams reach a destination and can arrive out of order, or possibly they don't arrive at all.

UDP  generally is used for real-time communication, where a little percentage of the packet loss rate is preferable to the overhead of a TCP connection.

## Additional information
{:#network-infrastructure-additional-information}

An overview of the OSI 7-layer model with examples of internet protocols at each layer is available [here](https://www.webopedia.com/quick_ref/OSI_Layers.asp).
