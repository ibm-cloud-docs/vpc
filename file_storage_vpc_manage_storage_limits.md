---

copyright:
  years: 2023, 2024
lastupdated: "2024-10-16"

keywords: share, capacity, Block Storage

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing file share count and capacity limits
{: #manage-fs-limit}

{{site.data.keyword.filestorage_vpc_full}} is a zonal file storage offering that provides NFS-based file storage services. You can create up to 300 file shares in your account, across all VPCs. You can request an increase of this quota by contacting [IBM Support](/unifiedsupport/cases/add){: external} and submitting a support case. Answer the following questions when you're requesting an increase in your File share provisioning limit. 
{: shortdesc}

## File share count and storage limits checklist
{: #manage-fs-limit-checklist}

Review the following checklist items and record your answers. Provide this information when you create a support case.

- **Ticket Subject**: 
    >"*Request to Increase VPC File Share Count Limit*".

- **How many extra shares do you need? Provide your account, region, and the zone where you want more shares.**
    >*"200 shares in US South-2"*.

- **What is the file share capacity that you require?**
    >*"75% of the file shares are under 250 GB and 25% are up to 32,000 GB."*

- **What is the performance level that you require for the new shares?**
    >*"We plan to provision the smaller, 250-GB file shares with 5000 IOPS, and the large, 32-TB file shares with 48,000 IOPS."*

- **How many total shares use customer-managed encryption?**
    >*"100 shares (or 50%) are to use customer-managed encryption".*

- **Provide an estimate of when you expect or plan to provision all of the requested shares.**
    >*"I expect to create these shares within 90 days".*

- **Provide a 90-day forecast of expected average capacity usage of these shares.**
    >*"I expect 25% of the shares to be used in 30 days, 50 percent to be used in 60 days and 75% to be used in 90 days".*
    >*"For secondary shares, I expect to create 50% of the secondary shares at less than 250 GB within 30 days and secondary shares greater than 250 GB within 60 days."*

Respond promptly to all questions and statements in your request. They're necessary for processing and approval. If some things are unclear, the support team contacts you to clarify your answers. 
{: important}

You're notified about the update to your limits throughout the case process.

## Increasing share capacity
{: #manage-fs-capacity}

Capacity for file shares ranges 10 - 32,000 GB. For more information, see [expanding File share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity).
