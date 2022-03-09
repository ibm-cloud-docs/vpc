---

copyright:
  years: 2022
lastupdated: "2022-03-08"

keywords: storage, backup, virtual private cloud

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:beta: .beta}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# About Backup for VPC (Beta)
{: #backup-service-about}

Use {{site.data.keyword.cloud}} Backup for VPC to automatically back up, manage, and restore block storage volumes from backups. By using this service, you prevent data loss, manage risk, and improve data compliance. The backup service let's you create backup policies for your block storage volumes. Apply these policies to your volumes to assure your data is backed up regularly. Retain the backups as long as you need.
{: shortdesc}

This service is available only to accounts with special approval to preview this beta feature. Contact IBM Support if you're interested in getting access.
{: beta}

## Backup service concepts
{: #backup-service-concepts}

The {{site.data.keyword.cloud_notm}} Backup for VPC service lets you create backup policies for your block storage volumes. A backup policy contains a backup plan, which defines a schedule for automated backups and defines a retention period, after which the backups are deleted. When the schedule is triggered, a backup is created of your volume contents. When the retention period is reached (number of days specified), the expired backups are deleted.

A backup is actually a snapshot, created automatically. Behind the scenes, the [Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about) service is used to create a point-in-time copy of your data. The entire contents of the volume are copied (snapshotted) and retained. Subsequent backups of the same volume capture the changes since the previous backup.

You can take up to 100 backups of a volume, but it's best to set a retention policy so that older backups are deleted and your costs are reduced.

Backup's require that the volume you're backing up is attached to a running virtual server instance. Put another way, you can't backup an unattached volume.

A volume is backed up when [user-provided tags](#backup-service-about-tags) associated with a block storage volume match tags for target resources in a backup policy. The volume must have at least one of the backup policy’s tags. When the scheduled backup is triggered according to the backup plan schedule, all volumes with matching tags in the policy are backed up. If a volume has multiple tags, only one tag has to match for a backup to trigger.

Use the UI, CLI, or API to create backup policies and plans for your block storage volumes. Before creating backups, see this information on [planning your backups](/docs/vpc?topic=vpc-backups-vpc-planning). For best practices and other considerations when creating backups, see [Best practices for creating backups](/docs/vpc?topic=vpc-backups-vpc-best-practices).

### Comparison of backups and snapshots
{: #baas-comparison}

While the snapshots service is used in the background to create backups, there are similarities and differences between backups and snapshots. Table 1 compares backups to snapshots.

Backups and snapshots services are not the same as a disaster recovery (DR) solution, where your data is continually backed up and has automatic failover. Restoring a volume from a backup or snapshot is a manual operation that takes time.
{: note}

| Feature | Backup | Snapshot |
| ------- | ------ | -------- |
| Backs up block storage boot and data volumes | ![Checkmark icon](../icons/checkmark-icon.svg)  | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Manually restore a volume from a snapshot | ![Checkmark icon](../icons/checkmark-icon.svg)  | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Backup data by a schedule | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Backup data immediately | | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Older data is automatically deleted when retention period is reached | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Manually delete snapshot data | | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Data copied is at a specific point in time | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Set retention period for automatic deletion | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Take up to 100 snapshots/volume | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Costs are based on GBs per month | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Table 1. Comparison of backups and snapshots" caption-side="bottom"}

### Benefits of creating backups
{: #backup-service-benefits}

The {{site.data.keyword.cloud_notm}} Backup for VPC offers you the following benefits:

* Prevent data loss - Protect your critical data by scheduling regular backups. Establish a data restoration plan to quickly restore your compromised volumes. Reduce technical and financial impacts from unplanned outages. 

* Protect against small-scale failures - Restore from a bootable snapshot in case of host failue or malicious attack.

* Improve compliance - By ensuring backups are in place and data can be easily restored, you prevent compliance and regulatory issues.

* Save costs - A backup policy retention plan means that backups are regularly deleted, reducing costs from too much overhead.

* Easier management - Using IBM's backup service is far easier to manage than a third-party backup service because it's fully integrated with your VPC resources.

### Backup policies and backup plans
{: #backup-service-policies}

Backup policies consist of plans that define schedules for automatic backup creation and data retention. You can schedule backups to from one day up to 30 days. The default creation and retention interval is 30 days.

The interval for creating a backup and its retention period can be the same or different. Backups created by the backup plan inherit the parent volume resource group details. Backups are automatically deleted when the retention period (in days) is reached. For example, setting _7_ will delete backups that are a week old.

The backups created by the policy can be restored to create a new volume.

For Beta, you specify one backup plan per policy. You can't modify a plan after it's created. If you want to change it, you must delete the policy and create a new policy with a new plan.
{: note}

### Tags for target resources
{: #backup-service-about-tags}

Backup policies specify tags for target resources that associate the policy with block storage volumes, for the purpose of creating a backup.

The tag applied to a volume must match the tag for target resources in the backup policy. If a volume has multiple tags, only one tag needs to match a backup policy tag. Based on the schedule in the backup plan, a matching tag triggers a backup. If multiple volume tags match backup policy tags, only one backup is created at the scheduled interval. If you have multiple volumes with the same tag, backups are created for all the volumes.

There is no limit to the number of tags you can specify for your resources. However, using a reasonable number will make it easy to keep track of the number of backups you're creating. For information about adding tags, see [Applying backup policies to resources using tags](/docs/vpc?topic=vpc-backup-use-policies).

### Restoring a volume from a backup
{: #backup-service-restore-concepts}

You can create a new volume from a backup. This process is called restoring a volume from a backup. It works the same way as regular snapshots. Restoring a volume from a backup creates a fully provisioned block storage boot or data volume. You can restore a volume when you create a new instance or from an existing instance. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

The restored volume has the same capacity and IOPS tier profile as the original volume. Restoring a boot volume creates a volume you can use to start up an instance. Restoring a data volume provides a secondary volume attached to the instance.

## User roles for backup policies
{: #backup-service-user-roles}

Depending on your assigned role as a backup user you can create and administer backup policies. Table 1 describes these capabilities.

| Backup user role | What you can do: |
|------|------------------|
| **Viewer, Operator** | List all backup policies  |
| | View details of a backup policy |
| | View a backup plan | 
| **Editor, Administrator** | Organize and manage their snapshots using tags |
| | Associate backup policies to volumes using user tags |
| | Create, update, and delete backup policies |
| | Specify CRON format to schedule snapshot creation |
{: caption="Table 1. User roles for backup policies" caption-side="bottom"}

Volume users with an editor role can do the following:

* Create data volumes with user tags
* View volume details with user tags
* List volumes filtered by tag
* Update user tags post volume creation

Snapshots users with an editor role can do the following:

* Create snapshots with user tags
* View snapshot details with tags
* List snapshots filtered by tag
* Update user tags post snapshot creation

## Service-to-service authorizations
{: #baas-s2s-auth}

Specific IAM user roles are required to grant service-to-service authorizations. Granting authorization allows the backup service to detect volume tags to create a backup. For the VPC infrastructure service, define these service-to-service authorizations:

| Source service | target service | Minimum user role |
|----------------|----------------|-----------|
| IBM Cloud Backup for VPC | Block Storage for VPC | operator |
| IBM Cloud Backup for VPC | Block Storage Snapshots for VPC | editor |
| IBM Cloud Backup for VPC | Virtual Server for VPC | operator |
{: caption="Table 2. Service-to-service authorizations" caption-side="bottom"}

## Limitations in the Beta release
{: #backup-service-limitations}

Limitations for this Beta release include the following:

* Backups are restricted to a single region. You cannot share backups or create backup policies across regions.
* You can create up to 10 backup polices per account.
* You can take a total of 100 backups per volume based on your backup policy, in your account and region. 
* The first backup and/or the entire volume backup cannot exceed 10 TB.
* One backup plan per policy is supported.
* You can't take a backups of a detached volume.

## Next Steps
{: #backup-next-steps}

Review [best practices](/docs/vpc?topic=vpc-backups-vpc-best-practices) for creating a backup policy and the [planning checklist](/docs/vpc?topic=vpc-backups-vpc-planning). Then, you can start 
[create a backup policies](/docs/vpc?topic=vpc-backup-policy-create).
