---

Copyright:
  years:  2022
lastupdated: "2022-02-17"

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

# Best practices when creating backups (Beta)
{: #backups-vpc-best-practices}

To assure you're using the VPC Backup Service most effectively and economically, consider following these best practices.
{: shortdesc}

This service is available only to accounts with special approval to preview this beta feature.
{: beta}

## General best practices
{: #baas-general-bp}

* Assess the type of data you have when creating backup policies. Critical data that changes more often might require more frequent backups than static data. Ask which data is most critical and which should be archived.

* Keep costs down by retaining backups for only as long as you need to prevent data loss. Plan timely backups to restore data that might be deleted or corrupted. Think about they type of events that might happen. Ask how much data you can afford to lose. That will help you decide on a backup interval. 

* Ask how quickly you need to recover the data. Frequent backups cover smaller incremental changes to your data and afford quicker restoration. Run a test fail over and volume restore to get an idea of the time it will take.

* For very large volumes, consider a shorter retention period so that you don't exceed the 10 TB limit for all volume backups.

* If you have volumes in different regions, create separate backup policies for each region. You're limited to 10 backup policies per account in a region.

* Assure the volumes you've tagged for backups are attached to a virtual server instance. You can't backup detached volumes.

* Provide a unique name for your backup policies. If you have a convention for naming volumes, you might name a backup policy using a similar convention. Backups created by a policy should also follow the convention. As the number of backups grow, a good naming convention will make them more identifiable.

## Best practices for user actions
{: #baas-user-actions-bp}

* Organize tags that you apply to volumes and specify in backup policies. Ensure multiple policies aren't using the same tags for target resources, which will trigger duplicate volume backups. You only need one tag to match to trigger a backup.

* Determine what tags a volume already has from the list of block storage volumes. If a volume has multiple tags, make sure the tags are not triggering duplicate backups from multiple policies.

* Decide which way you prefer to add tags. You can create new tags for target resources first in your backup policy and then apply them to volumes. Or, you can specify tags already applied to the volume in the backup policy. If you choose to use existing tags, be aware that the same tags might be in other volumes you might not want to backup from this policy.

* If you want to change a backup policy schedule in the Beta release, you must delete the policy and create a new backup plan. You're limited to one plan per policy and you can't change the plan after it's created.

## Administrator best practices
{: #baas-admin-bp}

* Determine which users will have permission to add tags to target resources.  For more information, see [Granting users access to tag resources](/docs/account?topic=account-access).

## Applying best practices
{: #baas-apply-bp}

Apply best practices when creating a backup strategy. The following example illustrates how you might consider setting up a backup solution.

Assume you have 10 volumes spread across different departments. Some volumes contain time-critical information that you need to backup hourly. Other volumes contain archived information that doesn't change much, so you'd prefer to back them up weekly. For Beta, you can have one backup plan per policy, so your solution might look like this:

 Create a backup policy with an hourly plan for the time-critical volumes:

* When creating the new policy, create a new tag for the target volumes (for example, `hourly-finance`). Provide a policy name that reflects the internval and type of data. This will help organize policies in the list and volumes associated with them.

* Add the new tags to your volumes - Specify tags in **Apply tags to target resources** in the UI or specify them in the CLI or API. his will ensure all volumes you want to backup hourly are applied to the hourly backup policy.

* If your volume already contains tags; verify that another backup policy isn't already backing up the volume. If that's the case, you should remove that tag from the volume so that it's not backed up twice, incurring additional costs.

* Set a retention period that will not exceed the total backup limitation of 10 TB. For hourly backups, you might need a briefer retention period than daily or weekly backups. You are also allowed up to 100 backups of the volume. Ensure that you don't exceed that quota.

Create a backup policy with a weekly plan for archived data:

* Create a new policy and define the retention period as 7 days. Assess the length of time you can afford not to back up archived data, the amount of data currently in the volume, and anticipated changes. The limit is 10 TB for all backups of the volume.

* Set a longer retention period to have multiple copies of your archive volume. For weekly backups, you might want to retain the backups for a month.

## Next Steps
{: #baas-bp-next-steps}

[Plan a strategy](/docs/vpc?topic=vpc-backups-vpc-planning) for backing up your block storage volumes.
[Create a backup policy](/docs/vpc?topic=vpc-backup-policy-create).
