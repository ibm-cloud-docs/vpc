---

copyright:
  years: 2019, 2024
lastupdated: "2024-10-10"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing {{site.data.keyword.block_storage_is_short}} volumes
{: #viewing-block-storage}

View details about a {{site.data.keyword.block_storage_is_short}} volume or summary information about all volumes.
{: shortdesc}

## Viewing information about {{site.data.keyword.block_storage_is_short}} volumes in the UI
{: #viewvols}
{: ui}

List all {{site.data.keyword.block_storage_is_short}} volumes and view details for a single volume. View attached {{site.data.keyword.block_storage_is_short}} volume details in instance details. View all snapshots that were created from the {{site.data.keyword.block_storage_is_short}} volume.

### Viewing information about all {{site.data.keyword.block_storage_is_short}} volumes in the UI
{: #viewvols-ui}

Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.

By default, {{site.data.keyword.block_storage_is_short}} volumes display for all resource groups in your region. In the list of all **{{site.data.keyword.block_storage_is_short}} volumes**, you see the following information.

| Field | Description |
|-------|-------------|
| Region | The region where these volumes are located, for example, US South. Click the down arrow to see volumes in a different region in which you have an account. |
| Name | Click the name of the volume to see individual volume details. |
| Status | Status of the volume, which functions as the default filter for all rows. |
| Location | Availability zone in your region, inherited from the VPC (for example, US South 1).|
| Size | Size of the volume you specified, in GBs.|
| Attachment type | Data, for a secondary volume attached to an instance, boot when attached as a boot volume, or blank for an unattached volume.|
| Health | Health monitors the overall health of the volume, such as I/O performance and data consistency. Volume health statuses are `OK` or `degraded`. Volumes in a degraded state have degraded performance, capacity, or experience connection problems. Volumes being restored from a snapshot also show a degraded state. The service displays a possible reason for the degraded state so that you can resolve any issues. For more information, see [{{site.data.keyword.block_storage_is_short}} volume health states](/docs/vpc?topic=vpc-block-storage-vpc-monitoring#block-storage-vpc-health-states). |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also use your own root keys in a {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance to protect your data. For more information, see [Creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption). |
| Tags | Number of user tags that are applied to the volume. Click the number in this column to view or edit the tags in the new window. If no tags were applied to the volume, click **Add tags** and add them in the new window. User tags can associate the volume with a [backup policy](/docs/vpc?topic=vpc-backup-service-about) for creating backups of the volume. For more information, see [Adding user tags that are associated with a backup policy to a volume in the UI](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#add-user-tags-volumes-ui).
| Actions | Click the  **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") to display a menu of context-specific actions you can take. For example, an available, unattached volume would have menu options for attaching to an instance, renaming, and deleting the volume. An attached volume would allow for unattaching the volume from an instance and creating an [image from the volume](/docs/vpc?topic=vpc-image-from-volume-vpc).|
{: caption="Details about all volumes" caption-side="bottom"}

By default, 10 volumes are shown in the list of all {{site.data.keyword.block_storage_is_short}} volumes. Change this default by clicking the Page Control down arrow and increase the list to 20 or 50 volumes. Use the Page Control arrows after the list to go to the following page or return to the current page.

Actions menu selections change, depending on whether the volume is a boot volume, an attached data volume, or an unattached data volume. Table 2 describes these actions.

| Volume type | Action | Description |
|-------------|--------|-------------|
| **Boot** | Create image. | Create an image from the boot volume. For more information, see [Creating an image from a volume in the UI](/docs/vpc?topic=vpc-create-ifv&interface=ui#create-image-from-volume-vpc-ui).
| | Create snapshot. | Create a "bootable" snapshot from the boot volume. A snapshot is a point in time copy of the volume. For more information, see [Creating a snapshot in the UI](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui#snapshots-vpc-create-ui).
| | Detach from instance. | If the boot volume is attached as a secondary volume, you can [detach it](/docs/vpc?topic=vpc-managing-block-storage#detach) from the instance. |
| **Data** | Create snapshot. | Create a point in time copy of the data volume. |
| | Detach from instance. | [Detach](/docs/vpc?topic=vpc-managing-block-storage#detach) the data volume from the instance. |
| | Delete. | [Delete](/docs/vpc?topic=vpc-managing-block-storage#delete) the volume. You must first detach the volume from an instance before you attempt to delete it. |
| Unattached (-) | Attach to an instance. | [Attach](/docs/vpc?topic=vpc-attaching-block-storage) the volume to an available virtual server instance. |
| | Delete. | Delete the unattached volume. |
{: caption="Actions menu options for volumes." caption-side="bottom"}

### Viewing details about a {{site.data.keyword.block_storage_is_short}} volume
{: #view-vol-details-ui}

To view details about a {{site.data.keyword.block_storage_is_short}} volume, go to the list of all {{site.data.keyword.block_storage_is_short}} volumes and select a volume. By default, the Overview tab is selected for volume details. Click the **Snapshots and Backups** tab to view a list of snapshots that were created manually or by a backup policy.

Next to the name of the volume is the [volume status](/docs/vpc?topic=vpc-block-storage-vpc-monitoring#block-storage-vpc-status) and tags that are associated with this volume. [User tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-user-tags) identify the resource and when associated with a backup policy, are used for creating [backup snapshots](/docs/vpc?topic=vpc-backup-service-about) of the volume. With [Access management tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-mgt-tags), you can create flexible resource groupings for managing access. For more information about these tags, see [Working with tags](/docs/account?topic=account-tag).

The Actions menu on the volume details page shows the actions that you can take, depending on whether the volume is a boot or data volume, and attached or unattached. For more information, see Table 4.

The {{site.data.keyword.block_storage_is_short}} volumes details page shows volume details, attached virtual server instances, and backup policies. Table 3 describes this information.

| Field | Description |
|-------|-------------|
| **Volume details** | |
| Name  | Name of the volume you specified when you created the volume. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit the volume name. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. Volume names must be unique for the account and for the region. |
| Volume ID | System-generated volume ID. |
| Health |  Health monitors the overall health of the volume, such as I/O performance and data consistency. Volume health statuses are `OK` or `degraded`. Volumes in a degraded state have less than OK performance, capacity, or experience connection problems. Volumes that are being restored from a snapshot also show a degraded state. The service displays a possible reason for the degraded state so you can resolve it. For more information, see [{{site.data.keyword.block_storage_is_short}} volume health states](/docs/vpc?topic=vpc-block-storage-vpc-monitoring#block-storage-vpc-health-states). |
| Resource group | Resource group defined when you set up your VPC. Resource groups manage access to resources but do not affect billing or monitoring.|
| Attachment type | Data, for a secondary volume attached to an instance, boot when attached as a boot volume, or blank for an unattached volume.|
| Created date | System-generated date when the volume was created.|
| Location | Availability zone in your region.|
| Size | Size of the volume you specified. When the volume is attached to a virtual server instance and the volume is not at maximum capacity for its range, you can click the icon to expand the volume. For more information, see [expanding {{site.data.keyword.block_storage_is_short}} volume capacity (Beta)](/docs/vpc?topic=vpc-expanding-block-storage-volumes). |
| Profile | [IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) or [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile. Click the icon to [adjust IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) by selecting a different profile. |
| Max IOPS | Maximum IOPS value for a predefined IOPS tier or the value you specified for custom IOPS. |
| Throughput | The performance a disk can deliver, measured in Gigabits/second (Gbps). Based on your volume profile, throughput is calculated as the amount of IOPS * 16 K block size.|
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also use your own root keys to protect your data. The Encryption field shows the name of the key management service (KMS) you provisioned (for example, {{site.data.keyword.keymanagementserviceshort}}) and **customer-managed**. For more information, see [Creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).|
| Encryption Instance | _Optional._ A link to the provisioned KMS instance for a customer-managed encryption volume. |
| Key | _Optional._ The name and copiable ID of the root key that is used to encrypt the passphrase, which secures a customer-managed encryption volume. |
| Backup policies | The number of backup policies that are associated with the volume. Click the number link to go to the backup policies tab. |
| Snapshots | The number of snapshots that were created of the volume. Click the number link to go to the Snapshots and Backups tab. |
| **Attached virtual server** | Volumes attached to a virtual server instance are listed here. Click **Attach** to select an instance to attach this volume. For more information, see [Attaching a volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage). |
| Status | Tracks the overall lifecycle state of the volume, which ranges from volume creation to volume deletion. Attachment status, for example, _attached_ when the volume is attached to an instance and _attaching_ when in progress. |
| Name | Click the name of the virtual server instance to see instance details. |
| Auto delete | When _enabled_, the volume is automatically deleted when you delete the instance. Click the toggle to enable automatic deletion. |
| **Backup policies** | Shows backup policies that are associated with this volume. To associate backup policies, you can add a backup policy's tags for target resources to this volume. Click **Apply** to select a backup policy, then apply its tags for the target resource to the volume. |
{: caption="Volume details" caption-side="bottom"}

Table 4 shows Actions menu options from the volume details page.

| Action | Description |
|--------|-------------|
| Create snapshot | Create a snapshot from a data volume or a "bootable snapshot" from a boot volume. Data volumes must be attached to a virtual server instance. For more information, see [Create a snapshot in the UI](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui#snapshots-vpc-create-ui).
| Create image | Create an image from the boot volume. For more information, see [Creating an image from a volume in the UI](/docs/vpc?topic=vpc-create-ifv&interface=ui#create-image-from-volume-vpc-ui).
| Expand volume | [Increase the size](/docs/vpc?topic=vpc-about-increasing-volume-capacity) of a data volume in GBs. |
| Edit IOPS profile | For data volumes that are attached to a virtual server instance, increase or decrease IOPS by [editing the IOPS profile](/docs/vpc?topic=vpc-adjusting-volume-iops). |
| Delete | [Delete](/docs/vpc?topic=vpc-managing-block-storage#delete) the volume. You must first detach the volume from an instance before you attempt to delete it. |
{: caption="Actions menu options one the volume details page." caption-side="bottom"}

### Viewing attached {{site.data.keyword.block_storage_is_short}} volume details in instance details
{: #view-vol-details-instance-ui}

You can view information about an attached {{site.data.keyword.block_storage_is_short}} volume from the **Virtual server instance details** page:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances** and select an instance.

2. Under **Attached Block Storage volumes**, click the name of a volume to go to the volume details page.

### Viewing all snapshots that were created from the {{site.data.keyword.block_storage_is_short}} volume
{: #view-snapshots-for-volume}

If you created snapshots of a {{site.data.keyword.block_storage_is_short}} boot or data volume, you can see the snapshots on the volume details page.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.

2. Select a volume from the list.

3. On the volume details page, click the **Snapshots and Backups** tab. A list of snapshots is displayed with the name, status, size, encryption type, and when it was created. It also shows whether the snapshot was created by the user or a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#baas-comparison). The snapshots display in descending order, with the most recently created snapshot in first place.

You can see details for a snapshot, create a snapshot, and manage snapshots from the Volume details page. For example, from the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), you can delete the most recent snapshot. For more information, see one of the following topics.

* [View details of a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).
* [Create a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).
* [Delete all snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-all-ui).

### Viewing all backup policies associated with a volume
{: #view-backup-policies-for-volume}

View all backup policies associated with a {{site.data.keyword.block_storage_is_short}} volume. All policies that have the user tag that is applied to this volume are listed. To add volumes to a policy, add user tags to the volume that are in the backup policy's tags for target resources. When you remove tags from a volume that are in a backup policy, the volume is no longer be backed up.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.

2. Locate the volume that you want and click the name link.

3. From the {{site.data.keyword.block_storage_is_short}} volumes details page, click the **Backup policies** tab. Table 5 describes the information on the Backup policies page.

4. Click **Attach** to apply a new backup policy to this volume. In the side panel, select a backup policy from the menu, and select the policy tags to apply to the volume. You can also view the plan details that can help you decide whether to use that policy. If satisfied, click **Apply policy and tags**.

| Field | Description |
|-------|-------------|
| Policy name | Click the policy name to go to that backup policy. |
| Status | [Status of the backup policy](/docs/vpc?topic=vpc-backup-vpc-monitoring&interface=ui). |
| Last run time | The last scheduled run of the backup policy that created a backup. |
{: caption="Backup policies associated with a {{site.data.keyword.block_storage_is_short}} volume." caption-side="bottom"}

## Viewing {{site.data.keyword.block_storage_is_short}} volumes from the CLI
{: #viewing-block-storage-cli}
{: cli}

View details about a {{site.data.keyword.block_storage_is_short}} volume or summary information about all volumes from the CLI.

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

Log in to the IBM Cloud.
```sh
ibmcloud login --sso -a cloud.ibm.com
```
{: pre}

This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

### Viewing details about a {{site.data.keyword.block_storage_is_short}} volume from the CLI
{: #viewvol-cli}

Specify this command to show details about a volume.

```sh
ibmcloud is volume VOLUME_ID [--json]
```
{: pre}

The following example uses the volume ID to show volume details.

```sh
$ ibmcloud is volume demo-volume-update
Getting volume demo-volume-update under account Test Account as user test.user@ibm.com...
                                          
ID                                     r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac   
Name                                   demo-volume-update   
CRN                                    crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac   
Status                                 available   
Attachment state                       attached   
Capacity                               100   
IOPS                                   3000   
Bandwidth(Mbps)                        393   
Profile                                general-purpose   
Encryption key                         -   
Encryption                             provider_managed   
Resource group                         defaults   
Created                                2023-06-29T16:14:59+00:00   
Zone                                   us-east-1   
Health State                           ok   
Volume Attachment Instance Reference   Attachment type   Instance ID                                 Instance name   Auto delete   Attachment ID                               Attachment name      
                                       data              0757_11f5db7f-35a1-4678-bcbd-c85204e09507   kj-test-ro      false         0757-4dfc4384-c4b5-497e-bab3-6415f9c4d44b   otp      
                                          
Active                                 true   
Adjustable Capacity States             attached 
Adjustable IOPS States                     
Busy                                   false   
Tags                                   -  
```
{: screen}

In the example, the volume is attached to a virtual server instance, so the named and IDd of the volume attachment and instance are also displayed in the command output. The Active property is `true` because the virtual server instance to which the volume is attached is running. The `busy` property with the value `false` indicates that this volume is not performing an operation that must be serialized.

For more information about available command options, see [`ibmcloud is volume`](/docs/cli?topic=cli-vpc-reference#volume-view).

### Viewing all {{site.data.keyword.block_storage_is_short}} volumes from the CLI
{: #viewall-vol-cli}

Run this command to list summary information about all volumes:

```sh
ibmcloud is volumes [--json]
```
{: pre}

Specifying the resource group ID or name filters the list to volumes that belong to a resource group. When you omit this argument, it defaults to all resource groups. You can also view all volumes in a particular availability zone.

By default, the first 25 volumes are displayed per page.

The following example shows all volumes for all resource groups in your availability zone.

```sh
$ ibmcloud is volumes
Listing volumes in all resource groups and region us-east under account Test Account as user test.user@ibm.com...
ID                                          Name                                      Status      Capacity   IOPS   Profile           Attachment state   Attachment type   Zone        Resource group   
r014-0a7c28f3-3612-46e6-b874-51136c7f1def   concurrent-vol-09afy4vz700                unusable    20         3000   general-purpose   unattached         -                 us-east-1   defaults   
r014-faefcc1d-f899-4688-ae97-67e5079da702   concurrent-vol-1s26tgtqg70                unusable    20         3000   general-purpose   unattached         -                 us-east-1   defaults   
r014-f0e809bf-9afb-4006-b2a8-274f81f0f34e   concurrent-vol-8xif5f1tid0                unusable    20         3000   general-purpose   unattached         -                 us-east-1   defaults   
r014-b8e23307-e93e-4f7b-918f-7b2c2b14b132   concurrent-vol-gpwucqfpni0                available   20         3000   general-purpose   attached           data              us-east-1   defaults   
r014-a64beeee-be50-4c03-8cee-639106cea1e2   concurrent-vol-mh16478vln0                available   20         3000   general-purpose   attached           data              us-east-1   defaults   
r014-f14f8d39-2cf3-4f5d-b366-1d234f1c74aa   concurrent-vol-n7fcoxmb860                available   20         3000   general-purpose   attached           data              us-east-1   defaults   
r014-84ff8138-4f4f-434b-bdc3-45d1aaaa4329   csi-boot-vol-pgb1-oqpm7een                available   100        3000   general-purpose   attached           boot              us-east-1   Default   
r014-a1f6b311-6e4b-4e27-a216-a0b602471268   csi-boot-vol-qgbi-h76dy77d                available   100        3000   general-purpose   attached           boot              us-east-1   Default   
r014-158e904d-0d48-4090-b6c1-57617c1fcc20   csi-boot-vol-txmz-54wzen5m                available   100        3000   general-purpose   attached           boot              us-east-1   Default   
r014-dee9736d-08ee-4992-ba8d-3b64a4f0baac   demo-volume-update                        available   100        3000   general-purpose   attached           data              us-east-1   defaults   
r014-eef16365-17e3-4627-bc8b-c7c3dd1d6a81   kj-test-ro-boot-1629867631000             available   100        3000   general-purpose   attached           boot              us-east-1   defaults 
```
{: screen}

For more information about available command options, see [`ibmcloud is volumes`](/docs/cli?topic=cli-vpc-reference#volumes-list).

## Viewing {{site.data.keyword.block_storage_is_short}} volumes with the API
{: #viewing-block-storage-api}
{: api}

View {{site.data.keyword.block_storage_is_short}} volumes programmatically by making calls to the [VPC REST APIs](/apidocs/vpc). You can list all volumes and view details for a specific volume.

Before you begin, make sure that you [set up your API environment](/docs/vpc?topic=vpc-creating-block-storage#block-storage-api-prereqs).

### Viewing all {{site.data.keyword.block_storage_is_short}} volumes with the API
{: #viewall-vol-api}

Make a `GET /volumes` call to list summary information about all volumes. See the following example.

```sh
curl -X GET "$vpc_api_endpoint/v1/volumes?version=2022-12-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example. This example shows three data volumes. The first in the list is attached to an instance.

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes?limit=50"
  },
  "limit": 50,
  "volumes": [
    {
      "active": true,
      "bandwidth": 128,
      "busy": false,
      "capacity": 100,
      "created_at": "2022-12-09T06:26:17Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
      "health_reasons": [],
      "health_state": "ok",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/ccbe6fe1-5680-4865-94d3-687076a38293",
      "id": "ccbe6fe1-5680-4865-94d3-687076a38293",
      "iops": 1000,
      "name": "my-volume-1",
      "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
        "name": "general-purpose"
      },
      "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
        "id": "4bbce614c13444cd8fc5e7e878ef8e21",
        "name": "Default"
      },
      "status": "available",
      "status_reasons": [],
      "user_tags": [],
      "volume_attachments": [
        {
          "delete_volume_on_instance_delete": true,
          "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/33bd5872-7034-462b-9f3e-d400c49d347a/volume_attachments/b31c1a5a-122a-4e32-a10b-f2c31271de85",
          "id": "b31c1a5a-122a-4e32-a10b-f2c31271de85",
          "instance": {
            "crn": "crn:[...]",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/33bd5872-7034-462b-9f3e-d400c49d347a",
            "id": "33bd5872-7034-462b-9f3e-d400c49d347a",
            "name": "instance-1",
            "resource_type": "instance"
          },
          "name": "volume-attachment-1",
          "type": "data"
        }
      ],
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
        "name": "us-south-2"
      }
    },
    {
      "active": false,
      "bandwidth": 128,
      "busy": false,
      "capacity": 100,
      "created_at": "2022-12-08T16:46:54Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
      "health_reasons": [],
      "health_state": "ok",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/9de3e18c-cec9-4cac-a64a-0bdfab21e9d4",
      "id": "9de3e18c-cec9-4cac-a64a-0bdfab21e9d4",
      "iops": 1000,
      "name": "volume-2",
      "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
        "name": "general-purpose"
      },
      "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
        "id": "4bbce614c13444cd8fc5e7e878ef8e21",
        "name": "Default"
      },
      "status": "available",
      "status_reasons": [],
      "user_tags": [],
      "volume_attachments": [],
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
        "name": "us-south-2"
      }
    },
    {
      "active": false,
      "bandwidth": 128,
      "busy": false,
      "capacity": 100,
      "created_at": "2022-12-08T02:22:43Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
      "health_reasons": [],
      "health_state": "ok",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/89ba05e9-6e35-4964-9747-7ae3f9b30303",
      "id": "89ba05e9-6e35-4964-9747-7ae3f9b30303",
      "iops": 1000,
      "name": "volume-3",
      "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/5iops-tier",
        "name": "5iops-tier"
      },
      "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
        "id": "4bbce614c13444cd8fc5e7e878ef8e21",
        "name": "Default"
      },
      "status": "available",
      "status_reasons": [],
      "user_tags": [],
      "volume_attachments": [],
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
        "name": "us-south-2",
        "resource_type": "zone"
      }
    }
  ]
}
```
{: codeblock}


### Viewing volume details with the API
{: #viewvol-details-api}

Make a `GET /volumes/{id}` call to see details of a volume. See the following example.

```sh
curl -X GET "$vpc_api_endpoint/v1/volumes/$volume_id?version=2022-12-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response provides details of the volume, including capacity and IOPS, the volume status, and whether the volume is attached to an instance.

```json
{
  "active": true,
  "bandwidth": 128,
  "busy": false,
  "capacity": 100,
  "created_at": "2022-12-09T06:26:17Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "health_reasons": [],
  "health_state": "ok",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/ccbe6fe1-5680-4865-94d3-687076a38293",
  "id": "ccbe6fe1-5680-4865-94d3-687076a38293",
  "iops": 1000,
  "name": "my-volume-1",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
    "name": "general-purpose"
  },
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/4bbce614c13444cd8fc5e7e878ef8e21",
    "id": "4bbce614c13444cd8fc5e7e878ef8e21",
    "name": "Default"
  },
  "status": "available",
  "status_reasons": [],
  "user_tags": [],
  "volume_attachments": [
    {
      "delete_volume_on_instance_delete": true,
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/33bd5872-7034-462b-9f3e-d400c49d347a/volume_attachments/b31c1a5a-122a-4e32-a10b-f2c31271de85",
      "id": "b31c1a5a-122a-4e32-a10b-f2c31271de85",
      "instance": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/33bd5872-7034-462b-9f3e-d400c49d347a",
        "id": "33bd5872-7034-462b-9f3e-d400c49d347a",
        "name": "instance-1",
        "resource_type": "instance"
      },
      "name": "volume-attachment-1",
      "type": "data"
    }
  ],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
    "name": "us-south-2"
  }
}
```
{: codeblock}

### Extra properties for boot volumes
{: #viewvol-boot}

When you request to view details of boot volumes, two extra properties are returned in a `GET /volumes` and `GET /volumes/{id}` response.

* The `active` property indicates whether the virtual server instance to which a volume is attached is running or stopped. When `active = true`, the instance is running and operations that require a running instance such as creating an [image from that boot volume](/docs/vpc?topic=vpc-create-ifv#image-from-volume-vpc-api) also work.

* The `busy` property indicates whether this volume is performing an operation that must be serialized. If an operation requires serialization, the operation fails unless this property is `false`.

See the following example.

```json
  "active": "true",
  "busy": "false",
  "capacity": 100,
  "created_at": "2022-12-08T06:26:17Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/ccbe6fe1-5680-4865-94d3-687076a38293",
  "id": "ccbe6fe1-5680-4865-94d3-687076a38293",
  "iops": 300,
  "name": "boot-volume-1",
  .
  .
  .
```
{: codeblock}


## Next steps
{: #next-step-viewing-block-storage}

Create more volumes or manage your existing {{site.data.keyword.block_storage_is_short}} volumes.

* [Creating {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-managing-block-storage)
