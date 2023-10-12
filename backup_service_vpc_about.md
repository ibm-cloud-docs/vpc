---

copyright:
  years: 2022, 2023
lastupdated: "2023-09-29"

keywords: Backup, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About Backup for VPC
{: #backup-service-about}

Use {{site.data.keyword.cloud}} Backup for VPC to automatically create backups and manually restore {{site.data.keyword.block_storage_is_short}} volumes from backup snapshots. By using this service, you can prevent data loss, manage risk, and improve data compliance. You can ensure that your data is backed up regularly, and you can retain the backups while you need them. You can create and manage backup policies and plans for your {{site.data.keyword.block_storage_is_short}} volumes by using the UI, the CLI, the API, or Terraform.
{: shortdesc}

Backups and snapshot services are different than a disaster recovery (DR) solution, where your data is continually backed up with automatic failover. Restoring a volume from a backup or a snapshot is a manual operation that takes time. If you require a higher level of service for disaster recovery, see IBM's [Cloud disaster recovery solutions](https://www.ibm.com/cloud/disaster-recovery).
{: important}

## Backup service concepts
{: #backup-service-concepts}

You can create up to 10 backup policies for your {{site.data.keyword.block_storage_is_short}} volumes in one region with the {{site.data.keyword.cloud_notm}} Backup for VPC service. A backup policy contains one or more backup plans, which define a schedule for automated backups. You can create up to four plans per policy, and edit and delete them as needed. If you're undecided on the backup schedule or you don't know all the tags yet, you can create a backup policy without a plan and add one later. 

As an enterprise account administrator, you can view and manage the backup policies and plans for the subaccounts for compliance reporting and billing from one place. For more information, see the [Scope of backup policy](#backup-service-about-scope) section.

In a backup plan, you schedule the frequency of your backups. In the UI, you can choose daily, weekly, or monthly. You can also use a `cron` expression to specify the frequency in the [UI](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=ui), from the [CLI](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=cli), with the [API](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=api) or [Terraform](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=terraform).

You must set a [retention period](/docs/vpc?topic=vpc-backup-policy-create&interface=ui#backup-automated-plan-ui) for your backups. It can be based on the number of days that you want to keep the backups. Or it can be based on the total number of backups that you want to retain before the oldest ones are deleted. Or you can set it up by specifying both the time limit and the maximum number of backups that you want to keep. Planning for the retention and deletion of your backups can keep the costs down.

Backups require that the volume that you're backing up is attached to a running virtual server instance. Put another way, you can't back up an unattached volume.

A volume is backed up when a [user-provided tag](#backup-service-about-tags) that is associated with a {{site.data.keyword.block_storage_is_short}} volume match the tags for target resources in a backup policy. The volume must have at least one of the backup policy’s tags for the target resources. When the scheduled backup is triggered by a backup plan, all volumes with matching tags are backed up. You can [view the status of the backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs) in the UI, from the CLI, or the API.

If a volume has multiple tags, only one tag needs to match for a backup to trigger. You can add user tags to boot and data volumes at any time, when you [create a virtual server instance](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-from-vsi) or in an instance template.

When the backup is triggered at the scheduled interval, a backup copy is created of your volume contents. Behind the scenes, the [Snapshot for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about) service is used to create a point-in-time copy of your data. When the first backup snapshot is taken, the entire contents of the volume are copied and retained in {{site.data.keyword.cos_full}}. Subsequent backups of the same volume capture the changes that occurred since the previous backup. You can take up to 750 backups of a volume.

You can copy a backup snapshot from one region to another region, and later use that snapshot to restore a volume in the new region. The [cross-regional copy](#backup-service-crc) can be used in disaster recovery scenarios when you need to turn on your virtual server instance and data volumes in a different region. The remote copy can be created automatically as part of a backup plan, or manually later.

You can [restore](#backup-service-restore-concepts) data from a backup snapshot to a new, fully provisioned volume. If the backup is of a boot volume, you can use it to provision a new instance. However, when you provision an instance by restoring a boot volume from a bootable backup snapshot, you can expect degraded performance in the beginning. During the restoration process, the data is copied from {{site.data.keyword.cos_full}} to {{site.data.keyword.block_storage_is_short}}, and thus the provisioned IOPS cannot be fully realized until that process finishes.

With the fast restore feature, you can cache snapshots in a specified zone of your choosing. This way, volumes can be restored from snapshots nearly immediately and the new volumes operate with full IOPS instantly. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular backup snapshot. When you opt for fast restore, your existing regional plan is adjusted, including billing.

### Comparison of backups and snapshots
{: #baas-comparison}

Backups are in effect, backup snapshots. In the console, backups appear in the list of {{site.data.keyword.block_storage_is_short}} snapshots. Backups are identified by how they were created, in this case, by backup policy. These terms are used interchangeably in the documentation, depending on the context.

The snapshots service is used to create backups, similarities and differences exist between backups and snapshots. Table 1 compares backups to snapshots.

| Feature | Account-level backup | Enterprise-level backup | Snapshot |
|---------|----------------------|-------------------------|----------| 
| Backs up {{site.data.keyword.block_storage_is_short}} boot and data volumes. | ![Checkmark icon](../icons/checkmark-icon.svg)  |  ![Checkmark icon](../icons/checkmark-icon.svg)  | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Manually restore a volume from a snapshot. | ![Checkmark icon](../icons/checkmark-icon.svg)  |  ![Checkmark icon](../icons/checkmark-icon.svg) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| Back up data by a schedule. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | |
| Back up data immediately. | | | ![Checkmark icon](../icons/checkmark-icon.svg)|
| Data is copied at a specific point in time. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Set a retention period for automatic deletion. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| Take up to 750 snapshots per volume. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Fast restore clone | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Cross-regional copy | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Costs are based on GBs per month. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Table 1. Comparison of backups and snapshots" caption-side="bottom"}

### Benefits of creating backups
{: #backup-service-benefits}

The {{site.data.keyword.cloud_notm}} Backup for VPC offers you the following benefits.

* Prevent data loss - Protect your critical data by scheduling regular backups. Establish a data restoration plan to quickly restore your compromised volumes. Reduce technical and financial impacts from unplanned outages.

* Protect against small-scale failures - Restore from a bootable snapshot if a host failure or malicious attack occurs.

* Improve compliance - You can prevent issues that are related to compliance and regulatory requirements by ensuring that backups are in place and data can be restored easily.

* Save costs - A backup policy retention plan means that backups are regularly deleted, reducing costs by not storing more data than needed.

* Ease of management - IBM's backup service is easier to manage than a third-party backup service because it is fully integrated with your VPC resources.

### Backup policies and backup plans
{: #backup-service-policies}

Backup policies consist of plans that define schedules for automatic backup creation and data retention. You can schedule backups on a daily, weekly, or monthly basis.

You can specify the retention period or the total number of backups before the oldest are deleted. The interval for creating a backup and its retention period can be the same or they can be different. The default retention period is 30 days. You can also set the total number of backups to retain up to 750 per volume. When that number is exceeded, the oldest backups are deleted. 

If you specify both the age and the number of backups, age takes priority in determining when to delete a snapshot. The count applies only if the oldest snapshot is within the age range.

Consider the following examples:
- Example 1 - You create a daily plan with a retention period of 14 days, and no maximum number of snapshots to keep. You're going to keep 14 backups and the oldest is going to be 2 weeks old.
- Example 2 - You create a weekly plan that has the retention period of 365 days, and the maximum count of 8. In practice, you're going to get a maximum of 8 snapshots in the chain, with the oldest being 8 weeks old.
- Example 2 - You create a monthly plan and set the retention period for 80 days with a maximum count of 10. In practice, you keep a maximum of 3 snapshots always. By the time when your 4th backup is taken, the first one meets the 80-day mark, and gets deleted.

You can create up to four plans per backup policy and modify the backup schedule and retention policy anytime. Your four plans can have different frequencies. For example, one can be daily. Another one can be weekly, or monthly. All plans apply to the volumes with tags that match the backup policy. Backups that are created by the backup plan inherit the parent volume resource group details. 

You can [view backup job status](/docs/vpc?topic=vpc-backup-view-policy-jobs) while backups are being created, modified, or deleted.

### Scope of the backup policies
{: #backup-service-about-scope}

[New]{: tag-new}

As an enterprise account administrator, you can manage backup plans and policies collectively across the child accounts under the enterprise account. Enterprise account users can see all the backup policies that were created by the Enterprise account. The Enterprise account user can see all the backup jobs that are initiated by the enterprise backup policy, even if the jobs run in the child accounts.

Users within each account in the enterprise can create, use, and collaborate on resources just as you can in a stand-alone account. For more information, see [Working with resources in an enterprise](/docs/secure-enterprise?topic=secure-enterprise-enterprise-best-practices#child-resources-enterprise).

While the Enterprise administrator sees the references of all the backup snapshots that are created by their policy, the child accounts see only the backups that are created in their account.

The users of the child account can see the backup snapshots that are created in their accounts by the enterprise-level policy. They can identify these backups by the CRN of the enterprise-level backup policy that created the backups. However, they have no visibility to the enterprise-level backup policy itself. Users of the child account can use the backups to create other volumes.

Authorized users in the child accounts can also create account-specific backup policies in their accounts.

The ability to create enterprise-wide backup policies is available in most regions, except Osaka and Tokyo MZRs in Japan.
{: restriction}

### Tags for target resources
{: #backup-service-about-tags}

Backup policies contain user tags for target resources that associate the policy with {{site.data.keyword.block_storage_is_short}} volumes with the same user tag. To create backups, at least one user tag that is applied to a volume must match the tag for target resources in the backup policy. User tags are added by an authorized user in the account. Any user with the correct access role can list and can delete user tags in the account on the condition that the tags are not attached to any resource.

In addition to user tags, tags can be access management tags and service tags. Only user tags are applied to backup policies. [Access management tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags) are used to manage access to resources; only the account administrator can create access management tags. Service tags are a privileged construct that only authorized services can manage. Users are not authorized to attach and detach service tags on a resource, even if they have access to manage tags on the resource. Service tags are helpful to distinguish which snapshots were created manually or automatically by a backup policy.
{: note}

If a volume has multiple tags, only one tag needs to match a backup policy tag. Based on the schedule in the backup plan, a matching tag triggers a backup. If multiple volume tags match backup policy tags, only one backup is created at the scheduled interval. If you have multiple volumes with the same tag, backups are created for all the volumes.

You can add up to 1,000 tags for your resources. However, keeping the number low can make it easier to track the number of backups that you're creating. For more information, see [Applying backup policies to resources with tags](/docs/vpc?topic=vpc-backup-use-policies).

When you decide on the tags to use for your target resources, make sure that other policies are not already using the same tags.

### Restore a volume from a backup
{: #backup-service-restore-concepts}

You can create a separate volume from a backup snapshot. This process is called restoring a volume. It works the same way as restoring data from manually created snapshots. Restoring a volume from a backup snapshot creates a fully provisioned {{site.data.keyword.block_storage_is_short}} boot or data volume.

A volume can be restored when you create an instance, modify an instance, or when you create a stand-alone volume (no instance provisioning is required). Volume data restoration begins immediately as the volume is hydrated, but [performance is degraded](/docs/vpc?topic=vpc-baas-vpc-restore&interface=ui#baas-performance-considerations) until the volume is fully restored. Restoring from a bootable snapshot is slower than using a regular boot volume.

While the creation of the backup requires the volume to be attached to a running virtual server instance, you can also restore a data volume from a snapshot of an unattached volume. Backups, like snapshots, have a lifecycle that is independent from the source {{site.data.keyword.block_storage_is_short}} volume.

The restored volume has the same capacity and IOPS tier profile as the original volume. For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).

### Restore a volume by using fast restore
{: #backup-service-fastrestore}

When you restore a volume by using fast restore, a fully hydrated volume is created.

You can create a [backup policy plan](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=ui#backup-plan-ui) with fast restore zones, and add or remove zones later as needed. The fast restore feature caches one or more copies of a backup snapshot to the zones that you selected. Later, you can use these backup clones to create volumes in any zone within the same region.

For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).

### Cross-regional backup copies
{: #backup-service-crc}

You can copy a backup snapshot from one region to another region, and later use that snapshot to restore a volume in the new region. You can use and manage the cross-regional snapshot in the target region independently from the parent volume or the original snapshot.

If the source snapshot is not encrypted with a customer key, the encryption of the copy remains provider-managed. If the source snapshot is protected by a customer-managed key, you must specify the customer-managed key that you want to use to encrypt the new copy.

Only one copy of the backup snapshot can exist in each region. You can't create a copy of the backup snapshot in the source (local) region.
{: restriction}

Creating a cross-regional copy affects billing. You're charged for the data transfer and the storage consumption in the target region separately.

## User roles for backup policies
{: #backup-service-user-roles}

Depending on your assigned role as a backup user, you can create and administer backup policies. Table 1 describes these capabilities.

| Backup user role | What you can do: |
|------|------------------|
| **Viewer, Operator** | * List all backup policies. \n * View details of a backup policy. \n * View a backup plan. |
| **Editor, Administrator** | * Organize and manage snapshots by using tags. \n * Associate backup policies to volumes by using user tags. \n * Create, update, and delete backup policies and plans. \n * Specify a CRON format to schedule snapshot creation. |
{: caption="Table 1. Backup user roles for backup policies" caption-side="bottom"}

| Volume user role | What you can do: |
|------|------------------|
| **Editor** | * Create data volumes with user tags. \n * View volume details with user tags. \n * List volumes filtered by tag. \n * Update user tags post volume creation. |
{: caption="Table 2. Volume user roles for backup policies" caption-side="bottom"}

| Snapshot user role | What you can do: |
|------|------------------|
| **Editor** | * Create snapshots with user tags. \n * View snapshot details with tags. \n * List snapshots filtered by tag. \n * Update user tags post snapshot creation.|
{: caption="Table 3. Snapshot user roles for backup policies" caption-side="bottom"}

## Service-to-service authorizations
{: #baas-s2s-auth}

Specific IAM user roles are required to grant service-to-service authorizations. Service to service authorizations between the Backup service and Cloud Block Storage, Snapshots for VPC, and Virtual server for VPC are needed so the backup service can detect volume tags and create snapshots. For more information, see [Establishing service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).

## Limitations in this release
{: #backup-service-limitations}

This release has the following limitations.

* You can create up to 10 backup policies per account.
* You can take a total of 750 backups per volume based on your backup policy, in your account and region. If you exceed this limit, no further backups are taken.
* The first backup and the entire volume backup cannot exceed 10 TB.
* You can't take a backup of a detached volume.
* You can't create a copy of a backup snapshot in the source (local) region. 
* You can create a copy of a backup in another region. However, only one copy of the backup snapshot can exist in each region.
* Enterprise-scoped backup policies are not available in MZRs in Japan.

## Next steps
{: #backup-next-steps}

Review [best practices](/docs/vpc?topic=vpc-backups-vpc-best-practices) for creating a backup policy and the [planning checklist](/docs/vpc?topic=vpc-backups-vpc-planning). Afterward, you can [create backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
