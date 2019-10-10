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


# FAQs for VPC
{: #faqs-for-VPC}

## Can a subnet's size be changed after it's created?
{: faq}

No.

## What is the limit on the number of characters in a VPC name?
{: faq}

Currently, the limit is 100. If this limit is exceeded, you might receive an "internal error" message.

## Can any of my VPC resource names begin with a number?
{: faq}

No, although the name can contain numbers, it must begin with a letter.

## Are there restrictions on what characters I can use in a name?
{: faq}

Yes, the UI blocks consecutive double dashes, underscores, and periods from being part of a VSI name.

## Can a subnet size be changed after it is created in a VPC?
{: faq}

No.

## During the PGW creation, do I need to reserve the FIP, or does the system automatically reserve the FIP? Will I see that Floating IP when I query all of the Floating IPs?
{: faq}

The VPC API automatically creates a floating IP along with the public gateway if an existing floating IP is not specified. And yes, that floating IP shows up in the list.

## Who enforces that there must be only one public gateway per zone for a VPC?
{: faq}

The VPC API service enforces this limit.
