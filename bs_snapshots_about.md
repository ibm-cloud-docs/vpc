---

copyright:
  years: 2022, 2025
lastupdated: "2025-03-18"

keywords: snapshots, Block Storage, volumes, cross-regional snapshot, restore volume, copy snapshot

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-vpc-about}

{{site.data.keyword.block_storage_is_short}} snapshot is a regional offering that is used to create a point-in-time copy of your boot or data volume. The initial snapshot that you take is a full backup of the volume. Subsequent snapshots of the same volume are incremental, so they capture only the changes that occurred after the last snapshot was taken. You can restore data to a new volume during instance provisioning, from an existing instance, and when you create an unattached volume.
{: shortdesc}

## Snapshots concepts
{: #snapshots-vpc-concepts}

### Single volume snapshots
{: #single-volume-snapshots}

A snapshot is a copy of your volume that you take manually in the UI or from the CLI, or create programmatically with the API, or Terraform. You can take a snapshot of a boot or data volume that is attached to a running virtual server instance. A "bootable" snapshot is a snapshot of a boot volume. A "nonbootable" snapshot is of a data volume. You can take snapshots as often as you want. However, you can't take a snapshot of a volume that's in a degraded state.

Do you want to automatically create snapshots of your {{site.data.keyword.block_storage_is_short}} volumes? With Backup for VPC, you can create backup policies to schedule regular volume backups. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).
{: tip}

The first time that you take a snapshot of a volume, all the volume's contents are copied. The snapshot has the same encryption as the volume (customer-managed or provider-managed). Snapshots are stored and retrieved from {{site.data.keyword.cos_full}}. Data is encrypted while in transit and stored in the same region as the original volume.

When you take a second snapshot, it captures only the changes that occurred since the last snapshot was taken. As such, the size of the snapshots can grow or shrink, depending on what is being uploaded to {{site.data.keyword.cos_full}}. The number of snapshots increases with each successive snapshot that you take. You can take up to 750 snapshots per volume in your region. Within this limit, you can take and keep an hourly snapshot for 30 days, plus some extra snapshots. Deleting snapshots from this quota frees up space for more snapshots. A snapshot of a volume can't be greater than 10 TB.

You can create a virtual server instance with a boot volume that is initialized from a snapshot. The instance profile of the new instance is not required to match the instance that was used to create the snapshot. You can also import a snapshot of a data volume when you create and attach a data volume to the instance. You can specify user tags for these snapshots.

You can create a volume from a snapshot at any time. This process is called restoring a volume, and it can be performed when you create an instance, modify an instance, or when you create a stand-alone volume. For more information, see [Restoring a volume from a snapshot](#bs-snapshots-restore-overview). You can also restore a fully provisioned volume by using the fast restore feature after initial provisioning.

Snapshots have a lifecycle that is independent from the source {{site.data.keyword.block_storage_is_short}} volume. You can delete the original volume and the snapshot persists. However, do not detach the volume from the instance during snapshot creation. You need to wait until the snapshot becomes `stable` before you detach, otherwise you can't reattach the volume to an instance. Snapshots are crash-consistent. If the virtual server stops for any reason, the snapshot data is safe on the disk.

The cost for snapshots is calculated based on GB capacity that is stored per month, unless the duration is less than one month. Because the snapshot is based on the capacity that was provisioned for the original volume, the snapshot capacity does not vary.

With {{site.data.keyword.iamlong}}, you can set up resource groups in your account to provide user-access to your snapshots. Your IAM role determines whether you can create and manage snapshots. For more information, see [IAM roles for creating and managing snapshots](/docs/account?topic=account-iam-service-roles-actions#is.snapshot-roles).

You can share a snapshot with another account and allow the other account to create volumes with that snapshot. To do so, set up [cross-account authorization](/docs/vpc?topic=vpc-block-s2s-auth&interface=ui#block-s2s-auth-xaccountrestore-ui) with the *Snapshot Remote Account Restorer* role, and share the CRN of the snapshot with the other account. The other account's user with *Snapshot Remote Account Restorer* role can use the CRN to create a volume in the console, from the CLI, with the API, or Terraform.

Before you take a snapshot, make sure that all cached data is present on disk, especially when you're taking snapshots of instances with Windows and Linux&reg; operating systems. For example, on Linux operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{: note}

For customers with special access, data isolation is provided to store snapshots that you created from your dedicated hosts. With data isolation's extra security, your data is encrypted at rest with a unique key and access to your data is protected by a private firewall.
{: beta}

### Fast restore snapshot clones
{: #snapshots_vpc_fast_restore}

By using the snapshots fast restore feature, you create and keep a clone of the data in a zone within your VPC region. In other words, your data is fully available when the volume is created because it is already in the VPC region and not in {{site.data.keyword.cos_short}}. In contrast, when data is not restored from a fast restore snapshot clone, volume data is copied from {{site.data.keyword.cos_short}} when the volume is attached to an instance over time. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular snapshot.

The fast restore feature can be enabled in the UI, from the CLI, with the API, or Terraform. In the CLI, API, and Terraform, the responses show `clones`. The UI shows fast restore snapshot clones as fast restore snapshots, but they are the same thing.

Billing for the fast restore feature is set at a flat rate based on instance hours. The feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots.

For more information, see [Restoring a volume with the fast restore feature](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-use-fast-restore).

### Cross-regional snapshot copies
{: #snapshots_vpc_crossregion_copy}

You can copy a snapshot from one region to another region, and later use that snapshot to restore a volume in the new region. This feature can be used in disaster recovery scenarios when you need to start your virtual server instance and data volumes in a different region. Or you can use the remote copy to create storage volumes in a new region to expand your VPC.

When you choose to create a cross-regional copy of a snapshot, you need to specify a single snapshot to be copied to the target region. The snapshot is created as normal, and stored in your regional {{site.data.keyword.cos_short}}. When the snapshot is stable, a copy of the snapshot is created in the {{site.data.keyword.cos_short}} bucket in the target region.

If the source snapshot is not encrypted with a customer key, the encryption of the copy remains provider-managed. If the source snapshot is protected by a customer-managed key, you must specify the customer-managed key that you want to use to encrypt the new copy.

Only one copy of the snapshot can exist in each region. You can't create a copy in the local (source) region.
{: restriction}

The first time that you create a cross-regional copy, the snapshot is a full copy of the parent volume's data. Subsequent snapshots of the same volume contain only the changes that occurred since the previous snapshot was taken. The only exception to this rule is when you change the encryption type or the encryption key of the parent snapshot. When you change the encryption, the new remote copy needs to be a full copy of the parent snapshot, not an incremental copy.

You can use and manage the cross-regional snapshot in the target region independently from the parent volume or the original snapshot.

Creating a cross-regional copy affects billing. You're charged for the data transfer and the storage consumption in the target region separately.

When you create a snapshot or list details of a snapshot, the system lists only the direct copies of the snapshot that you specified. If you create a copy of a copy, the second copy is not returned when you query the original snapshot.
{: note}

### Snapshot consistency groups
{: #multi-volume-snapshots}

A snapshot consistency group contains snapshots of multiple Block Storage volumes that are attached to the same virtual server instance. You can include or exclude boot volumes. Instance storage is not included.

When you request a snapshot of a consistency group, the system ensures that all write operations are complete before it takes the snapshots. Then, the system generates snapshots of all the tagged Block Storage volumes that are attached to the virtual server instance at the same time. Depending on the number and size of the attached volumes, plus the amount of data that is to be captured, you might observe a slight IO pause. This IO pause can range from a few milliseconds up to 4 seconds.

The consistency group must have a unique name that can't exceed 64 characters. If you do not provide one, the system auto-generates a name for you. The snapshots in the consistency group are named by using the first 16 characters of the group name and 3-4 auto-generated characters, so their names are unique, too. Later, you can rename the member snapshots if you want to.

The snapshot consistency group has its own lifecycle, and it keeps references to the member snapshots. So if a member snapshot is deleted or renamed, the consistency group is also updated.

The snapshots in the group are loosely coupled. You can manage the snapshots within a consistency group the same way you manage any other snapshot. You can delete individual snapshots from the consistency group if you want to. You can keep individual snapshots after you decide to delete the consistency group.

You can use the members of the snapshot consistency group to restore volumes separately. Restoring an instance directly from snapshot consistency group identifier is not supported. You can create cross-regional copies of the members of the snapshot set separately, but you can't create a copy of a consistency group in another zone or region.

## How snapshots work
{: #snapshots-vpc-operation}

When you take a snapshot, read and write operations from your virtual server instance to the volume continue uninterrupted. After volume data is retrieved, the snapshot enters a `pending` state until the snapshot is created. When the snapshot is successfully created and in a `stable` state, you can resume volume management activities, such as deleting, resizing, or detaching a volume. You can also take more snapshots.

Volume data that is retrieved for the requested snapshot is encrypted while in transit from the hypervisor to {{site.data.keyword.cos_full}}. The initial snapshot is the entire copy of your {{site.data.keyword.block_storage_is_short}} volume. Subsequent snapshots copy only what was changed since the last snapshot.

You can [restore](/docs/vpc?topic=vpc-snapshots-vpc-restore) a boot or data volume from a running virtual server instance in the UI, CLI, API, or Terraform. Restoring a data from a snapshot creates a new, fully provisioned volume. 

Restoring from a snapshot of a boot volume creates a new boot volume that you can use to provision another instance. Restoring from a snapshot of a data volume creates a secondary volume that is attached to the instance. You can also restore a stand-alone (unattached) data volume from a snapshot.

When you restore volumes from snapshots in a consistency group, you can select some or all of the snapshots.

## IAM roles for creating, managing, and restoring from single and consistency group snapshots
{: #snapshots-vpc-iam}

Snapshots require IAM permissions for role-based access control. You need the right platform role to create and manager with snapshots in your own account, and the correct service roles to use a snapshot for restoring data from another account. 

When you share a snapshot with another account, you must assign the *Snapshot Remote Account Restorer* role to the other account's user to allow them access to the snapshot. They must also have the *Restore Volume From Remote Account Snapshot* role in their account to create a volume with the CRN of the remote snapshot in the console.{: ui}

When you share a snapshot with another account, you must assign the `SnapshotRemoteAccountRestorer` role to the other account's user to allow them access to the snapshot. They must also have the `VolumeRemoteAccountSnapshotRestorer` role in their account to create a volume with the CRN of the remote snapshot from the CLI.{: cli}

When you share a snapshot with another account, you must assign the `SnapshotRemoteAccountRestorer` role to the other account's user to allow them access to the snapshot. They must also have the `VolumeRemoteAccountSnapshotRestorer` role in their account to create a volume with the CRN of the remote snapshot with the API.{: api}

When you share a snapshot with another account, you must assign the `SnapshotRemoteAccountRestorer` role to the other account's user to allow them access to the snapshot. They must also have the `VolumeRemoteAccountSnapshotRestorer` role in their account to create a volume with the CRN of the remote snapshot with Terraform.{: terraform}

For more information, see [IAM roles and actions for Block Storage Snapshots for VPC](/docs/account?topic=account-iam-service-roles-actions#is.snapshot-roles), [IAM roles, and actions for Multi Volume Snapshots for VPC](/docs/account?topic=account-iam-service-roles-actions#is.snapshot-consistency-group-roles) and [IAM roles and actions for Block Storage for VPC](/docs/account?topic=account-iam-service-roles-actions#is.volume-roles).

For more information, see the [best practices for assigning access](/docs/account?topic=account-account_setup#account_setup). For the complete IAM process, which includes inviting users to your account and assigning Cloud IAM access, see the [IAM getting started tutorial](/docs/account?topic=account-iamoverview).
{: tip}

## Limitations
{: #snapshots-vpc-limitations}

The following limitations apply to this release:

* You can take up to 750 snapshots per volume in a region.
* You can't take a snapshot of a volume in a [degraded state](/docs/vpc?topic=vpc-block-storage-vpc-monitoring#block-storage-vpc-health-states).
* You can't create a copy of a snapshot in the source (local) region.
* When you create copies of a snapshot in other regions, only one copy can exist in each region. That means 9 copies globally.
* Cross-regional copies are not supported in Montreal (`ca-mon`) MZR.
* Taking a snapshot of a volume greater than 10 TB is not supported.
* You can delete any snapshot that you take. However, snapshots must be in a `stable` or `pending` state and not actively restoring a volume.
* You can delete a {{site.data.keyword.block_storage_is_short}} volume and all its snapshots. All snapshots must be in a `stable` or `pending` state. No snapshot can be actively restoring a volume.
* Restoring an instance directly from snapshot consistency group identifier is not supported.

## Create and work with snapshots
{: #snapshots-vpc-procedure-overview}

You can create and manage your snapshots by using the UI, CLI, API, and Terraform.
* To use the UI, log in to the [{{site.data.keyword.cloud_notm}} console](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui).{: ui}
* To use the [CLI](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=cli), download and install the required CLI plug-ins. For more information, see the [CLI reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli).{: cli}
* To use the [API](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=api), set up the [VPC API](/apidocs/vpc).{: api}
* To use [Terraform](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=terraform), download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).{: terraform}

For more information about creating and managing snapshots, and restoring a volume from a snapshot, see the following topics.
* [Create](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create) your snapshots.
* [View](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view) and [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-manage) your snapshots.
* [Restore](/docs/vpc?topic=vpc-snapshots-vpc-restore#snapshots-vpc-restore) a volume from a snapshot.

For more information about creating and managing consistency groups, see the following topics.
* [Create](/docs/vpc?topic=vpc-snapshots-vpc-create-consistency-groups) your consistency group.
* [View](/docs/vpc?topic=vpc-snapshots-vpc-view) and [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage-consistency-groups) your consistency groups.

## Restore a volume from a snapshot
{: #bs-snapshots-restore-overview}

You can create a volume from a snapshot at any time. This process is called restoring. Volume data restoration begins immediately as the volume is hydrated, but performance is degraded until the volume is fully restored. The volume does not need to be attached to an instance. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

## Tags for {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-about-tags}

You can apply [user tags](#snapshots-about-user-tags) and [access management tags](#snapshots-about-mgt-tags) to snapshots of your {{site.data.keyword.block_storage_is_short}} volumes to better control and organize them across the VPC.

### User tags for snapshots
{: #snapshots-about-user-tags}

User tags are uniquely identified by a Cloud Resource Name (CRN) identifier. When you create a user tag, you provide a unique name within your billing account. You can define user tags in label or key-value format. You can create user tags or add existing tags to snapshots.

You can create, view, and manage user tags with the [UI](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-add-tags-ui){: ui}[CLI](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=cli#snapshots-vpc-add-tags-cli){: cli}[API](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=api#snapshots-vpc-add-tags-api){: api}[Terraform](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=terraform#snapshots-vpc-rename-terraform){: terraform}, and remove them at any time.

### Access management tags for {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-about-mgt-tags}

You can use access management tags to organize access control by creating flexible resource groupings, enabling your storage resources to grow without requiring updates to {{site.data.keyword.iamshort}} (IAM) policies. You can create access management tags in IAM or with the Global Search and Tagging API, and then [add them to new or existing snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-add-tags-ui).

For more information about managing tags for your account, see [Working with tags](/docs/account?topic=account-tag&interface=ui).

## Next steps
{: #snapshots-vpc-about-next-steps}

Review [creating snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create).
