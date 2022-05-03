---

copyright:
  years: 2019, 2022
lastupdated: "2022-05-02"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Viewing block storage volumes
{: #viewing-block-storage}

View details about a {{site.data.keyword.block_storage_is_short}} volume or summary information about all volumes.
{: shortdesc}

## View information about block storage volumes in the UI
{: #viewvols}
{: ui}

List all block storage volumes and view details for a single volume. View attached block storage volume details in instance details. View all snapshots created from the block storage volume.

### View information about all block storage volumes in the UI
{: #viewvols-ui}

Navigate to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.

By default, block storage volumes display for all resource groups in your region. In the list of all **Block storage for VPC volumes**, you see the following information.

Want to set up automatic backups of your volumes? The cards at the top of the block storage list page tells you how! For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).
{: tip}

| Field | Description |
|-------|-------------|
| Region | The region where these volumes are located, for example, US South. Click the down arrow to see volumes in a different region in which you have an account.)
| Name | Click the name of the volume to see individual volume details. |
| Status | Status of the volume, which functions as the default filter for all rows. For information about volume statuses, see [Block Storage volume statuses](/docs/vpc?topic=vpc-managing-block-storage#status). |
| Location | Availability zone in your region, inherited from the VPC (for example, US South 1).|
| Size | Size of the volume you specified, in GBs.|
| Max IOPS | Maximum IOPS based on the volume profile for the volume. |
| Attachment type | Data, for a secondary volume attached to an instance, boot when attached as a boot volume, or blank for an unattached volume.|
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also use your own root keys in a Key Protect or HPCS instance to protect your data. For information, see [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption). |
| Tags | Number of user tags or access management tags applied to the volume. A tag is a label that you assign to the volume for easy filtering of resources. Click on the number in this column to view or edit the tags in the popup window. If there are no tags applied to the volume, click **Add tags** and add them in the popup window. For example, user tags applied to a volume can associate the volume with a backup policy for creating backups of the volume. For information about adding tags to a block storage volume, see [Add tags to block storage volumes for backup policies](https://test.cloud.ibm.com/docs/vpc?topic=vpc-managing-block-storage&interface=ui#block-storage-add-tags). For information about creating backups, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-about). Backup for VPC is only available to accounts authorized to preview the feature.
| Actions (...) | Click the icon (...) to display a menu of context-specific actions you can take. For example, an available, unattached volume would have menu options for attaching to an instance, renaming, and deleting the volume. An attached volume would allow for unattaching the volume from an instance and creating an [image from the volume](/docs/vpc?topic=vpc-image-from-volume-vpc). |
{: caption="Table 1. Details about all volumes" caption-side="bottom"}

By default, 10 volumes are shown in the list of all block storage volumes.  Change this default by clicking the Page Control down arrow and increase the list to 20 or 50 volumes.  Use the Page Control arrows after the list to go to the following page or return to the current page.

Actions menu selections change, depending on whether the volume is a boot volume, an attached data volume, or an unattached data volume. Table 2 describes these actions.

| Volume type | Action | Description |
|-------------|--------|-------------|
| **Boot** | Create image | Create an image from the boot volume. For more information, see [Create an image from the list of boot volumes](/docs/vpc?topic=vpc-create-ifv#import-custom-image-vol).
| | Create snapshot | Create a "bootable snapshot" from the boot volume. A snapshot is a point in time copy of the volume. For more information, see [Create a snapshot in the UI](https://test.cloud.ibm.com/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create-from-volume-list).
| | Detach from instance | If the boot volume is attached as a secondary volume, you can [detach it](/docs/vpc?topic=vpc-managing-block-storage#detach) from the instance. |
| **Data** | Create snapshot | Create a point in time copy of the data volume. | 
| | Detach from instance | [Detach](/docs/vpc?topic=vpc-managing-block-storage#detach) the data volume from the instance. |
| | Delete | [Delete](/docs/vpc?topic=vpc-managing-block-storage#delete) the volume. You must first detach the volume from an instance before deleting it. |
| Unattached (-) | Attach to an instance | [Attach](/docs/vpc?topic=vpc-attaching-block-storage) the volume to an available virtual server instance. |
| | Delete | Delete the unattached volume. |
{: caption="Table 2. Actions menu options for volumes." caption-side="bottom"}

### View details about a block storage volume
{: #view-vol-details-ui}

To view details about a block storage volume, navigate to the list of all block storage volumes and select a volume. By default, the Overview tab is selected for volume details. Click the **Snapshots and Backups** tab to view a list of snapshots created manually or by a backup policy.

Next to the name of the volume is the [volume status](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#status) and tags associated with this volume. Tags identify the resource. For example, user tags applied to a volume can associate the volume with a backup policy for creating backup snapshots of the volume. For information about creating backups, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-about). For more information about tags, see [Working with tags](/docs/account?topic=account-tag).

The Actions menu on the volume details page shows actions you can take, depending on whether the volume is a boot or data volume, and attached or unattached. For more information, see Table 4.

The block storage volumes details page shows volume details, attached virtual server instances, and backup policies. Table 3 describes this information.

| Field | Description |
|-------|-------------|
| **Volume details** | |
| Name  | Name of the volume you specified when you created the volume. Click the pencil icon to edit the volume name. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure. |
| Resource group | Resource group defined when you set up your VPC. Resource groups manage access to resources but do not affect billing or monitoring.|
| Attachment type | Data, for a secondary volume attached to an instance, boot when attached as a boot volume, or blank for an unattached volume.|
| ID | System-generated volume ID. |
| Created | System-generated date when the volume was created.|
| Location | Availability zone in your region.|
| Size | Size of the volume you specified. When the volume is attached to a virtual server instance and the volume is not at maximum capacity for its range, you can click the icon to expand the volume. For information on expanding volume capacity, see [Expanding block storage volume capacity (Beta)](/docs/vpc?topic=vpc-expanding-block-storage-volumes). |
| Profile | [IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) or [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile. Click the icon to [adjust IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) by selecting a different profile. |
| Max IOPS | Maximum IOPS value for a predefined IOPS tier or the value you specified for custom IOPS. |
| Throughput | The performance a disk can deliver, measured in Gigabits/second (Gbps). Based on your volume profile, throughput is calculated as the amount of IOPS * 16 K block size.|
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also use your own root keys to protect your data. The Encryption field shows the name of the key management service (KMS) you provisioned (for example, Key Protect) and **customer-managed**. For more information, see [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption)|
| Encryption Instance | _Optional._ A link to the provisioned KMS instance for a customer-managed encryption volume. |
| Key | _Optional._ The name and copiable ID of the root key used to encrypt the passphrase securing a customer-managed encryption volume. |
| Backup policies | Number of backup policies associated with the volume. Click the number link to go to the backup policies tab. |
| Snapshots | Number of snapshots created of the volume. Click the number link to go to the Snapshots and Backups tab. |
| **Attached instances** | Volumes attached to a virtual server instance are listed here. Click **Attach instance** to select an instance to attach this volume. For more information, see [Attaching a volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage). |
| Status | Attachment status, for example, _attached_ when the volume is attached to an instance and _attaching_ when in progress. |
| Name | Click the name of the virtual server instance to see instance details. |
| Auto delete | When _enabled_, the volume is automatically deleted when you delete the instance. Click the toggle to enable automatic deletion. |
{: caption="Table 3. Volume details" caption-side="bottom"}

Table 4 shows Actions menu options from the volume details page.

| Action | Description |
|--------|-------------|
| Create snapshot | Create a snapshot from a data volume or a "bootable snapshot" from a boot volume. Data volumes must be attached to a virtual server instance. For more information, see [Create a snapshot in the UI](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui#snapshots-vpc-create-ui).
| Create image | Create an image from the boot volume. For more information, see [Create an image from the list of boot volumes](/docs/vpc?topic=vpc-create-ifv#import-custom-image-vol).
| Expand volume | [Increase the size](/docs/vpc?topic=vpc-expand-block-storage-vol-concepts) of a data volume in GBs. |
| Edit IOPS profile | For data volumes that are attached to a virtual server instance, increase or decrease IOPS by [editing the IOPS profile](/docs/vpc?topic=vpc-adjusting-volume-iops). |
| Delete | [Delete](/docs/vpc?topic=vpc-managing-block-storage#delete) the volume. You must first detach the volume from an instance before deleting it. |
{: caption="Table 4. Actions menu options one the volume details page." caption-side="bottom"}

### View attached block storage volume details in instance details
{: #view-vol-details-instance-ui}

You can view information about an attached block storage volume from the **Virtual server instance details** page:

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances** and select an instance.

2. Under **Attached block storage volumes**, click the name of a volume to go to the volume details page.

### View all snapshots created from the block storage volume
{: #view-snapshots-for-volume}

If you created snapshots of a block storage boot or data volume, you can see the snapshots on the volume details page.

1. Navigate to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**. 

2. Select a volume from the list.

3. On the volume details page, click the **Snapshots and Backups** tab. A list of snapshots displays with the name, status, size, encryption type, and when it was created. It also shows whether the snapshot was created by the user or a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#baas-comparison). The snapshots display in descending order, with the most recently created snapshot on the top.

You can see details for a snapshot, create a new snapshot, and manage snapshots from this page. For example, the overflow menu (ellipsis) lets you delete the most recent snapshot. For more information, see:

* [View details of a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).
* [Create asnapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).
* [Delete all snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-all-ui).

### View all backup policies associated with a volume
{: #view-backup-policies-for-volume}

View all backup policies associated with a block storage volume. All policies that have the user tag applied to this volume are listed. To add volumes to a policy, add user tags to the volume that are in the backup policy's tags for target resources. When you remove tags from a volume that are in a backup policy, the volume won't be backed up.

1. Navigate to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.

2. Locate the volume you want and click the name link.

3. From the block storage volumes details page, click the **Backup policies** tab. Table 5 describes the information on the Backup policies page.

4. Click **Attach** to apply a new backup policy to this volume. In the side panel, select a backup policy from the drop down menu, and select the policy tags to apply to the volume. You can also view the plan details that will help you decide whether to use that policy. If satisfied, click **Apply policy and tags**.

| Field | Description |
|-------|-------------|
| Policy name | Click the policy name to go to that backup policy. |
| Status | [Status of the backup policy](/docs/vpc?topic=vpc-backup-service-manage&interface=ui#backup-policy-statuses). |
| Last run time | The last scheduled run of the backup policy that created a backup. |
{: caption="Table 5. Backup policies associated with a block storage volume." caption-side="bottom"}

## Viewing block storage volumes from the CLI
{: #viewing-block-storage-cli}
{: cli}

View details about a block storage volume or summary information about all volumes from the CLI.

Before you begin, [install the IBM Cloud CLI and VPC CLI plug-in](/docs/vpc?topic=vpc-creating-block-storage#before-creating-block-storage-cli).

### View details about a block storage volume from the CLI
{: #viewvol-cli}

Specify this command to show details about a volume.

```zsh
ibmcloud is volume VOLUME_ID [--json]
```
{: pre}

Example:

This example uses the volume ID to show volume details.

```bash
$ ibmcloud is volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Getting volume 933c8781-f7f5-4a8f-8a2d-3bfc711788ee under account MyAccount01 as user user1@mycompany.com...
ID                                      0738-933c8781-f7f5-4a8f-8a2d-3bfc711788ee
Name                                    demo_volume
Capacity                                100
IOPS                                    1000
Profile                                 custom
Encryption Key                          -
Encryption                              provider_managed
Profile                                 custom
Status                                  available
Resource Group                          Default(c16d1edde3fd4a71a0130aed371405a0)
Created                                 2021-04-22 10:09:28
Zone                                    us-south-1
Volume Attachment Instance Reference    none
```
{: screen}

If your volume is attached to a virtual server instance, the name and ID of the volume attachment and instance is also displayed.

### View all block storage volumes from the CLI
{: #viewall-vol-cli}

Run this command to list summary information about all volumes:

```zsh
ibmcloud is volumes [--json]
```
{: pre}

Example:

The following example shows all volumes for all resource groups in your availability zone.

```bash
$ ibmcloud is volumes
Listing volumes under account MyAccount 01 as user user1@mycompany.com...
ID                                     Name              Capacity   IOPS   Auto Delete   Encryption        Profile         Created               Status      Zone         Resource Group
0738-a3f4d60a-c9cf-4666-9a2a-0b1d85f92c19   demo_volume1      50         10     Manual        provider managed  generalpurpose   2021-04-22 11:04:46  pending     us-south-1   (c16d1edd-.)
0738-3aaa0beb-83ac-4b2f-b4f5-eab382d1a5aa   demo_volume2      50         100    Manual        provider managed  custom           2021-04-22 10:26:34  available   us-south-1   (c16d1edd-.)
0738-6d9713cf-9688-486d-b8c9-d9f1c6e7876c   demo_volume3      50         100    Manual        provider managed  custom           2021-04-22 10:39:24  available   us-south-1   (c16d1edd-.)
```
{: screen}

Specifying the resource group ID or name filters the list to volumes that belong to a resource group. When you omit this parameter, it defaults to all resource groups. You can also view all volumes in a particular availability zone.

By default, the first 25 volumes are displayed per page.

### View details about a volume attachment from the CLI
{: #viewvol-attachment-cli}

Run this command to view details of a volume attachment to a virtual server instance:

```zsh
ibmcloud is instance-volume-attachment INSTANCE_ID VOLUME_ATTACHMENT_ID [--json]
```
{: pre}

Specify an instance ID and a volume attachment ID.  Optionally, export the details in JSON format.

### View details about all volume attachments from the CLI
{: #viewvol-all-attachments-cli}

Run this command to view all volume attachments for a virtual server instance.

```zsh
ibmcloud is instance-volume-attachments INSTANCE_ID [--json]
```
{: pre}

### View volume profiles from the CLI
{: #viewvol-profiles-cli}

Run this command to list all volume profiles available in your region.

```zsh
ibmcloud is volume-profiles [--json]
```
{: pre}

Example:

```bash
$ ibmcloud is volume-profiles
Listing volume profiles under account MyAccount 01 as user user1@mycompany.com...
Name             Family   Crn
generalpurpose1  tiered   crn:v1:public:globalcatalog::::volume.profile:generalpurpose
generalpurpose2  tiered   crn:v1:public:globalcatalog::::volume.profile:generalpurpose
custom           custom   crn:v1:public:globalcatalog::::volume.profile:custom
generalpurpose3  tiered   crn:v1:public:globalcatalog::::volume.profile:generalpurpose
```
{: screen}

### View a specific volume profile from the CLI
{: #viewvol-profile-cli}

Run this command to view a specific volume profile in your region.

```zsh
ibmcloud is volume-profile PROFILE_NAME [--json]
```
{: pre}

PROFILE_NAME values are 10iops-tier, 5iops-tier, general-purpose, and custom.

Example:

```bash
$ ibmcloud is volume-profile generalpurpose1
Getting volume profile generalpurpose1 under account MyAccount 01 as user user1@mycompany.com...
Name     generalpurpose1
Family   tiered
Crn      crn:v1:public:globalcatalog::::volume.profile:generalpurpose
```
{: screen}

## Viewing block storage volumes with the API
{: #viewing-block-storage-api}
{: api}

View block storage volumes programically by making calls to the [VPC REST APIs](https://{DomainName}/apidocs/vpc){: external}. You can list all volumes and view details for a specific volume.

Before you begin, make sure that you [set up your API environment](/docs/vpc?topic=vpc-creating-block-storage#block-storage-api-prereqs).

### View all block storage volumes with the API
{: #viewall-vol-api}

Make a `GET /volumes` call to list summary information about all volumes. For example:

```curl
curl -X GET "$vpc_api_endpoint/v1/volumes?version=2021-04-20&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response looks like this. This example shows three data volumes. The first in the list is attached to an instance.

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
      "created_at": "2019-01-29T06:26:17Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
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
      "created_at": "2019-03-23T16:46:54Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
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
      "created_at": "2019-07-13T02:22:43Z",
      "crn": "crn:[...]",
      "encryption": "provider_managed",
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


### View a specific volume profile with the API
{: #viewvol-details-api}

Make a `GET /volumes/{id}` call to see details of a volume. For example:

```curl
curl -X GET "$vpc_api_endpoint/v1/volumes/$volume_id?version=2021-04-20&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response will provide details of the volume, including capacity and IOPS, the volume status, and whether the volume is attached to an instance.

```json
{
  "active": true,
  "bandwidth": 128,
  "busy": false,
  "capacity": 100,
  "created_at": "2021-06-29T06:26:17Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
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

### Additional properties for boot volumes
{: #viewvol-boot}

When viewing details for boot volumes, two additional properties are returned in a `GET /volumes` and `GET /volumes/{id}` response.

* The `active` property indicates whether the virtual server instance to which a volume is attached is running or stopped. When `active = true`, the instance is running and operations that require a running instance such as creating an [image from that boot volume](/docs/vpc?topic=vpc-create-ifv#image-from-volume-vpc-api) will work. 

* The `busy` property indicates whether this volume is performing an operation that must be serialized. If an operation requires serialization, the operation will fail unless this property is `false`.

Example response:

```json
  "active": "true",
  "busy": "false",
  "capacity": 100,
  "created_at": "2021-06-08T06:26:17Z",
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

### View volume profiles withh the API
{: #viewvol-profiles-api}

To list all volume profiles available in your region, make a `GET /volume/profiles` call. For example:

```curl
curl -X GET "$vpc_api_endpoint/v1/volume/profiles?version=2021-04-20&generation=2" 
-H "Authorization: $iam_token"
```
{: pre}

When all volume profiles are available, you can see a response like this:

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles?limit=50"
  },
  "limit": 50,
  "profiles": [
    {
      "family": "tiered",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/10iops-tier",
      "name": "10iops-tier"
    },
    {
      "family": "tiered",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/5iops-tier",
      "name": "5iops-tier"
    },
    {
      "family": "tiered",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
      "name": "general-purpose"
    },
    {
      "family": "custom",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/custom",
      "name": "custom"
    }
  ],
  "total_count": 4
}
```
{: codeblock}

## Next steps
{: #next-step-viewing-block-storage}

Create more volumes or manage your existing block storage volumes.

* [Creating block storage volumes](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing block storage volumes](/docs/vpc?topic=vpc-managing-block-storage)
