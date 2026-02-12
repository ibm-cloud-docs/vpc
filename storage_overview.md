---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-12"

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

{{site.data.keyword.block_storage_is_full}} provides hypervisor-mounted, high-performance data storage for your virtual server instances that you can provision within a VPC. The VPC infrastructure provides rapid scaling across zones and more performance and security.

By using this service, you can create boot volumes and data volumes for your virtual server instances. Use the IBM Cloud console, CLI, API, or Terraform to create volumes, rename volumes, attach, and detach a volume of a virtual server instance, transfer volumes to a different instance, tag volumes, delete volumes, and access metrics.

Pay for only the capacity that you need. You can start with a smaller volume, and increase the capacity later.

You can specify your own performance limits, and adjust them later if your requirements change. You can adjust IOPS up or down, for greater performance or when you want to reduce costs.

   * The second-generation volume profile, `sdp` provides the most flexibility, as you can specify your capacity, IOPS, and throughput maximum values. You can create boot and data volumes with maximum storage capacity of 32,000 GB, a performance level of 64,000 IOPS, and maximum throughput of 1024 MBps. The capacity, IOPS, and throughput values of volumes that are created with the `sdp` profile can be modified even when the volume is not attached to a virtual server instance. The `sdp` profile is available in the Dallas, Frankfurt, London, Madrid, Osaka, Sao Paulo, Sydney, Tokyo, Toronto, and Washington, DC region.

   * You can create first-generation block storage volumes with maximum storage capacity of 16,000 GB, a performance level of 48,000 IOPS, and maximum throughput of 1024 MBps. You can create a first-generation volume with a predefined IOPS tier profile (3, 5, or 10 IOPS per GB) that best meets your storage requirements. Or you can choose the custom profile and specify your performance. IOPS value per volume ranges from 100 IOPS to 48,000 IOPS, based on volume size.
   
You can choose between provider- and customer-managed encryption for your block storage volumes, and secure your data with your own encryption keys. 

The following table provides a comparison between the different generations of block storage.

| Features            | First-generation volumes | Second-generation volumes |
|---------------------|--------------------------|---------------------------|
| Availability        | Generally available in all VPC regions for all customers. | Available in most MZRs, except Chennai and Montreal.|
| Expandable capacity | Yes, up to 16,000 GB     | Yes, up to 32,000 GB |
| Adjustable IOPS     | Yes, up to 48,000. IOPS depends on capacity range. | Yes, up to 64,000.| 
| Adjustable Bandwidth| No. Bandwidth can be increased by increasing capacity and IOPS. The maximum is 1024 MBps.| Yes, bandwidth can be adjusted to any value between 125 and 1024 MBps.|
| Customer-managed encryption at rest | Yes. | Yes.|
| Importing encrypted custom image for boot volumes |  Yes.  | Not supported yet.|
| Creating encrypted custom image from boot volume | Yes. | Not supported yet.|
| On-demand snapshots | Yes, up to 750 snapshots per region. | Yes, up to 512 snapshots per region.|
| Scheduled snapshots | Yes, up to 750 snapshots per region. | Yes, up to 512 snapshots per region.|
| Secure boot supported | Yes. | Not supported yet. |
{: caption="Block Storage volume generations comparison." caption-side="bottom"}

