---

copyright:
  years: 2021
lastupdated: "2021-05-12"

keywords: snapshots, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance
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

# About Snapshots for VPC
{: #snapshots-vpc-about}

Snapshots for VPC is a regional offering that lets you create a point-in-time copy of your block storage boot or data volume. The initial snapshot you take is a full backup of the volume. Subsequent snapshots of the same volume are incremental; only the changes since the last snapshot are captured. You can select a snapshot during instance provisioning, and restore a new, fully-provisioned boot volume to start the instance. You can also create and attach a data volume from a snapshot within a running virtual server instance.
{:shortdesc}

## Snapshots concepts
{: #snapshots-vpc-concepts}

A snapshot is a copy of your volume that you take manually by using the UI or CLI, or create programatically from the API. You can snaphot a boot or data volume that is attached to a running virtual server instance. A "bootable" snapshot is a snapshot of a boot volume. You can also take a snapshot of a data volume. Both actions require the volumes to be attached to a running server instance.

The first time you take a snapshot of a volume, all the volume's contents are copied. The snapshot has the same encryption as the volume (customer-managed or provider-managed). Snapshots are stored and retrieved from IBM Cloud Object Storage (COS). Data is encrypted while in transit and stored in the same region as the original volume.

When you take a second snapshot, only the changes to the volume since the last snapshot are recorded. As such, the size of snapshots you take can grow or shrink, depending on what is being uploaded to Cloud Object Storage (COS). The chain of snapshots increases with each successive snapshot you take, up to a predefined limit.

You can take up to 100 snapshots per volume in your region. Deleting a snapshot does not increase this limit; after you take 100 snapshots per volume, you can't take any more. The cumulative size of all snapshots for a given volume can not exceed 10 TB.
{:note}

You can create a new virtual server instance with a boot volume initialized from a snapshot. This process is called [restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore). The instance profile of the new instance doesn't have to match the original profile used to create the snapshot. 

You can create a new virtual server instance with a boot volume initialized from a snapshot. The instance profile of the new instance is not required to match the instance used to create the snapshot. 

Snapshots have their own lifecycle, independent of the block storage volume. You can delete the original volume and the snapshot persists. Do not detach the volume from the instance during or after snapshot creation. You won't be able to reattach the volume to the instance. Snapshots are also crash-consistent; if the virtual server stops for any reason, the snapshot data are safe on the disk.

Snapshots are billed hourly, per GB of data based on capacity. Since the snapshot is based on the capacity provisioned for the original volume, the snapshot capacity won't vary.

With IBM Cloud IAM, you can set up resource groups in your account to provide user-access to your snapshots. Your IAM role determines whether you can create and manage snapshots. For more information, see [IAM roles for creating and managing snaphots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-iam).

Before you take a snapshot, make sure all cached data is present on disk. This applies to instances with Windows and Linux operating systems. For example, on Linux operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{:note}

## How snapshots work
{: #snapshots-vpc-operation}

When you take a snapshot, read and write operations from your virtual server instance to the volume continue uninterrupted. After volume data is retrieved, the snapshot enters a `pending` state until it's created. When the snapshot is successfully created and in a `stable` state, you can resume volume management activities, such as deleting, resizing, or detaching a volume. You can also take additional snapshots.

Volume data retrieved for the requested snapshot is encrypted while in transit from the hypervisor to Cloud Object Storage (COS).

The initial snapshot is the entire copy of your block storage volume. Subsequent snapshots copy only what's changed since the last snapshot.

You restore a boot or data volume from a running virtual server instance using the UI, CLI, or API. Restoring a volume from a snapshot creates a new, fully-provisioned volume. Restoring from a snapshot of a boot volume creates a new boot volume that you can use when you provision a new instance. Restoring from a snapshot of a data volume creates a secondary volume attached to the instance.

## Limitations
{: #snapshots-vpc-limitations}

The following limitations apply to this release:

* You can take 100 snapshots per volume.
* Snapshots of a detached volume are not supported.
* Snapshots of volumes greater than 10 TB are not supported.
* You can delete only latest snapshot or all snapshots of a volume.

In addition, to delete a block storage volume, all snapshots must be in a `stable` state. 

## General procedure for creating and using snapshots
{: #snapshots-vpc-procedure-overview}

To create a snapshot:

1. Identify the boot or data volume you want to snapshot.
1. Decide which interface to use, the UI, CLI, or API.
   * To use the UI, log into the [{{site.data.keyword.cloud_notm}} console](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create-ui).
   * To use the [CLI](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create-cli), download and install the following CLI plug-ins. See the [CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference) for more information.
      - {{site.data.keyword.cloud_notm}} CLI
      - The infrastructure-service plug-in
   * To use the [API](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create-api), set up the [VPC API](https://{DomainName}/apidocs/vpc).
1. [Create](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) your snapshots.
1. [View](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view) and [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-manage) your snapshots.
1. [Restore](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-vpc-restore) a volume from a snapshot.

## Next steps
{: #snapshots-vpc-about-next-steps}

Start [creating snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) of your volumes.
