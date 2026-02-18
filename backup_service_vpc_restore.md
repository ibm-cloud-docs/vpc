---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-18"

keywords: Backup, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Restoring a volume from a backup snapshot
{: #baas-vpc-restore}

Restoring data from a backup snapshot creates a fully provisioned boot or data volume that you can use to boot an instance or attach as auxiliary storage. You can restore volumes during instance creation or when you want to create stand-alone boot or data volumes to be used later. You can restore a data volume to add more storage to an existing instance. You can use backup snapshots to restore volumes in a different region for business continuity purposes or geographic expansion. You can restore volumes from backup snapshots in the console, from the CLI, with the API, or Terraform.
{: shortdesc}

## About restoring a volume from a backup snapshot
{: #baas-vpc-restore-concepts}

Restoring a volume from a backup snapshot creates a boot or a data volume, depending on whether the snapshot is bootable or nonbootable.

   * Restoring from a **bootable** snapshot creates a boot volume that you can use to start a virtual server instance. The boot volume capacity is limited to 10-250 GB.

   * A new data volume that was created from **nonbootable** snapshot inherits its properties from the original volume, such as [profile](/docs/vpc?topic=vpc-block-storage-profiles), capacity, storage generation, data, and metadata. If the source volume used [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), the volume inherits that encryption with the original customer root key (CRK). However, you can specify a larger volume size, a different profile of the same storage generation, and a different CRK if you prefer.

First- and second-generation volume profiles are not interchangeable. You can use a backup of a second-generation volume to create another second-generation volume, but you can't switch the volume profile to a first-generation volume profile. In the same way, you can use a backup of a first-generation volume to create another first-generation volume with the same data, and you can't switch the new volume to the `sdp` profile.

In this release of second-generation storage volumes, consistency group snapshots are not supported.
{: note}

Restoring a volume from a backup snapshot creates a boot or data volume, depending on whether the snapshot is bootable or nonbootable. The volume appears first as _pending_ while it's being created. During the restoration, your data is copied from a regional storage repository to {{site.data.keyword.block_storage_is_short}}. After the volume is hydrated (fully provisioned), you can use the new boot or data volume. For more information about how performance is affected during restoration, see [Performance impact when backup snapshots are used](#baas-performance-considerations).

For best performance, use backups with fast restore. You can enable fast restore for backup snapshots in multiple zones and use them to restore a volume that is fully provisioned when the volume is created. The fast restore feature can achieve a [recovery time objective](#x3167918){: term} (RTO) quicker than restoring from a regular backup snapshot. For more information, see [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_fast_restore).

To restore a volume, the backup snapshot must be in a _stable_ state.
{: requirement}

All snapshots inherit their encryption type and key from the parent volume. When you create a volume from a first generation snapshot, you can either keep the encryption the same as the snapshot or change your keys. When the first-generation snapshot is protected with provider-managed keys, you can also opt to add customer managed keys. If your snapshot was protected with customer-managed keys, your new volume must also have customer-managed keys. When you create a volume from a second-generation snapshot, your volume must have the same encryption type (provider-managed or customer-managed) as the snapshot.

### Restoring from a bootable backup
{: #baas-restore-concept-boot}

When you restore from a bootable backup snapshot, you create a boot volume that you use to provision another instance.

Because the bootable backup snapshot is not fully provisioned, when you start your virtual server instance, the performance is slower than when you use a regular boot volume. For more information, see [Performance impact](#baas-boot-perf).

You can restore a volume from a backup snapshot of a boot volume in multiple ways.

- You can restore a boot volume when you provision an instance to start the new instance.
- You can restore a boot volume as a stand-alone volume, and use it later to start a new instance. The restoration process starts when the boot volume is attached to the instance.

### Restoring from a nonbootable backup
{: #baas-restore-concept-data}

You can restore a volume from a backup snapshot of a data volume in multiple ways.

- You can restore a data volume when you provision an instance. During provisioning, you can select a nonbootable backup snapshot to create a data volume that is then attached to the instance as auxiliary storage.
- You can restore a data volume when you want to add more storage to an existing instance.
- You can restore a data volume to create a stand-alone volume, which you can attach to an instance later.

## Restoring a volume from a backup snapshot in the console
{: #baas-vpc-restore-ui}
{: ui}

Restoring a volume from a backup snapshot in the console creates a fully provisioned boot or data volume. You can restore a volume from the list of Block Storage snapshots and from the snapshot details page.

You can restore a volume from a backup snapshot in the following ways.

- When you [provision an instance](#baas-vpc-restore-vol-ui), specify a snapshot of a boot or data volume. Data volumes are automatically attached to the instance as auxiliary storage. Use the restored boot volume to start the new instance.
- From a snapshot of a [previously created volume](#baas-vpc-create-from-vol-ui). The created volume from snapshot is automatically attached to the instance as auxiliary storage.
- From the [list of {{site.data.keyword.block_storage_is_short}} snapshots](#baas-vpc-restore-snapshot-list-ui), or [snapshot details page](#baas-vpc-restore-vol-details-ui).
- When you [create a stand-alone {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-standalone-vol-from-snapshot), you can attach the volume to an instance later.

### Creating a volume from the list of snapshots
{: #baas-vpc-restore-snapshot-list-ui}

From the list of {{site.data.keyword.block_storage_is_short}} snapshots, you can create a {{site.data.keyword.block_storage_is_short}} volume and specify whether it is attached to a virtual server instance or unattached (stand-alone). If you choose to attach a volume, you can select an existing virtual server instance or choose to create an instance. The new volumes are added to the [list of {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

1. Go to the list of {{site.data.keyword.block_storage_is_short}} snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **Storage > {{site.data.keyword.block_storage_is_short}} snapshots**.

2. Select a backup snapshot from the list. The **Created by** column shows which snapshots were created by a backup policy. Snapshots must be in a `stable` state for restoration.

3. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Create volume**.

4. The snapshot's details are displayed in the Create a volume from snapshot side panel. Select one of the three options:
   - **Do not attach**- By selecting this option, you start to create a stand-alone volume
   - **Attach new volume to an existing virtual server** - By choosing this option, you create a volume and select an existing server to attach it, too.
   - **Attach new volume to a new virtual server** - By choosing this option, you create a volume and a new virtual server instance at the same time.

5. After the selection is made, click **Configure volume**.{: #configure-volume}
   - If you chose the **Do not attach** or the **Attach new volume to an existing virtual server** options, you are taken to the Block storage volume for VPC provisioning page. The snapshot is preselected for you, and you must complete the rest of the volume details:
     1. Review the **Location** information. The geography, region, and zone are inherited from the VPC (for example, North America, Dallas, Dallas-1). You can select a different location by clicking the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"). 
     1. In the **Details** section, you must specify the name of the volume and the resource group that the volume is to be added to. Optionally, you can add user and access management tags.
        - Specify a meaningful name for your volume. For example, provide a name that describes your compute or workload function. The volume name must begin with a lowercase letter. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-). Volume names must be unique across the VPC. You can edit the name later.
        - Specify a [Resource group](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups).
        - Specify [user tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-user-tags) to organize your resources and for use by [backup policies](/docs/vpc?topic=vpc-backup-service-about).
        - Specify [access management tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-mgt-tags) that were created in IAM to help you manage access to your volumes.

     1. In the **Optional configurations** section, the snapshot is preselected for you. 
        - You can also apply a backup policy to the new volume if you want to. Click **Apply** to see available policies and plans.
        - Review the Attach to existing virtual server section. If you want to create a stand-alone volume, verify that the Do not attach box is checked. If you want to attach the volume to an instance, remove the check from the box and select an existing virtual server from the table.

     1. In the **Profile** section, you can specify the performance profile of your volume, its performance limits, and capacity. The storage generation value of the snapshot must match the storage generation value of the volume profile select.
         - You can select the [`sdp` profile](/docs/vpc?topic=vpc-block-storage-profiles#defined-performance-profile) if your snapshot was created from a Generation 2 volume. Then, specify the capacity of your volume, the requested bandwidth limit, and the required IOPS. Volume size can range from 1 - 32,000 GB. You can specify IOPS in the range of 100 - 64,000. The throughput limit range is 1000-8192 Mbps.
         - If your snapshot is from a Generation 1 volume, you can select one of the [_tiered_ profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#tiers). After you select `general-purpose`, `5iops-tier`, or `10iops-tier`, the next step is to specify the volume capacity. Volume size can be 10 - 16,000 GB. 
         - If your snapshot is from a Generation 1 volume and your performance requirements don't fall within any of the IOPS tiers, you can select the `custom` profile. Then, specify the size of your volume and the IOPS in the appropriate range for the volume capacity. As you type the IOPS value, the console shows the acceptable range. You can also click the **storage size** link to see the size and IOPS ranges of the [custom volume profile](/docs/vpc?topic=vpc-block-storage-profiles#custom).

     1. In the **Encryption at rest** section, you can choose to keep the encryption with IBM-managed keys that is enabled by default on all volumes. Or you can choose to use [your own encryption key](/docs/vpc?topic=vpc-block-storage-vpc-encryption) by selecting your key management service: {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. To locate your encryption key, select one of the following options:
        - **Locate by Instance**:
           1. Select the data encryption instance from the list. If you don't have an instance yet, you can click the link to create one.
           1. Select the data encryption key that is stored within the {{site.data.keyword.keymanagementserviceshort}} instance to use for encrypting the volume.
        - **Locate by CRN**: Enter the CRN of the customer root key to be used for encrypting the volume.

        If you selected a second-generation snapshot to create the volume, your volume's encryption type must match the snapshot's encryption type. If the snapshot has provider-managed keys, do not specify a key from a key management service.
        {: note}

     1. When you're finished, click **Create block storage volume**. 
        - If you're not ready to order yet or just looking for pricing information, you can add the information that you see in the side panel to an Estimate. For more information about how this feature works, see [Estimating your costs](/docs/account?topic=account-cost).
     1. The {{site.data.keyword.block_storage_is_short}} volumes page is shown, where a message indicates that the volume is being created (volume status is _pending_). A second message displays when the volume is created (volume status is _available_).
     1. To see details of the new volume, select the **View resource** link in the second message to go to the Volume details page.

     When you refresh the list of Block Storage volumes in the console, the new volume appears at the beginning of the list of volumes. For stand-alone volumes, the Attachment Type column is blank (-). The **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions") at the end of a table row provides a link for [attaching a Block Storage volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).

   - If you chose the **Attach new volume to an existing virtual server** option, you are taken to the Virtual server for VPC provisioning page. The snapshot is preselected for you, if it's s bootable snapshots, its data is used to create the boot volume. If it's a nonbootable snapshot, its data is used to create a data volume. You must complete the rest of the instance details as described in [Creating a virtual server instance with the UI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui).

Hover over the name of a volume that is attached to an instance in the instance details. The text shows whether the volume was created from a backup or regular snapshot.
{: tip}

### Creating a volume from the snapshot details page
{: #baas-vpc-restore-vol-details-ui}

Use the following steps to create a volume from the snapshot details page.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **Storage > {{site.data.keyword.block_storage_is_short}} snapshots**.

   You can go to the backup snapshots details page in other ways. For example, when you view a list of [backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs&interface=ui#backup-view-jobs-ui), you can click the snapshots link for a backup job and go to the details page.
   {: tip}

2. On the volume details page, select the **Snapshots and Backups** tab. A list of snapshots that were created by backup policies or manually is shown in the list. Cross-regional copies of snapshots are included in the list, too.

3. From the list, click the backup snapshot name to go to its details page.

4. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Create volume**.

5. The snapshot's details are displayed in the Create a volume from snapshot side panel. Select one of the three options:
   - **Do not attach**- By selecting this option, you start to create a stand-alone volume
   - **Attach new volume to an existing virtual server** - By choosing this option, you create a volume and select an existing server to attach it, too.
   - **Attach new volume to a new virtual server** - By choosing this option, you create a volume and a new virtual server instance at the same time.
6. After the selection is made, click **Configure volume**. The configuration steps are the same as when you create a volume from the [list of snapshots](#configure-volume).

### Creating a volume from a backup snapshot when you provision an instance
{: #baas-vpc-restore-vol-ui}

Follow these steps to create a boot and data volume from snapshots when you provision a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure ![VPC icon](../icons/vpc.svg) > Compute > Virtual server instances**.

2. Click **Create** and provision your new instance with the information from the table in [Creating virtual server instances in the console](/docs/vpc?topic=vpc-creating-virtual-servers).

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

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **Compute > Virtual server instances**.

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

The performance of boot and data volumes is initially degraded when data is restored from a Gen 1 snapshot. Performance degradation occurs during the restoration because your data is copied from the regional storage repository to {{site.data.keyword.block_storage_is_short}} in the background. After the restoration process is complete, you can realize full IOPS on the new volume.

### Performance impact when a bootable backup snapshot is used to provision an instance
{: #baas-boot-perf}

Before you select a backup snapshot of a boot volume to provision an instance, consider the following points.

- Because the bootable backup snapshot is not fully hydrated, initial performance is slower than compared to a regular boot volume. Volumes that are restored from fast restore clones do not require hydration. The data is available when the volume is created.

- To achieve the best performance and efficiency when multiple instances are provisioned, boot from an existing image. Custom images that you provide are better than stock images for this purpose. Images can have a maximum size of 250 GB.


## Next steps
{: #baas_vpc_restore_next_steps}

After you restore a volume, you can [Manage your backup policies](/docs/vpc?topic=vpc-backup-service-manage&interface=api).