For more information about this service, see [About {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-about).

## {{site.data.keyword.block_storage_is_short}} snapshots
{: #vpc-bs-snapshots-overview}

Block storage snapshots are a regional offering. A snapshot is a point-in-time copy of your block storage volume that you create manually in the console, from the CLI, with the API or Terraform. The initial snapshot is a full copy of the volume. Subsequent snapshots of the same volume are incremental; only those changes are captured that occurred since the last snapshot was taken. Snapshots inherit encryption from the source volume.

You can create, list, view details, and manage snapshots in the console, from the CLI, and with the API or Terraform. You can select a nonbootable snapshot to create a data volume and attach it to a running virtual instance. You can also select a bootable snapshot during instance provisioning to restore its data to a new boot volume to start the instance. You can use the snapshot to create a stand-alone volume and attach it to an instance later.

You can copy the snapshot to another region and use it to provision new volumes in that region, as a BCDR measure or geographic expansion.

You can also share a snapshot with another account and allow the other account to create volumes with the snapshot. To do so, set up cross-account authorization in {{site.data.keyword.iamshort}}, and share the CRN of the snapshot with the other account. The other account's authorized storage administrator can use the CRN to create a volume in the console, from the CLI, with the API, or Terraform.

Snapshots are independent of the source block storage volumes. You can delete the original volume and the snapshot persists. However, you cannot delete a snapshot that is being used to hydrate a newly restored storage volume.

You can create a snapshot consistency group that contains snapshots of multiple Block Storage volumes that are attached to the same virtual server instance. You can include or exclude boot volumes. The snapshot consistency group has its own lifecycle, and it keeps references to the member snapshots. So if a member snapshot is deleted or renamed, the consistency group is also updated.

Second-generation snapshots are available in Dallas, Frankfurt, London, Madrid, Osaka, Sao Paulo, Sydney, Tokyo, Toronto, and Washington, DC. In this release, you can create up to 512 snapshots of these volumes. You can even create snapshots when the volumes are unattached.

You can use your snapshots to create other second-generation volumes in the same region. You can't use your second-generation snapshot to create a volume with a first-generation volume profile. Similarly, you can't use first-generation volume's snapshot to create a volume with the `sdp` profile. Consistency group snapshots of multiple `sdp` volumes are not supported.

| Features            | First-generation snapshots | Second-generation snapshots |
|---------------------|----------------------------|-----------------------------|
| Availability        | Generally available in all VPC regions for all customers. | Available in most regions, except Chennai and Montreal.|
| On-demand snapshots | Yes, up to 750 snapshots per region. | Yes, up to 512 snapshots per region.|
| Scheduled snapshots | Yes, up to 750 snapshots per region. | Yes, up to 512 snapshots per region. |
| Fast restore clones | Yes. You can cache a copy of your snapshot in any zone of the region. |Yes, the feature is available in most regions, except Chennai and Montreal. You can cache a copy of your snapshot in any zone of the region. |
| Cross-regional copy | The feature is available in most regions, except Chennai. You can create one cross-regional copy per snapshot per region. | The feature is available in most regions, except Chennai and Montreal. You can create one cross-regional copy per snapshot per region.|
| Consistency group   | Multi-volume snapshots are supported. | Not supported in the current release. |
{: caption="Block Storage snapshot generations comparison." caption-side="bottom"}

First- and second-generation volume profiles are not interchangeable. You can't create a second-generation block volume with a snapshot that was taken of a first-generation volume. You can't use the snapshot with a second-generation volume profile to create a first-generation volume.
{: note}

For more information, see [About Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about).

## {{site.data.keyword.filestorage_vpc_short}}
{: #vpc-file-storage-overview}

{{site.data.keyword.filestorage_vpc_short}} provides NFS-based file storage services. You create file shares in an availability zone within a region. You can share them with multiple virtual server instances or bare metal servers within the same zone across multiple VPCs. Cross-zone mounting is available. You can share file share data with other accounts or services.

By using this service, you can create file shares that best meet your storage requirements. Use the IBM Cloud console, CLI, API, or Terraform to create shares, rename shares, create mount targets, set up cross-zonal or cross-regional replication, tag shares, add supplemental IDs, and delete shares.

You can choose between provider- and customer-managed encryption for your file shares, and secure your data with your own encryption keys.

Pay for only the capacity that you need. You can start with a smaller file share, and increase the capacity later. You can specify your own performance limits when you create a file share, and adjust them later if your requirements change.

You can create zonal file shares with the `dp2` profile. The first-generation file shares come with a maximum storage capacity of 32 TB, a performance level of 96,000 IOPS, and maximum throughput of 1024 MBps. You can adjust IOPS up or down, for greater performance or when you want to reduce costs. You can create read-only replicas of your file shares in another zone within your VPC, or another zone in a different region if you have multiple VPCs in the same geography. The replica is updated regularly based on the replication schedule that you specify. You can schedule to replicate your data as often as every 15 minutes. You can fail over to the replica and make it active if an outage occurs at the primary site.

Customers with special access to preview the second-generation file storage offering can provision file shares with the new `rfs` profile. The `rfs` profile is available in the Dallas, Frankfurt, London, Madrid, Osaka, Sao Paulo, Sydney, Toronto, Tokyo, and Washington, DC regions in this release.
{: preview}

Second-generation file shares can be created with capacity in the range of 1 - 32,000 GB. Customers can directly adjust their file share's bandwidth up to 8192 Mbps (1024 MBps). The preset value is 800 Mbps. The maximum IOPS that a share with the `rfs` profile can support is 35,000. Second-generation profiles provide regional data availability across all 3 zones of an MZR. Data is regionally available, setting up replication between different zones is unnecessary.

You can mount file shares on Red Hat, CentOS, or Ubuntu Linux distributions. Windows OS is not supported.

The following table provides a comparison between the different generations of file storage.

| Features            | First-generation shares | Second-generation shares |
|---------------------|--------------------------|---------------------------|
| Availability        | Generally available in all VPC regions for all customers. | Available in the Dallas, Frankfurt, London, Madrid, Osaka, Sydney, Tokyo, Toronto, and Washington, DC regions for allowlisted customers.|
| Data Availability   | Zonal. While the share can be accessed from multiple zones across the region, if the share becomes unavailable, data access is lost. Operations can be recovered by setting up replicas in advance and failover to the replica during an outage. Cross-zone mounting of a file share can also introduce latency as data traverses zonal boundaries. | Regional. The file share exists in all 3 zones simultaneously, the clients connect to the closest instance of the share. When one zone becomes unavailable, data is still accessible. Storage traffic remains within the zone of the compute host. |
| Expandable capacity | Yes, up to 16,000 GB     | Yes, up to 32,000 GB |
| Adjustable IOPS     | Yes, up to 96,000. IOPS depends on capacity range. | No. Maximum IOPS is preset at 35,000.| 
| Adjustable Bandwidth| No. Bandwidth can be increased by increasing capacity and IOPS, up to 8192 Mbps.| Yes, bandwidth can be increased up to 8192 Mbps, and it can be reduced to the preset value that is based on the file share capacity. No capacity increase needed.|
| Customer-managed encryption at rest | Yes. | Yes. |
| Customer-managed encryption in transit | Yes. IPsec protocol with strongSwan. | Yes. TLS protocol with stunnel.|
| On-demand snapshots | Yes, up to 750 per share in a region. | Yes, up to 30 per share in a region. This quota can be increased upon request. |
| Scheduled snapshots | Yes, up to 750 snapshots per region. | Yes, Up to 30 per share in a region. This quota can be increased upon request. |
| Cross-zonal replication| Yes, as often as every 15 minutes. | Not applicable. Data is synchronously available in all zones of the region. |
| Cross-regional replication | Yes, as often as every 15 minutes. Cross-regional replication is not supported in Chennai. |  Not supported in the [Select availability]{: tag-green} release. |
| Cross-zonal mounting | Yes. | Not applicable. Data is synchronously available in all zones of the region. Storage traffic does not cross zone-boundaries. |
| Cross-account access | Yes. A share can have up to 100 accessor bindings. | Yes.|
| Monitoring integration with Sysdig | Yes. | Yes.|
{: caption="File share generations comparison." caption-side="bottom"}

First- and second-generation profiles in the defined performance profile family are not interchangeable. You can't convert a zonal file share to a regional share, or a regional share to a zonal share.
{: note}

For more information, see [About {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-file-storage-vpc-about).

## {{site.data.keyword.filestorage_vpc_short}} snapshots
{: #vpc-fs-snapshots-overview}

{{site.data.keyword.filestorage_vpc_short}} snapshot is a point-in-time copy of your file share that you create manually in the console, from the CLI, with the API, or Terraform. The initial snapshot is a full copy of the share. Subsequent snapshots of the same share are incremental; only those changes are captured that occurred since the last snapshot was taken. Snapshots inherit encryption from the source share.

You can create, list, view details, and manage snapshots in the console, from the CLI, and with the API or Terraform. You can use the snapshot to create another file share or to retrieve previous versions of files that are stored in the share.

Snapshots are tied to their source share. If you delete the original share and the snapshot is also deleted. However, you cannot delete a snapshot that is being used to hydrate a newly restored file.

Customers with special access to preview the second-generation File Storage offering can take snapshots of their regional file shares. The `rfs` profile is available in the Dallas, Frankfurt, London, Madrid, Osaka, Sao Paulo, Sydney, Toronto, Tokyo, and Washington, DC regions in this release.

| Features            | First-generation shares | Second-generation shares |
|---------------------|--------------------------|---------------------------|
| Availability        | Generally available in all VPC regions for all customers. | Available in most regions, except for Montreal, for allowlisted customers.|
| On-demand snapshots | Yes, Up to 750 per share in a region. | Yes, Up to 30 per share in a region. This quota can be increased upon request. |
| Scheduled snapshots | Yes, up to 750 snapshots per region. | Yes, Up to 30 per share in a region. This quota can be increased upon request.|
{: caption="File share snapshot generations comparison." caption-side="bottom"}

For more information, see [About {{site.data.keyword.filestorage_vpc_short}} snapshots](/docs/vpc?topic=vpc-fs-snapshots-about).

## Backup for VPC
{: #vpc-backup-serv-overview}

The {{site.data.keyword.cloud}} provides the means to create backup copies of your block storage volumes and file shares automatically. You can create a backup policy with up to 4 plans, and associate tags to the policy in the console, from the CLI, with the API or Terraform. 

The user-defined tags can be added to block storage volumes, file shares, and virtual server instances. When tags match, the backup policy is applied to the resources, and backup snapshots of the data are created based on the backup plan. You can set your own retention schedule to automatically delete older backups. This way, you can control how much space is used and how long backup snapshots are retained. By using Backup for VPC service, you can prevent data loss, manage risk, and improve data compliance.

Backup jobs that create or delete backup snapshots run according to the backup plan and the retention policy. You can view the status of the backup jobs in the console, from the CLI, with the API, or Terraform. If a job fails, the health status code shows the reason for the failure. You can also set up a connection to {{site.data.keyword.en_short}} and receive notifications to your preferred destinations.

For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

## Instance storage
{: #vpc-instance-storage-overview}

Instance storage is allocated from one or more local SSDs on the server that is hosting the instance. An instance storage disk provides fast, affordable, temporary storage to improve the performance of cloud native workloads with scratch space, caching buffers, or a place for replicated data.

Data that is stored on instance storage is tied directly to the instance lifecycle and is only held temporarily. The instance storage disk is automatically created and destroyed with the instance. However, instance storage data is not lost when an instance is rebooted. For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

## Storage for bare metal servers
{: #vpc-baremetal-storage-overview}

All profiles of {{site.data.keyword.bm_is_short}} provide one 0.96 TB SATA M.2 mirrored SSD as the boot disk. Profile `bx2d-metal-96x384` provides an extra set of NVMe (Non-Volatile Memory Express) U.2 solid-state drives (SSD) as secondary local storage. For more information, see the [Storage overview for Bare Metal Servers for VPC](/docs/vpc?topic=vpc-bare-metal-servers-storage).

## {{site.data.keyword.cos_full_notm}}
{: #intro-cos}

{{site.data.keyword.cos_full}} is a web-scale platform that stores unstructured data. It provides reliability, security, availability, and disaster recovery without replication. Information that is stored in {{site.data.keyword.cos_short}} is encrypted and dispersed across multiple geographic locations. It is accessible through the {{site.data.keyword.cloud}} console, {{site.data.keyword.cos_full_notm}} CLI, and API. For more information, see [About IBM Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage). 

Within the VPC environment, {{site.data.keyword.cos_short}} has many uses. For example, you can import and store [custom images](/docs/vpc?topic=vpc-custom-image-using-COS) for your compute instances. In addition, you need {{site.data.keyword.cos_short}} to collect and store [flow logs](/docs/vpc?topic=vpc-flow-logs) that summarize the network traffic between two virtual network interface cards (vNICs) within a certain time window.

## {{site.data.keyword.baas_full_notm}}
{: #vpc-backup-recovery-overview}

{{site.data.keyword.baas_full}} is a fully managed, agent-based, and application-consistent backup service that provides backup solutions for various {{site.data.keyword.cloud_notm}} services and customer workloads, which include {{site.data.keyword.vsi_is_short}}. For more information about the service, see [Getting started with Backup and Recovery](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery). To learn more about the differences between {{site.data.keyword.baas_full_notm}} and Backup for VPC, see the [Offering comparison chart](/docs/backup-recovery?topic=backup-recovery-baas_comp_chart).

## Next steps
{: #vpc-storage-next-steps}

* [Create block storage volumes](/docs/vpc?topic=vpc-creating-block-storage).
* [Create {{site.data.keyword.block_storage_is_short}} snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create).
* [Create file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create).
* [Create {{site.data.keyword.filestorage_vpc_short}} snapshots](/docs/vpc?topic=vpc-fs-snapshots-create).
* [Create backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan).
* [Manage instance storage](/docs/vpc?topic=vpc-instance-storage-provisioning).
* [Provision {{site.data.keyword.cos_short}}](/docs/cloud-object-storage?group=provisioning-storage)
* [Connecting to {{site.data.keyword.cos_full_notm}}](/docs/vpc?topic=vpc-connecting-vpc-cos)
