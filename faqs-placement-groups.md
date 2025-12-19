---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-19"

keywords: virtual private cloud, private cloud network, placement group, placement group strategy, host spread, power spread, faq, faqs

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for Placement groups
{: #faqs-for-placement-groups}

## Can I assign my instance to more than one placement group?
{: #faq-placement-groups-0}
{: faq}
{: support}

No, an instance can be assigned to only one placement group.

## What is the maximum number of instances that I can have per placement group?
{: #faq-placement-groups-1}
{: faq}

If you are using the host spread placement strategy, you can have a maximum of 12 instances per placement group. If you are using the power spread placement strategy, you can have a maximum of four instances per placement group.

If more instances are needed, you can request a quota increase through IBM [customer support](/docs/account?topic=account-using-avatar).

## Can I move an instance from one placement group to another?
{: #faq-placement-groups-2}
{: faq}

No, the placement group strategy can't be modified after the placement group is created. Also, to remove an instance from a placement group, the instance must be deleted first.

## Can I use an instance that has a placement group strategy within an instance group?
{: #faq-placement-groups-3}
{: faq}

Yes, you can use instances that are provisioned with a placement group strategy within an instance group. The instance template includes the placement group attribute. Any instances that are started within an instance group that has a specified placement group is placed according to the placement group strategy. Placement groups allow instances from multiple zones to allow instance groups to support instances with subnets that span multiple zones.

## Can I resize an instance that is part of a placement group?
{: #faq-placement-groups-4}
{: faq}

Yes, you can resize an instance that is part of a placement group. When an instance is resized, the instance is stopped, the profile is updated, and the instance is restarted. When the instance is restarted, the instance is placed according to the placement group strategy.

## Can I provision an instance with both a placement group strategy and a dedicated host placement at the same time?
{: #faq-placement-groups-5}
{: faq}

No, placement groups and dedicated host are mutually exclusive. An instance can be provisioned with one or the other, not both.

## Can instances that are provisioned in different zones be assigned to the same placement group?
{: #faq-placement-groups-6}
{: faq}

Yes, instances that are provisioned in different zones can be placed into the same placement group for both the host spread and power spread placement group strategies.
