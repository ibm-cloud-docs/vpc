---

copyright:
  years: 2021, 2023
lastupdated: "2023-02-07"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Restoring a volume from a snapshot
{: #snapshots-vpc-restore}

Restoring data from a snapshot creates a new, fully provisioned volume that you can use to start an instance or attach as auxiliary storage. You can restore boot and data volumes during instance creation or when you modify an existing instance. You can also restore a data volume from a snapshot of a volume that is not attached to an instance. Volumes can be restored from snapshots that were created manually or by a backup policy.
{: shortdesc}

## About restoring a volume from a snapshot
{: #snapshots-vpc-restore-concepts}

Restoring a volume from a snapshot creates a new boot or data volume, depending on whether the snapshot is "bootable" or "nonbootable". The new volume that was created from the snapshot inherits properties from the original volume, such as [profile](/docs/vpc?topic=vpc-block-storage-profiles), capacity, data, and metadata. If the source volume used [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), the volume inherits that encryption. However, you can specify a larger volume size, different IOPS profile, and customer-managed encryption if you prefer.

To restore a volume, the snapshot must be in a _stable_ state. You can restore volumes from a manually created snapshot or from a snapshot that was created by a backup policy. This type of snapshot is called a backup. When you restore a volume from a backup, tags are inherited from the source volume. When any of these tags match a tag in the backup policy, a backup is created by that policy according to its schedule. For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).

At first, the volume appears as _pending_, and the service begins pulling data from the snapshot that is stored in {{site.data.keyword.cos_short}}. While the data is being restored, the volume's health state is monitored by the volume resource. If the volume that was created from snapshot is attached to an instance before its data is fully restored, volume hydration pauses on the service node and continues on the compute node. If the volume is detached while the hydration is in progress, the hydration pauses on compute node and continues on the storage node. You cannot delete a snapshot that is being used to hydrate a volume. The volume's [performance](#snapshots-performance-considerations) is initially degraded by this operation.

You can also choose to create a volume by using a [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-use-fast-restore) snapshot clone. The volumes that are restored from fast restore clones do not require hydration. The data is available as soon as the volume is created.

Restore a volume from a snapshot in the following ways:

* When you [provision a new virtual server instance](#snapshots-vpc-restore-vol-ui), you specify a snapshot of a boot or data volume. The restored boot volume is used to start the new instance. Data volumes are automatically attached to the instance as auxiliary storage.
* When you [add a new auxiliary storage to your existing instance](#snapshots-vpc-create-from-vol-ui), you can restore data from a nonbootable snapshot.
* When you [create an unattached (stand-alone) {{site.data.keyword.block_storage_is_short}} volume](#snapshots-vpc-restore-unattached-vol) from a snapshot, in the UI, from the CLI, or with the `volumes` API, you can later attach the data volume to an instance.
* When you restore a volume from the [list of {{site.data.keyword.block_storage_is_short}} snapshots](#snapshots-vpc-restore-snaphot-list-ui) or [snapshot details page](#snapshots-vpc-restore-vol-details-ui). The volume from which the snapshot was created does not have to be attached to an instance.

### Restore a volume during instance provisioning or update
{: #snapshots-vpc-restore-vsi-concepts}

You can use the UI, CLI, or API to restore a boot and data volumes during instance provisioning or when you modify an instance.

During the "create and attach" phase of instance provisioning, you can select a snapshot to restore the volume. Restoring from a bootable snapshot creates a boot volume that you use to provision the virtual server instance. The boot volume uses a general-purpose profile and is limited to 250 GB. Data volumes are created and attached to the instance.

- [Restore a volume from a snapshot in the UI](#snapshots-vpc-restore-ui})
- [Restore a volume from a snapshot from the CLI](#snapshots-vpc-restore-CLI)
- [Restore a volume from a snapshot with the API](#snapshots-vpc-restore-API)

### Restore a stand-alone data volume from a snapshot
{: #snapshots-vpc-restore-unattached-vol}

You can use the UI, CLI, or API to create a stand-alone volume from a snapshot. The volume does not have to be attached to an instance, but you can attach it later when the volume data is fully restored.

Use this option when you're not sure which virtual server instance you want to attach the volume to. Or, if you want to restore data from an unattached volume that was detached from an instance.

Create the volume from a snapshot from the [list of {{site.data.keyword.block_storage_is_short}} snapshots](#snapshots-vpc-restore-snaphot-list-ui), from the [snapshot details page](#snapshots-vpc-restore-vol-details-ui), or when you [create an unattached {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-vol-from-snapshot-ui) from a snapshot.

With the VPC API, you can make a `POST /volumes` call and specify the snapshot ID when you create a data volume. Restoring a boot volume with the API is not supported. Volume performance is initially degraded until the volume data is fully restored. For an example API call, see [Restoring a stand-alone data volume from a snapshot](#snapshots-vpc-restore-unattached-api).

### Restoring a volume with the fast restore feature
{: #snapshots-vpc-use-fast-restore}

Restoring a volume by using the fast restore feature creates a fully provisioned volume at creation time. With this feature, you create and keep a clone of the snapshot in a zone or zones within your region instead of a {{site.data.keyword.cos_short}} bucket. You can also use the fast restore feature with the backup service. For more information, see [backup policy plan](/docs/vpc?topic=vpc-backup-policy-create).

In the console, you can filter the list of available snapshots to see which ones have fast restore clones. Select the fast restore snapshot clone to restore the volume and have it ready to use faster. When you restore data from a fast restore snapshot, the data is pulled from the clone within your region and not from {{site.data.keyword.cos_short}}. Thus the data is immediately available, and no hydration is necessary. Performance levels are not affected.

When you use the fast restore feature, your existing regional plan is adjusted, including billing. Billing is based on instance hours, $0.75 per hour regardless of snapshot size.

## Limitations of restoring a volume from a snapshot
{: #snapshots-vpc-restore-limitations}

The following limitations apply when you restore a volume from a snapshot.

* A boot volume that is created from a snapshot can't be attached as a secondary volume.
* Restoring a boot volume with the API is not supported.
* When the new volume is created, data restoration begins immediately, but [performance is degraded](#snapshots-performance-considerations) until the volume is fully hydrated.
* You can delete the new volume at any time, including the time before it is fully hydrated. However, you can't delete the snapshot from which the volume is restored from unless the hydration is complete or the volume is deleted, whichever happens first.
* If snapshot is encrypted and you don't specify a different root key CRN, the new volume is encrypted with the snapshot's encryption key.

## Performance impact
{: #snapshots-performance-considerations}

Performance of boot and data volumes is initially degraded when data is restored from a snapshot. Performance degradation occurs during the restoration because your data is copied from {{site.data.keyword.cos_full}} to {{site.data.keyword.block_storage_is_short}} in the background. After the restoration process is complete, you can realize full IOPS on the new volume.

Volumes that are restored from fast restore clones do not require hydration. The data is available as soon as the volume is created.

However, to achieve the best performance and efficiency when you provision multiple instances, boot from an existing image. Custom images that you provide are better than stock images for this purpose. Images can have a maximum size of 250 GB.

## Restore a volume from a snapshot in the UI
{: #snapshots-vpc-restore-ui}
{: ui}

Restoring a volume from a snapshot with the UI creates a new, fully provisioned boot or data volume.

### Create a volume from the list of snapshots in the UI
{: #snapshots-vpc-restore-snaphot-list-ui}

From the list of {{site.data.keyword.block_storage_is_short}} snapshots, you can create an {{site.data.keyword.block_storage_is_short}} volume and specify whether it's to be attached to a virtual server instance or unattached (stand-alone). If you choose to attach a data volume, you can select an existing virtual server instance or choose to create an instance. The new volumes are added to the [list of {{site.data.keyword.block_storage_is_short}}{{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

1. Go to the list of {{site.data.keyword.block_storage_is_short}} snapshots. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**.

2. Select a snapshot from the list. It must be in a `stable` state.

3. From the overflow menu, select **Create volume**.

4. In the side panel, choose whether you want to attach a data volume to an existing instance, restore a boot volume and create a new instance, or create an unattached data volume.

    * For an unattached data volume, leave **Attach volume to virtual server** clear and provide the required volume details.

       | Field | Description |
       |-------|-------------|
       | Attach volume to virtual server | Cleared by default, creates an unattached volume. When selected, you can either attach the volume to an existing instance, or to a new instance you must create. |
       | **Virtual server selection** | Displays when you choose to attach the volume to a new or existing instance. For an existing instance: |
       | Zones | Select a zone within your region from the menu. |
       | Virtual server | Select a virtual server instance from the list. |
       | **Volume details** | Define the new volume (attached or unattached). |
       | Name | Enter a name for the new volume. By default, the snapshot name is used and appended with _restore_. |
       | Resource group | Use default or select from the list. |
       | Zone | Inherited from the snapshot. |
       | Auto-delete | Inherited from the snapshot. |
       | Size | Enter a volume size allowed by the profile. The default is the minimum provisioning size based on the snapshot. |
       | **Profile** | Defaults to the snapshot's IOPS tier or custom profile. You can change the profile. |
       | IOPS | For IOPS tiers, specify IOPS tier profile. For custom IOPS, select a range. |
       | Size | Enter a volume size allowed by the profile. The default is the minimum size required. |
       | **Encryption** | Inherited from the snapshot. If the encryption is provider-managed, you can change it to customer-managed encryption and select either Key Protect or HPCS. If the encryption is already customer-managed, you can choose a different key management service but not to provider-managed encryption. |
       {: caption="Table 1. Create volume options." caption-side="bottom"}

    * To attach a data volume to an existing instance, select **Attach volume to virtual server** and provide zone and virtual server instance information.
    * To restore a boot volume, select **Attach volume to virtual server** and click **Configure virtual server**, which takes you to the [virtual server provisioning page](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui).

    The new volume is shown in the **Boot volume** or **Data volume** section of the instance provisioning page. The placement depends on whether the volume that you created was from a bootable snapshot or a nonbootable snapshot.
    {: note}

5. When finished, click **Save**. The new volume is created and attached to the instance.

Hover over the name of a volume in the instance details. The text shows whether the volume was created from a snapshot.
{: tip}

### Create a volume from the snapshot details page in the UI
{: #snapshots-vpc-restore-vol-details-ui}

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes and select a volume. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. On the {{site.data.keyword.block_storage_is_short}} volume details page, select the **Snapshots and Backups** tab. A list of snapshots that were created manually or by backup policies is shown.
3. From the list, click the snapshot name to go to its details page.
4. From the **Actions** menu, click **Create volume**.
5. Define the new volume in the side panel. The information is the same as when you create a volume from the [list of snapshots](#snapshots-vpc-restore-snaphot-list-ui).
6. When you're finished, click **Save**. The new volume is created and attached to the instance.

### Create a volume from a snapshot when you provision a new instance in the UI
{: #snapshots-vpc-restore-vol-ui}

Follow these steps to create a boot and data volume from snapshots when you provision a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. Click **Create** and provision your new instance. For more information about the required fields, see the table in [Creating virtual server instances in the UI](/docs/vpc?topic=vpc-creating-virtual-servers).
3. For the operating system, click the tab for **Snapshots**. The most recent bootable snapshot is listed.
4. Optionally, to select a different bootable snapshot and create a boot volume, click **Change**.
5. From the list of snapshots, select a bootable snapshot for your instance's operating system and click **Save**. This action imports snapshot data to the boot volume. The new boot volume is created from the snapshot and is listed on the instance provisioning page.
6. To create a data volume, under Data Volumes, click **Create**. A side panel is displayed.
7. Select **Import from snapshot**. Expand the list to select a data snapshot. The most recent data snapshot is selected by default. However, you select a different snapshot from the list if you want. The snapshot contains the properties of the original source volume, including the IOPS tier and encryption.
8. Optionally, change the size of the volume.
9. Click **Save**. The new data volume is created from the snapshot and attached to the instance. It is shown in the Data volume list.
10. Optionally, to see the API POST request for restoring the volume, click **Get sample API call**. The example call is displayed, similar to the example in [Restore a volume from a snapshot with the API](#snapshots-vpc-restore-API) section.
11. When you're finished provisioning your instance, click **Create virtual server instance**. The new instance appears in the list of virtual server instances.

After the instance is created, you can click the instance name to see the instance details. The boot volume that you restored from a snapshot is listed under **Storage volumes**. The camera icon indicates that the volume was created from a snapshot.

### Create a data volume from a snapshot for an existing virtual server instance with the UI
{: #snapshots-vpc-create-from-vol-ui}

You can also create a data volume from a snapshot for an existing instance. Choose from the list of virtual server instances.

1. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, go to **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. From the list, click the name of an instance. The instance must be in a _running_ state.
3. On the Instance details page, scroll to the list of Storage volumes and click **Attach volumes**. A side panel opens for you to define the volume attachment.
4. From the Attach data volume panel, expand the list of Block volumes, and select **Create a data volume**.
5. Select **Import from snapshot**. Expand the Snapshot list and select a snapshot.
6. Optionally, increase the size of the volume within the specified range.
7. Click **Save**. The side panel closes and messages indicate that the restored volume is being attached to the instance.

The new volume appears in the list of Storage volumes. Hover over the camera icon to see the name of the snapshot from which it was created.

## Restore a volume from a snapshot from the CLI
{: #snapshots-vpc-restore-CLI}
{: cli}

Restoring a volume from a snapshot from the CLI creates a new, fully provisioned boot or data volume.

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

See the following example.

```sh
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

A successful response looks like the following example.

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

For more information about available command options, see [`ibmcloud is instance-create`](/docs/vpc?topic=vpc-vpc-reference#instance-create).

### Create a data volume from a snapshot for a new instance from the CLI
{: #snapshots-vpc-restore-data-CLI}

When you create an instance by using the `ibmcloud is instance-create` command, specify the `source_snapshot` parameter and snapshot name or ID in the volume attachment.

See the following example.

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

For more information about available command options, see [`ibmcloud is instance-create`](/docs/vpc?topic=vpc-vpc-reference#instance-create).

### Create a data volume from a snapshot for an existing instance from the CLI
{: #snapshots-vpc-restore-data-inst-cli}

For an existing instance, specify the `ibmcloud is instance-volume-attachment-add` command with the `source-snapshot` parameter, and the name or ID of the snapshot. To find the ID of the snapshot from the CLI, see [View snapshots from the CLI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-cli).

See the following example.

```zsh
ibmcloud is instance-volume-attachment-add data-vol-1 a67f49de-fccc-4e5c-824e-dcbd06d009af --profile general-purpose --source-snapshot 52de6e85-7068-4247-90fd-d2fa91fd9864
```
{: codeblock}

For more information about available command options, see [`ibmcloud is instance-volume-attachment-add`](/docs/vpc?topic=vpc-vpc-reference#instance-volume-attachment-add).


### Create a stand-alone volume from the snapshots details from the CLI
{: #snapshots-restore-create-vol-cli}

You can create a stand-alone {{site.data.keyword.block_storage_is_short}} data volume from a snapshot from the CLI. The volume is not attached to a virtual server instance. When you run the `ibmcloud is volume` command, the response shows the attachment type as `unattached`. You can attach the volume to an instance later.

Run the `ibmcloud is volume-create` command and specify the `snapshot` parameter and name or ID of the snapshot. For more information, see [Create a stand-alone {{site.data.keyword.block_storage_is_short}} volume from a snapshot](/docs/vpc?topic=vpc-creating-block-storage&interface=cli#create-vol-from-snapshot-cli).

For more information about available command options, see [`ibmcloud is volume-create`](/docs/vpc?topic=vpc-vpc-reference#volume-create).

## Restore a volume from a snapshot with the API
{: #snapshots-vpc-restore-API}
{: api}

Use the VPC API to restore a volume from a snapshot. Restoring creates a new, fully provisioned volume.

### Creating a boot volume when you create a new instance with the API
{: #snapshots-vpc-restore-boot-api}

To restore a boot volume from a bootable snapshot, make a `POST /instances` request and specify the boot volume attachment and snapshot ID. See the following example.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2022-06-14&generation=2" \
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

### Creating a data volume when you create an instance with the API
{: #snapshots-vpc-restore-data-api}

To restore a data volume from a snapshot and attach it at boot time, make a `POST /instances` request and specify the data volume attachment and snapshot ID.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2022-06-14&generation=2" \
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

### Create a stand-alone data volume from a snapshot with the API
{: #snapshots-vpc-restore-unattached-api}

To restore a stand-alone data volume from a snapshot, make a `POST /volumes` request and specify the ID, CRN, or URL of the snapshot in the `source_snapshot` property. The restored volume capacity (in GBs) must be at least the snapshot's minimum_capacity.

The following example request creates a 100-GB volume that is based on a 5 IOPS/GB profile. It specifies a different root key than the original snapshot. The source snapshot is specified by ID.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/volumes/?version=2022-06-14&generation=2" \
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

## Next Steps
{: #snapshots_vpc_restore_next_steps}

[Create](/docs/vpc?topic=vpc-snapshots-vpc-create) more snapshots or [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage) existing snapshots.
