---

copyright:
  years: 2022, 2023
lastupdated: "2023-06-27"

keywords: snapshots, block storage, volumes, cross-regional snapshot, restore volume, copy snapshot

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About Block Storage Snapshots for VPC
{: #snapshots-vpc-about}

Block Storage Snapshots for VPC are a regional offering that is used create a point-in-time copy of your {{site.data.keyword.block_storage_is_short}} boot or data volume. The initial snapshot that you take is a full backup of the volume. Subsequent snapshots of the same volume are incremental, so only the changes since the last snapshots are captured. You can restore data to a new volume during instance provisioning, from an existing instance, and when you create an unattached volume.
{: shortdesc}

## Snapshots concepts
{: #snapshots-vpc-concepts}

A snapshot is a copy of your volume that you take manually in the UI or from the CLI, or create programmatically with the API, or Terraform. You can take a snapshot of a boot or data volume that is attached to a running virtual server instance or from an unattached data volume. A "bootable" snapshot is a snapshot of a boot volume. A "nonbootable" snapshot is of a data volume. You can take snapshots as often as you want. However, you cannot take a snapshot of a volume that's in a degraded state.

Do you want to automatically create snapshots of your {{site.data.keyword.block_storage_is_short}} volumes? With Backup for VPC, you can create backup policies to schedule regular volume backups. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).
{: tip}

The first time that you take a snapshot of a volume, all the volume's contents are copied. The snapshot has the same encryption as the volume (customer-managed or provider-managed). Snapshots are stored and retrieved from {{site.data.keyword.cos_full}}. Data is encrypted while in transit and stored in the same region as the original volume.

When you take a second snapshot, it captures only the changes that occurred since the last snapshot was taken. As such, the size of the snapshots can grow or shrink, depending on what is being uploaded to {{site.data.keyword.cos_full}}. The number of snapshots increases with each successive snapshot that you take. You can take up to 750 snapshots per volume in your region. Within this limit, you can take and keep an hourly snapshot for 30 days, plus some extra snapshots. Deleting snapshots from this quota frees up space for more snapshots. A snapshot of a volume cannot be greater than 10 TB.

You can create a virtual server instance with a boot volume that was initialized from a snapshot. The instance profile of the new instance is not required to match the instance that was used to create the snapshot. You can also import a snapshot of a data volume when you create and attach a new data volume to the instance. You can specify user tags for these snapshots.

You can create a volume from a snapshot at any time. This process, called restoring a volume, can be performed when you create an instance, modify an instance, or when you create a stand-alone volume (no instance provisioning is required). For more information, see [Restoring a volume from a snapshot](#snapshots_vpc_restore_overview). You can also restore a fully provisioned volume by using the [fast restore feature](#snapshots-fast-restore) after initial provisioning.

Snapshots have a lifecycle that is independent from the source {{site.data.keyword.block_storage_is_short}} volume. You can delete the original volume and the snapshot persists. However, do not detach the volume from the instance during snapshot creation. You need to wait until the snapshot is `stable` before you detach, otherwise you can't reattach the volume to an instance. Snapshots are crash-consistent. If the virtual server stops for any reason, the snapshot data is safe on the disk.

Cost for snapshots is calculated based on GB capacity that is stored per month, unless the duration is less than one month. Because the snapshot is based on the capacity that was provisioned for the original volume, the snapshot capacity does not vary.

With {{site.data.keyword.iamlong}}, you can set up resource groups in your account to provide user-access to your snapshots. Your IAM role determines whether you can create and manage snapshots. For more information, see [IAM roles for creating and managing snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-iam).

Before you take a snapshot, make sure that all cached data is present on disk, especially when you're taking snapshots of instances with Windows and Linux&reg; operating systems. For example, on Linux operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{: note}

For customers with special access, data isolation is provided to store snapshots that you created from your dedicated hosts. With data isolation's extra security, your data is encrypted at rest with a unique key and access to your data is protected by a private firewall.
{: beta}

## How snapshots work
{: #snapshots-vpc-operation}

When you take a snapshot, read and write operations from your virtual server instance to the volume continue uninterrupted. After volume data is retrieved, the snapshot enters a `pending` state until the snapshot is created. When the snapshot is successfully created and in a `stable` state, you can resume volume management activities, such as deleting, resizing, or detaching a volume. You can also take more snapshots.

Volume data that is retrieved for the requested snapshot is encrypted while in transit from the hypervisor to {{site.data.keyword.cos_full}}. The initial snapshot is the entire copy of your {{site.data.keyword.block_storage_is_short}} volume. Subsequent snapshots copy only what was changed since the last snapshot.

You can restore a boot or data volume from a running virtual server instance in the UI, CLI, API, or Terraform. Restoring a data from a snapshot creates a new, fully provisioned volume. Restoring from a snapshot of a boot volume creates a new boot volume that you can use to provision a new instance. Restoring from a snapshot of a data volume creates a secondary volume that is attached to the instance.

You can also restore a [stand-alone (unattached) data volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#restoring-from-an-unattached-snapshot) by using the UI, CLI, or the `volumes` API and specifying the snapshot ID. The volume from which the snapshot was created does not have to be attached to an instance.

## Limitations
{: #snapshots-vpc-limitations}

The following limitations apply to this release:

* You can take up to [750 snapshots](#snapshots_vpc_considerations) per volume in a region.
* You cannot take a snapshot of a volume in a [degraded state](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#block-storage-vpc-health-states).
* You can't create a copy of a snapshot in the source (local) region.
* When you create copies of a snapshot in other regions, only one copy can exist in each region. That means 9 copies globally.
* Taking a snapshot of a volume greater than 10 TB is not supported.
* You can delete any snapshot that you take. However, snapshots must be in a `stable` or `pending` state and not actively restoring a volume.
* You can delete a {{site.data.keyword.block_storage_is_short}} volume and all its snapshots. All snapshots must be in a `stable` or `pending` state. No snapshot can be actively restoring a volume.

## Create and work with snapshots
{: #snapshots-vpc-procedure-overview}

You can create and manage your snapshots. For more information about creating a snapshot, viewing a list of snapshots, and restoring a volume from a snapshot, see the following items.

* To use the UI, log in to the [{{site.data.keyword.cloud_notm}} console](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui).
* To use the [CLI](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=cli), download and install the following CLI plug-ins. For more information, see the [CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
* To use the [API](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=api), set up the [VPC API](/apidocs/vpc).
* To use [Terraform](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=terraform), download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
* [Create](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) your snapshots.
* [View](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view) and [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-manage) your snapshots.
* [Restore](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-vpc-restore) a volume from a snapshot.

## Restore a volume from a snapshot
{: #snapshots_vpc_restore_overview}

You can create a volume from a snapshot at any time. This process, called restoring a volume, can be performed when you create an instance, modify an instance, or when you create a stand-alone volume (no instance provisioning is required). Volume data restoration begins immediately as the volume is hydrated, but performance is degraded until the volume is fully restored. The volume does not need to be attached to an instance. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

## Fast restore snapshot clones
{: #snapshots_vpc_fast_restore}

By using the snapshots fast restore feature, you create and keep a clone of the data in a zone within your VPC region. In other words, your data is fully available when the volume is created because it is already in the VPC region and not in {{site.data.keyword.cos_short}}. In contrast, when data is not restored from a fast restore snapshot clone, volume data is copied from {{site.data.keyword.cos_short}} when the volume is attached to an instance over time. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular snapshot.

The fast restore feature can be enabled in the UI, from the CLI, with the API, or Terraform. In the CLI, API, and Terraform, the responses show `clones`. The UI shows fast restore snapshot clones as fast restore snapshots, but they are the same thing.

Billing for the fast restore feature is set at a flat rate based on instance hours.

For more information, see [Restoring a volume with the fast restore feature](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-use-fast-restore).

## Cross-regional snapshot copies
{: #snapshots_vpc_crossregion_copy}

You can copy a snapshot from one region to another region, and later use that snapshot to restore a volume in the new region. This feature can be used in disaster recovery scenarios when you need to start your virtual server instance and data volumes in a different region. Or you can use the remote copy to create storage volumes in a new region to expand your VPC.

When you choose to create a cross-regional copy of a snapshot, you need to specify a single snapshot to be copied to the target region. The snapshot is created as normal, and stored in your regional {{site.data.keyword.cos_short}}. A copy of the snapshot is created in an {{site.data.keyword.cos_short}} bucket in the target region.

If the source snapshot is not encrypted with a customer key, the encryption of the copy remains provider-managed. If the source snapshot is protected by a customer-managed key, you must specify the customer-managed key that you want to use to encrypt the new copy.

Only one copy of the snapshot can exist in each region. You can't create a copy in the local (source) region.
{: restriction}

The first time that you create a cross-regional copy, the snapshot is a full copy of the parent volume's data. Subsequent snapshots of the same volume contain only the changes that occurred since the previous snapshot was taken. The only exception to this rule is when you change the encryption type or the encryption key of the parent snapshot. When you change the encryption, the new remote copy needs to be a full copy of the parent snapshot, not an incremental copy.

You can use and manage the cross-regional snapshot in the target region independently from the parent volume or the original snapshot.

Creating a cross-regional copy affects billing. You're charged for the data transfer and the storage consumption in the target region separately.

You can create and manage cross-regional snapshot copies in the UI, from the CLI, with the API, or Terraform.

## Tags for {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-about-tags}

You can apply [user tags](#snapshots-about-user-tags) and [access management tags](#snapshots-about-mgt-tags) to snapshots of your {{site.data.keyword.block_storage_is_short}} volumes to better control and organize them across the VPC.

### User tags for snapshots
{: #snapshots-about-user-tags}

User tags are uniquely identified by a Cloud Resource Name (CRN) identifier. When you create a user tag, you provide a unique name within your billing account. You can define user tags in label or key-value format. You can create user tags or add existing tags to snapshots. You can also add tags when you use a snapshot of a volume during [instance creation](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-instancevol-tags-ui).

You can create, view, and manage user tags in the [UI](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-add-tags-ui), from the [CLI](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=cli#snapshots-vpc-add-tags-cli), or with the [API](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=cli#snapshots-vpc-add-tags-cli), and remove them at any time.

### Access management tags for {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-about-mgt-tags}

You can use access management tags to organize access control by creating flexible resource groupings, enabling your storage resources to grow without requiring updates to {{site.data.keyword.iamshort}} (IAM) policies. You create access management tags in IAM or with the Global Search and Tagging API, and then [add them to new or existing snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-add-tags-ui).

For more information about managing tags for your account, see [Working with tags](/docs/account?topic=account-tag&interface=ui).

## Next steps
{: #snapshots-vpc-about-next-steps}

Review [creating snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create).
