---

Copyright:
  years:  2022
lastupdated: "2022-07-12"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Planning backups
{: #backups-vpc-planning}

When you plan a strategy for backing up your {{site.data.keyword.block_storage_is_short}} volumes, you might find this checklist helpful to set up and use the backup service.
{: shortdesc}

## Planning backups
{: #baas-planning}

Consider the following prerequisites before you set up the VPC Backup Service.

| Considerations |
|-------------------|
| __ Evaluate your [IAM access permissions](/docs/vpc?topic=vpc-backup-service-manage#baas-vpc-iam) to create and manage backups. |
| __ Evaluate which volumes are most important to backup. You can create backups of boot and data volumes. |
| __ Determine a backup schedule based on the type of volumes you're backing up. For example, you might want to backup critical data that changes frequently more often than static data. |
| __ Determine a retention policy for backups in the backup plan. As subsequent backups are created, keep in mind that you incur costs for each backup you retain. Deleting older backups keeps costs down. |
| __ Decide how many backup plans you need for a policy. For example, you can have separate plans for daily backups, weekly, and monthly. |
| __ Choose the UI, CLI, or API for creating and managing your backups. |
| __ Evaluate the volumes that you intend to backup. A volume with numerous changes and a lengthy retention period requires more attention than a volume with moderate changes. Also, the cumulative size of all backups for a volume can't exceed 10 TB. |
| __ Consider how many backup snapshots you want to take and [billing considerations](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=api#snapshots_vpc_considerations) as the number of backup snapshots grows. |
| __ Evaluate when you might want to restore a volume from a backup. Keep in mind that restoring from a backup is a manual operation and not immediate such as a disaster recovery solution. |
| __ Make sure you have a unique name for your backup policies. For example, if you have a method for naming volumes, you might name a backup policy using a similar conventions. Naming conventions for backups created by the plan are the same as snapshots. For more information, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
|**When creating backups**: |
|__ Verify that the volume is attached to a virtual server instance and that the instance is in a running state. |
|__ Verify the plan you selected is creating backups at the desired interval. Note that backups do not occur instaneously like a manually-created snapshots. Backups usually occur within an hour of being triggered by a backup plan schedule. |
|__ Verify that at at least one of your policy tags match at least one tag of each volume you want to back up. |
|**When restoring a volume from a backup snapshot**: |
|__ Take into consideration [performance considerations](://test.cloud.ibm.com/docs/vpc?topic=vpc-baas-vpc-restore&interface=ui#baas-performance-considerations) when restoring a volume from a backup snapshot. You might also experience different regional performance when restoring a volume. |
|__ Review these performance considerations when [provisioning a new instance from a bootable snapshot](/docs/vpc?topic=vpc-baas-vpc-restore&interface=ui#baas-boot-perf).
{: caption="Table 1. Checklist for planning backup policies" caption-side="bottom"}

## Next Steps
{: #baas-planning-next-steps}

[Create backup policies](/docs/vpc?topic=vpc-backup-policy-create) to backup your volumes.
