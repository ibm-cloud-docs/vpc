---

copyright:
  years: 2022
lastupdated: "2022-05-24"

keywords: 
subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:note: .note}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Restoring a volume from a backup snapshot
{: #baas-vpc-restore}

Restoring from a backup snapshot create a new, fully-provisioned boot or data volume. When you restore a backup snapshot of a boot volume, you create a new boot volume that you can use when you provision a new instance. Similarly, restoring from a backup of a data volume creates a secondary volume that's attached to the instance.
{: shortdesc}

## About restoring a volume from a backup snapshot
{: #baas-vpc-restore-concepts}

Restoring a volume from a backup snapshot creates a new boot or data volume. The new volume inherits properties from the original volume, but you can specify a larger volume size, different IOPS, and customer-managed encryption, if the source snapshot used provider-managed encryption. 

When you restore from a "bootable" backup snapshot, you create a boot volume that you can use when [provisioning a new instance](#baas-vpc-restore-vol-ui). The boot volume uses a general purpose profile and is limited to 250 GB. Because the bootable backup snapshot is not fully hydrated (that is, provisioned), performance will be slower than using a regular boot volume. For more information about this and other factors, see [Performance considerations when using a bootable backup snapshot to provision a new instance](#baas-boot-perf).

When you restore from a backup of a data volume, you can attach it to an instance when provisioning a new instance or to an existing instance. During the create and attach process, you select a backup snapshot of the volume to restore the volume. A new volume is then created and attached to the instance as secondary storage. The restored volume inherits the same [profile](/docs/vpc?topic=vpc-block-storage-profiles), capacity, data, and metadata as the original volume. You can choose a different profile and capacity if you prefer. If the source volume used [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), the volume inherits that encryption.

You can restore a volume from a backup snapshot from the [list of block storage snapshots](#baas-vpc-restore-ui), from the [snapshot details page](#baas-vpc-restore-vol-details-ui), or when you [provision a new instance](#baas-vpc-restore-vol-ui). You can also restore a volume from a snapshot to create a volume for [an existing instance](#baas-vpc-create-from-vol-ui).  

To restore a volume, it must be in a _stable_ state. When you restore, the service immediately creates the new volume. The volume appears first as _pending_ while it's being created. After the volume is hydrated (that is, fully provisioned), you can use the new boot volume to start a new instance or write data to the new attached data volume. For more performance considerations, see [Performance considerations when using backup snapshots]().

You can't simultaneously restore a boot and data volume.
{: note}

## Restore a volume from a backup snapshot with the UI
{: #baas-vpc-restore-ui}
{: ui}

### Create a volume from the list of block storage snapshots with the UI
{: #baas-vpc-restore-snaphot-list-ui}

1. Navigate to the list of block storage snapshots. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**.

2. Select a backup snapshot from the list. The **Created by** column shows which snapshots were created by a backup policy. Snapshots must be in a `stable` state for restoration.

3. From the overflow menu, select **Create volume**.

4. In the side panel, select whether you want to attach the volume to an existing instance or create a new instance. Table 1 describes the options on the side panel.

5. When finished, click **Save**. The new volume is created and attached to the instance.

| Field | Description |
|-------|-------------|
|**_Existing instance_** | Select **Attach new volume to an existing virtual server** |
| **Virtual server selection** | Select a virtual server from the dropdown menu. |
| **Volume details** | Define the new volume. |
| Name | Name of the new volume. |
| Resource group | Use default or select from the dropdown list. |
| Zone | Inherited from the snapshot. |
| Auto-deletion | Inherited from the snapshot. |
| **Profile** | Select IOPS tier or Custom. |
| IOPS | For IOPS tiers, specify IOPS tier profile. For custom IOPS, select a range. |
| Size | Enter a volume size allowed by the profile. The default is the minimum size required. |
| **Encryption** | Inherited from the snapshot. |
| **_New instance_** | Select **Attach new volume to a new virtual server** |
| **Configure virtual server** | Select this option to go to the Virtual Server provisioning page and [create the instance](/docs/vpc?topic=vpc-creating-virtual-servers) as usual. Note that the new volume is preselected and is shown in the **Boot volume** or **Data volume** section, depending on whether the volume you created was from a bootable snapshot or data snapshot. |
{: caption="Table 1. Create volume options caption-side="top"}

Hover over the name of a volume attached to an instance in the instance details. The text will tell you whether the volume was created from a backup or regular snapshot.
{: tip}

### Create a volume from the block storage details page with the UI
{: #baas-vpc-restore-vol-details-ui}

1. Navigate to the list of block storage volumes. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**. 

   There are other ways to view a backup snapshots details page. For example, when viewing a list of [backup jobs](), you can click the snapshots link for a backup job and navigate to the details page.
   {: tip}

2. Select a volume that you're backing up with a backup policy.

3. On the volume details page, select the **Snapshots and Backups** tab. A list of snapshots created by backup policies or manually is shown in the list.

4. Click the overflow menu for a backup snapshot from which you want to restore the volume and select **Create volume**.

5. In the side panel, select whether you want to attach the volume to an existing instance or create a new instance. Table 1 describes the options on the side panel.

6. When finished, click **Save**. The new volume is created and attached to the instance.

## Create a volume from a backup snapshot when you provision a new instance with the UI
{: #baas-vpc-restore-vol-ui}

Follow these steps to create a boot and data volume from snapshots when you provision a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**. 

2. Click **Create** and provision your new instance with the information from the table in [Creating virtual server instances in the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

3. For the operating system, click the tab for **Snaphots**. The most recent bootable snapshot is listed. 

4. Optionally, to select a different bootable snapshot and create a boot volume, click **Change**.

5. From the list of snapshots, select a bootable snapshot for your instance's operating system and click **Save**.

  This action imports snapshot data to the boot volume. The new boot volume is created from the snapshot and is listed under **Boot Volume** on the instance provisioning page.

6. To create a data volume, under Data Volumes, click **Create**. A side panel displays for creating a new data volume.

7. Select **Import from snapshot**. Expand the list to select a data snapshot. The most recent data snapshot is selected by default. Optionally, select a different snapshot from the list. The snapshot contains the properties of the original source volume, including the IOPS tier and encryption.

8. Optionally, change the size of the volume.

9. Click **Save**. The new data volume is created from the snapshot and attached to the instance, shown in the Data volume list. 

10. When you're finished provisioning your new instance, click **Create virtual server instance**. The new instance appears in the list of virtual server instances.

After the instance is created, you can click on the instance name to see the instance details. The boot volume you restored from the backup snapshot is listed under **Storage volumes**. The camera icon indicates the volume was created from a snapshot.

### Create a data volume from a backup snapshot for an existing virtual server instance with the UI
{: #baas-vpc-create-from-vol-ui}

You can create a data volume from a snapshot for an existing instance. Choose from the list of virtual server instances.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**. 

2. From the list, click on the name of an instance. The instance must be in a _Running_ state.

3. On the Instance details page, scroll to the list of Storage volumes and click **Attach volumes**. A side panel opens for you to define the volume attachment.

4. From the Attach data volume panel, expand the list of Block volumes and select **Create a data volume**.

5. Select **Import from snapshot**. Expand the Snapshot list and select a backup snapshot. 

6. Optionally, increase the size of the volume within the specified range.

7. Click **Save**. The side panel closes and messages indicate that the restored volume is being attached to the instance. 

The new volume appears in the list of Storage volumes. Hover over the camera icon to see the name of the backup snapshot from which it was created. 


## Restore a volume from a snapshot with the CLI
{: #baas-vpc-restore-CLI}
{: cli}

Use the CLI to create a boot or data volume from a backup snapshot. The commands are the same as creating a volume from a manually-created snapshot. For more information, see [Restore a volume from a snapshot with the CLI](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=cli#snapshots-vpc-restore-CLI). Be sure to [review the prerequisites](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=cli#snapshots-vpc-restore-prereq-CLI).

Also see the [VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference) for information about all backup service commands.

## Restore a volume from a backup snapshot with the API
{: #baas-vpc-restore-API}
{: api}

You can restore boot and data volumes from a backup snapshot with the VPC API. To begin, make a `GET /snapshots` request to see a list of snapshots. You can filter by resource group and source volume ID in the request if you like.

To restore a boot volume from a bootable backup snapshot, specify the boot volume attachment and snapshot ID in the `POST /instances` request.

To restore a data volume from a backup snapshot and attach it at boot time, specify the data volume attachment and snapshot ID in the `POST /instances` request.

These are the same API requests as restoring from a manually-created snapshot. For more information and examples, see [Restore a volume from a snapshot with the API](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-API).  Also see the [VPC API reference](/apidocs/vpc/latest) for information about using the API and all backup service API methods.

## Performance considerations
{: #baas-performance-considerations}

Restoring boot and data volumes from a backup snapshot have the following performance implications:

### Performance considerations when using restoring a volume from a backup snapshot
{: #baas-vol-perf}

Boot and data volume performance is initially degraded when restoring from a snapshot. During the restoration, your data is copied from IBM COS to VPC Block Storage. After the restoration process has completed, you should realize full IOPS on your new volume.

### Performance considerations when using a bootable backup snapshot to provision a new instance
{: #baas-boot-perf}

Before you select a backup snapshot of a boot volume to provision a new instance, keep in mind these performance considerations:

* Because the bootable backup snapshot is not fully hydrated, performance will be slower than using a regular boot volume.

* For disaster recovery, it's better to restore from a manually-created snapshot than from a backup snapshot. Your volume will be created sooner.

* To achieve the best performance and efficiency when provisioning multiple instances, boot from an existing image. Custom images you provide are better than stock images for this purpose. Images can have a maximum size of 250 GB.

## Next Steps
{: #baas_vpc_restore_next_steps}

[Manage your backup policies](/docs/vpc?topic=vpc-backup-service-manage&interface=api).
