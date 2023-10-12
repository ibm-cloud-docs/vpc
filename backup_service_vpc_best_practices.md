---

copyright:
  years: 2022, 2023
lastupdated: "2023-09-29"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Best practices for backups
{: #backups-vpc-best-practices}

To ensure that you're using the VPC Backup Service most effectively and economically, consider the following suggestions.
{: shortdesc}

## General best practices
{: #baas-general-bp}

* Assess the type of data that you have before you create backup policies. Critical data that changes more often might require more frequent backups than static data. Ask which data is most critical and which is to be archived.

* Keep costs down by retaining backups for only while you need them to prevent data loss. Plan timely backups to restore data that might be deleted or corrupted. Think about the type of events that might happen. Ask how much data you can afford to lose. The answers can help you decide on a backup interval and retention policy.

* Ask how quickly you need to recover the data. Frequent backups that cover smaller incremental changes to your data afford quicker restoration. Run a volume restore and a test failover to get an idea of the time it can take.

* For best performance, stagger your backup jobs by creating backup plans with different intervals. You can have up to four different backup plans per backup policy.

* For large volumes, consider a shorter retention period so that you don't exceed the 10 TB limit for all volume backups.

* If you have volumes in different regions, create separate backup policies for each region. You're limited to 10 backup policies per account in a region.

* Ensure that the volumes that you tagged for backups are attached to a virtual server instance. You can't back up detached volumes.

* Provide a unique name for your backup policy. If you have a convention for naming volumes, you might name a backup policy by using a similar convention. Backups that are created by a policy can also follow the convention. As the number of backups grow, a good naming convention can make them more identifiable.

## Best practices for user actions
{: #baas-user-actions-bp}

* Organize tags that you apply to volumes and specify in backup policies. Ensure that multiple policies aren't using the same tags for target resources because that might trigger duplicate volume backups. You need only one tag to match to trigger a backup.

* Determine what tags are already assigned to a volume from the list of {{site.data.keyword.block_storage_is_short}} volumes. If a volume has multiple tags, make sure that the tags are not triggering duplicate backups for multiple policies.

* Decide how you prefer to add tags. You can create tags for target resources in your backup policy first and then apply them to volumes. Or, you can specify tags that are applied to the volume already in the backup policy. If you choose to use existing tags, be aware that the same tags might be attached to other volumes that you might not want to back up from this policy.

* Decide whether creating one or multiple backup plans suits your needs. For example, multiple plans can trigger backups at different intervals. You might want to back up some volumes monthly, others more frequently in a daily or weekly plan.

## Administrator best practices
{: #baas-admin-bp}

* Determine which users can have permission to add tags to target resources. For more information, see [Granting users access to tag resources](/docs/account?topic=account-access).

## Applying best practices
{: #baas-apply-bp}

Apply best practices when you create a backup strategy. The following example illustrates how you might set up a backup solution.

Assume you have 10 volumes that are spread across different departments. Some volumes contain time-critical information that you need to back up hourly. Other volumes contain archived information that doesn't change much, so you'd prefer to back them up weekly. You can create four backup plans per policy. Your solution might look like this:

Create a backup policy with an hourly backup plan for the time-critical volumes:

* When you create the policy, create a tag for the target volumes (for example, `hourly-finance`). Provide a policy name that reflects the interval and type of data. It can help organize policies in the list and the volumes that are associated with them.

* Add the new tags to your volumes - Specify tags in **Apply tags to target resources** in the UI or specify them in the CLI or API. It ensures that all volumes that you want to back up hourly are included by the hourly backup policy.

* If your volume already contains tags: verify that another backup policy isn't already backing up the volume. If that's the case, remove the extra tag from the volume so that it is not backed up twice, and incurring extra costs.

* Set a retention period that does not exceed the total backup limitation of 10 TB. For hourly backups, you might need a shorter retention period than daily or weekly backups.

Create a weekly backup plan for archived data:

* Create another plan and define the backup frequency as 7 days. Assess the amount of data that is in the volume, and anticipated changes. The limit is 10 TB for all backups of the volume.

* Set a longer retention period to have multiple copies of your archive volume. For weekly backups, you might want to retain the backups for a month.

If you specify both age and the number of backups in your retention policy, age takes priority in determining when to delete a snapshot. The count applies only if the oldest snapshot is within the age range.

For example, when you create the weekly plan and specify the retention period as 365 days, you can also specify the maximum count of 8. In this scenario, you're going to get a maximum of 8 backups in the chain, with the oldest being 8 weeks old. Alternatively, if you specify 30 days as your retention period and set the number of maximum backups to 8, by the time the 5th backup is taken, the first one is going to be deleted as it is outside of the 30-day retention period.

## Next Steps
{: #baas-bp-next-steps}

[Plan a strategy](/docs/vpc?topic=vpc-backups-vpc-planning) for backing up your {{site.data.keyword.block_storage_is_short}} volumes.
[Create a backup policy](/docs/vpc?topic=vpc-backup-policy-create).
