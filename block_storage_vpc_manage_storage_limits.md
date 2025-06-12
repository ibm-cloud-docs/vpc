---

copyright:
  years: 2019, 2025
lastupdated: "2025-06-12"

keywords: volume, capacity, Block Storage

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing volume count and capacity limits
{: #manage-storage-limit}

{{site.data.keyword.block_storage_is_short}} offers block-level data storage volumes that can be attached to an instance as either a boot volume or as a data volume. You can create up to 300 boot and data Block Storage volumes per account in a region. You can request an increase of this quota by contacting [IBM Support](/unifiedsupport/cases/add){: external} and submitting a support case. Answer the following questions when you're requesting an increase in your volume provisioning limit. 
{: shortdesc}

## Volume count and storage limits checklist
{: #manage-storage-limit-checklist}

Review the following checklist items and record your answers. Provide this information when you create a support case.

- **Ticket Subject**: 
    >"*Request to Increase VPC Volume Count Limit*".

- **How many extra volumes do you need? Provide your account, region, and the zone where you want more volumes.**
    >*"200 volumes in US South-2"*.

- **How many volumes are primary boot volumes versus secondary data volumes?**
    >*"50% primary volumes, 50% secondary volumes" or "100 primary volumes, 100 secondary volumes".*

- **Of the secondary volumes, how many secondary volumes do you need and of what capacity?**
    >*"75% of the secondary volumes are under 250 GB and 25% are up to 16,000 GB."*

- **What is the performance level that you require for the new volumes? Provide the name of the volume profile or custom IOPS value.**
    >*"We plan to provision the smaller, 250-GB volumes with general-purpose (3 IOPS/GB) profile, and the large, 16-TB volumes with the custom profile and 48,000 IOPS."*

- **How many total volumes use customer-managed encryption?**
    >*"100 volumes (or 50%) are to use customer-managed encryption".*

- **Provide an estimate of when you expect or plan to provision all of the requested volumes.**
    >*"I expect to create these volumes within 90 days".*

- **Provide a 90-day forecast of expected average capacity usage of these volumes.**
    >*"I expect 25% of the volumes to be used in 30 days, 50 percent to be used in 60 days and 75% to be used in 90 days".*
    >*"For secondary volumes, I expect to create 50% of the secondary volumes at less than 250 GB within 30 days and secondary volumes greater than 250 GB within 60 days."*

- **Which volume profiles are used: general-purpose, 5iops-tier, 10iops-tier, custom, or sdp?**
    >*"As an allow-listed customer, we request the capacity increase for `sdp` volumes."*.

Respond promptly to all questions and statements in your request. They're necessary for processing and approval. If some things are unclear, the support team contacts you to clarify your answers. 
{: important}

You're notified about the update to your limits throughout the case process.

## Increasing volume capacity
{: #manage-storage-capacity}

Capacity for secondary volumes ranges 10 - 16,000 GB. For more information, see [expanding Block Storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

Customers with special access to volume profiles within the defined performance family can expand their `sdp` volumes to the maximum capacity is 32,000 GB.
{: preview}
