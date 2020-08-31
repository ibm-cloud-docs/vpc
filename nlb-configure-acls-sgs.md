---

copyright:
  years: 2020
lastupdated: "2020-06-29"

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

# Configuring ACLs and security groups for use with network load balancers
{: #nlb-configuring-acls}

Access control lists (ACLs) and security groups can be used to control the traffic in your {{site.data.keyword.cloud}} Network Load Balancer (NLB) for VPC deployment. ACLs can be used to filter traffic going to and from NLB. In order for your NLB to function, ACL rules need to be configured to allow the following traffic:

| Inbound/Outbound| Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound |TCP| AnyIP | AnyPort| AnyIP | 56501|
| Inbound |TCP| AnyIP | 443, 10514, 8834 | AnyIP | AnyPort|
| Inbound |UDP| 161.26.0.0/16 | 53 | AnyIP | AnyPort|
| Outbound | TCP | AnyIP | 56501| AnyIP | AnyPort|
| Outbound | TCP | AnyIP | AnyPort| AnyIP |443, 10514, 8834 |
| Outbound | UDP | AnyIP | AnyPort| 161.26.0.0/16 |53| 

Additionally, if you configure listeners on your network load balancer, you must also configure the corresponding inbound and outbound ACL rules for traffic to go through.

Security groups come into play to control the traffic between the members and the load balancer. Make sure the security groups attached to the subnet(s) of your member(s) allow inbound and outbound traffic on the health check and data ports of those member(s).

Ideally, you should configure the ACLs and security groups before an action is taken on a network load balancer, such as creation or update. Refer to [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls#using-acls) and [Using security groups](/docs/vpc?topic=vpc-using-security-groups) for more information.
