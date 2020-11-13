---

copyright:
  years: 2020
lastupdated: "2020-11-13"

keywords: virtual private network, faq, faqs, frequently asked questions, vpn, vpn gateway

subcollection: vpc

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:beta: .beta}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# FAQs for VPN gateways
{: #faqs-vpn}

You might encounter these frequently asked questions when you use {{site.data.keyword.cloud}} VPN for VPC.
{: #vpn-faq}

### When I create a VPN gateway, can I create VPN connections at the same time?
{: #faq-vpn-0}
{: faq}

In the {{site.data.keyword.cloud_notm}} console, you can create the gateway and a connection at the same time. If you use the API or CLI, VPN connections must be created after the VPN gateway is created.

### If I delete a VPN gateway with attached VPN connections, what happens to the connections?
{: #faq-vpn-1}
{: faq}

The VPN connections are deleted along with the VPN gateway.

### Are IKE or IPsec policies deleted if I delete a VPN gateway or VPN connection?
{: #faq-vpn-2}
{: faq}

No, IKE and IPsec policies can apply to multiple connections.

### What happens to a VPN gateway if I try to delete the subnet that the gateway is on?
{: #faq-vpn-3}
{: faq}

The subnet cannot be deleted if any virtual server instances are present, including the VPN gateway.

### Are there default IKE and IPsec policies?
{: #faq-vpn-4}
{: faq}

When you create a VPN connection without referencing a policy ID (IKE or IPsec), auto-negotiation is used.

### Why do I need to choose a subnet during VPN gateway provisioning?
{: #faq-vpn-5}
{: faq}

The VPN gateway must be deployed in the VPC to provide connectivity. The VPN gateway provides connectivity to the entire zone where it is deployed. A VPN gateway needs four available private IP addresses in the subnet to provide high availability and automatic maintenance. It is best if you use a dedicated subnet for the VPN gateway of size 16, where the length of the subnet prefix is shorter or equal to 28.

### What should I do if I am using ACLs on the subnet that is used to deploy the VPN gateway?
{: #faq-vpn-6}
{: faq}

Make sure that ACL rules are in place to allow management traffic and VPN tunnel traffic. For more information, see [Configuring ACLs and security groups for use with VPN](/docs/vpc?topic=vpc-acls-security-groups-vpn).

### What should I do if I am using ACLs on the subnets that must communicate with an on-premises private network?
{: #faq-vpn-7}
{: faq}

Make sure that ACL rules are in place to allow traffic between virtual server instances in your VPC and your on-premises private network. For more information, see [Configuring ACLs and security groups for use with VPN](/docs/vpc?topic=vpc-acls-security-groups-vpn).

### Does VPN for VPC support high-availability configurations?
{: #faq-vpn-8}
{: faq}

Yes, it supports high availability in an Active-Standby configuration for policy-based VPN, and Active-Active configuration for a static, route-based VPN.

### Are there plans to support SSL VPN?
{: #faq-vpn-9}
{: faq}

No, only IPsec site-to-site is supported.

### Are there any caps on throughput for site-to-site VPNaaS?
{: #faq-vpn-10}
{: faq}

Up to 650 Mbps of throughput is supported.

### Is Pre-Shared Key (PSK) and certificate-based IKE authentication supported for VPNaaS?
{: #faq-vpn-11}
{: faq}

Only PSK authentication is supported.

### Can you use VPN for VPC as a VPN gateway for your {{site.data.keyword.cloud_notm}} classic infrastructure?
{: #faq-vpn-12}
{: faq}

No. To set up a VPN gateway in your classic environment, you must use an [IPsec VPN](https://{DomainName}/catalog/infrastructure/ipsec-vpn){: external}.

### What {{site.data.keyword.cloud_notm}} infrastructure classic resources can be accessed with VPN for VPC?
{: #faq-vpn-13}
{: faq}

Virtual server instances are the only classic resources that can be accessed with VPN for VPC along with a classic access VPC.

### What does rekey collision cause?
{: #faq-vpn-14}
{: faq}

If you use IKEv1, rekey collision deletes the IKE/IPsec security association (SA). To re-create the IKE/IPsec SA, set the connection admin state to `down` and then `up` again. You can use IKEv2 to minimize rekey collisions.

### Is it possible to view logs from the VPN gateway for debugging purposes?
{: #faq-vpn-15}
{: faq}

Yes. You can find more information in [Using LogDNA to view VPN logs](/docs/vpc?topic=vpc-using-logdna-to-view-vpn-logs).
