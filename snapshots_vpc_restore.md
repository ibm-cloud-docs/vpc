---

copyright:
  years: 2021, 2022
lastupdated: "2022-06-09"

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

# Restoring a volume from a snapshot
{: #snapshots-vpc-restore}

Restoring from a snapshot create a new, fully-provisioned boot or data volume. You can restore boot and data volumes during instance creation or by modifying an existing instance. For data volumes, you can also use the `volumes` API to restore a volume that's not attached to an instance.
{: shortdesc}

When you restore a boot volume from the instance, you can use it when you provision the new instance. When you a data volume on the instance, it's automatically attached. You can create an unattached data volume from a snapshot and attach it later. 

Volumes can be restored from snapshots created manually or by a backup policy.

## About restoring a volume from a snapshot
{: #snapshots-vpc-restore-concepts}

Restoring a volume from a snapshot creates a new volume. The new volume inherits properties from the original volume, but you can specify a larger volume size, different IOPS, and customer-managed encryption.

Restore a volume from a snapshot in the following ways:

* When provisioning a new instance, specify a snapshot of a boot or data volume. Data volumes are automatically attached to the instance as secondary storage. Use the restored boot volume to start the new instance.
* When modifying an existing instance, specify a snapshot of a boot or data volume. Data volumes are automatically attached to the instance as secondary storage.
* From the `volumes` API, create a new stand-alone (unattached) data volume from the snapshot. You can later attach the data volume to an instance. Restoring boot volumes in this way is not supported.

### Restoring a volume during instance provisioning or update
{: #snapshots-vpc-restore-vsi-concepts}

Use the UI, CLI, or API to restore boot and data volumes during instance provisioning or when modifying an instance.

Restoring from a "bootable" snapshot creates a boot volume that you can use when [provisioning a new instance](#snapshots-vpc-restore-vol-ui). The boot volume uses a general purpose profile and is limited to 250 GB. Note that when you provision an instance using a bootable snapshot, performance might be slower as the boot volume is hydrated (that is, fully provisioned).

Restoring from a data volume snapshot creates secondary storage attached to the instance. You can restore the volume when provisioning a new instance or by modifying an existing instance. During the create and attach process, you select a snapshot of the volume to restore the volume. A new volume is then created and attached to the instance. 

You can restore a volume from a snapshot from the [list of block storage snapshots](#snapshots-vpc-restore-snaphot-list-ui), from the [snapshot details page](#snapshots-vpc-restore-vol-details-ui), or when you [provision a new instance](#snapshots-vpc-restore-vol-ui). You can also restore a volume from a snapshot to create a volume for [an existing instance](#snapshots-vpc-create-from-vol-ui).

### Restoring an unattached data volume from a snapshot
{: #snapshots-vpc-restore-unattached-vol}

With the API, you can create an new unattached data volume from a snapshot by making a `POST /volumes` call and specifying the snapshot ID. The stand-alone volume created from the snapshot is fully hydrated (data is restored) when you later attach it to an instance. For an example API call, see [Restoring a standalone data volume from a snapshot](#snapshots-vpc-restore-unattached-api) Also see the [limitations](#snapshots-vpc-restore-unattach-limits) when restoring an unattached volume from a snapshot.

### Additional considerations
{: #snapshots-vpc-restore-vsi-concepts}

The restored volume inherits the same [profile](/docs/vpc?topic=vpc-block-storage-profiles), capacity, data, and metadata as the original volume. You can choose a different profile and capacity if you prefer. If the source volume used [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), the volume inherits that encryption.

To restore a volume, it must be in a _stable_ state. When you restore from a snapshot attached to an instance, the service immediately creates the new volume. The volume appears first as _pending_ while it's being created. After the volume is hydrated, you can use the new boot volume to start a new instance or write data to the new attached data volume. [Performance](#snapshots-performance-considerations) is initially degraded by this operation.

You can also restore a volume from a snapshot created by a backup policy. This type of snapshot is called a backup. When you restore a volume from a backup, tags are inherited from the source volume. When any of these tags match a tag in the backup policy, a backup is created. For more information, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

Backup for VPC is available only to accounts with special approval to preview the feature.
{: note}

## Limitations when restoring a volume from a snapshot
{: #snapshots-vpc-restore-limitations}

The following limitations apply when restoring a volume from a snapshot.

### General limitations
{: #snapshots-vpc-restore-general-limitations}

* Simultaneously restoring a boot and data volume during instance provisioning or when modifying an instance is not allowed.
* UI, CLI, and API can be used to restore a volume from a snapshot during instance provisioning or when modifying an instance. However, you must use the API to restore an unattached data volume.
* Restoring a boot volume from a snapshot using the `volumes` API is not supported.

### Limitations when restoring an unattached data volume
{: #snapshots-vpc-restore-unattach-limits}

* The new volume is created but is hydrated (data is restored) only after the volume is attached to an instance.
* You can delete the new volume at any time, including the time before it is fully hydrated. However, you can't delete the snapshot from which the volume is restored unless the hydration is completed or the volume is deleted, whichever happens first.
* The parent snapshot is used to restore the volume.
* After attaching the restored volume to an instance, the volume will have [degraded performance](#snapshots-performance-considerations) until the volume is fully hydrated.
* If snapshot is encrypted and you don't specify a root key CRN in the `POST /volumes` request, the new volume is encrypted with the snapshot's encryption key.

## Restore a volume from a snapshot with the UI
{: #snapshots-vpc-restore-ui}
{: ui}

### Create a volume from the list of snapshots with the UI
{: #snapshots-vpc-restore-snaphot-list-ui}

1. Navigate to the list of block storage snapshots. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**. 

2. Select a snapshot from the list. It must be in a `stable` state.

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
| **Configure virtual server** | Select this option to go to the Virtual Server provisioning page and [create the instance](/docs/vpc?topic=vpc-creating-virtual-servers) as usual. Note that the new volume is preselected and is shown in the *Boot volume** or **Data volume** section, depending on whether the volume you created was from a bootable snapshot or data snapshot. |
{: caption="Table 1. Create volume options caption-side="top"}

Hover over the name of a volume attached to an instance in the instance details. The text will tell you whether the volume was created from a snapshot.
{: tip}

### Create a volume from the snapshots details page with the UI
{: #snapshots-vpc-restore-vol-details-ui}

1. Navigate to the list of block storage volumes and select a volume. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**. 

2. On the block storage volume details page, select the **Snapshots and Backups** tab. A list of snapshots created by manually or by backup policies is shown in the list. 

3. Click the overflow menu for a snapshot from which you want to restore the volume and select **Create volume**.

4. In the side panel, select whether you want to attach the volume to an existing instance or create a new instance. Table 1 describes the options on the side panel.

5. When finished, click **Save**. The new volume is created and attached to the instance.

### Create a volume from a snapshot when you provision a new instance with the UI
{: #snapshots-vpc-restore-vol-ui}

Follow these steps to create a boot and data volume from snapshots when you provision a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**. 

   Be sure to select VPC infrastructure from the menu icon.   
   {: tip}

1. Click **Create** and provision your new instance with the information from the table in [Creating virtual server instances in the UI](/docs/vpc?topic=vpc-creating-virtual-servers). 

1. For the operating system, click the tab for **Snaphots**. The most recent bootable snapshot is listed. 

1. Optionally, to select a different bootable snapshot and create a boot volume, click **Change**.

1. From the list of snapshots, select a bootable snapshot for your instance's operating system and click **Save**.

  This action imports snapshot data to the boot volume. The new boot volume is created from the snapshot and is listed under Boot Volume on the instance provisioning page.

1. To create a data volume, under Data Volumes, click **Create**. A side panel displays for creating a new data volume.

1. Select **Import from snapshot**. Expand the list to select a data snapshot. The most recent data snapshot is selected by default. Optionally, select a different snapshot from the list. The snapshot contains the properties of the original source volume, including the IOPS tier and encryption. 

1. Optionally, change the size of the volume.

1. Click **Save**. The new data volume is created from the snapshot and attached to the instance, shown in the Data volume list. 

1. Optionally, to see the API POST request for restoring the volume, click **Get sample API call**. Information displays for the call, similar to the example in [this section](#snapshots-vpc-restore-API).

1. When you're finished provisioning your new instance, click **Create virtual server instance**. The new instance appears in the list of virtual server instances.

After the instance is created, you can click on the instance name to see the instance details. The boot volume you restored from a snapshot is listed under **Storage volumes**. The camera icon indicates the volume was created from a snapshot.

### Create a data volume from a snapshot for an existing virtual server instance with the UI
{: #snapshots-vpc-create-from-vol-ui}

You can also create a data volume from a snapshot for an existing instance. Choose from the list of virtual server instances.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**. 

1. From the list, click on the name of an instance. The instance must be in a _Running_ state.

1. On the Instance details page, scroll to the list of Storage volumes and click **Attach volumes**. A side panel opens for you to define the volume attachment.

1. From the Attach data volume panel, expand the list of Block volumes and select **Create a data volume**.

1. Select **Import from snapshot**. Expand the Snapshot list and select a snapshot. 

1. Optionally, increase the size of the volume within the specified range.

1. Click **Save**. The side panel closes and messages indicate that the restored volume is being attached to the instance. 

The new volume appears in the list of Storage volumes. Hover over the camera icon to see the name of the snapshot from which it was created. 

## Restore a volume from a snapshot with the CLI
{: #snapshots-vpc-restore-CLI}
{: cli}

Use the CLI to create a boot or data volume from a snapshot.

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

Specify the `source_snapshot` parameter and snapshot ID when using the `instance-create` command to create a new instance.

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
CRN              crn:v1:staging:public:is:us-south-1:a/2d1bace7b46e4815a81e52c6ffeba5cf
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

When you create a new instance using the `instance-create` command, specify the `source_snapshot` parameter and snapshot ID in the volume attachment.

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

For an existing instance, specify the `instance-volume-attachment-add` command with the `source-snapshot` parameter, and the ID of the snapshot. To find the ID of the snapshot from the CLI, see [View snapshots from the CLI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-cli).

For example:

```zsh
ibmcloud is instance-volume-attachment-add data-vol-name 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --profile general-purpose --source-snapshot eaf9d6ca-35bf-4ac7-bc45-d0f2507f2830
```
{: codeblock}

## Restore a volume from a snapshot with the API
{: #snapshots-vpc-restore-API}
{: api}

Use the VPC API to restore a volume from a snapshot.

### Restore a boot volume when creating a new instance with the API
{: #snapshots-vpc-restore-boot-api}

To restore a boot volume from a bootable snapshot, make a `POST /instances` request and specify the boot volume attachment and snapshot ID. For example:

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

### Restoring a data volume when creating a new instance with the API
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

### Restoring an unattached data volume from a snapshot with the API
{: #snapshots-vpc-restore-unattached-api}

To restore an unattached (stand-alone) data volume from a snapshot, make a `POST /volumes` request and specify the ID, CRN, or URL of the snapshot in the `source_snapshot` property. The restored volume capacity (in GBs) must be at least the snapshot's minimum_capacity.

This example request creates a new 100 GB volume based on a 5 IOPS/GB profile. It specifies a different root key than the original snapshot. The source snapshot is specified by ID.

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
        id": "bdcdc984-ba4e-4aef-84fb-e8448c3116b1"
     }
   }`
```
{: pre}

## Performance considerations
{: #snapshots-performance-considerations}

Restoring boot and data volumes from a snapshot have the following performance implications:

### Performance considerations when using restoring a volume from a snapshot
{: #snapshot-vol-perf}

Boot and data volume performance is initially degraded when restoring from a snapshot. During the restoration, your data is copied from IBM COS to VPC Block Storage. After the restoration process has completed, you should realize full IOPS on your new volume.

### Performance considerations when using a bootable snapshot to provision a new instance
{: #snapshot-boot-perf}

Before you select a snapshot of a boot volume to provision a new instance, keep in mind these performance considerations:

* Because the bootable snapshot is not fully hydrated, performance will be slower than using a regular boot volume.

* To achieve the best performance and efficiency when provisioning multiple instances, boot from an existing image. Custom images you provide are better than stock images for this purpose. Images can have a maximum size of 250 GB.

## Next Steps
{: #snapshots_vpc_restore_next_steps}

[Create](/docs/vpc?topic=vpc-snapshots-vpc-create) more snapshots or [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage) existing snapshots.
