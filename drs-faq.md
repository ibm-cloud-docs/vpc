---

copyright:
  years: 2026
lastupdated: "2026-10-21"

keywords: dynamic route server

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for dynamic route servers
{: #faqs-drs}

You might encounter these frequently asked questions when you use a dynamic route server.
{: shortdesc}

## What is a dynamic route server?
{: #faq-drs-0}
{: faq}

A dynamic route server is a VPC infrastructure service that enables dynamic route exchange by using BGP. It learns routes from connected peers and automatically adds or removes routes in VPC routing tables based on policies and peer health, helping you build resilient and scalable network architectures.

## How does a dynamic route server help?
{: #faq-drs-1}
{: faq}

A dynamic route server reduces the need for manual route management in VPCs. It allows routes to be dynamically learned, advertised, and removed as network conditions change, such as when peers go up or down, making it easier to support hybrid, multi-VPC, and self-managed overlay network designs.

## What is a dynamic route server member?
{: #faq-drs-2}
{: faq}

A dynamic route server member represents an appliance that is deployed in a specific availability zone. Members are the components that establish BGP sessions with peers and exchange routing information on behalf of the dynamic route server instance.

## What is a dynamic route server peer?
{: #faq-drs-3}
{: faq}

A dynamic route server peer is a BGP-capable network entity that establishes a BGP session with a member appliance. Peers can be customer-managed devices, such as routers or firewalls, or {{site.data.keyword.cloud_notm}}-managed services such as Transit Gateway.

## What types of peers can connect to a dynamic route server?
{: #faq-drs-4}
{: faq}

A dynamic route server can peer with BGP-capable devices, such as routers, firewalls, and {{site.data.keyword.cloud_notm}}-managed services like Transit Gateway. Peers are organized into peer groups, which allow routing behavior to be controlled by using policies.

## Why are peer groups used instead of configuring peers individually?
{: #faq-drs-5}
{: faq}

Peer groups simplify management by allowing routing policies, health monitoring, and route filtering to be defined once and applied consistently to all peers in the group. This setup makes it easier to manage complex routing topologies.

## What is a routing policy in a dynamic route server?
{: #faq-drs-6}
{: faq}

A routing policy defines how routes are learned, advertised, or added by a dynamic route server. Policies are attached to peer groups and control behaviors, such as reflecting learned routes, advertising static routes, or updating VPC routing tables.

## What types of routing policies does a dynamic route server support?
{: #faq-drs-7}
{: faq}

A dynamic route server supports policies of the following types:

* Learned routes (reflecting routes learned from peers)
* Custom routes (advertising static routes with defined next hops)
* VPC address prefixes (advertising VPC prefixes)
* VPC routing tables (adding routes into ingress or egress tables)

## What is the role of BFD in a dynamic route server?
{: #faq-drs-8}
{: faq}

Bidirectional Forwarding Detection (BFD) provides faster failure detection than BGP alone. When enabled, it allows a dynamic route server to react more quickly to peer failures and remove affected routes sooner.

## What are learned routes in a dynamic route server?
{: #faq-drs-9}
{: faq}

Learned routes are routes that the dynamic route server receives from peers through BGP. These routes can be filtered, reflected to other peers, or added to VPC routing tables depending on the configured policies.

## What are static routes in a dynamic route server?
{: #faq-drs-10}
{: faq}

Static routes are customer-defined routes that are managed by a dynamic route server. These routes can be advertised to peers and added to VPC routing tables with specified next hops, such as firewalls or routers.
