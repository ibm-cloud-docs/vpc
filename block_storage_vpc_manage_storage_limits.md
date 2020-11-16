---

copyright:
  years: 2019, 2020
lastupdated: "2020-11-12"

keywords: volume, capacity, block storage

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="DomainName"}
{:pre: .pre}
{:beta: .beta}

# Managing volume count and capacity limits
{: #manage-storage-limit}

{{site.data.keyword.block_storage_is_short}} offers block-level data storage volumes that can be attached to an instance as either a boot volume or as a data volume. Answer the questions in this topic when you're ordering block storage volumes or requesting an increase in your volume or capacity limits. 
{:shortdesc}

## Overview
{: #manage-storage-limit-overview}

{{site.data.keyword.block_storage_is_short}} lets you create up to 750 Gen 2 boot and data block storage volumes per account in a region. You can request an increase of this quota by contacting [IBM Support](https://{DomainName}/unifiedsupport/cases/add){: external} and submitting a support ticket. 

If you provisioned block storage volumes for Gen 1 Compute instances, you are limited to 300 total volumes across the Gen 1 and Gen 2 infrastructure. For example, if you have 200 Gen 1 block storage volumes, you can request 100 Gen 2 block storage volumes for a total of 300.
{: note}

Capacity for secondary volumes ranges from 10 to 2,000 GB. You can also request to increase this capacity for greater than 2,000 GB volumes. Also, when you participate in the Beta release, new volumes greater than 250 GB can be expanded in GB increments up to 16,000 GB capacity, depending on your profile. For more information, see [Expanding block storage volume capacity (Beta)](https://test.cloud.ibm.com).

Expandable volumes and volume capacity greater than 2,000 GB are Beta features available for evaluation and testing purposes. These features are available in the US South, US East, London, and France regions. Contact your IBM Sales representative if you are interested in getting access.
{:beta}

## Volume count and storage limits checklist
{: #manage-storage-limit-checklist}

Review the following checklist items and record your answers. Provide this information when you create a support ticket.

- **Ticket Subject**: "Request to Increase VPC Volume Count Limit".

- **How many extra volumes do you need? Provide your account, region, and in what zone you'd like more volumes.**
  For example, *"200 volumes in US South-2".*

- **How many volumes are primary boot volumes versus secondary data volumes?**
  For example, *"50% primary volumes, 50% secondary volumes" or "100 primary volumes, 100 secondary volumes".*

- **Of the secondary volumes, how many secondary volumes do you need that are under 250 GB?**
  For example, *"75% of the secondary volumes are under 250 GB.* 

- **For secondary volumes over 250 GB, estimate the number of volumes and capacity you need.**
  For example, *"25 volumes (or 25%) will be up to 2,000 GB."* Note that you can request volumes over 2,000 GB capacity by participating in the Beta program. You must contact IBM support for access.

- **How many total volumes will use customer-managed encryption?**
  For example, *"100 volumes (or 50%) will use customer-managed encryption".*

- **Provide an estimate of when you expect or plan to provision all of the requested volumes.**
  For example, *"I expect to create these volumes within 90 days".*

- **Provide a 90-day forecast of expected average capacity usage of these volumes.**
  For example, *"I expect 25% of the volumes to be used in 30 days, 50 percent to be used in 60 days and 75% to be used in 90 days".* You might also want to break it down further, by primary and secondary volumes, For example, for secondary volumes, *I expect to create 50% of the secondary volumes at less than 250 GB within 30 days and secondary volumes greater than 250 (if possible, estimate volume size) within 60 days*.

You'll have an opportunity to clarify any of your answers. Respond promptly to all questions and statements in your request. They're necessary for processing and approval.
{:important}

You're notified about the update to your limits throughout the case process.