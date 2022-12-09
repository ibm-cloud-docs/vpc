---

copyright:
  years: 2021, 2022
lastupdated: "2022-12-09"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Restoring a volume from a snapshot
{: #snapshots-vpc-restore}

Restoring from a snapshot creates a new, fully-provisioned boot or data volume that you can use to boot an instance or attach as secondary storage. You can restore boot and data volumes during instance creation or by modifying an existing instance. You can also restore a data volume from a snapshot of a volume not attached to an instance. Volumes can be restored from snapshots created manually or by a backup policy.
{: shortdesc}

## About restoring a volume from a snapshot
{: #snapshots-vpc-restore-concepts}

Restoring a volume from a snapshot creates a new boot or data volume, depending on whether the snapshot is "bootable" or "nonbootable". The new volume created from the snapshot inherits properties from the original volume, but you can specify a larger volume size, different IOPS profile, and customer-managed encryption.

You can restore a volume from a snapshot in the following ways:

* When [provisioning a new instance](#snapshots-vpc-restore-vol-ui), specify a snapshot of a boot or data volume. Data volumes are automatically attached to the instance as secondary storage. Use the restored boot volume to start the new instance.
* From a snapshot of a [previously created volume](#snapshots-vpc-create-from-vol-ui). The created volume from snapshot is automatically attached to the instance as secondary storage.
* From the [list of block storage snapshots](#snapshots-vpc-restore-snaphot-list-ui) or [snapshot details page](#snapshots-vpc-restore-vol-details-ui). The volume from which the snapshot was created does not have to be attached to an instance.
* When you [restore a new unattached (stand-alone) block storage volume](#snapshots-vpc-restore-unattached-vol) from a snapshot, from the UI, CLI, or from the `volumes` API. You can later attach the data volume to an instance.

While a volume is being restored, its health state is monitored by the volume resource. If the volume created from snapshot is attached to an instance before it's data is fully restored, volume hydration pauses on service node and continues on compute node. If the volume is detached while hydration is in progress, the hydration pauses on compute node and continues on storage node. You cannot delete a snapshot that is being used to hydrate a volume.

### Restore a volume during instance provisioning or update
{: #snapshots-vpc-restore-vsi-concepts}

You can use the UI, CLI, or API to restore a boot and data volumes during instance provisioning or when modifying an instance.

During the create and attach process of instance provisioning, you select a snapshot of the volume to restore the volume. Data volumes are created and attached to the instance.

Restoring from a bootable snapshot creates a boot volume that you use to provision a new instance. The boot volume uses a general purpose profile and is limited to 250 GB. Note that when you provision an instance using a bootable snapshot, performance might be slower until the boot volume is fully provisioned.

### Restore a stand-alone data volume from a snapshot
{: #snapshots-vpc-restore-unattached-vol}

You can use the UI, CLI, or API to create a new stand-alone volume from a snapshot. The volume does not have to be attached to an instance, but you can attach it later at which time the volume data is restored.

Use this option when you're not sure which virtual server instance you want to attach the volume to. Or, if you want to  restore data from an unattached volume that was detached from an instance.

Create the volume from a snapshot from the [list of block storage snapshots](#snapshots-vpc-restore-snaphot-list-ui), from the [snapshot details page](#snapshots-vpc-restore-vol-details-ui), or when you [create a new unattached block storage volume](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-vol-from-snapshot-ui) from a snapshot.

With the VPC API, you can make a `POST /volumes` call and specify the snapshot ID when creating a data volume.
Volume performance is initially degraded until the volume data is fully restored. For an example API call, see [Restoring a stand-alone data volume from a snapshot](#snapshots-vpc-restore-unattached-api). Also see the [limitations](#snapshots-vpc-restore-limitations) when restoring a stand-alone volume from a snapshot.

### Additional considerations
{: #snapshots-vpc-restore-considerations}

The restored volume inherits the same [profile](/docs/vpc?topic=vpc-block-storage-profiles), capacity, data, and metadata as the original volume. You can choose a different profile and capacity if you prefer. If the source volume used [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), the volume inherits that encryption.

To restore a volume, it must be in a _stable_ state. When you restore from a snapshot attached to an instance, the service immediately creates the new volume. The volume appears first as _pending_ while it's being created. After the volume is hydrated, you can use the new boot volume to start a new instance or write data to the new attached data volume. [Performance](#snapshots-performance-considerations) is initially degraded by this operation.

You can also restore a volume from a snapshot created by a backup policy. This type of snapshot is called a backup. When you restore a volume from a backup, tags are inherited from the source volume. When any of these tags match a tag in the backup policy, a backup is created. For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).

## Limitations when restoring a volume from a snapshot
{: #snapshots-vpc-restore-limitations}

The following limitations apply when restoring a volume from a snapshot.

* A boot volume created from a snapshot can't be attached as a secondary volume.
* Restoring a boot volume with the API is not supported.
* When the new volume is created, data restoration begins immediately but [performance is degraded](#snapshots-performance-considerations) until the volume is fully hydrated.
* You can delete the new volume at any time, including the time before it is fully hydrated. However, you can't delete the snapshot from which the volume is restored unless the hydration is completed or the volume is deleted, whichever happens first.
* The parent snapshot is used to restore the volume.
* If snapshot is encrypted and you don't specify a different root key CRN, the new volume is encrypted with the snapshot's encryption key.

## Restore a volume from a snapshot with the UI
{: #snapshots-vpc-restore-ui}
{: ui}

Restoring a volume from a snapshot with the UI creates a new, fully-provisioned boot or data volume.

### Create a volume from the list of snapshots with the UI
{: #snapshots-vpc-restore-snaphot-list-ui}

From the list of block storage snapshots, you can create a new block storage volume and specify whether it's attached to a virtual server instance or unattached (stand-alone). If you choose to attach a data volume, you can select an existing virtual server instance or choose to create a new instance. The new volumes are added to the [list of block storage volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

1. Navigate to the list of block storage snapshots. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](/login), go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**.

2. Select a snapshot from the list. It must be in a `stable` state.

3. From the overflow menu, select **Create volume**.

4. In the side panel, choose whether you want to attach a data volume to an existing instance, restore a boot volume and create a new instance, or create an unattached data volume.

    * For an unattached data volume, leave **Attach volume to virtual server** unselected and provide the volume details in Table 1.
    * When attaching a data volume to an existing instance, select **Attach volume to virtual server** and provide zone and virtual server instance.
    * When restoring a boot volume, select **Attach volume to virtual server** and click **Configure virtual server**, which takes you to the [virtual server provisioning page](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui).

    Note that the new volume is preselected and is shown in the **Boot volume** or **Data volume** section of the instance provisioning page, depending on whether the volume you created was from a bootable snapshot or data snapshot.

5. When finished, click **Save**. The new volume is created and attached to the instance.

| Field | Description |
|-------|-------------|
| Attach volume to virtual server | Unselected by default, creates an unattached volume. When selected, you can either attach the volume to an existing instance or to a new instance you must create. |
| **Virtual server selection** | Displays when you choose to attach the volume to a new or existing instance. For an existing instance: |
| Zones | Select a zone within your region from the dropdown menu. |
| Virtual server | Select a virtual server instance from the dropdown list. |
| **Volume details** | Define the new volume (attached or unattached). |
| Name | Enter a name for the new volume. By default, the snapshot name is used and appended with _restore_. |
| Resource group | Use default or select from the dropdown list. |
| Zone | Inherited from the snapshot. |
| Auto-delete | Inherited from the snapshot. |
| Size | Enter a volume size allowed by the profile. The default is the minimum provisioning size based on the snapshot. |
| **Profile** | Defaults to the snapshot's IOPS tier or custom profile. You can change the profile. |
| IOPS | For IOPS tiers, specify IOPS tier profile. For custom IOPS, select a range. |
| Size | Enter a volume size allowed by the profile. The default is the minimum size required. |
| **Encryption** | Inherited from the snapshot. If the encryption is provider-managed, you can change it to customer-managed encryption and select either Key Protect or HPCS. If the encryption is already customer-managed, you can choose a different key management service but not to provider-managed encryption. |
{: caption="Table 1. Create volume options caption-side="top"}

Hover over the name of a volume attached to an instance in the instance details. The text tells you whether the volume was created from a snapshot.
{: tip}

### Create a volume from the snapshot details page with the UI
{: #snapshots-vpc-restore-vol-details-ui}

1. Navigate to the list of block storage volumes and select a volume. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.

2. On the block storage volume details page, select the **Snapshots and Backups** tab. A list of snapshots created by manually or by backup policies is shown in the list.

3. From the list, click on the  snapshot name to go to its details page.

4. From the **Actions** menu, click **Create volume**.

5. Define the new volume in the side panel. The information is the same as when you create a volume from the [list of snapshots](#snapshots-vpc-restore-snaphot-list-ui). Table 1 describes the options on the side panel.

6. When finished, click **Save**. The new volume is created and attached to the instance.

### Create a volume from a snapshot when you provision a new instance with the UI
{: #snapshots-vpc-restore-vol-ui}

Follow these steps to create a boot and data volume from snapshots when you provision a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

   Be sure to select VPC infrastructure from the menu icon.
   {: tip}

2. Click **Create** and provision your new instance with the information from the table in [Creating virtual server instances in the UI](/docs/vpc?topic=vpc-creating-virtual-servers).

3. For the operating system, click the tab for **Snaphots**. The most recent bootable snapshot is listed.

4. Optionally, to select a different bootable snapshot and create a boot volume, click **Change**.

5. From the list of snapshots, select a bootable snapshot for your instance's operating system and click **Save**.

   This action imports snapshot data to the boot volume. The new boot volume is created from the snapshot and is listed under Boot Volume on the instance provisioning page.

6. To create a data volume, under Data Volumes, click **Create**. A side panel displays for creating a new data volume.

7. Select **Import from snapshot**. Expand the list to select a data snapshot. The most recent data snapshot is selected by default. Optionally, select a different snapshot from the list. The snapshot contains the properties of the original source volume, including the IOPS tier and encryption.

8. Optionally, change the size of the volume.

9. Click **Save**. The new data volume is created from the snapshot and attached to the instance, shown in the Data volume list.

10. Optionally, to see the API POST request for restoring the volume, click **Get sample API call**. Information displays for the call, similar to the example in [this section](#snapshots-vpc-restore-API).

11. When you're finished provisioning your new instance, click **Create virtual server instance**. The new instance appears in the list of virtual server instances.

After the instance is created, you can click on the instance name to see the instance details. The boot volume you restored from a snapshot is listed under **Storage volumes**. The camera icon indicates the volume was created from a snapshot.

### Create a data volume from a snapshot for an existing virtual server instance with the UI
{: #snapshots-vpc-create-from-vol-ui}

You can also create a data volume from a snapshot for an existing instance. Choose from the list of virtual server instances.

1. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

2. From the list, click on the name of an instance. The instance must be in a _Running_ state.

3. On the Instance details page, scroll to the list of Storage volumes and click **Attach volumes**. A side panel opens for you to define the volume attachment.

4. From the Attach data volume panel, expand the list of Block volumes and select **Create a data volume**.

5. Select **Import from snapshot**. Expand the Snapshot list and select a snapshot.

6. Optionally, increase the size of the volume within the specified range.

7. Click **Save**. The side panel closes and messages indicate that the restored volume is being attached to the instance.

The new volume appears in the list of Storage volumes. Hover over the camera icon to see the name of the snapshot from which it was created.

## Restore a volume from a snapshot with the CLI
{: #snapshots-vpc-restore-CLI}
{: cli}

Restoring a volume from a snapshot with the CLI creates a new, fully-provisioned boot or data volume.

### Before you begin
{: #snapshots-vpc-restore-prereq-CLI}

1. Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

2. To use the CLI, set the `IBMCLOUD_IS_FEATURE_SNAPSHOT` environment variable to `true`. Copy the following code:

   ```zsh
   export IBMCLOUD_IS_FEATURE_SNAPSHOT=true
   ```
   {: pre}

3. After you install the vpc-infrastructure plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

4. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-cli#create-a-vpc-cli).

### Create a boot volume from a snapshot for a new instance from the CLI
{: #snapshots-vpc-restore-boot-CLI}

Run the `ibmcloud is instance-create` command with the `source_snapshot` parameter and ID or name of a bootable snapshot. The restored boot volume is used to initialize the instance.

For example:

```zsh
bmcloud is instance-create my-instance-restore1 ea002578-ff10-41fe-9652-e63f7e0e3cba us-south-1 bx2-2x8 ba11a6f2-6c17-4fee-a4b5-5c016fe64376 --boot-volume
'{
   "name":"boot-from-snapshot1",
   "volume":{
      "name":"boot-from-snapshot1",
      "profile":{
         "name":"general-purpose"
      },
      "source_snapshot":{
         "id":"d857c69f-d795-46ac-85e4-f26ca3001033"
      }
   }
}'
```
{: codeblock}

```zsh
Creating instance my-instance-restore1 in resource group under account VP01 as user rtuser1@mycompany.com...

ID               7101_eded6dcd-4f3c-4e79-a0cb-00f7c72f38cd
Name             my-instance-restore1
CRN              crn:v1:staging:public:is:us-south-1/2d1bace7b46e4815a81e52c6ffeba5cf
                 ::instance:7101_eded6dcd-4f3c-4e79-a0cb-00f7c72f38cd
Status           pending
Profile          bx2-2x8
Architecture     amd64
vCPUs            2
Memory           8
Network(Gbps)    4
Image            ID                                         Name
                 6f153c4d-6a9a-496d-8063-5c39932f6ded       ibm-centos-7-6-minimal-amd64-2
VPC              ID                                         Name
                 ea002578-ff10-41fe-9652-e63f7e0e3cba       my-vpc
Zone             us-south-1
Resource group   ID                                         Name
                 cdc21b72d4f557b195de988b175e3d81           Default

Created          2022-06-14T17:03:30+08:00
Boot volume      ID                                   Name                  Attachment ID                            Attachment name
                 0651dacb-4589-4147-86b3-a77544598f93 boot-from-snapshot1                  7101-abf9dd2b-9d5d-41f1-849d-55a8ab580ddb                            boot-from-snapshot1

```
{: screen}

### Create a data volume from a snapshot for a new instance from the CLI
{: #snapshots-vpc-restore-data-CLI}

When you create an instance by using the `ibmcloud is instance-create` command, specify the `source_snapshot` parameter and snapshot name or ID in the volume attachment.

For example:

```zsh
ibmcloud is instance-create my-instance-restore1 ea002578-ff10-41fe-9652-e63f7e0e3cba us-south-1 bx2-2x8 ba11a6f2-6c17-4fee-a4b5-5c016fe64376 --volume-attach
'{
   "name":"datavol-from-snapshot",
   "volume":{
      "name":"datavol-from-snapshot",
      "profile":{
         "name":"general-purpose"
      },
      "source_snapshot":{
         "id":"6daaaa39-3d81-4d1d-81f8-e1f6a14f97f3"
      }
   }
}'
.
.
.
```
{: codeblock}

### Create a data volume from a snapshot for an existing instance from the CLI
{: #snapshots-vpc-restore-data-inst-cli}

For an existing instance, specify the `ibmcloud is instance-volume-attachment-add` command with the `source-snapshot` parameter, and the name or ID of the snapshot. To find the ID of the snapshot from the CLI, see [View snapshots from the CLI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-cli).

For example:

```zsh
ibmcloud is instance-volume-attachment-add data-vol-1 a67f49de-fccc-4e5c-824e-dcbd06d009af --profile general-purpose --source-snapshot 52de6e85-7068-4247-90fd-d2fa91fd9864
```
{: codeblock}

### Create a stand-alone volume from the snapshots details from the CLI
{: #snapshots-restore-create-vol-cli}

You can create a stand-alone block storage data volume from a snapshot from the CLI. The volume is not attached to a virtual server instance. When you run the `ibmcloud is volume` command, the response shows the attachment type as `unattached`. You can attach the volume to an instance later.

Run the `ibmcloud is volume-create` command and specify the `snapshot` parameter and name or ID of the snapshot. For more information, see [Create an stand-alone block storage volume from a snapshot](/docs/vpc?topic=vpc-creating-block-storage&interface=cli#create-vol-from-snapshot-cli).

## Restore a volume from a snapshot with the API
{: #snapshots-vpc-restore-API}
{: api}

Use the VPC API to restore a volume from a snapshot. Restoring creates a new, fully provisioned volume.

### Restore a boot volume when creating a new instance with the API
{: #snapshots-vpc-restore-boot-api}

To restore a boot volume from a bootable snapshot, make a `POST /instances` request and specify the boot volume attachment and snapshot ID. For example:

```curl
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2022-12-09&generation=2" \
-H "Authorization: $iam_token" \
-H "Content-Type: application/json" \
d '{
      "name": "my-server-name",
      "zone": {
         "name": "us-south-1"
      },
      "vpc": {
         "id": "4d27c489-8ad7-3c18-cbf4-2103d9f8da93"
      },
      "profile": {
         "name": "cx2-2x4"
      },
      "primary_network_interface": {
        "name": "region1example-net1",
        "subnet": {
           "id": ""
        }
      },
      "boot_volume_attachment": {
        "delete_volume_on_instance_delete": true,
        "volume": {
            "profile": {
                "name": "general-purpose"
            },
            "source_snapshot": {
                "id": "eb373975-4171-4d91-81d2-c49efb033753"
            }
        }
     },
    "resource_group": {
        "id": "2fab2c7f-c09d-4c64-baf7-1453b7461493"
    }
  }'
```
{: codeblock}

### Restore a data volume when creating a new instance with the API
{: #snapshots-vpc-restore-data-api}

To restore a data volume from a snapshot and attach it at boot time, make a `POST /instances` request and specify the data volume attachment and snapshot ID.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2022-12-09&generation=2" \
-H "Authorization: $iam_token" \
-H "Content-Type: application/json" \
-d '{
      "name": "my-server-name",
      "zone": {
         "name": "us-south-1"
      },
      "vpc": {
         "id": "4d27c489-8ad7-3c18-cbf4-2103d9f8da93"
      },
      "profile": {
         "name": "cx2-2x4"
      },
      "primary_network_interface": {
         "name": "region1example-net1",
         "subnet": {
            "id": ""
         }
      },
     "volume_attachments": [
        {
            "name": "restore-data-vol1",
            "delete_volume_on_instance_delete": true,
            "volume": {
                "profile": {
                    "name": "general-purpose"
                },
                "source_snapshot": {
                    "id": "bdcdc984-ba4e-4aef-84fb-e8448c3116b1"
                }
            }
        }
    "resource_group": {
        "id": "2fab2c7f-c09d-4c64-baf7-1453b7461493"
    }
  }'
```
{: codeblock}

### Restore a stand-alone data volume from a snapshot with the API
{: #snapshots-vpc-restore-unattached-api}

To restore a stand-alone data volume from a snapshot, make a `POST /volumes` request and specify the ID, CRN, or URL of the snapshot in the `source_snapshot` property. The restored volume capacity (in GBs) must be at least the snapshot's minimum_capacity.

This example request creates a new 100 GB volume based on a 5 IOPS/GB profile. It specifies a different root key than the original snapshot. The source snapshot is specified by ID.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/volumes/?version=2022-12-09&generation=2" \
-H "Authorization: $iam_token" \
-H "Content-Type: application/json" \
-d '{
     "name": "volume-from-snapshot-1",
     "capacity": 100,
     "profile": {
        "name": "5iops-tier"
     },
     "zone": {
        "name": "us-south-1"
     },
     "encryption_key":{
        "crn":"crn:[...]"
     },
     "source_snapshot:" {
        "id": "bdcdc984-ba4e-4aef-84fb-e8448c3116b1"
     }
   }`
```
{: codeblock}

## Performance considerations
{: #snapshots-performance-considerations}

Restoring boot and data volumes from a snapshot have the following performance implications:

### Performance considerations for restoring a volume from a snapshot
{: #snapshot-vol-perf}

Boot and data volume performance is initially degraded when restoring from a snapshot. During the restoration, your data is copied from IBM COS to VPC Block Storage. After the restoration process has completed, you should realize full IOPS on your new volume.

### Performance considerations when using a bootable snapshot to provision a new instance
{: #snapshot-boot-perf}

Before you select a snapshot of a boot volume to provision another instance, keep in mind these performance considerations:

* Because the bootable snapshot is not fully hydrated, performance is expected to be slower than when you use a regular boot volume.

* To achieve the best performance and efficiency when you provision multiple instances, boot from an existing image. Custom images that you provide are better than stock images for this purpose. Images can have a maximum size of 250 GB.

## Next Steps
{: #snapshots_vpc_restore_next_steps}

[Create](/docs/vpc?topic=vpc-snapshots-vpc-create) more snapshots or [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage) existing snapshots.
