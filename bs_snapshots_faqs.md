---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-04"

keywords: Block Storage, snapshots, cross-regional copy, fast restore, backup, restore volume

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-vpc-faqs}

The following questions often arise about the {{site.data.keyword.block_storage_is_short}} snapshots offering. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links at the end of the topic.
{: shortdesc}

## What is a snapshot?
{: faq}
{: #faq-snapshot-1}

Snapshots are a point-in-time copy of your {{site.data.keyword.block_storage_is_short}} boot or data volume that you manually create. To create a snapshot, the original volume must be attached to a running virtual server instance. The first snapshot is a full backup of the volume. Subsequent snapshots of the same volume record only the changes since the last snapshot. You can access a snapshot of a boot volume and use it to provision a new boot volume. You can create a data volume from a snapshot (called _restoring_ a volume) and attach it to an instance. Snapshots are persistent; they have a lifecycle that is independent from the original volume.

## What is a backup snapshot?
{: faq}
{: #faq-backup-snapshot}

Backup snapshots, also called _backups_, are scheduled snapshots that are created by using the Backup for VPC service. For more information, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

## What is a bootable snapshot?
{: faq}
{: #faq-snapshot-2}

A bootable snapshot is a copy of a boot volume. You can use this snapshot to create another boot volume when you provision a new instance.

## What is a fast restore snapshot?
{: faq}
{: #faq-snapshot-fr}

A [fast restore snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-use-fast-restore) is a clone of a snapshot that is stored within one or more zones of a VPC region. The original snapshot is stored in the regional storage repository. When you perform a restore, data can be restored faster from a clone than from the snapshot in a regional storage repository.

## What is a cross-regional copy of a snapshot?
{: faq}
{: #faq-snapshot-crc}

You can copy a snapshot from one region to another region, and later use that snapshot to restore a volume in the new region. This feature can be used in disaster recovery scenarios when you need to start your virtual server instance and data volumes in a different region. Or you can use the remote copy to create storage volumes in a new region to expand your VPC. For more information, see [Cross-regional snapshot copies](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots_vpc_crossregion_copy).

[Beta]{: tag-cyan} Customers with special access can create cross-regional copies of the second-generation volume backups if the source volume exceeds 10 TB. This feature is not generally available yet.

## What is the retention policy for cross-regional copies?
{: faq}
{: #faq-snapshot-crc-retention}

How long the copy is kept in another regions depends on how often your backup plan generates backup snapshots and the number of copies that you chose to keep. When you create your backup policy plan with the option to create remote clones, you must specify where you want to create those copies. You must also specify the maximum number of snapshot copies that you want to keep.

For example, if your backup plan takes snapshots daily and you specified 5 remote copies to keep, then at any time the oldest remote copy is less than 5 days old. If you already have 5 remote copies in a region, then the system deletes the oldest one to make room for the new snapshot copy. 

Manually created copies remain in the other region until you delete them.

## How many snapshots can I take?
{: faq}
{: #faq-snapshot-3}

You can take up to 750 snapshots per first-generation volume in a region. Deleting snapshots from this quota makes space for more snapshots. A snapshot of a first-generation volume cannot be greater than 10 TB. Also, consider how your billing is affected when you increase the number of snapshots that you take and retain.

In the current release, you can create up to 512 manual and backup snapshots of second-generation volumes. You can even create snapshots when the `sdp` volumes are unattached.Second-generation volume snapshots can exceed 10 TB.

## Is there a limit on the size of a volume that I can take a snapshot of?
{: faq}
{: #faq-snapshot-4}

You can create snapshots of first-generation volumes only if they are less than 10 TB. Snapshot creation fails if the first-generation volume is over that limit.

Second-generation volumes have no such limits, you can take snapshots up to 32 TB.

## How secure are snapshots?
{: faq}
{: #faq-snapshot-5}

Snapshots are stored and retrieved from the regional storage repository. Data is encrypted while in transit and stored in the same region as the original volume.

Snapshots retain the encryption from the original volume, IBM-managed or customer-managed.

## What happens when I restore a volume from a snapshot?
{: faq}
{: #faq-snapshot-6}

Restoring a volume from a snapshot creates an entirely new boot or data volume. The new volume has the same properties of the original volume, including encryption. If you restore from a bootable snapshot, you create a boot volume. Similarly, you can create a data volume from a snapshot of a data volume. The volume that you create from the snapshot uses the same volume profile and contains the same data and metadata as the original volume. You can restore a volume when you provision an instance, update an existing instance, or create a stand-alone volume by using the UI, CLI, or the `volumes` API. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

For best performance, you can enable snapshots for fast restore. By using the fast restore feature, you can create a volume from a snapshot that is fully provisioned when the volume is created. For more information, see [Snapshots fast restore](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_fast_restore).

## I want to migrate my first-generation volume to the new second-generation profile. Can I use its snapshot to create an `sdp` volume?
{: faq}
{: #faq-cross-gen-compatibility}

No. First- and second-generation volume profiles are not interchangeable. You use a snapshot of a first-generation volume to create another first-generation volume with the same data, and switch to another first-generation profile from the `tiered` or `custom` family. However, you can't switch to the `sdp` profile.

## Is it normal for a volume that I restored from a snapshot to not perform at the expected level?
{: faq}
{: #faq-snapshot-performance}

Performance of boot and data volumes is initially degraded when data is restored from a snapshot. Performance degradation occurs during the restoration because your data is copied from the regional storage repository to {{site.data.keyword.block_storage_is_short}} in the background. After the restoration process is complete, you can realize full IOPS on the new volume.

Volumes that are restored from fast restore clones do not require hydration. The data is available as soon as the volume is created.

## What happens to snapshots when I delete my volume?
{: faq}
{: #faq-snapshot-7}

Deleting a volume from which you created a snapshot has no effect on the snapshot. Snapshots exist independently of the original source volume and have their own lifecycle. To delete a volume, all snapshots must be in a `stable` state.

## Can I set up a snapshot schedule?
{: faq}
{: #faq-snapshot-8}

Yes, you can use Backup for VPC to create a backup policy and plan. In the plan, you can schedule daily, weekly, or monthly backup snapshots, or more frequent backups with a `cron-spec` expression. For more information about scheduling backup snapshots and how it works, see the [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about) overview.

## Can I detach or delete a volume after I take a snapshot?
{: faq}
{: #faq-snapshot-9}

Snapshots have their own lifecycle, independent of the {{site.data.keyword.block_storage_is_short}} volume. You can separately manage the source volume. However, when you take a snapshot, you must wait for the snapshot creation process to complete before you detach or delete the volume.

## How am I charged for usage?
{: faq}
{: #faq-snapshot-pricing}

The cost for snapshots is calculated based on GB capacity that is stored per month, unless the duration is less than one month. Because the snapshot space is based on the capacity that was provisioned for the original volume, the snapshot capacity does not vary. Deleting snapshots reduces cost, so keep fewer snapshots to keep the cost down. For more information about billing and usage, see [How you're charged](/docs/account?topic=account-charges).

Creating consistency group snapshots does not incur extra charges other than the cost associated with the size of the member snapshots.

Pricing for snapshots is also set by region. When you use the [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_fast_restore) feature, your existing regional plan is adjusted. Billing for fast restore is based on instance hours. So the fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots.

## Can I add tags to a {{site.data.keyword.block_storage_is_short}} snapshot?
{: faq}
{: #faq-snapshot-tags}

Depending on the action that you're performing, you can add user tags and access management tags to your snapshots. User tags are used by the backup service to periodically create backup snapshots of the volume. Access management tags help organize access to your {{site.data.keyword.block_storage_is_short}} snapshots. For more information, see [Tags for {{site.data.keyword.block_storage_is_short}} snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots-about-tags).

## Can I use volume snapshot for disaster recovery?
{: faq}
{: #faq-snapshot-dr}

You can use your snapshots and backups to create volumes when an emergency occurs. You can also create copies of your snapshot in other regions and use them to create volumes there. However, the snapshot and backup services do not provide continual backup with automatic failover. Restoring a volume from a backup or snapshot is a manual operation that takes time. If you require a higher level of service for automatic disaster recovery, see IBM's [Cloud disaster recovery solutions](https://www.ibm.com/solutions/disaster-recovery).

## How many copies of my snapshot can I create in other regions?
{: faq}
{: #faq-snapshot-cross-regional-limits}

You can copy a snapshot from one region to another region, and later use that snapshot to restore a volume in the new region. Only one copy of the snapshot can exist in each region. You can't create a copy of the snapshot in the source (local) region.

In this release of second-generation storage volumes, cross-regional copies of snapshots of volumes that exceed 10 TB are supported only for allow-listed [beta]{: tag-cyan} customers.

## What is a consistency group?
{: faq}
{: #faq-snapshot-consistency-group-snap}

A consistency group is a collection of snapshots that are managed as a single unit. It is used to create snapshots of multiple volumes that are attached to the same virtual server instance at the same time to preserve data consistency. 

The snapshots are loosely coupled. The snapshots can be used to create new volumes. They can be copied to another region individually, and can be preserved after the consistency group is deleted. However, you can't copy a consistency group to another region or use the ID of the consistency group to create a virtual server instance.

For more information, see [Snapshot consistency groups](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#multi-volume-snapshots).

In this release, consistency group snapshots of second-generation storage volumes are not supported.
