---

Copyright:
  years:  2022
lastupdated: "2022-02-22"

keywords: bsckup, block storage, virtual private cloud, volume, data storage, virtual server instance, instance, snapshots

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:beta: .beta}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Planning backups (Beta)
{: #backups-vpc-planning}

When you plan a strategy for backing up your {{site.data.keyword.block_storage_is_short}} volumes, you might find this checklist helpful to set up and use the backup service.
{: shortdesc}

This service is available only to accounts with special approval to preview this beta feature.
{: beta}

## Planning backups
{: #baas-planning}

Consider the following prerequisites before you set up the VPC Backup Service.

| Considerations |
|-------------------|
| __ Evaluate your [IAM access permissions](/docs/vpc?topic=vpc-backup-service-manage#baas-vpc-iam) to create and manage backups. |
| __ Evaluate which volumes are most important to backup. You can create backups of boot and data volumes. |
| __ Determine a backup schedule based on the type of volumes you're backing up. For example, you might want to backup critical data that changes frequently more often than static data. |
| __ Determine a retention policy for backups in the backup plan. As subsequent backups are created, keep in mind that you incur costs for each backup you retain. Deleting older backups keeps costs down. |
| __ Choose the UI, CLI, or API for creating and managing your backups. |
| __ Evaluate the volumes that you intend to backup. A volume with numerous changes and a lengthy retention period requires more attention than a volume with moderate changes. Also, the cumulative size of all backups for a volume can't exceed 10 TB. |
| __ Evaluate when you might want to restore a volume from a backup. Keep in mind that restoring from a backup is a manual operation and not immediate such as a disaster recovery solution. |
| __ Make sure you have a unique name for your backup policies. For example, if you have a method for naming volumes, you might name a backup policy using a similar conventions. Naming conventions for backups created by the plan are the same as snapshots. For more information, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
| __ To create a backup, verify that the volume is attached to a virtual server instance and that the instance is in a running state. |
{: caption="Table 1. Checklist for planning backup policies" caption-side="top"}

## Next Steps
{: #baas-planning-next-steps}

[Create backup policies](/docs/vpc?topic=vpc-backup-policy-create) to backup your volumes.
