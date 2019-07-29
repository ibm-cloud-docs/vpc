---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

keywords: security, groups, encryption, traffic, rules, subnet, instance, VSI, firewall, encryption, vpc, vpc network

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Security in your VPC
{: #security-in-your-vpc}

In the early access release, you can keep your VPC and workloads secure by using security groups. Security groups control traffic on a per-instance basis.
{:shortdesc}

### Security groups
{: #sgs-security}
A **security group** acts as a virtual firewall that controls the traffic for one or more virtual server instances. A security group is a collection of rules that specify whether to allow or deny traffic for an associated instance. You can associate an instance with one or more security groups and edit the security group rules. For more information, see [Using security groups](/docs/vpc?topic=vpc-using-security-groups).

### End-to-end encryption
{: #end-to-end-encryption}

Although {{site.data.keyword.vpc_short}} doesn't provide end-to-end encryption, it allows for it. For example, if you have a secure endpoint on a virtual server instance (such as an HTTPS server on port 443), you can attach a floating IP to that instance, and then your connection is end-to-end encrypted from the client to the server on port 443. Nothing in the path forces a decryption. However, if you use an insecure protocol such as HTTP on port 80, your data is not encrypted from end to end.
