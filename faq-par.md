---

copyright:
  years: 2024, 2025
lastupdated: "2025-05-19"

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for public address ranges
{: #faq-public-address-ranges}

Public Address Ranges for VPC is only available for evaluation and testing purposes for users with special access.
{: beta}

The following questions are commonly asked about public address ranges. If you have additional questions you'd like to see addressed here, open an issue by using the **Open doc issue** or **Edit topic** links at the end of this page.
{: shortdesc} 

## Can I bring my own public IP address range?
{: #assign-public-ips}
{: faq}
{: support}

No. Only IBM-provided public IPs are supported. BYOIP (Bring Your Own IP) is not available for this service.

## What public IP address range sizes can I reserve?
{: #prefix-sizes}
{: faq}
{: support}

You can reserve ranges with the following prefix sizes:

* `/28` = 16 IPs
* `/29` = 8 IPs
* `/30` = 4 IPs
* `/31` = 2 IPs
* `/32` = 1 IP

After you reserve a range, you can't change the size of the range. Make sure to reserve a public address range size large enough to meet your needs.

IPs in different public address ranges aren't guaranteed to be contiguous.
{: note}

## Can I divide a public address range across multiple VPCs or zones?
{: #divide-par}
{: faq}
{: support}

No. A public address range is indivisible. It cannot be split or shared across multiple VPCs or availability zones.

## Can I reserve and use multiple public address ranges in a single VPC?
{: #reserve-use-multiple-public-address-ranges}
{: faq}
{: support}

Yes. You can reserve and bind multiple public address ranges to a single VPC to meet scaling or isolation requirements.

## Are public address ranges automatically redundant across zones?
{: #redundant-by-default}
{: faq}
{: support}

Not by default. However, you can create separate public address ranges in different zones and configure routing to ensure high availability across zones.

## Related link
{: #faq-related-links-par}

[About public address ranges](/docs/vpc?topic=vpc-about-par)
