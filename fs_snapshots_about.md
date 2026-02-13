---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-13"

keywords: snapshots, File Storage, shares, restore share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About {{site.data.keyword.filestorage_vpc_short}} snapshots
{: #fs-snapshots-about}

File Storage snapshots are used to create a point-in-time copy of your {{site.data.keyword.filestorage_vpc_short}} share. The initial snapshot that you take is a full backup of the share. Subsequent snapshots of the same share are incremental, so they capture only the changes that occurred after the last snapshot was taken. You can use a snapshot to restore data to a new share in the console, from the CLI, with the API, or Terraform.
{: shortdesc}

## Snapshots concepts
{: #fs-snapshots-concepts}

### Single share snapshots
{: #single-share-snapshots}

A snapshot is a copy of your share that you take manually in the console, or from the CLI, or create programmatically with the API, or Terraform. You can take snapshots as often as you want. However, you can't take a snapshot of a share that's in a degraded state.

The first time that you take a snapshot of a share, all the share's contents are copied. The snapshot has the same encryption as the share (customer-managed or provider-managed). Snapshots are stored in the same location as the file share.

When you take a second snapshot, it captures only the changes that occurred since the last snapshot was taken. As such, the size of the snapshots can grow or shrink, depending on what changed in the meantime. The number of snapshots increases with each successive snapshot that you take. You can take up to 750 snapshots per share, and one snapshot per minute.

When a snapshot is deleted, only the data blocks that are no longer needed by other snapshots are freed on the share. 

The lifecycle of snapshots is tied to the lifecycle of the shares that they belong to. When a file share is replicated, its snapshots are replicated as well. When the share is deleted, the snapshots are also deleted automatically.

The cost for snapshots is calculated based on GB capacity that is stored per month, unless the duration is less than one month. Because the snapshot is based on the capacity that was provisioned for the original share, the snapshot capacity does not vary.

Before you take a snapshot, make sure that all cached data is present on disk. For example, on Linux operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{: note}

You can restore files to your share from a snapshot. To perform a single-file restoration, you can use native OS functions of your virtual server instance. Browse to the share's NFS mount target to open the `/.snapshot` directory and see the data that is contained within each snapshot of your share.

Snapshots of regional file shares are stored in the `.snap` directory. If you can't find your new snapshot in that folder, follow the instruction in the troubleshooting topic: [Why is my snapshot not showing up in the `.snap` folder?](/docs/vpc?topic=vpc-fs-snapshots-delayed-regional-snapshot)
{: note}

You can use the snapshot to create a share as well. The share that you create by using a snapshot must have the same file share profile as the snapshot. However, you can increase the share's capacity beyond the size of the snapshot, and you can adjust the IOPS, too. For more information, see [Restoring a share from a snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore).

Do you want to automatically create snapshots of your {{site.data.keyword.filestorage_vpc_short}} shares? With Backup for VPC, you can create backup policies to schedule regular share backups. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).
{: tip}

## Limitations
{: #fs-snapshots-limitations}

The following limitations apply to this release:

* File share snapshots cannot be copied to another zone or region. They are stored in the same location as the file share. If you want the snapshots to survive the loss of the availability zone, you need to configure replication for the file share. When a replica share is created, all snapshots that are present on the source volume are also copied to the replica.
* Snapshots are not supported on shares with Access control mode "VPC". 
* Taking snapshots are also not supported on replica shares or Accessor shares. However, the `/.snapshot` and `.snap` directory is accessible both on replica and Accessor shares.

## Snapshots for second-generation file shares
{: #rfs-snapshots}

As a customer with special access, you can provision second-generation file shares with the `rfs` profile and create snapshots of these shares. During the [select availability]{: tag-green} phase, second-generation file share can range in size from 1 GB to 32 TB. You can create snapshots of the entire volume as needed. You can take up to 30 snapshots per share in a region. If needed, this limit can be increased upon request.
{: preview}

You can use your snapshots to create other second-generation file shares in the same region. You can't use your second-generation snapshot to create a zonal file share. Similarly, you can't use first-generation file share's snapshot to create a share with the `rfs` profile.

## Securing your data
{: #fs-snapshot-data-security}

{{site.data.keyword.cloud}} offers security-specific tools and features to help you securely manage your data when you use {{site.data.keyword.vpc_full}}. The following section provides information about access control, data encryption, configuration management, and auditing options that are available for your file share snapshots.

### Activity tracking and auditing
{: #fs-snapshots-at-events}

When you initiate an activity on a snapshot, specific Activity tracking events are generated. These activities include creating, listing, modifying, and deleting snapshots. For more information, see [File storage snapshots events](/docs/vpc?topic=vpc-at_events#events-fs-snapshots).

## Tags for {{site.data.keyword.filestorage_vpc_short}} snapshots
{: #fs-snapshots-about-tags}

You can apply [user tags](#snapshots-about-user-tags) to snapshots of your {{site.data.keyword.filestorage_vpc_short}} shares to better control and organize them across the VPC.

User tags are uniquely identified by a Cloud Resource Name (CRN) identifier. When you create a user tag, you provide a unique name within your billing account. You can define user tags in label or key-value format. You can create user tags or add existing tags to snapshots when you create the snapshots.

## Observability
{: #fs-snapshots-observability}

When snapshots are added or deleted, the change in the snapshot size is reported within 15 minutes. For more information, see [Monitoring metrics for File Storage for VPC](/docs/vpc?topic=vpc-fs-vpc-monitoring-sysdig).


## Next steps
{: #fs-snapshots-about-next-steps}

You can [create](/docs/vpc?topic=vpc-fs-snapshots-create#fs-snapshots-create) and [manage](/docs/vpc?topic=vpc-fs-snapshots-manage) your snapshots in the console, from the CLI, with the API, and Terraform.
* To use the console, log in to the [{{site.data.keyword.cloud_notm}} console](/docs/vpc?topic=vpc-fs-snapshots-create&interface=ui).{: ui}
* To use the [CLI](/docs/vpc?topic=vpc-fs-snapshots-create&interface=cli), download and install the required CLI plug-ins. For more information, see the [CLI reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli).{: cli}
* To use the [API](/docs/vpc?topic=vpc-fs-snapshots-create&interface=api), set up the [VPC API](/apidocs/vpc).{: api}
* To use [Terraform](/docs/vpc?topic=vpc-fs-snapshots-create&interface=terraform), download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).{: terraform}

For more information about creating and managing snapshots, and restoring a share from a snapshot, see the following topics.
* [Create](/docs/vpc?topic=vpc-fs-snapshots-create#fs-snapshots-create) your snapshots.
* [View](/docs/vpc?topic=vpc-fs-snapshots-view#fs-snapshots-view) and [manage](/docs/vpc?topic=vpc-fs-snapshots-manage#fs-snapshots-manage) your snapshots.
* [Restore](/docs/vpc?topic=vpc-fs-snapshots-restore#fs-snapshots-restore) a share from a snapshot.
