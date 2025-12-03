---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-03"

keywords: backup planning, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning backups
{: #backups-vpc-planning}

When you plan a strategy for backing up your {{site.data.keyword.block_storage_is_short}} volumes, you might find this checklist helpful to configure and use the backup service.
{: shortdesc}

## Planning backups
{: #baas-planning}

Consider the following prerequisites before you set up the VPC Backup Service.

| Item | Considerations |
|------|----------------|
| {{site.data.keyword.iamshort}} (IAM) | Verify that you have [IAM access permissions](/docs/account?topic=account-iam-service-roles-actions#is.backup-policy-roles) to create and manage backups for the account. |
| Enterprise-level backups | Make sure that all [Service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth) are in place for the services in the child account and the enterprise account. If the authorization is missing in any one of the child accounts, the backup service generates an {{site.data.keyword.at_full}} event and marks the policy health degraded.|
| Volumes | Evaluate which volumes are most important to back up. You can create backups of boot and data volumes. A volume with numerous changes and a lengthy retention period requires more attention than a volume with moderate changes. Also, the cumulative size of all backups for a volume can't exceed 10 TB. |
| Shares  | Evaluate which shares are most important to back up. A file share with numerous changes and a lengthy retention period requires more attention than a share with moderate changes. Also, the cumulative size of all snapshots and backups for a share can't exceed 100 TB. Snapshots are not supported for shares that have "VPC" access control mode. |
| Multi-volume consistency groups | You can create backups of multiple Block Storage volumes that are attached to the same virtual server. When you create backups this way, you tag the virtual server instance. You can choose to include the boot volume in the backup or exclude it. |
| Backup schedule | Determine a backup schedule based on the type of resources that you're backing up. For example, you might want to back up critical data that changes frequently more often than static data. |
| Retention | Determine a retention policy for backups in the backup plan. As subsequent backups are created, you incur costs for each backup that you retain. Deleting older backups keeps costs down. \n The interval for creating a backup and its retention period can be the same or they can be different. The default retention period is 30 days. The maximum retention period is 1000 days. You can also set the total number of backups to retain up to 750 per volume or share. When that number is exceeded, the oldest backups are deleted. If you specify both the age and the number of backups, age takes priority in determining when to delete a snapshot. The count applies only if the oldest snapshot is within the age range. For more information, see [Backup policies and backup plans](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-policies).|
| Backup plans | Decide on the number of backup plans that you need for a policy. For example, you can have separate plans for daily backups, weekly, and monthly. |
| Naming | Make sure that you have a unique name for your backup policy. For example, if you have a method for naming volumes, you might name a backup policy by using a similar convention. Naming conventions for backups that are created by the plan are the same as snapshots. For more information, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
| Interface | Choose the UI, CLI, API, or Terraform for creating and managing your backups. |
| Volume or share restore | Evaluate when you might want to restore a volume or a share from a backup. Keep in mind that restoring from a backup is a manual operation and not immediate such as a disaster recovery solution. |
| Fast-restore | You can create and cache a copy of the backup snapshot in one or more zones of the region where your volume is. Fast-restore can be used in disaster recovery scenarios when you need to restore volumes in a different zone of the same region. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular snapshot. |
| Cross-regional copy | You can create and store a copy of the backup snapshot in another region, and use it to create volumes in the target region. This feature can be used in disaster recovery scenarios when you need to start your virtual server instance and data volumes in a different region. Think about whether you need to restore data in other regions. The creation of a snapshot copy in a remote region takes time, up to several hours if your volumes are large. You're also charged for the data transfer and storage consumption in the target region separately. |
| Monitoring | Backup jobs that create or delete backup snapshots run according to the backup plan and the retention policy. You can check their status in the console, from the CLI, with the API, or Terraform. If a job fails, the health status code shows the reason for the failure. For more information, see [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs). You can also set up a connection to {{site.data.keyword.en_short}} and receive alerts to your preferred destinations. For more information, see [Enabling event notifications for Backup for VPC](/docs/vpc?topic=vpc-event-notifications-events). |
|**Creating backups**: |
| Prerequisites | Verify that the volume is attached to a virtual server instance and that the instance is in a running state. |
| Backup frequency | Verify that the plan that you selected is creating backups at the interval that you want. Backups do not occur instaneously like manually created snapshots. Backups usually occur within an hour of being triggered by a backup plan schedule. |
| Timing | Consistency group backups: Creating crash-consistent snapshots of multiple volumes that are attached to the same virtual server instance leads to a short-lived I/O suspension that can last from a few milliseconds to a few seconds. The duration depends on the number and size of volumes that are connected to your virtual server instance. It is recommended to run your automated backup job during off-peak hours to minimize any impact on performance. |
| Tags | Verify that at least one of your policy tags matches at least one tag of each resource that you want to back up. When you decide on the tags for your target resources, confirm that other policies are not using the same tags unless you want the resource to be backed up by multiple policies. |
|**Restoring a volume from a backup snapshot**: |
| Volume restore performance | Review these [performance considerations](/docs/vpc?topic=vpc-baas-vpc-restore#baas-performance-considerations) when you restore a volume from a backup snapshot. You might also experience different regional performance when you restore a volume. \n Evaluate when to enable [fast restore clones](/docs/vpc?topic=vpc-backup-service-about#backup-service-fastrestore). Fast restore snapshots reduce latency by restoring a volume from a snapshot clone. The new volume data is immediately restored. |
| Instance provisioning performance | Review these performance considerations when you decide on [provisioning an instance from a bootable snapshot](/docs/vpc?topic=vpc-baas-vpc-restore#baas-boot-perf). |
| Billing | Think about the number of backup snapshots that you want to take and other billing considerations as the number of backup snapshots grows. The fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots. When you choose to create cross-regional copies, you are also charged for data transfer and storage capacity that is used in the other region. For more information, see the [FAQs](/docs/vpc?topic=vpc-backup-service-vpc-faq&interface=ui#faq-baas-pricing). |
{: caption="Checklist for planning backup policies" caption-side="bottom"}

## Next steps
{: #baas-planning-next-steps}

After you plan your backups, you can [Create backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan) to back up your resources (individual volumes, or multi-volume consistency groups, or file shares).
