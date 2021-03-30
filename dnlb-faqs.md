---

copyright:
  years: 2019, 2020
lastupdated: "2020-12-01"

keywords: load balancer, distributed, network, faqs,

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

# FAQs for distributed network load balancers
{: #dnlb-faqs}

This section contains answers to some frequently asked questions about the {{site.data.keyword.cloud}} Service Distributed Network Load Balancer service.

## Does the distributed network load balancer support layer 7 switching?
{: #does-the-load-balancer-support-layer-7-switching}
{: faq}

No. The distributed network load balancer does not support layer 7 switching.

## What's the maximum number of front-end listeners I can define with my load balancer?
{: #what-s-the-maximum-number-of-front-end-listeners-i-can-define-with-my-dnlb}
{: faq}

You can define a maximum of 25 front-end listeners for a distributed network load balancer.

## What's the maximum number of virtual server instances I can attach to my back-end pool?
{: #max-number-server-instances-dnlb}
{: faq}

You can attach a maximum of 100 virtual server instances to your back-end pool for a distributed network load balancer.

## Is a distributed network load balancer horizontally scalable?
{: #is-dnlb-horizontally-scalable}
{: faq}

Yes. Within every zone, the Service DNLB is inherently horizontally scalable, without requiring additional service_ip addresses or any other user action

## What should I do if I'm using ACLs on the subnets that are used to deploy a distributed network load balancer?
{: #dnlb-subnets-deploy}

Make sure that the proper ACL rules are in place to allow traffic to and from the pool member's port. Traffic between a distributed network load balancer and back-end instances should also be allowed.

For detailed information on the ACLs configuration required, see [Configuring ACLs and security groups for use with a service distributed network load balancer](/docs/vpc?topic=vpc-dnlb-configuring-acls)


## Why am I receiving a `401 Unauthorized Error` code?
{: #dnlb-401-unauthorized-error}
{: faq}

If you are receiving a `401` error code for your distributed network load balancer, check the following access policies for your user:
* The access policy for the load balancer resource type
* The access policy for the resource group

## Why is the back-end member health under my pool `unknown`?
{: #back-end-member-health-unknown-dnlb}
{: faq}

After being created, your back-end member health is `unknown` until its health status is determined

## What are the default settings and allowed values for health check parameters?
{: #default-settings-values-dnlb}
{: faq}

The following default settings apply to distributed network load balancer health check parameters:

* **Health check interval:** Five seconds, and the range is 2 - 60 seconds
* **Health check response timeout:** Two seconds, and the range is 1 - 59 seconds
* **Maximum retry attempts:** Two retry attempts, and the range is 1-10 retries

## Why is my DNLB traffic flow not working?
{: #dnlb-traffic-flow-not-working}
{: faq}

If your DNLB traffic flow is not working, it could be for one of the following reasons:

* Access control lists (ACLs) and security groups are used to control the traffic to the {{site.data.keyword.cloud}} Service Distributed Network Load Balancer (DNLB) pool members. Check [Configuring ACLs and security groups for use with a service distributed network load balancer](/docs/vpc?topic=vpc-dnlb-configuring-acls) for more information and possible solutions.

* Member instances need to some applications running and responding to the load balancer health monitors.

* Your VPC should not have any `address prefix` CIDRs that intersect with `10/8`, even if they are not used for load balancer member instances.
