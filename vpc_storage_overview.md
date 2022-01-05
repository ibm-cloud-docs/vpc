---

copyright:
  years: 2021
lastupdated: "2022-01-05"

keywords: block storage, boot volume, data volume, volume, data storage, file storage, snapshots

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:preview: .preview}
{:note: .note}

# VPC storage service overview
{: #storage-overview}

The {{site.data.keyword.cloud}} Virtual Private Cloud (VPC) provides block storage, file storage, and snapshots solutions to meet your cloud storage needs. Instance storage provides dedicated drives directly attached to your virtual server instances. 
{: shortdesc}

## Block Storage for VPC
{: #vpc-block-storage-overview}

{{site.data.keyword.block_storage_is_short}} provides hypervisor-mounted, high-performance data storage for your virtual server instances that you can provision within a VPC. The VPC infrastructure provides rapid scaling across zones and extra performance and security. 

Using this service, you can:

* Create block storage data volumes from the UI, CLI, and API.
* When you create the volume, specify an IOPS profile that best meets your storage requirements. You can choose a standard IOPS profile with 3, 5, or 10 IOPS per GB performance. Volume capacity can be up to 16 TB and 48,000 IOPS.
* Create a volume using a custom profile. Choose IOPS performance from 100 IOPS to 48,000 IOPS, based on volume size up to 16 TB.
* Attach a data volume to a virtual server instance.
* Choose customer-managed encryption for your block storage boot or data volume, and secure your data using your own encryption keys.
* View a list of volumes and select a volume to see its details.
* Manage volumes: Rename or delete volumes, transfer volumes to a different instance, detach or reattach a volume, assign access to a volume, and access performance metrics.
* Expand volume capacity to meet your needs.
* Adjust IOPS up or down, for greater performance or when you want to reduce costs.
* Adjust the volume bandwidth ratio for volumes attached to a virtual server instance.

For more information about this service, see [About Block Storage for VPC](/docs/vpc?topic=vpc-block-storage-about).

## File Storage for VPC
{: #vpc-file-storage-overview}

{{site.data.keyword.cloud}} File Storage for {{site.data.keyword.vpc_full}} (VPC) is a zonal file storage offering that provides NFS-based file storage services. You create file shares in an availability zone within a region. You can share them with multiple virtual server instances within the same zone across multiple VPCs.

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, London, and Sydney regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

Using this service, you can:

* Create file shares and mount targets from the UI, CLI, and API.
* When you create a file share, specify 
*  When you create a file share, specify an IOPS profile that best meets your storage requirements. You can choose a standard IOPS profile with 3, 5, or 10 IOPS per GB performance. File share capacity can be up to 32 TB and 96,000 IOPS.
* Create a file share using a custom profile. Choose IOPS performance from 100 IOPS to 48,000 IOPS, based on file share size up to 16 TB.
* View a list of file shares. Select a file share to see its details.
* Manage file shares: Rename or delete file shares and mount targets, add mount targets to a file share, mount and unmount a file share from virtual server instances, and add supplemental IDs to a file share.
* Expand file share capacity.
* Mount file shares on Red Hat, CentOS, or Ubuntu Linux distributions.

For more information, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about).

## Snapshots for VPC
{: #vpc-snapshots-overview}

Snapshots for VPC are a regional offering that creates a point-in-time copy of your block storage boot or data volume that you manually create. The initial snapshot that you take is a full backup of the volume. Subsequent snapshots of the same volume are incremental; only the changes since the last snapshots are captured. 

You can select a snapshot during instance provisioning, and restore a new, fully provisioned boot volume to start the instance. You can also create and attach a data volume from a snapshot within a running virtual server instance.

Snapshots have a lifecycle independent of the source block storage volume. You can delete the original volume and the snapshot persists. Snapshots inherit encryption from the source volume. 

You can create, list, view details, and manage snapshots from the UI, CLI, and API.

For more information, see [About Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about).

## Instance storage
{: #vpc-instance-storage-overview}

Instance storage is a set of one or more solid state drives, directly attached to your virtual server instance when the instance is provisioned. An instance storage disk provides fast, affordable, temporary storage to improve the performance of cloud native workloads with scratch space, caching buffers, or a place for replicated data.

Data stored on instance storage is ephemeral, meaning that it is tied directly to the instance lifecycle. The instance storage disk is automatically created and destroyed with the instance. Instance storage data is not lost, however, when an instance is rebooted. For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

## Next steps
{: #vpc-storage-next-steps}

* [Create block storage volumes](/docs/vpc?topic=vpc-creating-block-storage).
* [Create file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).
* [Create snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create).
* [Manage instance storage](/docs/vpc?topic=vpc-instance-storage-provisioning).
