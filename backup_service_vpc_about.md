---

copyright:
 years: 2022, 2025
lastupdated: "2025-12-11"

keywords: Backup, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About Backup for VPC
{: #backup-service-about}

Use {{site.data.keyword.cloud}} Backup for VPC to automatically create backups and manually restore {{site.data.keyword.block_storage_is_short}} volumes and {{site.data.keyword.filestorage_vpc_short}} shares from backup snapshots. By using this service, you can prevent data loss, manage risk, and improve data compliance. You can ensure that your data is backed up regularly, and you can retain the backups while you need them. You can create and manage backup policies and plans in the console, from the CLI, with the API, or Terraform.
{: shortdesc}

Backups and snapshot services are different than a [disaster recovery (DR)](#x2113280){: term} solution, where your data is continually backed up with automatic failover. Restoring a volume or a share from a backup or a snapshot is a manual operation that takes time. If you require a higher level of service for disaster recovery, see IBM's [Cloud disaster recovery solutions](https://www.ibm.com/solutions/disaster-recovery).
{: important}

## Backup service concepts
{: #backup-service-concepts}

You can create up to 10 [backup policies](#backup-service-policies) in one region with the {{site.data.keyword.cloud_notm}} Backup for VPC service. You can create up to four plans per policy, and edit and delete them as needed. If you're undecided on the backup schedule or retention requirements, you can create a backup policy without a plan and add one later. 

You can back up individual {{site.data.keyword.block_storage_is_short}} volumes or {{site.data.keyword.filestorage_vpc_short}} shares that are identified by tags.

In the current release of the defined performance volume profile, you can automate the creation of snapshots of volumes that exceed 10 TB. Backup snapshots over 10 TB are not supported for Generation 1 (`custom` and `tiered`) block volume profiles.

You can create backups of all the {{site.data.keyword.block_storage_is_short}} volumes that are attached to a specific virtual server instance as members of a **consistency group**. When you configure backups for a [consistency group](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#multi-volume-snapshots), you can include the boot volume or exclude it. When you create multi-volume backups, you must add the tag to the virtual server instance, not the individual volumes.

In the current release of the defined performance volume profile, multi-volume backups are not supported. When you try to create consistency group snapshots with a mix of different volume generations, only the first-generation volumes are included in the backup snapshots. Second-generation, `sdp` volumes are skipped.
{: restriction}

When you request a backup snapshot of a consistency group, the system ensures that all write operations are complete before it takes the snapshots. Then, the system generates snapshots of all the selected Block Storage volumes that are attached to the virtual server instance at the same time. Depending on the number and size of the attached volumes, plus the amount of data that is to be captured, you might observe a slight IO pause. This IO pause can range from a few milliseconds up to 4 seconds. It is recommended to run your automated backup job during off-peak hours to minimize any impact on performance.

An individual volume or share is backed up when a user-provided [tag](#backup-service-about-tags) that is associated with a volume or share matches the tags for target resources in a backup policy. When you choose to back up all the Block Storage volumes that are attached to a virtual server instance, the user-provided tag is associated with the virtual server instance. When the scheduled backup is triggered by a backup plan, all resources with matching tags are backed up.

When you want to create backups of first-generation block volumes, they must be attached to a running virtual server instance. You can't create backup snapshots an unattached Gen 1 volume.
{: important}

If a volume or share has multiple tags, only one tag needs to match for a backup to trigger. You can add user tags to boot and data volumes at any time, when you [create a virtual server instance](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-from-vsi) or when you update the volume.

You must set a retention period for your backups. It can be based on the number of days that you want to keep the backups. Or it can be based on the total number of backups that you want to retain before the oldest ones are deleted. Or you can set it up by specifying both the time limit and the maximum number of backups that you want to keep. Planning for the retention and deletion of your backups can keep the costs down.

When the backup is triggered at the scheduled interval, a backup copy is created of your volume by the [Snapshot for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about) service. When the first backup snapshot is taken, the entire contents of the volume are copied and retained in a regional storage repository. Subsequent backups of the same volume capture the changes that occurred since the previous backup. You can take up to 750 backups of a first-generation volume. Second-generation volumes have a backup snapshot limit of 512.

Backup jobs that create or delete backup snapshots run according to the backup plan and the retention policy. You can [view the status of the backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs) in the console, from the CLI, with the API, or Terraform. If a job fails, the health status code shows the reason for the failure. You can also set up a connection to {{site.data.keyword.en_short}} and receive notifications to your preferred destinations.

Block storage backups, like block storage snapshots, have a lifecycle that is independent from the source {{site.data.keyword.block_storage_is_short}} volume. File storage backups, like file share snapshots, coexist with their parent file shares and their lifecycles are tied together. If a file share is deleted, its snapshots and backups are automatically deleted, too.

You can copy a Block storage backup snapshot from one region to another region, and later use that snapshot to restore a volume in the new region. The [cross-regional copy](#backup-service-crc) can be used in disaster recovery scenarios when you need to turn on your virtual server instance and data volumes in a different region. The remote copy can be created automatically as part of a backup plan, or manually later.

When the backup of a file share is triggered at the scheduled interval, a point-in-time snapshot is taken of your share. When the first backup snapshot is taken, the entire contents of the share are copied and retained in the same location as the share. Subsequent backups of the same volume capture the changes that occurred since the previous backup. You can take up to 750 backups of a share. If a file share has a replica in another zone, its backups are automatically copied to the replica location. However, file share backups cannot be independently copied to other zones or regions. For more information, see [About {{site.data.keyword.filestorage_vpc_short}} snapshots](/docs/vpc?topic=vpc-fs-snapshots-about).  

You can [restore](#backup-service-restore-concepts) data from a backup snapshot to a new, fully provisioned volume. If the backup is of a boot volume, you can use it to provision a new instance. However, when you provision an instance by restoring a boot volume from a bootable backup snapshot, you can expect degraded performance in the beginning. During the restoration process, the data is copied from the regional storage repository to {{site.data.keyword.block_storage_is_short}}, and thus the provisioned IOPS cannot be fully realized until that process finishes.

With the fast restore feature, you can cache snapshots in a specified zone of your choosing. This way, volumes can be restored from snapshots nearly immediately and the new volumes operate with full IOPS instantly. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular backup snapshot. When you opt for fast restore, your existing regional plan is adjusted, including billing. The fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots. The fast restore feature is supported only for individual volume backups, not for consistency group backups.

You can also restore data from a backup snapshot of a file share. You can either create a file share or restore a single file by accessing the backup snapshot directly through the mount target. A new share that is created from a backup is fully available for read and write operations immediately. 

As an enterprise account administrator, you can view and manage the backup policies and plans for the subaccounts for compliance reporting and billing from one place. For more information, see the [Scope of backup policy](#backup-service-about-scope) section.

### Comparison of backups and snapshots
{: #baas-comparison}

Backups are in effect, automated snapshots with a retention date. In the console, backups appear in the same lists as the snapshots. Block storage snapshots and backups are listed on the Block storage snapshots for VPC page. File share snapshots and backups can be found on the Snapshots tab of each file share. Backups are identified by how they were created, by backup policy instead of a user. The terms snapshots and backups are used interchangeably in the documentation, depending on the context. Many similarities exist between snapshots and backups, and some differences, too. The following table compares backups and snapshots:

| Feature | Account-level block volume backup | Enterprise-level block volume backup | Block volume snapshot | File share snapshot | File share backup |
|---------|----------------------|-------------------------|----------|--------|---------| 
| Backs up {{site.data.keyword.block_storage_is_short}} boot and data volumes. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | N/A | N/A |
| Backs up {{site.data.keyword.filestorage_vpc_short}} shares | N/A | N/A | N/A | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Manually restore an entire volume or share from a snapshot. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Manually restore a single file from a snapshot |N/A | N/A| N/A | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Back up data by a schedule. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | No | No |![Checkmark icon](../icons/checkmark-icon.svg) |
| Back up data immediately. | No | No | ![Checkmark icon](../icons/checkmark-icon.svg)| ![Checkmark icon](../icons/checkmark-icon.svg)| No |
| Data is copied at a specific point in time. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Set a retention period for automatic deletion. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | No | No | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Take up to 750 snapshots per volume¹ or share. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| Fast restore clone | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | Not supported | Not supported |
| Cross-regional copy | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | Not supported | Not supported |
| Multi-volume consistency groups | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | N/A | N/A |
| Costs are based on GBs per month. | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Comparison of backups and snapshots" caption-side="bottom"}

¹ You can have 750 snapshots of first-generation storage volumes and 512 snapshots of second-generation storage volumes.
  
### Benefits of creating backups
{: #backup-service-benefits}

The {{site.data.keyword.cloud_notm}} Backup for VPC offers you the following benefits.

* Prevent data loss - Protect your critical data by scheduling regular backups. Establish a data restoration plan to quickly restore your compromised volumes or shares. Reduce technical and financial impacts from unplanned outages.

* Protect against small-scale failures - Restore boot volume from a bootable snapshot if a host failure or malicious attack occurs.

* Improve compliance - You can prevent issues that are related to compliance and regulatory requirements by ensuring that backups are in place and data can be restored easily.

* Save costs - A backup policy retention plan means that backups are regularly deleted, reducing costs by not storing more data than needed.

* Ease of management - IBM's backup service is easier to manage than a third-party backup service because it is fully integrated with your VPC resources.

### Backup policies and backup plans
{: #backup-service-policies}

A backup policy identifies the target resource types and lists the tags that are used to identify the resources to be backed up. The policy contains one or more backup plans that define schedules for automatic backup creation and data retention.

In a backup plan, you schedule the frequency of your backups. In the [console](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=ui), you can choose daily, weekly, or monthly. Or you can use a `cron` expression to specify the frequency.
{: ui}

In a backup plan, you schedule the frequency of your backups. When you create a plan from the [CLI](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=cli), you can use a `cron` expression to specify the frequency.
{: cli}

In a backup plan, you schedule the frequency of your backups. When you create a plan with the [API](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=api), you can use a `cron` expression to specify the frequency.
{: api}

In a backup plan, you schedule the frequency of your backups. When you create a plan with [Terraform](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=terraform), you can use a `cron` expression to specify the frequency.
{: terraform}

You can specify the retention period or the total number of backups before the oldest is deleted. The interval for creating a backup and its retention period can be the same or they can be different. The default retention period is 30 days, but you can set the retention period value anywhere between 1 and 1000 days. You can also set the total number of backups to retain up to 750 per first-generation volume. The maximum number of backup snapshots for second-generation volumes is 512. When the maximum number is exceeded, the oldest backups are deleted.

If you specify both the age and the number of backups, age takes priority in determining when to delete a snapshot. The count applies only if the oldest snapshot is within the age range.

Consider the following examples:
- Example 1 - You create a daily plan with a retention period of 14 days, and no maximum number of snapshots to keep. You're going to keep 14 backups and the oldest is going to be 2 weeks old.
- Example 2 - You create a weekly plan that has a retention period of 365 days, and a maximum count of 8. In practice, you're going to get a maximum of 8 snapshots in the chain, with the oldest being 8 weeks old.
- Example 2 - You create a monthly plan and set the retention period for 80 days with a maximum count of 10. In practice, you keep a maximum of 3 snapshots always. By the time when your 4th backup is taken, the first one meets the 80-day mark, and gets deleted.

You can create up to four plans per backup policy and modify the backup schedule and retention policy anytime. Your four plans can have different frequencies. For example, one can be daily. Another one can be weekly, or monthly. All plans apply to the volumes with tags that match the backup policy. Backups that are created by the backup plan inherit the parent volume resource group details. 

You can [view backup job status](/docs/vpc?topic=vpc-backup-view-policy-jobs) while backups are being created, modified, or deleted.

You can delete your backup plans or the backup policy when you no longer need them. When you delete a backup policy, you also delete all the plans that are associated with it. When you delete or disable a backup plan, it can no longer initiate any backup jobs. It does not create or delete any backup snapshot anymore. The existing backups remain intact as their lifecycle is independent from the policy. Existing backups must be deleted separately.

### Scope of the backup policies
{: #backup-service-about-scope}

As an enterprise account administrator, you can manage backup plans and policies collectively across the child accounts under the enterprise account. Enterprise account users can see all the backup policies that were created by the Enterprise account. The Enterprise account user can see all the backup jobs that are initiated by the enterprise backup policy, even if the jobs run in the child accounts.

Users within each account in the enterprise can create, use, and collaborate on resources just as you can in a stand-alone account. For more information, see [Working with resources in an enterprise](/docs/enterprise-management?topic=enterprise-management-enterprise-best-practices#child-resources-enterprise).

While the Enterprise administrator sees the references of all the backup snapshots that are created by their policy, the child accounts see only the backups that are created in their account.

The users of the child account can see the backup snapshots that are created in their accounts by the enterprise-level policy. They can identify these backups by the CRN of the enterprise-level backup policy that created the backups. However, they have no visibility to the enterprise-level backup policy itself. Users of the child account can use the backups to create other volumes.

Authorized users in the child accounts can also create account-specific backup policies in their accounts.

### Tags for target resources
{: #backup-service-about-tags}

Backup policies contain user tags for target resources that associate the policy with block storage volumes, file shares, or virtual server instances with the same user tag. To create backups, at least one user tag that is applied to a resource must match the tag in the backup policy. User tags are added by an authorized user in the account. Any user with the correct access role can list and delete user tags in the account on the condition that the tags are not attached to any resource.

In addition to user tags, tags can be access management tags and service tags. Only user tags are applied to backup policies. [Access management tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags) are used to manage access to resources; only the account administrator can create access management tags. Service tags are a privileged construct that only authorized services can manage. Users are not authorized to attach and detach service tags on a resource, even if they have access to manage tags on the resource. Service tags are helpful to distinguish which snapshots were created manually or automatically by a backup policy.
{: note}

If a volume, a share, or a virtual server instance has multiple tags, only one tag needs to match a backup policy tag. Based on the schedule in the backup plan, a matching tag triggers a backup. If multiple tags match backup policy tags, only one backup is created at the scheduled interval. If you have multiple resources with the same tag, backups are created for all the matching resources.

You can add up to 1,000 user tags for your resources. However, only 100 tags can be attached or detached in the same operation. Keeping the number of tags low can make it easier to track the number of backups that you're creating. For more information, see [Applying backup policies to resources with tags](/docs/vpc?topic=vpc-backup-use-policies).

When you decide on the tags to use for your target resources, make sure that other policies are not already using the same tags.

### Restore a volume or a share from a backup
{: #backup-service-restore-concepts}

You can create a separate volume or share from a backup snapshot. This process is called restoring. It works the same way as restoring data from manually created snapshots. Restoring a volume from a backup snapshot creates a fully provisioned {{site.data.keyword.block_storage_is_short}} boot or data volume.

A volume can be restored when you create an instance, modify an instance, or when you create a stand-alone volume. Volume data restoration begins immediately as the volume is hydrated, but [performance is degraded](/docs/vpc?topic=vpc-baas-vpc-restore&interface=ui#baas-performance-considerations) until the volume is fully restored. Restoring from a bootable snapshot is slower than using a regular boot volume.

The restored volume has the same capacity and volume profile as the original volume. For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).

Restoring a virtual server instance directly from snapshot consistency group identifier is not supported. However, you can restore a virtual server instance by restoring all of its boot and data volumes from the snapshots that are part of a consistency group.

File share backups can be used to create new shares, too. Because the snapshot is colocated with the file share, the performance of the new share is not impacted during initialization. However, for the same reason, file share backups can't be copied to another region or zone by themselves to create new shares. For more information, see [Restoring a share from a snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore).

A snapshot of a first-generation volume or share cannot be used to create a second-generation volume or share. Similarly, you can't use a snapshot from a second-generation volume or share to create a block volume or file share with a first-generation profile.
{: important}

### Restore a volume by using fast restore
{: #backup-service-fastrestore}

When you restore a block storage volume by using fast restore, a fully hydrated volume is created.

You can create a [backup policy plan](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=ui#backup-plan-ui) with fast restore zones, and add or remove zones later as needed. The fast restore feature caches one or more copies of a backup snapshot to the zones that you selected. Later, you can use these backup clones to create volumes in any zone within the same region.

For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).

### Cross-regional backup copies
{: #backup-service-crc}

You can copy a Block storage backup from one region to another region, and later use that snapshot to restore a volume in the new region. You can use and manage the cross-regional snapshot in the target region independently from the parent volume or the original snapshot.

Currently, cross-regional copy of block storage snapshots is not supported in the Chennai region. It can't be selected as a source or target region.
{: restriction}

When a backup policy creates a job that includes a cross-regional copy, the service waits to initiate the request to create the copies in the target regions. The service start to create the copies after the source snapshot reached the _stable_ state.

When you create a remote copy of the backup snapshot for the first time, that remote copy contains all the data of the parent volume. Subsequent copies can be incremental or full copies. 

Whether the remote copy is incremental depends on the immediately preceding backup snapshot in the chain. If the immediately preceding snapshot exists in the destination region, the copy can be incremental. If the immediately preceding snapshot does not exist or is not stable, the new copy must be a full snapshot of the parent volume.

If your backup plan calls for the creation of a remote copy before the previous backup copy becomes stable, the Backup service initiates a full copy, not an incremental one.

| Volume capacity | Backup plan schedule | Incremental copies |
|-----------------|----------------------|--------------------|
| 10 GB           | 1-hour               | Enabled            |
| 50 GB           | 1-hour               | Enabled            |
| 100 GB          | 1-hour               | Enabled            |
| 200 GB          | 1-hour               | Enabled            |
| 250 GB          | 1-hour               | Disabled           |
| 250 GB          | 2-hour               | Enabled            |
| 500 GB          | 2-hour               | Disabled           |
| 500 GB          | 3-hour               | Enabled            |
| 1000 GB         | 4-hour               | Disabled           |
| 1000 GB         | 5-hour               | Enabled            |
| 2000 GB         | 8-hour               | Disabled           |
| 2000 GB         | 9-hour               | Enabled            |
| 3000 GB         | 12-hour              | Disabled           |
| 3000 GB         | Daily                | Enabled            |
{: caption="How storage volume capacity and backup schedules affect the creation of remote snapshot copies." caption-side="bottom"}

The more capacity that a volume has, the longer it takes to create the snapshot copy in another region. For example, creating a copy of a 3-TB storage volume in a remote region can take over 12.5 hours. Therefore, when your schedule specifies the creation of a snapshot with a remote copy every 12 hours, the system initiates a full copy because the previous copy is not complete and fully stable yet.

If the source snapshot is not encrypted with a customer key, the encryption of the copy remains provider-managed. If the source snapshot is protected by a customer-managed key, you must specify the customer-managed key that you want to use to encrypt the new copy.

Only one copy of the backup snapshot can exist in each region. You can't create a copy of the backup snapshot in the source (local) region.
{: restriction}

Creating a cross-regional copy affects billing. You're charged for the data transfer and the storage consumption in the target region separately.

## Limitations
{: #backup-service-limitations}

This release has the following limitations.

Backup policy:
* You can create up to 10 backup policies per account in a region. This quota can't be increased.

Volume backups:
* You can take a total of 750 backups per Gen 1 volume based on your backup policy, in your account and region. You can take a total of 512 backups per Gen 2 volumes. If you exceed the limit, no further backups are taken.
* The first backup and the entire volume backup cannot exceed 10 TB if the parent volume is based on a `tiered` or `custom` profile.
* You can't take a backup of a detached volume.
* You can't create a copy of a backup snapshot in the source (local) region. 
* You can create a copy of a block storage backup in another region. However, only one copy of the backup snapshot can exist in each region.
* Cross-regional copies are not supported in Montreal (`ca-mon`) and Chennai (`in-che`) MZRs.
* [Context-based restriction rules](#baas-cbr) are not supported in Montreal (`ca-mon`) and Chennai (`in-che`) MZRs.
* Consistency groups consist of the attached Block Storage volumes of virtual server instances, such as boot and data volumes. Instance storage volumes and virtual server instance configuration are not included.
* The fast restore feature is not supported for multi-volume backups of consistency groups.
* In the current release of second-generation volumes, the following limitations apply.
   - You can take up to 512 backup snapshots of your `sdp` volume.
   - You can't create consistency group backups that contain `sdp` volumes.

File share backups:
* You can take a total of 750 backups per zonal file share, and 30 backups for regional file shares.
* You can't create a copy of a file storage backup in another region. File share snapshots and backups are tied to their source shares. Backups remain available even if the source share is deleted. 
* Backup snapshots are not supported for shares that have "VPC" access control mode.
* The fast restore feature is not supported for file share backups.

## Securing your data
{: #backup-data-security}

{{site.data.keyword.cloud}} offers security-specific tools and features to help you securely manage your data when you use {{site.data.keyword.vpc_full}}. The following section provides information about access control, data encryption, configuration management, and auditing options that are available for you when you use the Backup service.

### IAM roles for backup policies
{: #baas-vpc-iam}

Backups require IAM permissions for role-based access control. Depending on your assigned role as a backup user, you can create and administer backup policies. For more information, see [IAM roles and actions for Regional Backup as a Service for VPC](/docs/account?topic=account-iam-service-roles-actions#is.backup-policy-roles).

For more information, see the [best practices for assigning access](/docs/account?topic=account-account_setup#account_setup). For the complete IAM process, which includes inviting users to your account and assigning Cloud IAM access, see the [IAM getting started tutorial](/docs/account?topic=account-iamoverview).
{: tip}

### Service-to-service authorizations
{: #baas-s2s-auth}

Specific IAM user roles are required to grant service-to-service authorizations. Service-to-service authorizations between the Backup service and Cloud Block Storage, Snapshots for VPC, and Virtual server for VPC are needed so the backup service can detect volume tags and create snapshots. If you want to create automated snapshots of your file shares, set up service-to-service authorizations between the Backup service and the Cloud File Storage service. For more information, see [Establishing service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).

### Context-based restrictions
{: #baas-cbr}

You can enable context-based restrictions (CBR) for all block volume and file share operations. These restrictions work with traditional IAM policies, which are based on identity, to provide an extra layer of protection. For more information, see [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr). 

When you create a [context-based rule](/docs/vpc?topic=vpc-cbr&interface=ui#cbr-rules), make sure that it allows private endpoints. All the inter-service connections that the Backup service needs to work are private.

### Encryption at rest and in transit
{: #backup-encryption}

The backup snapshot has the same encryption type and encryption key as the parent volume or share (customer-managed or provider-managed).

Volume backups are stored and retrieved from a regional storage repository. Data is encrypted while in transit and stored in the same region as the original volume.

### Activity tracking and auditing
{: #backup-activity-tracker}

When a backup is created, an event is triggered for the [Backup service](/docs/vpc?topic=vpc-at_events&interface=ui#events-backup-service) and [Snapshots service](/docs/vpc?topic=vpc-at_events&interface=ui#events-snapshots). Similarly, when the service fails to create a backup due to missing authorization, an event is triggered to notify you. Event logs are also created when backup policies or plans are created or deleted. For more information, see [Activity tracking events for IBM Cloud VPC](/docs/vpc?topic=vpc-at_events).


## Next steps
{: #backup-next-steps}

Review [best practices](/docs/vpc?topic=vpc-backups-vpc-best-practices) for creating a backup policy and the [planning checklist](/docs/vpc?topic=vpc-backups-vpc-planning). Afterward, you can [create backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
