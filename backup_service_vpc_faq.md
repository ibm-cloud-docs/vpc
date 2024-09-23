---

copyright:
  years: 2022, 2024
lastupdated: "2024-09-23"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data, faqs

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for Backup for VPC
{: #backup-service-vpc-faq}

The following questions pertain to the VPC Backup service. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links after the FAQs.
{: shortdesc}

## What is the VPC backup service?
{: faq}
{: #faq-baas-concepts}

With the VPC backup service, you can create backup policies for your {{site.data.keyword.block_storage_is_short}} volumes. A backup policy contains a backup plan, where you set a scheduled backup of your data. You can create up to four backup plans per policy. When a backup is triggered, it creates a snapshot of the volume contents. You can also set a retention period for your backups so that the oldest ones are deleted either by date or total count. For more information, see [Backup service concepts](/docs/vpc?topic=vpc-backup-service-about#backup-service-concepts).

## How do I set up the backup service?
{: faq}
{: #faq-baas-setup}

Before you can create backup policies, you need to grant [service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth&interface=api), and specify user roles for the backup service. Then, you add user tags for new or existing resources (individual Block Storage volumes, or virtual server instances) that you associate with a backup policy. Finally, you create backup policies and plans to schedule automatic backups. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=ui).

## How does the backup service work?
{: faq}
{: #faq-baas-function}

You can add [user tags](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-about-tags) to your volumes, or virtual server instances, and specify the same tags in a backup policy. When the tags match, a backup is triggered based on the backup plan schedule. You can [view backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs) to see the progress of the operation. The Snapshot for VPC service is used to create the backup. The entire contents of the volume are copied and retained for the number of days or total number of backups that are specified in the backup plan. When the retention period is reached, the older backups are deleted. 

## What resources are backed up?
{: faq}
{: #faq-baas-resources}

{{site.data.keyword.block_storage_is_short}} data and boot volumes with user tags that match the tags in a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-policies) are backed up. You can also tag virtual server instances, in that case, the attached Block Storage volumes are backed up as a consistency group.

## How do I enable my volumes to be backed up?
{: faq}
{: #faq-baas-enable}

Enabling your backups is a two-part process. First, you [specify user tags](/docs/vpc?topic=vpc-backup-use-policies) on the resources (Block Storage volumes, or virtual server instances) that you want to back up. You then [create a backup policy](/docs/vpc?topic=vpc-create-backup-policy-and-plan) and specify these tags, which identify the resources that you're backing up. Within a policy, you create a [backup plan](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=ui#backup-plan-ui) to schedule backups of these resources. You can schedule backups to be taken every daily, weekly, or monthly.

## How many backups can I create?
{: faq}
{: #faq-baas-total}

You can create up to 750 backups per volume. Consider how your billing changes when you increase the number of snapshots that you take and retain.

## What are backup policy jobs?
{: faq}
{: #faq-backup-jobs}

Backup policy jobs, or backup jobs for short, are triggered when a scheduled backup snapshot is being created or deleted. If the create or delete action is successful, the backup job contains information about the backup snapshot that was created or deleted. If the job ran unsuccessfully, the job contains the reason for the failure. For more information, see [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## Can I back up my volume snapshots?
{: faq}
{: #faq-baas-snapshots}

Tags for snapshots are inherited from the source volume. When you restore a volume from a snapshot, and the tags that are applied to the new volume match the tags in a backup policy, the new volume is backed up. But you can't directly back up a snapshot that has tags in a backup policy.

## How long are backups retained?
{: faq}
{: #faq-baas-retention}

You can specify that backups be kept 1 - 30 days (default). The retention period can't be shorter than the backup frequency or it returns an error.

You can also specify the number of backups to retain, up to 750 per volume, after which the oldest backups are deleted.

## Are there limitations on how many backups I can take?
{: faq}
{: #faq-baas-limitations}

Yes. You can create 10 backup policies per account and up to 750 backups of a volume. For other limitations of this release, see [Limitations in this release](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-limitations).

## How do I create a volume from a backup?
{: faq}
{: #faq-baas-restore}

Restoring data from a backup snapshot creates a volume with data from the snapshot. You can restore data from a backup by using the UI, the CLI, or the API. You can restore boot and data volumes during instance creation, when you modify an existing instance, or when you provision a stand-alone volume. When you restore data from a backup snapshot, the data is pulled from an {{site.data.keyword.cos_short}} bucket. For best performance, you can enable backup snapshots for fast restore. By using the fast restore feature, you can restore a volume that is fully provisioned when the volume is created. When you use fast restore, the data is pulled from a cached backup snapshot in another zone of your VPC. For more information, see [About restoring from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).



## Am I charged for usage?
{: faq}
{: #faq-baas-pricing}

Yes. The cost for backups is calculated based on GB capacity that is stored per month, unless the duration is less than one month. The backup exists on the account until it reaches its retention period, or when you delete it manually, or when you reach the end of a billing cycle, whichever comes first. Creating consistency group backups does not incur extra charges other than the cost associated with the size of the member snapshots.

Pricing of subsequent backups can also increase or decrease when you [increase source volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) or [adjust IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) by specifying a different volume profile for the source volume. For example, expanding volume capacity increases costs. However, changing a volume profile from a 5-IOPS/GB tier to a 3-IOPS/GB tier decreases the monthly and hourly rate. Billing for an updated volume is automatically updated to add the prorated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

The fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots.

You can use the Cost estimator ![Cost estimator icon](../icons/calculator.svg "Cost estimator") in {{site.data.keyword.cloud_notm}} console to see how changes in the stored volume affect the cost. For more information, see [Estimating your costs](/docs/billing-usage?topic=billing-usage-cost).     

## Can I use data backups for disaster recovery?
{: faq}
{: #faq-baas-dr}

Using the [backup service](/docs/vpc?topic=vpc-backup-service-about), you can regularly back up your data based on a schedule that you set up. You can create backup snapshots as frequently as 1 hour. You can also create copies of your volume backup snapshot in other regions. However, the backup service does not provide continual backup with automatic failover. Restoring a volume from a backup or snapshot is a manual operation that takes time. If you require a higher level of service for automatic disaster recovery, see IBM's [Cloud disaster recovery solutions](https://www.ibm.com/cloud/disaster-recovery).



## How many copies of my backup can I create in other regions?
{: faq}
{: #faq-baas-cross-regional-limits}

You can copy a backup snapshot of a Block storage volume from one region to another region, and later use that snapshot to restore a volume in the new region. Only one copy of the backup snapshot can exist in each region. You can't create a copy of the backup snapshot in the source (local) region.



## What is a consistency group for backups?
{: faq}
{: #faq-snapshot-consistency-group-backups}

A consistency group is a collection of backup snapshots that are created together at the same time. It is used to create backup snapshots of multiple volumes that are attached to the same virtual server instance simultaneously to preserve data consistency. 

The created snapshots are loosely coupled. The snapshots can be used to create new volumes. They can be copied to another region individually, and can be preserved after the consistency group is deleted. However, you can't copy a consistency group to another region or use the ID of the consistency group to create a virtual server instance.

## Why was no backup job created when my consistency group was deleted?
{: faq}
{: #faq-snapshot-consistency-group-delete}

If you modify the value of the backup consistency group's `delete_snapshots_on_delete` parameter to be `false`, the backup snapshots remain in the system as individual snapshots after the consistency group is deleted. Because the snapshots are kept unchanged, no backup job is created.

## Can I restore a virtual server from consistency group backups?
{: faq}
{: #faq-baas-cg-restore}

Restoring a virtual server instance directly from snapshot consistency group identifier is not supported. However, you can restore a virtual server instance by restoring all of its boot and data volumes from the snapshots that are part of a consistency group. Virtual server instance configuration is not part of the backup, and you must manually or programmatically configure the instance in the console, from the CLI, with the API, or Terraform. For more information, see [Creating volumes for a virtual server instance from a consistency group](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-restore-cr-details-ui).

## Can I enable event notifications for Backup for VPC?
{: faq}
{: #faq-baas-en}

Yes. If you have an {{site.data.keyword.en_full}} instance, you can connect your backup policy to it in the console or with the API. For more information, see [Enabling event notifications for Backup for VPC](/docs/vpc?topic=vpc-event-notifications-events).

For more information about working with {{site.data.keyword.en_short}}, see [Getting started with {{site.data.keyword.en_short}}](/docs/event-notifications?topic=event-notifications-getting-started).

## What service authorizations are needed for event notifications?
{: faq}
{: #faq-baas-en-s2s}

You need to create a service-to-service authorization between the Backup for VPC service (source service) and {{site.data.keyword.en_short}} (target service). Set the service access level to `Event Source Manager`. For more information, see [Enabling service-to-service authorization for Event Notifications](/docs/vpc?topic=vpc-backup-s2s-auth&interface=ui#backup-s2s-auth-procedure-en-ui){: ui}[Enabling service-to-service authorization for Event Notifications](/docs/vpc?topic=vpc-backup-s2s-auth&interface=cli#backup-s2s-auth-procedure-en-cli){: cli}[Enabling service-to-service authorization for Event Notifications](/docs/vpc?topic=vpc-backup-s2s-auth&interface=api#backup-s2s-auth-procedure-en-api){: api}[Enabling service-to-service authorization for Event Notifications](/docs/vpc?topic=vpc-backup-s2s-auth&interface=terraform#backup-s2s-auth-procedure-en-terraform){: terraform}.

## Can I receive notifications if backup jobs fail in a different region?
{: faq}
{: #faq-baas-en-location}

{{site.data.keyword.en_short}} are supported in Dallas (`us-south`), London (`eu-gb`), Frankfurt (`eu-de`), Sydney (`au-syd`), and Madrid (`eu-es`). For more information, see [Getting started with {{site.data.keyword.en_short}}](/docs/event-notifications?topic=event-notifications-getting-started). 

For locations that don't currently support {{site.data.keyword.en_short}}, the notifications can be routed to another region. For more details, see the following table.

| VPC Backup Region     | Receiving {{site.data.keyword.en_short}} region |
|-----------------------|-------------------------------------------------|
| Dallas (`us-south`)   | Dallas (`us-south`)  |
| Washington (`us-east`)| Dallas (`us-south`)  |
| Toronto (`ca-tor`)    | Dallas (`us-south`)  |
| Sao Paulo (`br-sao`)  | Dallas (`us-south`)  |
| Frankfurt (`eu-de`)   | Frankfurt (`eu-de`)  |
| Madrid (`eu-es`)      | Madrid (`eu-es`)     |
| London (`eu-gb`)      | London (`eu-gb`)     |
| Osaka (`jp-osa`)      | Sydney (`au-syd`)    |
| Tokyo (`jp-tok`)      | Sydney (`au-syd`)    |
| Sydney (`au-syd`)     | Sydney (`au-syd`)    |
 {: caption="Table 1. Backup regions and {{site.data.keyword.en_short}} destinations" caption-side="bottom"}

## Why am I receiving some notifications but not all?
{: faq}
{: #faq-baas-en-limit}

As a customer, you can register your backup service to your {{site.data.keyword.en_short}} instance. In this case, all backup job failure notifications from all policies are forwarded to that {{site.data.keyword.en_short}} instance. You can use multiple topics to filter and route the notifications. You can create as many topics as you want. However, if more than 20 topics are registered to a backup event source, then only the first 20 can receive backup service events per account.
