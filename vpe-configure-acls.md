---

copyright:
  years: 2020
lastupdated: "2020-09-03"

keywords: network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network

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

# Configuring ACLs for use with virtual private endpoints (Beta)
{: #vpe-configuring-acls} 

You can use access control lists (ACLs) to control the traffic in your IBM Cloud Virtual Private Endpoint (VPE) for VPC deployment. Use ACLs to filter traffic going to and from the VPE. 

Ideally, you should configure the ACLs before an action is taken on virtual private endpoint, such as creation or update. For instructions, see [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls#using-acls).

The following screen capture shows outbound rules.

![Outbound rules](/images/vpe-outbound-rules.png "Outbound rules") 
