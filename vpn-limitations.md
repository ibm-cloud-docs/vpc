---



copyright:
  years: 2019, 2020
lastupdated: "2020-09-03"

keywords: VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway, limitations

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:important: .important}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# VPN gateway limitations
{: #vpn-limitations}

Lists known limitations for a VPN gateway for VPC.
{: shortdesc}

* The VPN gateway resides in the zone associated with the subnet that you select during provisioning. The VPN gateway serves only the virtual server instances in the same zone of the VPC. Therefore, instances in other zones can't use the VPN gateway to communicate with an on-premises private network. For zone fault tolerance, you must deploy one VPN gateway per zone.

* IBM VPN gateway on VPC accepts VPN packets with [NAT-Traversal Encapsulation](https://tools.ietf.org/html/rfc3947) only. The [IP Encapsulating Security Payload (ESP)](https://tools.ietf.org/html/rfc4303) is not accepted. Make sure the NAT-T feature is enabled on your on-premises VPN device.

* Peer subnets of a VPN gateway connection cannot overlap.
