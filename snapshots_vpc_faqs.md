---

copyright:
  years: 2021
lastupdated: "2021-07-09"

keywords: snapshots, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance, faqs
subcollection: vpc

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:faq: data-hd-content-type='faq'}

# FAQs for snapshots
{: #snapshots-vpc-faqs}

The following questions often arise about the Snapshot for VPC offering. If you have other questions you'd like to see answered here, provide feedback by using the **Open Issue** or **Edit Topic** links.
{:shortdesc}

## What is a snapshot?
{: faq}
{: #faq-snapshot-1}
Snapshots are a point-in-time copy of your block storage boot or data volume. To create a snapshot, the original volume must be attached to a running virtual server instance. The first snapshot is a full backup of the volume. Subsequent snapshots of the same volume record only the changes since the last snapshot. You can use a snapshot of a boot volume to provision an instance. If the snapshot was of a data volume, you can attach it to a virtual server instance. You can create a new volume from a snapshot (called _restoring_ a volume). Snapshots are persistent; they have a lifecycle independent from the original volume.

## What is a bootable snapshot?
{: faq}
{: #faq-snapshot-2}

A bootable snapshot is a copy of a boot volume. You can use this new boot volume when provisioning new instances.

## How many snapshots can I take?
{: faq}
{: #faq-snapshot-3}

You can take up to 100 snapshots per volume in a region.  Deleting snapshots from this quota frees up space for additional snapshots.

## Is there a limit on the size of a volume that I can snapshot?
{: faq}
{: #faq-snapshot-4}

The maximum size of a single snapshot is 10 TB, and the cumulative size of all snapshots for a given volume cannot exceed 10 TB. When the number of changed blocks in the volume exceeds the 10 TB snapshot limit, snapshot creation fails.

## How secure are snapshots?
{: faq}
{: #faq-snapshot-5}

Snapshots are stored and retrieved from IBM Cloud Object Storage (COS). Data is encrypted while in transit and stored in the same region as the original volume. 

Snapshots retain the encryption from the original volume, IBM-managed or customer-managed.

## What happens when I restore a volume from a snapshot?
{: faq}
{: #faq-snapshot-6}

Restoring a volume from a snapshot creates an entirely new boot or data volume. The new volume has the same properties of the original volume, including encryption. If you restore from a bootable snapshot, you create a boot volume. Similarly, you can create a new data volume from a snapshot of a data volume. The volume you create from the snapshot uses the same volume profile and contains the same data and meta-data as the original volume. You restore a volume when you provision a new instance. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

## What happens to snapshots when I delete my volume?
{: faq}
{: #faq-snapshot-7}

Deleting a volume from which you created a snapshot has no effect on the snapshot. Snapshots exist independently of the original source volume and have their own lifecycle. To delete a volume, all snapshots must be in a `stable` state.

## Can I set up a snapshot schedule?
{: faq}
{: #faq-snapshot-8}

In the initial offering of Snapshots for VPC, you can only take a manual snapshot. If you want to set up a snapshot schedule, you have to take independent action, such as setting up a Linux cron job similar to taking backups. However, this is not supported by IBM.

## Can I detach or delete a volume after I take a snapshot?
{: faq}
{: #faq-snapshot-9}

Snapshots have their own lifecycle, independent of the block storage volume. You can separately manage the source volume. However, when taking a snapshot, you must wait for the snapshot creation process to complete before detaching or deleting the volume.
