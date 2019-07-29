---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}

# FAQs for virtual server instances
{: #faqs-for-vsis}

## Can a vNIC have both a floating IP and private IP address?
{: faq}

Yes.

## Instance1 has two vNICs, called vNIC1 and vNIC2. Can these two vNICs be attached to the same subnet? 
{: faq}
 
Yes, there is no limitation on having multiple network interfaces on the same instance attached to the same subnet.

## Can an instance be created without a subnet, such as with only a floating IP address?
{: faq}

No.

## Can an instance be attached to multiple VPCs?
{: faq}

No.

## During floating IP assignment in a VPC, a customer must specify the vNIC of the VSI, is that correct?
{:faq}

Yes, and in fact, it must be the primary network interface of a server.

However, if we add an "address" resource in the API under the networking area, we could very well change the association of the floating IP to the address rather than the interface.

## Can a vNIC on a virtual server instance in my VPC have both floating IP and private IP?
{:faq}
 
Yes.

## Imagine that VSI1 in a VPC has only vNIC1 and it is attached to Subnet1. Subnet1 is attached to a Public Gateway (PGW). Can a customer still assign a floating IP to VSI1?
{:faq}

Yes, a server can be on a subnet attached to a public gateway and also have a floating IP. The assignment of floating IP to a VSI is not related to whether there is a public gateway attached to the subnet. A floating IP associated to a VSI will take precedence over the public gateway attached to the subnet.

## In my VPC, VSI1 has 2 vNICs, called vNIC1 and vNIC2. Can these two vNICs be attached to the same subnet?
{:faq}
 
Yes, there is no limitation on having multiple network interfaces on the same server attached to the same subnet. However, multiple NICs on a VSI currently are not supported.

## Can a VSI be created without a subnet, just with floating IP?
{:faq}

No.

## Can a VSI be attached to more than one VPC?
{:faq}

No.

## How many servers can I start?
{: #concurrent}
{: faq}

An account has a limit of 100 instances that can run on public virtual servers, dedicated virtual servers, and bare metal servers, at any time.

## What regions are available?
{: faq}

You can create virtual server intances for {{site.data.keyword.vpc_full}} in Dallas (US-South).

## Can I use existing virtual server instances from my classic infrastructure with an {{site.data.keyword.vpc_short}}?
{: faq}

No. It is currently not supported to move a virtual server from classic infrastructure to {{site.data.keyword.vpc_short}} infrastructure.

## Why doesn't my {{site.data.keyword.vsi_is_short}} instance appear in the Device List in {{site.data.keyword.slportal}}?
{: faq}

{{site.data.keyword.vsi_is_short}} instances appear only in {{site.data.keyword.cloud_notm}} console.


## What virtual server families are supported with {{site.data.keyword.vpc_short}}?
{: faq}

Currently, only public virtual servers in the balanced, memory, and compute families are supported. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles#profiles).