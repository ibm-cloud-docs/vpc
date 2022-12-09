---

copyright:
  years: 2022
lastupdated: "2022-12-09"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About Block Storage Snapshots for VPC
{: #snapshots-vpc-about}

Block Storage Snapshots for VPC are a regional offering that is used create a point-in-time copy of your {{site.data.keyword.block_storage_is_short}} boot or data volume. The initial snapshot that you take is a full backup of the volume. Subsequent snapshots of the same volume are incremental; only the changes since the last snapshots are captured. You can restore a new volume during instance provisioning, from an existing instance, and when you create an unattached volume.
{: shortdesc}

## Snapshots concepts
{: #snapshots-vpc-concepts}

A snapshot is a copy of your volume that you take manually in the UI or from the CLI, or create programmatically with the API. You can take a snapshot of a boot or data volume that is attached to a running virtual server instance or from an unattached data volume. A "bootable" snapshot is a snapshot of a boot volume. A "nonbootable" snapshot is of a data volume. There is no minimal time interval for taking snapshots. However, you cannot take a snapshot of a volume that's in a degraded state.

Do you want to automatically create snapshots of your {{site.data.keyword.block_storage_is_short}} volumes? With Backup for VPC, you can create backup policies to schedule regular volume backups. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).
{: tip}

The first time that you take a snapshot of a volume, all the volume's contents are copied. The snapshot has the same encryption as the volume (customer-managed or provider-managed). Snapshots are stored and retrieved from {{site.data.keyword.cos_full}}. Data is encrypted while in transit and stored in the same region as the original volume.

When you take a second snapshot, only the change to the volume since the last snapshot is captured. As such, the size of snapshots that you take can grow or shrink, depending on what is being uploaded to {{site.data.keyword.cos_full}}. The number of snapshots increases with each successive snapshot you take. You can take up to 750 snapshots per volume in your region. This limit allows you to take and keep an hourly snapshot for 30 days, plus some extra snapshots. Deleting snapshots from this quota frees up space for more snapshots. A snapshot of a volume cannot be greater than 10 TB.

You can create a virtual server instance with a boot volume that was initialized from a snapshot. The instance profile of the new instance is not required to match the instance that was used to create the snapshot. You can also import a snapshot of a data volume when you create and attach a new data volume to the instance. You can also specify user tags for these snapshots.

You can create a volume from a snapshot at any time. This process, called restoring a volume, can be performed when you create an instance, modify an instance, or when you create a stand-alone volume (no instance provisioning is required). For more information, see [Restoring a volume from a snapshot](#snapshots_vpc_restore_overview).

Snapshots have a lifecycle that is independent from the source {{site.data.keyword.block_storage_is_short}} volume. You can delete the original volume and the snapshot persists. However, do not detach the volume from the instance during or after snapshot creation; you won't be able to reattach the volume. Snapshots are crash-consistent; if the virtual server stops for any reason, the snapshot data is safe on the disk.

Cost for snapshots is calculated based on GB capacity stored per month, unless the duration is less than one month. Because the snapshot is based on the capacity that was provisioned for the original volume, the snapshot capacity does not vary.

With {{site.data.keyword.iamlong}}, you can set up resource groups in your account to provide user-access to your snapshots. Your IAM role determines whether you can create and manage snapshots. For more information, see [IAM roles for creating and managing snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-iam).

Before you take a snapshot, make sure that all cached data is present on disk - which applies to instances with Windows and Linux&reg; operating systems. For example, on Linux&reg; operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{: note}

## How snapshots work
{: #snapshots-vpc-operation}

When you take a snapshot, read and write operations from your virtual server instance to the volume continue uninterrupted. After volume data is retrieved, the snapshot enters a `pending` state until the snapshot is created. When the snapshot is successfully created and in a `stable` state, you can resume volume management activities, such as deleting, resizing, or detaching a volume. You can also take more snapshots.

Volume data that is retrieved for the requested snapshot is encrypted while in transit from the hypervisor to {{site.data.keyword.cos_full}}. The initial snapshot is the entire copy of your {{site.data.keyword.block_storage_is_short}} volume. Subsequent snapshots copy only what was changed since the last snapshot.

You restore a boot or data volume from a running virtual server instance in the UI, CLI, or API. Restoring a volume from a snapshot creates a new, fully provisioned volume. Restoring from a snapshot of a boot volume creates a new boot volume that you can use to provision a new instance. Restoring from a snapshot of a data volume creates a secondary volume that is attached to the instance.

You can also restore a [stand-alone (unattached) data volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#restoring-from-an-unattached-snapshot) by using the UI, CLI, or the `volumes` API and specifying the snapshot ID. The volume from which the snapshot was created does not have to be attached to an instance.

## Restrictions
{: #snapshots-vpc-limitations}

The following restrictions apply to this release:

* You can take up to [750 snapshots](#snapshots_vpc_considerations) per volume in a region.
* You cannot take a snapshot of a volume in a [degraded state](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#block-storage-vpc-health-states).
* Taking a snapshot of a volume greater than 10 TB is not supported.
* You can delete any snapshot you take. Snapshots must be in a `stable` or `pending` state and not actively restoring a volume.
* You can delete a {{site.data.keyword.block_storage_is_short}} volume and all its snapshots. All snapshots must be in a `stable` or `pending` state. No snapshot can be actively restoring a volume.

## Creating and using snapshots
{: #snapshots-vpc-procedure-overview}

This general procedure shows how to create a snapshot, view a list of snapshots, and then restore a volume from a snapshot. For more information, click the links that are provided in each step.

1. Identify the boot or data volume that you want to snapshot.
2. Decide which interface to use, the UI, CLI, or API.
   * To use the UI, log in to the [{{site.data.keyword.cloud_notm}} console](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create-ui).
   * To use the [CLI](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create-cli), download and install the following CLI plug-ins. For more information, see the [CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
      - {{site.data.keyword.cloud_notm}} CLI
      - The infrastructure-service plug-in
   * To use the [API](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create-api), set up the [VPC API](/apidocs/vpc).
3. [Create](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) your snapshots.
4. [View](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view) and [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-manage) your snapshots.
5. [Restore](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-vpc-restore) a volume from a snapshot.

## Restoring a volume from a snapshot
{: #snapshots_vpc_restore_overview}

You can create a volume from a snapshot at any time. This process, called restoring a volume, can be performed when you create an instance, modify an instance, or when you create a stand-alone volume (no instance provisioning is required). Volume data restoration begins immediately as the volume is hydrated, but performance is degraded until the volume is fully restored. The volume does not need to be attached to an instance. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

## Tags for {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-about-tags}

You can apply [user tags](#snapshots-about-user-tags) and [access management tags](#snapshots-about-mgt-tags) to snapshots of your {{site.data.keyword.block_storage_is_short}} volumes to better control and organize them across the VPC.

### User tags for snapshots
{: #snapshots-about-user-tags}

User tags are uniquely identified by a Cloud Resource Name (CRN) identifier. When you create a user tag, you provide a unique name within your billing account. You can define user tags in label or key-value format. You can create user tags or add existing tags to snapshots of your {{site.data.keyword.block_storage_is_short}} volumes. You can also add tags when you use a snapshot of a volume during [instance creation](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-instancevol-tags-ui). Create, view, and manage user tags in the [UI](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-add-tags-ui), from the [CLI](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=cli#snapshots-vpc-add-tags-cli), or with the [API](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=cli#snapshots-vpc-add-tags-cli), and remove them at any time.

### Access management tags for {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-about-mgt-tags}

Access management tags help organize access control by creating flexible resource groupings, enabling your storage resources to grow without requiring updates to {{site.data.keyword.iamshort}} (IAM) policies. You create access management tags in IAM or with the Global Search and Tagging API, and then [add them to new or existing snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-add-tags-ui) from the snapshots service.

For more information about managing tags for your account, see [Working with tags](/docs/account?topic=account-tag&interface=ui).

## Next steps
{: #snapshots-vpc-about-next-steps}

Start [creating snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) of your volumes.
