---

copyright:
  years: 2018, 2019
lastupdated: "2019-09-30"

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
{: #faq-vsi-0}
{: faq}

Yes.

## Instance1 has two vNICs, called vNIC1 and vNIC2. Can these two vNICs be attached to the same subnet?
{: #faq-vsi-1}
{: faq}
 
Yes, you can attach multiple network interfaces of an instance to the same subnet.

## Can an instance be created without a subnet, such as with only a floating IP address?
{: #faq-vsi-2}
{: faq}

No.

## Can an instance be attached to multiple VPCs?
{: #faq-vsi-3}
{: faq}

No.

## During floating IP assignment in a VPC, a customer must specify the vNIC of the instance, is that correct?
{: #faq-vsi-4}
{: faq}

Yes, and in fact, it must be the primary network interface of a server.

## Can a vNIC on a virtual server instance in my VPC have both floating IP and private IP?
{: #faq-vsi-5}
{: faq}
 
Yes.

## Imagine that instance1 in a VPC has only vNIC1 and it is attached to Subnet1. Subnet1 is attached to a Public Gateway (PGW). Can a customer still assign a floating IP to instance1?
{: #faq-vsi-6}
{: faq}

Yes, a server can be on a subnet that is attached to a public gateway and also have a floating IP. The assignment of floating IP to an instance is not related to whether a public gateway is attached to the subnet. A floating IP associated to an instance takes precedence over the public gateway that is attached to the subnet.

## What regions are available?
{: #faq-vsi-7}
{: faq}

You can create virtual server instances for {{site.data.keyword.vpc_full}} in Dallas (US-South).

## Can I use existing virtual server instances from my classic infrastructure with an {{site.data.keyword.vpc_short}}?
{: #faq-vsi-8}
{: faq}

You can migrate a virtual server instance from the classic infrastructure to a VPC. You need to create an image template, export it to IBM Cloud Object Storage, and then customize the image to meet the requirements of the VPC infrastructure. For more information, see [Migrating a virtual server from the classic infrastructure](/docs/vpc?topic=vpc-migrate-vsi-to-vpc).

## Why doesn't my {{site.data.keyword.vsi_is_short}} instance appear in the Device List in {{site.data.keyword.slportal}}?
{: #faq-vsi-9}
{: faq}

{{site.data.keyword.vsi_is_short}} instances appear only in {{site.data.keyword.cloud_notm}} console.


## What virtual server families are supported in {{site.data.keyword.vpc_short}}?
{: #faq-vsi-10}
{: faq}

Currently, public virtual servers in the balanced, memory, and compute families are supported. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles#profiles).