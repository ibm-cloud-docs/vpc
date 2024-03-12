---

copyright:
  years: 2024
lastupdated: "2024-03-12"

keywords: virtual network interface, FAQs

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# FAQs for virtual network interfaces
{: #vni-faq}

You might encounter the following questions when you use {{site.data.keyword.cloud_notm}} virtual network interfaces for VPC.
{: shortdesc}

## What if I need more time to test and remediate the changes?
{: #faq-vni-remediation-time}
{: faq}
{: support}

Starting on 20 February 2024, you can open a Support case to request deferral of the virtual network interface API feature enhancements. Deferral removes access to the feature and grants accounts within your organization time to remediate and test API changes to instances, bare metal servers, and files shares.

For organizations with only one account for all users, the best practice is to create a second account where you can test and remediate before moving to production.

## How long can accounts within my organization defer access to the virtual network interface API feature?
{: #faq-vni-defer-time}
{: faq}
{: support}

You can defer access to the virtual network interface API features for up to 6 months from general availability. In September 2024, all accounts on the deferral list will be removed and will have immediate access to the new virtual network interface feature. The end of the deferral period will be announced before it goes into effect.

## Can I add virtual network interfaces to existing instances or bare metal servers?
{: #faq-vni-add-to-old-instance}
{: faq}
{: support}

No. When creating a virtual server instance or bare metal server, the use of virtual network interfaces is determined when the instance or server is created. A specific instance or bare metal server cannot have a mix of types.

## Can I still provision instances and bare metal servers with child network interfaces like I used to?
{: #faq-vni-classic-vnic}
{: faq}
{: support}

You can still use the older-style network interface. However, to benefit from the new style's many enhancements, you should plan to adopt the new virtual network interfaces. When planning to adopt virtual network interfaces, mitigate any risk associated with the adoption by referring to [Mitigating behavior changes to virtual network interfaces, instances, bare metal servers, and file shares](/docs/vpc?topic=vni-api-introduction).
