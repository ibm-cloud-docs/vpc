---

copyright:
  years: 2021, 2024
lastupdated: "2024-10-22"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Restoring a volume from a snapshot
{: #snapshots-vpc-restore}

Restoring data from a snapshot creates a new, fully provisioned volume that you can use to start an instance or attach as auxiliary storage. You can restore boot and data volumes during instance creation or when you modify an existing instance. You can also create a stand-alone data volume from a snapshot. Volumes can be restored from snapshots that were created manually or by a backup policy. You can restore volumes from fast restore clones and cross-regional copies of snapshots, too. You can create volumes from snapshots in the UI, from the CLI, with the API, or Terraform.
{: shortdesc}

## About restoring a volume from a snapshot
{: #snapshots-vpc-restore-concepts}

Restoring a volume from a snapshot creates a boot or data volume, depending on whether the snapshot is bootable or nonbootable.

* Restoring from a **bootable** snapshot creates a boot volume that you can use to start a virtual server instance. The boot volume uses a general-purpose profile and is limited to 250 GB.

* A new data volume that was created from **nonbootable** snapshot inherits its properties from the original volume, such as [profile](/docs/vpc?topic=vpc-block-storage-profiles), capacity, data, and metadata. If the source volume used [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), the volume inherits that encryption with the original customer root key (CRK). However, you can specify a larger volume size, different profile, and a different CRK if you prefer.

You can restore volumes from a manually created snapshot or from a snapshot that was created by a backup policy. This type of snapshot is called a backup. For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).

You can restore a volume in a different region by using a cross-regional copy of a snapshot. For more information, see [Cross-regional snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_crossregion_copy).

You can also choose to restore a volume by using a fast restore snapshot clone. For more information about fast restore, see the [FAQs](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-fr).

If you plan to use a snapshot from another account, make sure that the right [IAM authorizations](/docs/vpc?topic=vpc-block-s2s-auth&interface=ui#block-s2s-auth-xaccountrestore-ui) and [IAM roles](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots-vpc-iam) are in place. Contact the snapshot's owner for the CRN of the snapshot.
{: important}

You can restore volumes at various stages of the VPC lifecycle.

* When you provision a virtual server instance, you can specify a snapshot of a boot or a snapshot of data volume. The restored boot volume is used to start the new instance. Restored data volumes are automatically attached to the instance as auxiliary storage.
* When you want to add a new auxiliary storage to your existing instance, you can restore a data volume from a nonbootable snapshot.
* When you create an unattached (stand-alone) {{site.data.keyword.block_storage_is_short}} volume from a snapshot, you can still attach the volume to an instance later.

Restoring an instance directly from snapshot consistency group identifier is not supported. However, you can restore a virtual server instance by restoring all of its boot and data volumes from the snapshots that are part of a consistency group.

## Limitations
{: #snapshots-vpc-restore-limitations}

The following limitations apply when you restore a volume from a snapshot.

* To restore a volume, the snapshot must be in a _stable_ state.
* You can delete the new volume at any time. 
* You can't delete the snapshot from which the volume is restored from unless the hydration is complete or the volume is deleted.
* If snapshot is protected with customer-managed encryption and you don't specify a different root key CRN, the restored volume is encrypted with the snapshot's encryption key. The encryption cannot be changed later.
* When the new volume is created, data restoration begins immediately, but performance is degraded until the volume is fully hydrated.

### Performance impact
{: #snapshots-performance-considerations}

The performance of boot and data volumes is initially degraded when data is restored from a snapshot. Performance degradation occurs during the restoration because your data is copied from {{site.data.keyword.cos_full}} to {{site.data.keyword.block_storage_is_short}} in the background.

At first, the restored volume appears as _pending_ or _degraded_, and the service begins pulling data from the snapshot that is stored in {{site.data.keyword.cos_short}}. While the data is copied over, the volume's health state is monitored by the volume resource. During the hydration process, you can attach the volume to a virtual server or detach it. After the restoration process is complete, you can realize full IOPS on the new volume.

Volumes that are restored from fast restore clones don't require hydration. The data is available as soon as the volume is created. However, to achieve the best performance and efficiency when you provision multiple instances, boot from an existing image. Custom images that you provide are better than stock images for this purpose. For more information, see [Getting started with custom images](/docs/vpc?topic=vpc-planning-custom-images).

## Restoring a volume with fast restore
{: #snapshots-vpc-use-fast-restore}

Restoring a volume by using a [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-fr) snapshot clone creates a fully provisioned volume at creation time.

With fast restore, you create and keep a clone of the snapshot in a zone within your region instead of an {{site.data.keyword.cos_short}} bucket. When you restore data from a fast restore snapshot, the data is pulled from the clone within your region. Because the data is immediately available, no hydration is necessary. Performance levels are not affected. By using fast restore snapshots, you can achieve [recovery time objectives](#x3167918){: term} (RTO) quicker than by restoring from a regular snapshot.

You can also use the fast restore feature with the backup service. For more information, see [creating backup policies and plans](/docs/vpc?topic=vpc-create-backup-policy-and-plan).

## Restoring volumes from snapshots in the UI
{: #snapshots-vpc-restore-ui}
{: ui}

You can create volumes from various pages in the {{site.data.keyword.cloud_notm}} console. Restoring from a bootable snapshot creates a boot volume that you use to provision the virtual server instance. The boot volume uses a general-purpose profile and is limited to 250 GB. Data volumes are created and attached to the instance. You can restore volumes from a snapshot outside of instance provisioning as well, you can create stand-alone volumes and new auxiliary volumes for existing instances.

### Creating a volume from the list of snapshots in the UI
{: #snapshots-vpc-restore-snaphot-list-ui}

From the list of {{site.data.keyword.block_storage_is_short}} snapshots, you can create an {{site.data.keyword.block_storage_is_short}} volume and specify whether it's to be attached to a virtual server instance or unattached (stand-alone). If you choose to attach a data volume, you can select an existing virtual server instance or choose to create an instance. The new volumes are added to the [list of {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

1. Go to the list of {{site.data.keyword.block_storage_is_short}} snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage snapshots**.
2. Either select a snapshot from the list or search for it by its CRN. It must be in a `stable` state.
3. From the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Create volume**.
4. In the side panel, choose whether you want to create an unattached data volume, create and attach a volume to an existing instance, or create a volume and provision a new instance.

   * For a **stand-alone data volume**, leave **Attach volume to virtual server** clear.

   Use this option when you're not sure which virtual server instance you want to attach the volume to.
   {: tip}

   * To attach the volume to an existing instance, select **Attach volume to virtual server**. Click **Attach new volume to an existing virtual server**. Then, select the virtual server instance that you want to attach the volume to. You can filter the list of available servers by zone.
   * To restore a volume and use it to provision a virtual server instance, select **Attach volume to virtual server**, and click **Attach new volume to a new virtual server**. Then, click **Configure virtual server**. This action takes you to the [virtual server provisioning page](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui).

   The new volume is shown in the **Boot volume** or **Data volume** section of the instance provisioning page. The placement depends on whether the volume that you created was from a bootable snapshot or a nonbootable snapshot.
   {: note}

5. Provide the required volume details.

    | Field | Description |
    |-------|-------------|
    | **Volume details** | Define the new volume. |
    | Name | Enter a name for the new volume. |
    | Resource group | Use the defaults or select from the list. |
    | Zone | Inherited from the snapshot. Change it to another zone in your region if you want to. |
    | Size | Enter a volume size allowed by the profile. The default is the minimum provisioning size based on the snapshot. |
    | **Profile** | This value defaults to the snapshot's IOPS tier or custom profile. You can change the profile. |
    | IOPS | For IOPS tiers, specify an IOPS tier profile. For custom IOPS, select a range. |
    | Size | Enter a volume size that is allowed by the profile. |
    | **Encryption** | Inherited from the snapshot.|
    {: caption="Create volume options." caption-side="bottom"}

6. When finished, click **Save**. The new volume is created.

### Creating a volume from the snapshot details page in the UI
{: #snapshots-vpc-restore-vol-details-ui}

Follow these steps to create a volume from the snapshot details page in the UI.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes and select a volume. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
2. On the {{site.data.keyword.block_storage_is_short}} volume details page, select the **Snapshots and Backups** tab. A list of snapshots that were created manually or by backup policies is shown.
3. From the list, click the snapshot name to go to its details page.
4. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Create volume**.
5. Define the new volume in the side panel. The information that you need to provide is the same as when you create a volume from the [list of snapshots](#snapshots-vpc-restore-snaphot-list-ui). See Table 1 for the details.
6. When you're finished, click **Save**. The new volume is created.

### Creating volumes for a virtual server instance from a consistency group
{: #snapshots-vpc-restore-cr-details-ui}

Follow these steps to create volumes for virtual server instance from the consistency groups page in the UI.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} snapshot consistency groups. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage snapshots**.
1. Select a snapshot consistency group from the list. It must be in a `stable` state.
1. From the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Create virtual server**.
   * If the group has more than one bootable snapshot, you can choose the one that you want to use for the boot volume of the new virtual server instance. Then, click **Configure virtual server**.
   * If only one bootable snapshot is in the consistency group, you're taken directly to the VPC provisioning page. 
1. The information about your region, profile, boot volume, and data volumes is populated in the New virtual server for VPC provisioning page.

   If you change the profile selection to **Image** or **Existing volume**, the boot volume snapshot is removed. The data volumes section is also populated with the nonbootable snapshots from the consistency group. You can remove a snapshot or create another data volume. However, if you removed a data volume and want to add it back, you must return to the step of selecting the consistency group again.
   {: important}

1. Configure other aspects of the virtual server instance such as networking and advanced features.

### Creating a boot volume from a snapshot when you provision a virtual server instance in the UI
{: #snapshots-vpc-restore-vol-ui}

Follow these steps to create a boot and a data volume from snapshots when you provision a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute> Virtual server instances**.
2. Click **Create** and provision your new instance. For more information about the required fields, see the table in [Creating virtual server instances in the UI](/docs/vpc?topic=vpc-creating-virtual-servers).
3. For the operating system, click **Change image**, then click the tab for **Snapshots**. The most recent bootable snapshot is listed.
   * If you want to use a different snapshot, click **Edit**. Either choose a bootable snapshot from the list or search for a specific snapshot by its CRN. Click **Save**. This action populates the snapshot data in the boot volume field on the provisioning page.
   * If you want to change the properties of the boot volume, such as the name, auto-delete feature, encryption, or tags, click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"). Change the property that you want and click **Save**. The boot volume information is updated on the provisioning page.
4. To create a data volume, under Data Volumes, click **Create**. A side panel is displayed.
   1. Select **Import from snapshot**. Expand the list to select a data snapshot. The most recent data snapshot is selected by default. However, you can select any snapshot from the list.
   1. The snapshot contains the properties of the original source volume, including the auto-delete status, tags, size, profile, and encryption. You can change all these properties. Keep in mind that you can change the volume name, profile, size, and IOPS later, but you cannot change the encryption type or CRK after the volume is created.
   1. Select a unique name, specify the size, and click **Create**. The data volume information is added in the Data volume list.
5. Review your selections for volumes, instance profile, SSH keys, networking, and so on.
6. If you are satisfied with your choices, click **Create virtual server instance**. The new instance is created with the volumes that you specified. The new instance appears in the list of virtual server instances.
7. To see the instance details, click the instance name. The volume or the volumes that you restored from snapshots are listed under **Storage volumes**. The camera icon indicates that the volume was created from a snapshot.

### Creating a data volume from a snapshot for an existing virtual server instance with the UI
{: #snapshots-vpc-create-from-vol-ui}

You can also create a data volume from a snapshot for an existing instance. Choose from the list of virtual server instances.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute> Virtual server instances**.
2. From the list, click the name of an instance. The instance must be in a _running_ state.
3. On the Instance details page, scroll to the list of Storage volumes and click **Attach volumes**. A side panel opens for you to define the volume attachment.
4. From the Attach storage volume panel, expand the list of Block Volumes, and select **Create a data volume**. 
5. You can expand the list and select a snapshot, or search for a specific snapshot by its CRN.
6. The snapshot contains the properties of the original source volume, including the auto-delete status, tags, profile, and encryption. You can change all these properties. Keep in mind that you can change the volume name, profile, size, and IOPS later, but you cannot change the encryption type or CRK after the volume is created.
7. Select a unique name, specify the size, and click **Save**. The data volume information is added in the Data volume list.
8. Click **Save**. The side panel closes and messages indicate that the restored volume is being attached to the instance. The new volume appears in the list of Storage volumes. Hover over the camera icon to see the name of the snapshot from which it was created.

## Restoring a volume from a snapshot from the CLI
{: #snapshots-vpc-restore-CLI}
{: cli}

Restoring from a bootable snapshot creates a boot volume that you cab use to provision a virtual server instance. The boot volume uses a general-purpose profile and is limited to 250 GB. Data volumes are created and can be attached to the instance. You can restore volumes from a snapshot outside of instance provisioning as well, you can create stand-alone volumes and new auxiliary volumes for existing instances.

### Before you begin
{: #snapshots-vpc-restore-prereq-CLI}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to {{site.data.keyword.cloud}}.

   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in. 

1. Gather information about the snapshot or snapshots that you want to use to restore a volume.
   - If you want to restore a volume from a single snapshot, locate the snapshot first and view its details. You can use the CLI to [view all the snapshots of an account in a region](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-all-account-cli) and select from the list. Alternatively, you can also [list all the snapshots of a specific volume](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-all-snapshots-cli) and choose one from the output. Then, use the `ibmcloud is snapshots SNAPSHOT_ID` command to list the details of the chosen snapshot. If you want to restore a volume from a snapshot of another account, contact the snapshot's owner for the CRN of the snapshot.
   - If you want to restore an instance by restoring multiple volumes from a consistency group, you need to gather information about the snapshots in the consistency group. [List all the consistency groups in the region](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-all-consistency-groups-cli). Then, take the ID of the consistency group and use it the `ibmcloud is snapshots` command to filter the output to the snapshots in the specified consistency group. See the following example.

   ```sh
   ibmcloud is snapshots --snapshot-consistency-group CONSISTENCY_GROUP_ID
   ```
   {: pre}

### Creating a boot volume from a snapshot for a new instance from the CLI
{: #snapshots-vpc-restore-boot-CLI}

Run the `ibmcloud is instance-create` command with the `source_snapshot` property in the boot volume JSON. Specify the ID or name of a bootable snapshot. The restored boot volume is used to initialize the instance.

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

```sh
Creating instance my-instance-restore1 in resource group under account VP01 as user rtuser1@mycompany.com...

ID               r006-eded6dcd-4f3c-4e79-a0cb-00f7c72f38cd
Name             my-instance-restore1
CRN              crn:v1:bluemix:public:is:us-south-1/a1234567::instance:7101_eded6dcd-4f3c-4e79-a0cb-00f7c72f38cd
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
                 0651dacb-4589-4147-86b3-a77544598f93 boot-from-snapshot1                  r006-abf9dd2b-9d5d-41f1-849d-55a8ab580ddb                            boot-from-snapshot1

```
{: screen}

For more information about available command options, see [`ibmcloud is instance-create`](/docs/vpc?topic=vpc-vpc-reference#instance-create).

### Creating a data volume from a snapshot for a new instance from the CLI
{: #snapshots-vpc-restore-data-CLI}

When you create an instance by using the `ibmcloud is instance-create` command, specify the `source_snapshot` parameter and snapshot name or ID in the volume attachment.

See the following example.

```sh
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
```
{: codeblock}

For more information about available command options, see [`ibmcloud is instance-create`](/docs/vpc?topic=vpc-vpc-reference#instance-create).

### Creating a data volume from a snapshot for an existing instance from the CLI
{: #snapshots-vpc-restore-data-inst-cli}

For an existing instance, specify the `ibmcloud is instance-volume-attachment-add` command with the `source-snapshot` parameter, and the name or ID of the snapshot. To find the ID of the snapshot from the CLI, see [View snapshots from the CLI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-cli).

See the following example.

```sh
ibmcloud is instance-volume-attachment-add data-vol-1 a67f49de-fccc-4e5c-824e-dcbd06d009af --profile general-purpose --source-snapshot 52de6e85-7068-4247-90fd-d2fa91fd9864
```
{: codeblock}

For more information about available command options, see [`ibmcloud is instance-volume-attachment-add`](/docs/vpc?topic=vpc-vpc-reference#instance-volume-attachment-add).

### Creating a stand-alone volume from a snapshot from the CLI
{: #snapshots-restore-create-vol-cli}

You can create a stand-alone {{site.data.keyword.block_storage_is_short}} data volume from a snapshot from the CLI. The volume is not attached to a virtual server instance. When you run the `ibmcloud is volume` command, the response shows the attachment type as `unattached`. You can attach the volume to an instance later.

Use this option when you're not sure which virtual server instance you want to attach the volume to.

Run the `ibmcloud is volume-create` command and specify the `snapshot` parameter and name or ID of the snapshot. Run the `ibmcloud is volume-create` command and specify the `snapshot` parameter and the name, ID, or CRN of the snapshot. For more information, see [Create a stand-alone {{site.data.keyword.block_storage_is_short}} volume from a snapshot](/docs/vpc?topic=vpc-creating-block-storage&interface=cli#create-vol-from-snapshot-cli).

The following example creates a stand-alone data volume by using the CRN of a snapshot from another account.

```sh
$ ibmcloud is volume-create my-new-volume general-purpose us-east-1 --snapshot crn:v1:bluemix:public:is:eu-east-1:a/a7654321::snapshot:r014-4463eb2c-4913-43b1-b9bf-62a94f74c146
Creating volume my-new-volume under account Test Account as user test.user@ibm.com...

ID                                     r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac
Name                                   my-new-volume
CRN                                    crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac
Status                                 pending
Attachment state                       unattached
Capacity                               100
IOPS                                   3000
Bandwidth(Mbps)                        393
Profile                                general-purpose
Encryption key                         -
Encryption                             provider_managed
Resource group                         defaults
Created                                2024-07-15T16:14:59+00:00
Zone                                   us-east-1
Health State                           inapplicable
Volume Attachment Instance Reference   -
Active                                 false
Busy                                   false
Tags                                   -
```
{: screen}

For more information about available command options, see [`ibmcloud is volume-create`](/docs/vpc?topic=vpc-vpc-reference#volume-create).

## Restoring a volume from a snapshot with the API
{: #snapshots-vpc-restore-API}
{: api}

You can programmatically restore a volume during instance provisioning by calling the `/instances` method in the [VPC API](/apidocs/vpc/latest#create-instance){: external} as shown in the following sample request. You can also create a stand-alone volume by calling the `/volumes` method in the [VPC API](/apidocs/vpc/latest#create-volume).

Before you begin, gather information about the snapshot or snapshots that you want to use to restore a volume or volumes.
   - If you want to restore a volume from a single snapshot, locate the snapshot first and view its details. You can use the API to [list all the snapshots of an account in a region](/apidocs/vpc/latest#list-snapshots){: external} and select from the list. Then, [retrieve the snapshot](/apidocs/vpc/latest#get-snapshot){: external} details. If you want to restore a volume from a snapshot of another account, contact the snapshot's owner for the CRN of the snapshot.
   - If you want to restore an instance by restoring multiple volumes from a consistency group, you need to gather information about the snapshots in the consistency group. [List all the consistency groups in the region](/apidocs/vpc/latest#list-snapshot-consistency-groups){: external}. Then, take the ID of the consistency group that you want to restore and use it to [retrieve the snapshot consistency group](/apidocs/vpc/latest#get-snapshot-consistency-group){: external} details.

### Creating a boot volume when you provision an instance with the API
{: #snapshots-vpc-restore-boot-api}

To restore a boot volume from a bootable snapshot when you create an instance, make a `POST /instances` request and specify the `boot_volume_attachment` property and the bootable snapshot ID in the `source_snapshot` subproperty.

See the following example.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2023-03-07&generation=2" \
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

### Creating a data volume when you provision an instance with the API
{: #snapshots-vpc-restore-data-api}

To restore a data volume from a snapshot and attach it at boot time, make a `POST /instances` request and specify the data volume attachment and snapshot ID.

```sh
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
     ]
    "resource_group": {
        "id": "2fab2c7f-c09d-4c64-baf7-1453b7461493"
    }
}'
```
{: codeblock}

### Creating a stand-alone data volume from a snapshot with the API
{: #snapshots-vpc-restore-unattached-api}

You can use the API to create a stand-alone volume from a snapshot. Use this option when you're not sure which virtual server instance you want to attach the volume to. Or, if you want to restore data from an unattached volume that was detached from an instance.

To restore a stand-alone data volume from a snapshot, make a `POST /volumes` request and specify the ID, CRN, or URL of the snapshot in the `source_snapshot` property. The restored volume capacity (in GBs) must be at least the snapshot's minimum_capacity.

The following example request creates a 100-GB volume that is based on a 5 IOPS/GB profile. It specifies a different root key than the original snapshot. The source snapshot is specified by ID.

```sh
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

If you want to restore a volume from a snapshot of another account, you can use the _CRN_ to identify the snapshot by its CRN, instead of its ID.

## Restoring a volume from a snapshot with Terraform
{: #snapshots-vpc-restore-terraform}
{: terraform}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a specific regional endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: screen}

### Creating a boot volume when you provision an instance with Terraform
{: #snapshots-vpc-restore-boot-terraform}

To restore a boot volume from a bootable snapshot when you create an instance, use the `ibm_is_instance` resource. The following example defines the new instance with the name `my-server-name`, and a `cx2-2x4` profile, and creates the boot volume from the snapshot `eb373975-4171-4d91-81d2-c49efb033753`.

```terraform
resource "ibm_is_instance" "example" {
  name    = "my-server-name"
  profile = "cx2-2x4"
  boot_volume {
    name     = "boot-restore"
    snapshot = eb373975-4171-4d91-81d2-c49efb033753
    tags     = ["dev:test"]
  }
  primary_network_interface {
    subnet = ibm_is_subnet.example.id
  }
  vpc  = 4d27c489-8ad7-3c18-cbf4-2103d9f8da93
  zone = "us-south-1"
  keys = [ibm_is_ssh_key.example.id]
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance){: external}.

### Creating and attaching a data volume to an instance with Terraform
{: #snapshots-vpc-restore-data-terraform}

To restore a data volume from a nonbootable snapshot and attach the volume to an instance, use the `ibm_is_instance_volume_attachment` resource. See the following example.

```terraform
resource "ibm_is_instance_volume_attachment" "example" {
  instance = ibm_is_instance.example.id

  name = "test-attachment-1"
  profile = "general-purpose"
  snapshot = "ibm_is_snapshot.example.id"
  delete_volume_on_attachment_delete = true
  delete_volume_on_instance_delete = true
  volume_name = "restore-data-vol1"
  }
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_instance_volume_attachment](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_volume_attachment#example-usage-restoring-using-snapshot){: external}.

### Creating a stand-alone data volume from a snapshot with Terraform
{: #snapshots-vpc-restore-unattached-terraform}

To create a volume from a snapshot, use the `ibm_is_volume` resource. The following example creates a volume from the snapshot `bdcdc984-ba4e-4aef-84fb-e8448c3116b1` with the general-purpose performance profile.

```terraform
resource "ibm_is_volume" "storage" {
  name            = "restore-data-vol1"
  profile         = "general-purpose"
  zone            = "us-south-1"
  source_snapshot = "ibm_is_snapshot.example.id"
}
```
{: codeblock}

If you want to restore a volume from a snapshot of another account, you can use the `source_snapshot_crn` argument to identify the snapshot by its CRN.

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume){: external}.

## Next steps
{: #bs_snapshots_restore_next_steps}

You can [create](/docs/vpc?topic=vpc-snapshots-vpc-create) more snapshots or [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage) existing snapshots.

When you restore a volume from a snapshot by using the fast restore feature, you can change the encryption key. So the encryption key that is used for the new volume differs from the encryption key that was used for the snapshot. However, if you delete the snapshot encryption key from the key management service, the volume might still become inaccessible when it is attached to a virtual server instance. For more information, see [Known issues](/docs/vpc?topic=vpc-known-issues&interface=ui#snapshots-fast-restore-known-issue).
