---

copyright:
  years: 2022, 2024
lastupdated: "2024-06-14"

keywords: block storage for VPC, File Storage for VPC, Snapshots for VPC, Backup for VPC, block storage, file storage, snapshots, backup, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC storage services overview
{: #storage-overview}

The {{site.data.keyword.vpc_full}} (VPC) provides block storage, file storage, and snapshots solutions to meet your cloud storage needs. Instance storage provides dedicated drives that are directly attached to your virtual server instances. In addition, with the VPC Backup service, you can create scheduled backups of your block storage volumes.
{: shortdesc}

## {{site.data.keyword.block_storage_is_short}}
{: #vpc-block-storage-overview}

{{site.data.keyword.block_storage_is_full}} provides hypervisor-mounted, high-performance data storage for your virtual server instances that you can provision within a VPC. The VPC infrastructure provides rapid scaling across zones and extra performance and security.

By using this service, you can:

* Create block storage volumes with maximum storage capacity of 16 TB and performance level of 48,000 IOPS.
* Create a volume with a predefined IOPS tier profile (3, 5, or 10 IOPS per GB) that best meets your storage requirements. Or you can create a volume with a custom profile, and choose IOPS performance from 100 IOPS to 48,000 IOPS, based on volume size (up to 16 TB).
* Use a boot volume to start a virtual server instance and attach data volumes to the instance.
* Choose customer-managed encryption for your block storage volume, and secure your data with your own encryption keys.
* Use the UI, CLI, API, or Terraform to create volumes, rename volumes, attach, and detach a volume, transfer volumes to a different instance. You can assign access to a volume, access performance metrics or delete the volume.
* You can adjust IOPS up or down, for greater performance or when you want to reduce costs.
* Start with a smaller volume and expand volume capacity later when you need more storage.
* Adjust the volume bandwidth ratio for volumes that are attached to a virtual server instance.

For more information about this service, see [About {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-about).

## {{site.data.keyword.block_storage_is_short}} snapshots
{: #vpc-bs-snapshots-overview}

Block storage snapshots is a regional offering. A snapshot is a point-in-time copy of your block storage volume that you create manually in the UI, from the CLI, with the API or Terraform. The initial snapshot is a full copy of the volume. Subsequent snapshots of the same volume are incremental; only those changes are captured that occurred since the last snapshot was taken. Snapshots inherit encryption from the source volume.

You can create, list, view details, and manage snapshots in the UI, from the CLI, and with the API or Terraform. You can select a nonbootable snapshot to create a data volume and attach it to a running virtual instance. You can also select a bootable snapshot during instance provisioning to restore its data to a new boot volume to start the instance. You can use the snapshot to create a stand-alone volume and attach it to an instance later.

You can copy snapshot to another region and use it to provision new volumes in that region, as a BCDR measure or geographic expansion.

Snapshots are independent of the source block storage volumes. You can delete the original volume and the snapshot persists. However, you cannot delete a snapshot that is being used to hydrate a newly restored storage volume.

You can create a snapshot consistency group that contains snapshots of multiple Block Storage volumes that are attached to the same virtual server instance. You can include or exclude boot volumes. The snapshot consistency group has its own lifecycle, and it keeps references to the member snapshots. So if a member snapshot is deleted or renamed, the consistency group is also updated.

For more information, see [About Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about).

## {{site.data.keyword.filestorage_vpc_short}}
{: #vpc-file-storage-overview}

{{site.data.keyword.filestorage_vpc_short}} is a zonal storage offering that provides NFS-based file storage services. You create file shares in an availability zone within a region. You can share them with multiple virtual server instances within the same zone across multiple VPCs.

By using this service, you can:

* Create file shares and mount targets with maximum storage capacity of 32 TB and performance level of 96,000 IOPS.
* Create a file share that best meets your storage requirements by using the `dp2` profile and specifying the capacity and IOPS that you need.
* Use the UI, CLI, API, or Terraform to create file shares and mount targets, rename or delete file shares and mount targets, add mount targets to a file share. You can mount and unmount a file share from virtual server instances, and add supplemental IDs to a file share.
* You can adjust IOPS up or down, for greater performance or when you want to reduce costs.
* Start with a smaller file share and expand the capacity later when you need more storage.
* Mount file shares on Red Hat, CentOS, or Ubuntu Linux distributions. Windows OS is not supported.

For more information, see [About {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-file-storage-vpc-about).


## Backup for VPC
{: #vpc-backup-serv-overview}

The {{site.data.keyword.cloud}} provides the means to create backup copies of your block storage volumes automatically. You can create a backup policy with one or more plans, and associate tags to the policy in the UI, from the CLI, with the API or Terraform. The user-defined tags can be added to block storage volumes. When tags match, the backup policy is applied to the volume, and backup copies of the data are created based on the backup plan. You can set your own retention schedule to automatically delete older backups. This way, you can control how much space is used and how long backups are retained. By using Backup for VPC service, you can prevent data loss, manage risk, and improve data compliance. 



For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

## Instance storage
{: #vpc-instance-storage-overview}

Instance storage is allocated from one or more local SSDs on the server that is hosting the instance. An instance storage disk provides fast, affordable, temporary storage to improve the performance of cloud native workloads with scratch space, caching buffers, or a place for replicated data.

Data that is stored on instance storage is tied directly to the instance lifecycle and is only held temporarily. The instance storage disk is automatically created and destroyed with the instance. However, instance storage data is not lost when an instance is rebooted. For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

## Next steps
{: #vpc-storage-next-steps}

* [Create block storage volumes](/docs/vpc?topic=vpc-creating-block-storage).
* [Create {{site.data.keyword.block_storage_is_short}} snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create).
* [Create file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).
* [Manage instance storage](/docs/vpc?topic=vpc-instance-storage-provisioning).
* [Create backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
