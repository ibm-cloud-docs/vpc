---

copyright:
  years: 2018, 2019
lastupdated: "2019-09-30"

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

You can keep {{site.data.keyword.vpc_full}} and workloads secure by controlling network traffic using security groups.
{:shortdesc}

## Security overview
{: #security-overview}

Security groups provide a way to control the traffic across instances in your {{site.data.keyword.vpc_full}}, using rules that you specify.

* Security groups can control the traffic at the virtual server instance (VSI) level
* Set up a floating IP for VSI access to the internet, guarded by SGs

A security group acts as a virtual firewall that controls the traffic for one or more virtual server instances. A security group is a collection of rules that specify whether to allow or deny traffic for an associated instance. You can associate an instance with one or more security groups and edit the security group rules. For more information, see [Using security groups](/docs/vpc?topic=vpc-using-security-groups).

### End-to-end encryption
{: #end-to-end-encryption}

Although {{site.data.keyword.vpc_short}} doesn't provide end-to-end encryption, it allows for it. For example, if you have a secure endpoint on a virtual server instance (such as an HTTPS server on port 443), you can attach a floating IP to that instance, and then your connection is end-to-end encrypted from the client to the server on port 443. Nothing in the path forces a decryption. However, if you use an insecure protocol such as HTTP on port 80, your data is not encrypted from end to end.
