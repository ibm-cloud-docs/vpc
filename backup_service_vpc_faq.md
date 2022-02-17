---

copyright:
  years: 2022
lastupdated: "2021-02-10"

keywords: backup service, backups, volumes, block storage, virtual server instance, instance, FAQ

subcollection: vpc

---

{:faq: data-hd-content-type='faq'}
{:codeblock: .codeblock}
{:screen: .screen}
{:support: data-reuse='support'}
{:external: target="_blank" .external}
{:note: .note}
{:beta: .beta}

# FAQs for Backup for VPC (Beta)
{: #backup-service-vpc-faq}

The following questions pertain to the VPC Backup service. If you have other questions you'd like to see answered here, provide feedback by using the **Open Issue** or **Edit Topic** links.
{: shortdesc}

This service is available only to accounts with special approval to preview this beta feature. Contact your IBM Support if you are interested in getting access.
{: beta}

## What is the VPC backup service?
{: faq}
{: #faq-baas-concepts}

The VPC backup service lets you set a scheduled backup of your block storage volumes created from those volumes. When a backup is triggered, it creates a new snapshot of the volume contents. For more information, see [Backup service concepts](/docs/vpc?topic=vpc-backup-service-about#backup-service-concepts).

## How does the backup service work?
{: faq}
{: #faq-baas-function}

You add [user tags](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-about-tags) to your volumes, which are also specified in a backup policy. When the tags match, a backup is triggered based on a plan schedule. The Snapshots for VPC service is used to create the backup. The entire contents of the volume are copied and retained for the specified number of days.

## What resources are backed up?
{: faq}
{: #faq-baas-resources}

All resources with user tags that match the tags in a [backup policy](https://test.cloud.ibm.com/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-policies) are backed up. You can back up block storage boot and data volumes.

## How do enable my volumes to be backed up?
{: faq}
{: #faq-baas-enable}

Enabling your backups is two-part process. First, you [specify user tags](/docs/vpc?topic=vpc-backup-use-policies) on the block storage volumes you're backing up. You then [create a backup policy](/docs/vpc?topic=vpc-backup-policy-create) and specify these tags, which identify the volumes you're backing up. Within a policy, you create a [backup plan](/docs/vpc?topic=vpc-backup-policy-create&interface=ui#backup-plan-ui) to schedule backups of these resources. You can schedule backups be taken every 1 to 30 days.

## Can I backup snapshots of a volume?
{: faq}
{: #faq-baas-snapshots}

Tags for snapshots are inherited from the source volume. When you restore a volume from a snapshot, and the tags applied to the new volume match the tags in a backup policy, the new volume is backed up. But you can't directly backup a snapshot that has tags in a backup policy.

## How long are backups retained?
{: faq}
{: #faq-baas-retention}

For Beta, you can specify that backups be kept from 1 to 30 days (default). Note that the retention period can't be shorter than the backup frequency, or you'll get an error.

## Are there limitations on how many backups I can take?
{: faq}
{: #faq-baas-limitations}

Yes. For Beta, you can create 10 backup policies per account and up to 100 backups of a volume. For other limitations of this release, see [Limitations in the Beta release](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-limitations).

## How do I create a new volume from a backup?

{: faq}
{: #faq-baas-restore}

You create a new volume by _restoring_ a volume from a backup It works the same way as regular snapshots. Restoring a volume from a backup creates a fully provisioned block storage boot or data volume. For more information, see [Restoring a volume from a backup](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-restore-concepts).

### How am I charged for usage?
{: faq}
{: #faq-baas-pricing}

Cost for backups is calculated based on GB capacity stored per month, unless the duration is less than one month. The backup exists on the account until it reaches its retention period, or when you delete it manually, or when you reach the end of a billing cycle, whichever comes first.

Pricing of subsequent backups can also increase or decrease when you [expand source volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) or [adjust IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) by specifying a different IOPS profile for the source volume. For example, expanding volume capacity increases costs while changing an IOPS profile from a 5 IOPS/GB tier to a 3 IOPS/GB tier decreases the monthly and hourly rate. Billing for an updated volume is automatically updated to add the prorated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

Pricing for backus is also set by region of the source volume. For more information, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).
