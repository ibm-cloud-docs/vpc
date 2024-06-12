---

copyright:
  years: 2022, 2024
lastupdated: "2024-03-07"

keywords: Backup, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Restoring a volume from a backup snapshot
{: #baas-vpc-restore}

Restoring from a backup snapshot creates a fully provisioned boot or data volume that you can use to boot an instance or attach as auxiliary storage. You can restore volumes during instance creation or when you want to create stand-alone boot or data volumes to be used later. You can restore a data volume to add more storage to an existing instance. You can use backup snapshots to restore volumes in a different region for business continuity purposes or geographic expansion. You can restore volumes from backup snapshots in the UI, from the CLI, with the API, or Terraform.
{: shortdesc}

## About restoring a volume from a backup snapshot
{: #baas-vpc-restore-concepts}

When you restore a volume from a backup, the service creates another volume. The restored volume inherits the same [profile](/docs/vpc?topic=vpc-block-storage-profiles), capacity, data, and metadata as the original volume. However, you can choose a different profile and capacity if you prefer. If the source volume used [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), the new volume inherits that encryption.

Restoring a volume from a backup snapshot creates a boot or data volume, depending on whether the snapshot is bootable or nonbootable. The volume appears first as _pending_ while it's being created. During the restoration, your data is copied from {{site.data.keyword.cos_full}} to {{site.data.keyword.block_storage_is_short}}. After the volume is hydrated (fully provisioned), you can use the new boot or data volume. For more information about how performance is affected during restoration, see [Performance impact when backup snapshots are used](#baas-performance-considerations).

For best performance, use backups with fast restore. You can enable fast restore for backup snapshots in multiple zones and use them to restore a volume that is fully provisioned when the volume is created. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular backup snapshot. For more information, see [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_fast_restore).

To restore a volume, the backup snapshot must be in a _stable_ state.
{: requirement}

You can't simultaneously restore a boot and a data volume.
{: note}

### Restoring from a bootable backup
{: #baas-restore-concept-boot}

When you restore from a bootable backup snapshot, you create a boot volume that you use to provision another instance. The boot volume uses a general-purpose profile and is limited to 250 GB. Because the bootable backup snapshot is not fully provisioned, in the beginning the performance is slower than when you use a regular boot volume. For more information, see [Performance impact](#baas-boot-perf).

You can restore a volume from a backup snapshot of a boot volume in multiple ways.

- You can restore a boot volume when you provision an instance to start the new instance.
- You can restore a boot volume as a stand-alone volume, and use it later to start a new instance. The restoration process starts when the boot volume is attached to the instance.

### Restoring from a nonbootable backup
{: #baas-restore-concept-data}

You can restore a volume from a backup snapshot of a data volume in multiple ways.

- You can restore a data volume when you provision an instance. During provisioning, you can select a nonbootable backup snapshot to create a data volume hat is then attached to the instance as auxiliary storage.
- You can restore a data volume when you want to add more storage to an existing instance.
- You can restore a data volume to create a stand-alone volume, which you can attach to an instance later.

## Restoring a volume from a backup snapshot in the UI
{: #baas-vpc-restore-ui}
{: ui}

Restoring a volume from a backup snapshot in the UI creates a fully provisioned boot or data volume. You can restore a volume from the list of Block Storage snapshots and from the snapshot details page.

You can restore a volume from a backup snapshot in the following ways.

- When you [provision an instance](#baas-vpc-restore-vol-ui), specify a snapshot of a boot or data volume. Data volumes are automatically attached to the instance as auxiliary storage. Use the restored boot volume to start the new instance.
- From a snapshot of a [previously created volume](#baas-vpc-create-from-vol-ui). The created volume from snapshot is automatically attached to the instance as auxiliary storage.
- From the [list of {{site.data.keyword.block_storage_is_short}} snapshots](#baas-vpc-restore-snaphot-list-ui) or [snapshot details page](#baas-vpc-restore-vol-details-ui).
- When you [create a stand-alone {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-vol-from-snapshot-ui), you can attach the volume to an instance later.

### Creating a volume from the list of snapshots
{: #baas-vpc-restore-snaphot-list-ui}

From the list of {{site.data.keyword.block_storage_is_short}} snapshots, you can create a {{site.data.keyword.block_storage_is_short}} volume and specify whether it is attached to a virtual server instance or unattached (stand-alone). If you choose to attach a volume, you can select an existing virtual server instance or choose to create an instance. The new volumes are added to the [list of {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

1. Go to the list of {{site.data.keyword.block_storage_is_short}} snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **Storage > {{site.data.keyword.block_storage_is_short}} snapshots**.

2. Select a backup snapshot from the list. The **Created by** column shows which snapshots were created by a backup policy. Snapshots must be in a `stable` state for restoration.

3. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Create volume**.

4. In the side panel, choose whether you want to attach a data volume to an existing instance, a boot volume to a new instance, or leave either volume unattached.

   - To create a stand-alone (unattached) volume, leave **Attach new volume to virtual server** clear and specify the volume details that are shown in Table 1.
   - To attach a data volume to an existing instance, select **Attach volume to virtual server** and provide zone and virtual server instance.
   - To restore a boot volume, select **Attach volume to virtual server** and click **Configure virtual server**, which takes you to the [virtual server provisioning page](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui).

   The new volume is preselected and is shown in the **Boot volume** or **Data volume** section of the instance provisioning page. Placement depends on whether the volume that you created was from a bootable snapshot or a data snapshot.
   {: note}

   Table 1 describes the options on the side panel.

   | Field | Description |
   |-------|-------------|
   | Attach a new volume to a new virtual server. | This option is cleared by default, it creates an unattached volume. When the option is selected, you can either attach the volume to an existing instance, or to a new instance that you must create. |
   | **Virtual server selection** | The option displays when you choose to attach the volume to a new or existing instance. For an existing instance: |
   | - Zones | Select a zone within your region from the menu. |
   | - Virtual server | Select a virtual server instance from the list. |
   | **Volume details** | Define the volume (attached or unattached). |
   | - Name | Enter a name for the volume. By default, the snapshot name is used and appended with _restore_. |
   | - Resource group | Use default or select from the list of available groups. |
   | - Zone | Inherited from the snapshot. |
   | - Auto-delete | Inherited from the snapshot. |
   | **Profile** | Defaults to the snapshot's IOPS tier or custom profile. You can change the profile. |
   | - IOPS | For IOPS tiers, specify an IOPS tier profile. For custom IOPS, select a range. |
   | - Size | Enter a volume size allowed by the profile. The default is the minimum provisioning size based on the snapshot. |
   | **Encryption** | Inherited from the snapshot. If the encryption is provider-managed, you can change it to customer-managed encryption and select either {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. If the encryption is already customer-managed, you can choose a different key management service but you can't change to provider-managed encryption. |
   {: caption="Table 1. Create {{site.data.keyword.block_storage_is_short}} volume options." caption-side="bottom"}

5. When you're finished, click **Save**. The new volume is created.

Hover over the name of a volume that is attached to an instance in the instance details. The text shows whether the volume was created from a backup or regular snapshot.
{: tip}

### Creating a volume from the snapshot details page
{: #baas-vpc-restore-vol-details-ui}

Use the following steps to create a volume from the snapshot details page.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **Storage > {{site.data.keyword.block_storage_is_short}} snapshots**.

   You can go to the backup snapshots details page in other ways. For example, when you view a list of [backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs&interface=ui#backup-view-jobs-ui), you can click the snapshots link for a backup job and go to the details page.
   {: tip}

2. On the volume details page, select the **Snapshots and Backups** tab. A list of snapshots that were created by backup policies or manually is shown in the list. Cross-regional copies of snapshots are included in the list, too.

3. From the list, click the backup snapshot name to go to its details page.

4. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Create volume**.

5. Define the new volume in the side panel. The information is the same as when you create a volume from the [list of snapshots](#baas-vpc-restore-snaphot-list-ui). Table 1 describes the options on the side panel.

6. When you're finished, click **Save**. The new volume is created.

### Creating a volume from a backup snapshot when you provision an instance
{: #baas-vpc-restore-vol-ui}

Follow these steps to create a boot and data volume from snapshots when you provision a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure ![VPC icon](../icons/vpc.svg) > Compute > Virtual server instances**.

2. Click **Create** and provision your new instance with the information from the table in [Creating virtual server instances in the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

3. For the boot volume with the operating system, click the tab for **Snapshots**.
   1. The most recent bootable snapshot is listed. To select a different bootable snapshot and create a boot volume, click **Change**.
   2. From the list of snapshots, select a bootable snapshot for your instance's operating system and click **Save**.
      This action imports snapshot data to the boot volume. The new boot volume is created from the snapshot and is listed under **Boot Volume** on the instance provisioning page.

4. To create a data volume, under Data Volumes, click **Create**. A side panel displays for creating a new data volume.
   1. Select **Import from snapshot**. Expand the list to select a data snapshot. The most recent data snapshot is selected by default. Optionally, select a different snapshot from the list. The snapshot contains the properties of the original source volume, including the IOPS tier and encryption.
   2. Optionally, change the size of the volume.
   3. Click **Save**. The new data volume is created from the snapshot. It is attached to the instance, and shown in the Data volume list.

5. When you're finished provisioning your new instance, click **Create virtual server instance**. The new instance appears in the list of virtual server instances.

After the instance is created, you can click the instance name to see the instance details. The boot volume that you restored from the backup snapshot is listed under **Storage volumes**. The camera icon indicates that the volume was created from a snapshot.

### Creating a data volume from a backup snapshot for an existing virtual server instance
{: #baas-vpc-create-from-vol-ui}

You can create a data volume from a snapshot for an existing instance. Choose from the list of virtual server instances.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) Compute > Virtual server instances**.

2. From the list, click the name of an instance. The instance must be in a _Running_ state.

3. On the Instance details page, scroll to the list of Storage volumes and click **Attach volumes**. A side panel opens for you to define the volume attachment.

4. From the Attach data volume panel, expand the list of Block volumes, and select **Create a data volume**.

5. Select **Import from snapshot**. Expand the Snapshot list and select a backup snapshot.

6. Optionally, increase the size of the volume within the specified range.

7. Click **Save**. The side panel closes and messages indicate that the restored volume is being attached to the instance.

The new volume appears in the list of Storage volumes. Hover over the camera icon to see the name of the backup snapshot from which it was created.

## Restoring a volume from a snapshot from the CLI
{: #baas-vpc-restore-CLI}
{: cli}

Use the CLI to create a boot or data volume from a backup snapshot. The commands are the same as the ones that are used to restore a volume from a manually created snapshot. For more information, see [Restore a volume from a snapshot with the CLI](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=cli#snapshots-vpc-restore-CLI).

For more information about all backup service commands, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference&interface=cli).

## Restoring a volume from a backup snapshot with the API
{: #baas-vpc-restore-API}
{: api}

You can restore boot and data volumes from a backup snapshot with the VPC API. To begin, make a `GET /snapshots` request to see a list of snapshots. You can filter by resource group and source volume ID in the request.

- To restore a boot volume from a bootable backup snapshot, specify the boot volume attachment and snapshot ID in the `POST /instances` request.

- To restore a data volume from a backup snapshot and attach it, specify the data volume attachment and snapshot ID in the `POST /instances` request.

- To restore a stand-alone volume from a snapshot, specify the snapshot ID in the `POST /volumes` request.

The API requests are the same as the one you use to restore from a manually created snapshot. For more information, see [Restore a volume from a snapshot with the API](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-API).

For more information about using the API and all backup service API methods, see [VPC API reference](/apidocs/vpc/latest).

## Restoring a volume from a snapshot with Terraform
{: #baas-vpc-restore-terraform}
{: terraform}

You can create a boot or data volume from a backup snapshot with Terraform. The resource, arguments, and attributes are the same as the ones that are used to restore a volume from a manually created snapshot. For more information, see [Restore a volume from a snapshot with Terraform](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=terraform#snapshots-vpc-restore-terraform).

For more information about the Terraform resources, arguments, and attributes that are used for restoring a volume, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

## Restoring volumes of a virtual server from a consistency group
{: #baas-vpc-restore-instance}

You can use the members of the snapshot consistency group to restore volumes separately. Restoring an instance directly from snapshot consistency group identifier is not supported.

For more information, see [Creating volumes for a virtual server instance from a consistency group](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-restore-cr-details-ui).{: ui}

## Performance impact
{: #baas-performance-considerations}

The performance of boot and data volumes is initially degraded when data is restored from a snapshot. Performance degradation occurs during the restoration because your data is copied from {{site.data.keyword.cos_full}} to {{site.data.keyword.block_storage_is_short}} in the background. After the restoration process is complete, you can realize full IOPS on the new volume.

### Performance impact when a bootable backup snapshot is used to provision an instance
{: #baas-boot-perf}

Before you select a backup snapshot of a boot volume to provision an instance, consider the following points.

- Because the bootable backup snapshot is not fully hydrated, initial performance is slower than compared to a regular boot volume. Volumes that are restored from fast restore clones do not require hydration. The data is available when the volume is created.

- To achieve the best performance and efficiency when multiple instances are provisioned, boot from an existing image. Custom images that you provide are better than stock images for this purpose. Images can have a maximum size of 250 GB.

## Next steps
{: #baas_vpc_restore_next_steps}

After you restore a volume, you can [Manage your backup policies](/docs/vpc?topic=vpc-backup-service-manage&interface=api).
