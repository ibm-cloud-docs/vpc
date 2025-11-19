---

copyright:
  years: 2024, 2025
lastupdated: "2025-11-19"

keywords: File Storage, snapshots, cross-regional copy, backup, restore share

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for {{site.data.keyword.filestorage_vpc_short}} snapshots
{: #fs-snapshots-faqs}

The following questions often arise about the {{site.data.keyword.filestorage_vpc_short}} snapshots offering. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links at the end of the topic.
{: shortdesc}

## What is a snapshot?
{: faq}
{: #faq-fs-snapshot-1}

Snapshots are a point-in-time copy of your {{site.data.keyword.filestorage_vpc_short}} share that you manually create. The first snapshot is a full backup of the share. Subsequent snapshots of the same share record only the changes since the last snapshot. You can access a snapshot of a share and use it to provision a new share. Snapshots' lifecycle is linked to their parent share. Snapshots are not independent resources.

## What is a backup snapshot?
{: faq}
{: #faq-fs-backup-snapshot}

Backup snapshots, also called _backups_, are scheduled snapshots that are created by using the Backup for VPC service. You can schedule backups for file shares by creating a backup policy with a plan. Then, add the user tags that are specified in the policy to your shares so the service knows which shares to create a snapshot from. For more information, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

Backup schedules can be configured only on the source side of a replication pair. When you choose to failover operations to the replica share, the source and replica shares switch roles. After a failover is performed, backup policies need to be removed from what was previously the source and applied to the current source share. Make sure that you update the tags on the source and replica shares.

## How many snapshots can I take?
{: faq}
{: #faq-fs-snapshot-3}

For zonal file shares, you can take up to 750 snapshots per share in a zone. In the current release of regional file shares, you can take up to 30 snapshots per share in a region. The number of snapshots can be increased upon request/ Deleting snapshots from this quota makes space for more snapshots. A snapshot of a share cannot be greater than 10 TB for zonal file shares. Also, consider how your billing is affected when you increase the number of snapshots that you take and retain.

## Is there a limit on the size of a share that I can take a snapshot of?
{: faq}
{: #faq-fs-snapshot-4}

The maximum size of a zonal file share is 10 TB. Snapshot creation fails if the share is over that limit.

[Select availability]{: tag-green} In this release of regional file shares, the maximum supported share size is 32 TB, and snapshots can be taken of the full share size with no limitations. 

## How secure are snapshots?
{: faq}
{: #faq-fs-snapshot-5}

Snapshots are stored alongside the file shares. Snapshots retain the encryption from the original share, IBM-managed or customer-managed, and share the encryption with the source share.

Snapshots can be copied to another zone only by the replication process, which is always encrypted in transit.

## What happens when I restore a share from a snapshot?
{: faq}
{: #faq-fs-snapshot-6}

Restoring a share from a snapshot creates another share. The share that you create from the snapshot uses the same share profile and contains the same data and metadata as the original share. For more information, see [Restoring a share from a snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore).

## What happens to snapshots when I delete my share?
{: faq}
{: #faq-fs-snapshot-7}

Deleting a share from which you created a snapshot deletes the snapshot, too. Snapshots coexist with their source share.

## Can I set up a snapshot schedule?
{: faq}
{: #faq-fs-snapshot-8}

Yes, you can use Backup for VPC to create a backup policy and plan. In the plan, you can schedule daily, weekly, or monthly backup snapshots, or more frequent backups with a `cron-spec` expression. For more information about scheduling backup snapshots and how it works, see the [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about) overview.

## How am I charged for usage?
{: faq}
{: #faq-fs-snapshot-pricing}

The cost for snapshots is calculated based on GB capacity that is used during the billing cycle. Shares are configured for the maximum Snapshot size, a maximum of 99 times the size of the share or 100 TB. Because the snapshot space is based on the capacity that was provisioned for the original share, the snapshot capacity does not vary. However, you pay only for the capacity that is used by the snapshots. Deleting snapshots can reduce cost. For more information about billing and usage, see [How you're charged](/docs/account?topic=account-charges).

## What happens when I delete a snapshot?
{: faq}
{: #faq-fs-snapshot-9}

When a snapshot is deleted, only the data blocks that are no longer needed by another snapshot are freed on the storage. The size of individual snapshots is dynamic, dependent on the existence of past or future snapshots, and the current state of the file share. Due to its dynamic nature, the actual amount of space that can be reclaimed by deleting snapshots cannot be easily determined. The change in the amount of space that is used is reflected in the metrics within 15 minutes after the snapshot is deleted.

[Select availability]{: tag-green} In this release of regional file shares, the change in used space is not reflected in the metrics within the same timeframe.

## Can I use share snapshots for disaster recovery?
{: faq}
{: #faq-fs-snapshot-dr}

To help ensure that snapshots are able to survive the loss of an availability zone, configure replication for the file share. When a new replica share is created, all snapshots present on the source volume are transferred to the replica. When replication proceeds normally, any snapshots that are taken on the source are copied to the replica, and snapshots that are deleted from the source are also removed from the replica.

[Select availability]{: tag-green} In this release of regional file shares, cross-region replication is not supported.

## What are the different kinds of snapshots in my `/.snapshot` directory?
{: faq}
{: #faq-fs-customer-service-snapshots}

All the snapshots that are present in the share are visible as subdirectories inside a hidden `/.snapshot` or `/.snap` directory, and the snapshot directories are named the same as the snapshot fingerprint ID that you see in the console, from the CLI, or with the API. These snapshots are the snapshots that you took manually or that were created automatically by the backup service.

You can also see special "replication" snapshots that are named by using the word "replication" and the associated creation timestamp rather than the fingerprint of the snapshot. These snapshots are created by the system and are used to mirror data to the replica share. The replication snapshots are automatically released and deleted when they are no longer needed.

Snapshots of regional file shares are stored in the `.snap` directory. In this release of regional file shares, you cannot create cross-regional replicas. So no special replication snapshots appear in that directory.
{: note}

## Can I automate the creation of snapshots?
{: faq}
{: #faq-fs-backup-snapshots}

You can use the Backup for VPC service to schedule the creation and deletion of your snapshots. For more information, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

## Can the users of an accessor share see the snapshots of the origin share?
{: faq}
{: #faq-fs-snapshots-accessorshares}

Yes. Accessor shares have access to all the data within the origin share and that includes the snapshots of the origin share, too.
