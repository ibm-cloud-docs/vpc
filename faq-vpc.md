---

copyright:
  years: 2019, 2023
lastupdated: "2024-09-23"

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for VPC
{: #faqs-for-VPC}

## Can I connect my VPC to my other IBM Cloud workloads?
{: #faq-vpc-0}
{: faq}
{: support}

Yes, you can set up access to your {{site.data.keyword.cloud}} classic infrastructure from one VPC in each region. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

## Can a subnet's size be changed after it is created?
{: #faq-vpc-1}
{: faq}

No, a subnet cannot be resized after it is created.

## What is the limit on the number of characters in a VPC name?
{: #faq-vpc-2}
{: faq}

Currently, the limit is 100. If this limit is exceeded, you might receive an "internal error" message.

## Can any of my VPC resource names begin with a number?
{: #faq-vpc-3}
{: faq}

No, although the name can contain numbers, it must begin with a letter.

## Are there restrictions on what characters that I can use in a name?
{: #faq-vpc-4}
{: faq}

Yes, the UI blocks consecutive double dashes, underscores, and periods from being part of a virtual server instance name.

## During the public gateway creation, do I need to reserve the floating IP address, or does the system automatically reserve it? Can I see that floating IP address when I query all of the floating IP addresses?
{: #faq-vpc-6}
{: faq}

The VPC API automatically creates a floating IP along with the public gateway if an existing floating IP is not specified. And yes, that floating IP shows up in the list.

## Who enforces that only one public gateway can exist per zone for a VPC?
{: #faq-vpc-7}
{: faq}

The VPC API service enforces this limit.

## Does the VPC public gateway have a timeout function?
{: #faq-vpc-public-gateway-timeout}
{: faq}

Yes, the VPC public gateway has a fixed, 4-minute timeout for TCP connections, and it is not configurable.

## How do you obtain the Cloud Resource Name (CRN) of a VPC?
{: #faq-crn}
{: faq}

 To obtain the CRN of a VPC, click **menu** ![menu icon](images/icon_hamburger.svg) > **Resource list** from the {{site.data.keyword.cloud_notm}} console. Expand **Infrastructure** to list your VPCs. Select a VPC and then click the **Status** entry to view its details. Use the icon to copy the CRN and paste it where needed.
