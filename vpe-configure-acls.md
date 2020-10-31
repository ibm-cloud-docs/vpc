---

copyright:
  years: 2020
lastupdated: "2020-10-30"

keywords: vpe, acls, virtual private endpoints, endpoint gateways

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Configuring ACLs for use with virtual private endpoints
{: #vpe-configuring-acls}

You can use access control lists (ACLs) to control the traffic in your Virtual Private Endpoint (VPE) for VPC deployment. Use ACLs to filter traffic going to and from the VPE.
{: shortdesc}

Ideally, you should configure the ACLs before taking an action on a virtual private endpoint, such as creating or updating. For instructions, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls#using-acls).

The following screen capture shows an example of outbound rules.

![Outbound rules](/images/vpe-outbound-rules.png "Outbound rules")
