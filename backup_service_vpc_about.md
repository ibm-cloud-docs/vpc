---

copyright:
  years: 2022
lastupdated: "2022-12-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Backup for VPC
{: #backup-service-about}

Use {{site.data.keyword.cloud}} Backup for VPC to automatically backup, manage, and restore {{site.data.keyword.block_storage_is_short}} volumes from backup snapshots. By using this service, you can prevent data loss, manage risk, and improve data compliance. With the service, you can create and manage backup policies for your {{site.data.keyword.block_storage_is_short}} volumes. You can apply these policies to your volumes to assure your data is backed up regularly. Retain the backups as long as you need.
{: shortdesc}

## Backup service concepts
{: #backup-service-concepts}

You can create backup policies for your {{site.data.keyword.block_storage_is_short}} volumes with the {{site.data.keyword.cloud_notm}} Backup for VPC service. A backup policy contains a backup plan, which defines a schedule for automated backups. You can create up to four plans per policy, and edit and delete them as needed. You can view and manage backup jobs to see these actions. If you're undecided on how often to schedule backups or you don't know all the tags for your target resources yet, you can create a backup policy without a plan and add one later.

When the backup is triggered at the scheduled interval, a backup copy is created of your volume contents. Behind the scenes, the [Snapshot for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about) service is used to create a point-in-time copy of your data. When the first backup snapshot is taken, the entire contents of the volume are copied and retained. Subsequent backups of the same volume capture the changes since the previous backup. You can take up to [750 backups of a volume](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots_vpc_considerations).

Backups appear in the list of {{site.data.keyword.block_storage_is_short}} snapshots. They are identified by how they were created, in this case, by backup policy. Backups are in effect, backup snapshots. These terms are used interchangeably in the documentation, depending on the context.
{: note}

In a backup plan, you schedule the [frequency](/docs/vpc?topic=vpc-backup-policy-create&interface=ui#backup-automated-plan-ui) of your backups. In the UI, you can choose daily, weekly, or monthly. You can also add a schedule with a [`cron` expression](/docs/vpc?topic=vpc-backup-policy-create&interface=ui#backup-cron-plan-ui).

You also have to set a [retention period](/docs/vpc?topic=vpc-backup-policy-create&interface=ui#backup-automated-plan-ui) for your backups. It can be based on the number of days that you want to keep the backups or the total number of backups to retain before they are deleted. Planning for your retention and deletion of backups can keep your costs down.

Backups require that the volume you're backing up is attached to a running virtual server instance. Put another way, you can't back up an unattached volume.

A volume is backed up when [user-provided tags](#backup-service-about-tags) that are associated with a {{site.data.keyword.block_storage_is_short}} volume match the tags for target resources in a backup policy. The volume must have at least one of the backup policy’s tags for the target resources. When the scheduled backup is triggered by a backup plan, all volumes with matching tags in the policy are backed up. If a volume has multiple tags, only one tag has to match for a backup to trigger. You can add user tags to boot and data volumes at any time and when you [create a virtual server instance](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-from-vsi) or in an instance template. Any instance volumes with tags that match the backup policy are backed up.

Use the UI, CLI, or API to create backup policies and plans for your {{site.data.keyword.block_storage_is_short}} volumes. Before you create backups, review the [planning your backups](/docs/vpc?topic=vpc-backups-vpc-planning) topic. For more information about best practices and other considerations, see [Best practices for creating backups](/docs/vpc?topic=vpc-backups-vpc-best-practices).

You can [restore](#backup-service-restore-concepts) data to a new, fully provisioned volume from a backup snapshot. If the backup is of a boot volume, you can use it to provision a new instance. However, there are performance considerations when you provision an instance by restoring a boot volume from "bootable" backup snapshot. For more information, see [Performance considerations when using a bootable backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore&interface=ui#baas-boot-perf).

### Comparison of backups and snapshots
{: #baas-comparison}

While the snapshots service is used in the background to create backups, there are similarities and differences between backups and snapshots. Table 1 compares backups to snapshots.

Backups and snapshots services are different than a disaster recovery (DR) solution, where your data is continually backed up with automatic failover. Restoring a volume from a backup or snapshot is a manual operation that takes time.
{: note}

| Feature | Backup | Snapshot |
| ------- | ------ | -------- |
| Backs up {{site.data.keyword.block_storage_is_short}} boot and data volumes. | ![Checkmark icon](../icons/checkmark-icon.svg)  | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Manually restore a volume from a snapshot. | ![Checkmark icon](../icons/checkmark-icon.svg)  | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Back up data by a schedule. | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Back up data immediately. | | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Data is copied at a specific point in time. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Set retention period for automatic deletion. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Take up to 750 snapshots per volume. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Costs are based on GBs per month. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Table 1. Comparison of backups and snapshots" caption-side="bottom"}

### Benefits of creating backups
{: #backup-service-benefits}

The {{site.data.keyword.cloud_notm}} Backup for VPC offers you the following benefits:

* Prevent data loss - Protect your critical data by scheduling regular backups. Establish a data restoration plan to quickly restore your compromised volumes. Reduce technical and financial impacts from unplanned outages.

* Protect against small-scale failures - Restore from a bootable snapshot in case of host failure or malicious attack.

* Improve compliance - By ensuring backups are in place and data can be easily restored, you can prevent any issues that are related to compliance and regulatory requirements.

* Save costs - A backup policy retention plan means that backups are regularly deleted, reducing costs from too much overhead.

* Easier management - IBM's backup service is far easier to manage than a third-party backup service because it is fully integrated with your VPC resources.

### Backup policies and backup plans
{: #backup-service-policies}

Backup policies consist of plans that define schedules for automatic backup creation and data retention. You can schedule backups on a daily, weekly, or monthly basis.

You specify the retention period or total number of backups before the oldest are deleted. The default retention period is 30 days. Alternatively, you can set the total number of backups to retain up to 750 per volume. When that number is exceeded, the the oldest backups are deleted.

The interval for creating a backup and its retention period can be the same or different.

Backups that are created by the backup plan inherit the parent volume resource group details. You can create up to four plans per backup policy and modify the backup schedule and retention policy. Your four plans can have different frequencies, for example, one can be daily. Another one can be weekly, and so on. All plans apply to the volumes with tags that match the backup policy.

You can view backup job status while backups are being created, modified, or deleted.

The backups that are created by the policy can be restored to create another volume.

### Tags for target resources
{: #backup-service-about-tags}

Backup policies specify user tags for target resources that associate the policy with {{site.data.keyword.block_storage_is_short}} volumes with the same user tag, for the purpose of creating a backup. The user tag that is applied to a volume must match the tag for target resources in the backup policy. User tags are added by an authorized user in the account. Any user with the correct access role can list and can delete user tags in the account as long as the tags are not attached to any resource.

In addition to user tags, tags can be access management tags and service tags. Only user tags are applied to backup policies. [Access management tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags) are used to manage access to resources; only the account administrator can create access management tags. Service tags are a privileged construct that only authorized services can manage. Users are not authorized to attach and detach service tags on a resource, even if they have access to manage tags on the resource. Service tags are helpful to distinguish which snapshots were created manually or automatically by a backup policy.
{: note}

If a volume has multiple tags, only one tag needs to match a backup policy tag. Based on the schedule in the backup plan, a matching tag triggers a backup. If multiple volume tags match backup policy tags, only one backup is created at the scheduled interval. If you have multiple volumes with the same tag, backups are created for all the volumes.

There is no limit to the number of tags that you can specify for your resources. However, keeping the number low can make it easier to track the number of backups that you're creating. For more information about adding tags, see [Applying backup policies to resources by using tags](/docs/vpc?topic=vpc-backup-use-policies).

### Restoring a volume from a backup
{: #backup-service-restore-concepts}

You can create a separate volume from a backup snapshot that was created by a backup policy. This process is called restoring a volume. It works the same way as restoring data from manually created snapshots. Restoring a volume from a backup snapshot creates a fully provisioned {{site.data.keyword.block_storage_is_short}} boot or data volume.

Volume restoration can be performed when you create an instance, modify an instance, or when you create a stand-alone volume (no instance provisioning is required). Volume data restoration begins immediately as the volume is hydrated, but [performance is degraded](/docs/vpc?topic=vpc-baas-vpc-restore&interface=ui#baas-performance-considerations) until the volume is fully restored. Restoring from a bootable snapshot is slower than using a regular boot volume. You can also restore a data volume from a snapshot of an unattached volume. The restored volume has the same capacity and IOPS tier profile as the original volume. For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).

## User roles for backup policies
{: #backup-service-user-roles}

Depending on your assigned role as a backup user, you can create and administer backup policies. Table 1 describes these capabilities.

| Backup user role | What you can do: |
|------|------------------|
| **Viewer, Operator** | - List all backup policies. \n  - View details of a backup policy. \n - View a backup plan. |
| **Editor, Administrator** | Organize and manage snapshots by using tags. \n -  Associate backup policies to volumes by using user tags. \n - Create, update, and delete backup policies and plans. \n -  Specify CRON format to schedule snapshot creation. |
{: caption="Table 1. User roles for backup policies" caption-side="bottom"}

Volume users with an editor role can:

* Create data volumes with user tags.
* View volume details with user tags.
* List volumes filtered by tag.
* Update user tags post volume creation.

Snapshots users with an editor role can:

* Create snapshots with user tags.
* View snapshot details with tags.
* List snapshots filtered by tag.
* Update user tags post snapshot creation.

## Service-to-service authorizations
{: #baas-s2s-auth}

Specific IAM user roles are required to grant service-to-service authorization that enables the backup service to detect volume tags to create a backup. For more information, see [Establishing service to service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).

## Limitations in this release
{: #backup-service-limitations}

Limitations for this release include the following:

* Backups are restricted to a single region. You cannot share backups or create backup policies across regions.
* You can create up to 10 backup policies per account.
* You can take a total of 750 backups per volume based on your backup policy, in your account and region. If you exceed this limit, no further backups are taken.
* The first backup and the entire volume backup cannot exceed 10 TB.
* You can't take a backup of a detached volume.

## Next Steps
{: #backup-next-steps}

Review [best practices](/docs/vpc?topic=vpc-backups-vpc-best-practices) for creating a backup policy and the [planning checklist](/docs/vpc?topic=vpc-backups-vpc-planning). Afterward, you can [create backup policies](/docs/vpc?topic=vpc-backup-policy-create).
