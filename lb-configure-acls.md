---

copyright:
  years: 2020
lastupdated: "2020-07-30"

keywords: load balancer, autoscaling, instance groups, managed pool, load balancer for vpc, pool

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Configuring ACLs for use with application load balancers
{: #configuring-acls-for-use-with-load-balancers}

If you use access control lists (ACLs) to block traffic on the subnets where a load balancer is deployed, make sure the following ACL rules are configured.

| Inbound/Outbound| Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| Inbound |TCP| AnyIP | AnyPort| AnyIP | 56501|
| Inbound |TCP| AnyIP | 443, 10514, 8834 | AnyIP | AnyPort|
| Inbound |UDP| 161.26.0.0/16 | 53| AnyIP | AnyPort|
| Outbound | TCP | AnyIP | 56501| AnyIP | AnyPort|
| Outbound | TCP | AnyIP | AnyPort| AnyIP |443, 10514, 8834 |
| Outbound | UDP | AnyIP | AnyPort| 161.26.0.0/16 |53|

Additionally, if a load balancer has listeners configured, then the corresponding inbound and outbound ACL rules should be configured for traffic to go through.
