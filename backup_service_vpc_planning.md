---

copyright:
  years: 2022, 2023
lastupdated: "2023-11-13"

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
| {{site.data.keyword.iamshort}} (IAM) | Verify that you have [IAM access permissions](/docs/vpc?topic=vpc-backup-service-manage#baas-vpc-iam) to create and manage backups for the account. |
| Enterprise-level backups | Ensure that all [Service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth) are in place for the services in the child account and the enterprise account. If the authorization is missing in any one of the child accounts, the backup service generates an {{site.data.keyword.at_full}} event and marks the policy health degraded.|
| Volumes | Evaluate which volumes are most important to back up. You can create backups of boot and data volumes. A volume with numerous changes and a lengthy retention period requires more attention than a volume with moderate changes. Also, the cumulative size of all backups for a volume can't exceed 10 TB. |
| Backup schedule | Determine a backup schedule based on the type of volumes that you're backing up. For example, you might want to back up critical data that changes frequently more often than static data. |
| Retention | Determine a retention policy for backups in the backup plan. As subsequent backups are created, keep in mind that you incur costs for each backup that you retain. Deleting older backups keeps costs down. |
| Backup plans | Decide on the number of backup plans that you need for a policy. For example, you can have separate plans for daily backups, weekly, and monthly. |
| Interface | Choose the UI, CLI, API, or Terraform for creating and managing your backups. |
| Billing | Think about the number of backup snapshots that you want to take and other billing considerations as the number of backup snapshots grows. Billing for the fast restore feature is set at a flat rate based on instance hours. The feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots. For more information, see the [FAQs](/docs/vpc?topic=vpc-backup-service-vpc-faq&interface=ui#faq-baas-pricing). |
| Volume restore | Evaluate when you might want to restore a volume from a backup. Keep in mind that restoring from a backup is a manual operation and not immediate such as a disaster recovery solution. |
| Fast-restore | You can create and cache a copy of the backup snapshot in one or more zones of the region where your volume resides. Fast-restore can be used in disaster recovery scenarios when you need to restore volumes in a different zone of the same region. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular snapshot. Evaluate when to enable snapshots for [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-vpc-use-fast-restore).|
| Cross-regional copy | You can create and store a copy of the backup snapshot in another region, and use it to create volumes in the target region. This feature can be used in disaster recovery scenarios when you need to start your virtual server instance and data volumes in a different region. Think about whether you need to restore data in other regions. |
| Naming | Make sure you have a unique name for your backup policy. For example, if you have a method for naming volumes, you might name a backup policy by using a similar convention. Naming conventions for backups that are created by the plan are the same as snapshots. For more information, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
|**Creating backups**: |
| Prerequisites | Verify that the volume is attached to a virtual server instance and that the instance is in a running state. |
| Backup frequency | Verify that the plan that you selected is creating backups at the interval that you want. Backups do not occur instaneously like manually created snapshots. Backups usually occur within an hour of being triggered by a backup plan schedule. |
| Tags | Verify that at least one of your policy tags matches at least one tag of each volume that you want to back up. When you decide on the tags for your target resources, confirm that other policies are not using the same tags unless you want the volume to be backed up by multiple policies. |
|**Restoring a volume from a backup snapshot**: |
| Volume restore performance | Review these [performance considerations](/docs/vpc?topic=vpc-baas-vpc-restore#baas-performance-considerations) when you restore a volume from a backup snapshot. You might also experience different regional performance when you restore a volume. \n Evaluate when to enable [fast restore clones](/docs/vpc?topic=vpc-backup-service-about#backup-service-fastrestore). Fast restore snapshots reduce latency by restoring a volume from a snapshot clone. The new volume data is immediately restored. |
| Instance provisioning performance | Review these performance considerations when you decide on [provisioning an instance from a bootable snapshot](/docs/vpc?topic=vpc-baas-vpc-restore#baas-boot-perf). |
{: caption="Table 1. Checklist for planning backup policies" caption-side="bottom"}

## Next steps
{: #baas-planning-next-steps}

After you plan your backups, you can [Create backup policies](/docs/vpc?topic=vpc-backup-policy-create) to back up your volumes.
