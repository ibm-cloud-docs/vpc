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


# FAQs for VPC
{: #faqs-for-VPC}

## What are the costs for participating in this early access program, if any, beyond my usual account costs?
{: faq}

{{site.data.keyword.vpc_short}} and the related network services are free during the early access program.


## Can a subnet's size be changed after it's created?
{: faq}

No.

## What is the limit on the number of characters in a VPC name?
{:faq}

Currently, the limit is 100. If this limit is exceeded, you might receive an "internal error" message.

## Can any of my VPC resource names begin with a number?
{:faq}

No, although the name can contain numbers, it must begin with a letter.

## Are there restrictions on what characters I can use in a name?
{:faq}

Yes, the UI blocks consecutive double dashes, underscores, and periods from being part of a VSI name.


## Can a subnet size be changed after it is created in a VPC?
{:faq}

No.

## During the PGW creation, do I need to reserve the FIP, or does the system automatically reserve the FIP? Will I see that Floating IP when I query all of the Floating IPs?
{: faq}

As the spec is currently defined, the VPC API automatically creates a floating IP along with the public gateway if an existing floating IP is not specified. And yes, that floating IP will show up in the list.

## Who enforces that there must be only 1 public gateway per zone for a VPC?
{: faq}

The VPC API service enforces this limit.






