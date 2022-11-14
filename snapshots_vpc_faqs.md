---

copyright:
  years: 2022
lastupdated: "2022-11-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for snapshots
{: #snapshots-vpc-faqs}

The following questions often arise about the Snapshot for VPC offering. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links at the end of the topic.
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

## How many snapshots can I take?
{: faq}
{: #faq-snapshot-3}

You can take up to 750 snapshots per volume in a region. Deleting snapshots from this quota frees up space for more snapshots. A snapshot of a volume cannot be greater than 10 TB. Also, consider how your [billing changes](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=api#snapshots_vpc_considerations) when you increase the number of snapshots that you take and retain.

## Is there a limit on the size of a volume that I can snapshot?
{: faq}
{: #faq-snapshot-4}

The maximum size of a volume is 10 TB. Snapshot creation fails if the volume is over that limit.

## How secure are snapshots?
{: faq}
{: #faq-snapshot-5}

Snapshots are stored and retrieved from {{site.data.keyword.cos_full_notm}}. Data is encrypted while in transit and stored in the same region as the original volume.

Snapshots retain the encryption from the original volume, IBM-managed or customer-managed.

## What happens when I restore a volume from a snapshot?
{: faq}
{: #faq-snapshot-6}

Restoring a volume from a snapshot creates an entirely new boot or data volume. The new volume has the same properties of the original volume, including encryption. If you restore from a bootable snapshot, you create a boot volume. Similarly, you can create a data volume from a snapshot of a data volume. The volume that you create from the snapshot uses the same volume profile and contains the same data and metadata as the original volume. You can restore a volume when you provision an instance, update an existing instance, or create a stand-alone volume by using the UI, CLI, or the `volumes` API. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

## What happens to snapshots when I delete my volume?
{: faq}
{: #faq-snapshot-7}

Deleting a volume from which you created a snapshot has no effect on the snapshot. Snapshots exist independently of the original source volume and have their own lifecycle. To delete a volume, all snapshots must be in a `stable` state.

## Can I set up a snapshot schedule?
{: faq}
{: #faq-snapshot-8}

Yes, you can use Backup for VPC to create a backup policy and plan. In the plan you can schedule daily, weekly, or monthly backup snapshots, or more frequent backups with a `cron-spec` expression. For more information about scheduling backup snapshots and how it works, see the [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about) overview.

## Can I detach or delete a volume after I take a snapshot?
{: faq}
{: #faq-snapshot-9}

Snapshots have their own lifecycle, independent of the {{site.data.keyword.block_storage_is_short}} volume. You can separately manage the source volume. However, when taking a snapshot, you must wait for the snapshot creation process to complete before you detach or delete the volume.

## How am I charged for usage?
{: faq}
{: #faq-snapshot-pricing}

Cost for snapshots is calculated based on GB capacity stored per month, unless the duration is less than one month. Because the snapshot is based on the capacity that was provisioned for the original volume, the snapshot capacity does not vary. Deleting snapshots reduces cost, so the fewer snapshots you retain the lower the cost.

Pricing for snapshots is also set by region. For more information, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).

### Can I add tags to a {{site.data.keyword.block_storage_is_short}}  snapshot?
{: faq}
{: #faq-snapshot-tags}

Depending on the action that you're performing, you can add user tags and access management tags to your snapshots. User tags are used by the backup service to periodically create backup snapshots of the volume. Access management tags help organize access to your {{site.data.keyword.block_storage_is_short}} snapshots. For more information, see [Tags for {{site.data.keyword.block_storage_is_short}} snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots-about-tags.)
