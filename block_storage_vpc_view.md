---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-02"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing {{site.data.keyword.block_storage_is_short}} volumes
{: #viewing-block-storage}

View details about a {{site.data.keyword.block_storage_is_short}} volume or summary information about all volumes.
{: shortdesc}

## Viewing information about {{site.data.keyword.block_storage_is_short}} volumes in the console
{: #viewvols}
{: ui}

List all {{site.data.keyword.block_storage_is_short}} volumes and view details for a single volume. View attached {{site.data.keyword.block_storage_is_short}} volume details in instance details. View all snapshots that were created from the {{site.data.keyword.block_storage_is_short}} volume.

### Viewing information about all {{site.data.keyword.block_storage_is_short}} volumes in the console
{: #viewvols-ui}

Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.

By default, {{site.data.keyword.block_storage_is_short}} volumes are displayed in sets of 10 for all resource groups in your region. Change this preset by clicking the Page Control down arrow and increase the list to 20 or 50 volumes. Use the Page Control arrows to go to the following page or return. In the list of all **{{site.data.keyword.block_storage_is_short}} volumes**, you see the following information.

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
| Tags | Number of user tags that are applied to the volume. Click the number in this column to view or edit the tags in the new window. If no tags were applied to the volume, click **Add tags** and add them in the new window. User tags can associate the volume with a [backup policy](/docs/vpc?topic=vpc-backup-service-about) for creating backups of the volume. For more information, see [Adding user tags that are associated with a backup policy to a volume in the console](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#add-user-tags-volumes-ui).
| Actions | Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") to display a menu of context-specific actions you can take.|
{: caption="Details about all volumes" caption-side="bottom"}

Actions menu selections change depending on whether the volume is a boot volume, an attached data volume, or an unattached data volume.
- When the volume that you're viewing is a boot volume, the available actions are **Create image**, **Create snapshot**, and **Detach from instance**.
- When the volume that you're viewing is an attached data volume, the available actions are **Create snapshot**, **Detach from instance**, and **Delete**.
- When the volume that you're viewing is an unattached data volume, the available actions are **Attach to instance**, and **Delete**.

For more information about these actions, see the following topics:
- [Creating an image from a volume in the console](/docs/vpc?topic=vpc-create-ifv&interface=ui#create-image-from-volume-vpc-ui)
- [Creating a snapshot in the console](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui#snapshots-vpc-create-ui)
- [Detaching a volume from a virtual server instance](/docs/vpc?topic=vpc-managing-block-storage#detach)
- [Attaching a volume to a virtual server instance](/docs/vpc?topic=vpc-attaching-block-storage)
- [Deleting a volume in the console](/docs/vpc?topic=vpc-managing-block-storage#delete).

### Viewing details of a {{site.data.keyword.block_storage_is_short}} volume
{: #view-vol-details-ui}

To view details about a {{site.data.keyword.block_storage_is_short}} volume, go to the list of all {{site.data.keyword.block_storage_is_short}} volumes and select a volume.

Next to the name of the volume is the [volume status](/docs/vpc?topic=vpc-block-storage-vpc-monitoring#block-storage-vpc-status) and tags that are associated with this volume. [User tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-user-tags) identify the resource. When user tags are associated with a backup policy, they are used for creating [backup snapshots](/docs/vpc?topic=vpc-backup-service-about) of the volume. With [Access management tags](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-mgt-tags), you can create flexible resource groupings for managing access. For more information about these tags, see [Working with tags](/docs/account?topic=account-tag).

#### Volume-specific information.
{: #view-vol-details-overview}

The page has 3 tabs. By default, the Overview tab is selected for volume details.

| Field | Description |
|-------|-------------|
| **Volume details** | |
| Name  | The name of the volume you specified when you created the volume. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit the volume name. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. Volume names must be unique for the account and for the region. |
| Volume ID | System-generated volume ID. |
| Health |  Health monitors the overall health of the volume, such as I/O performance and data consistency. Volume health statuses are `OK` or `degraded`. Volumes in a degraded state have less than OK performance and capacity, or they experience connection problems. Volumes that are being restored from a snapshot also show a degraded state. The service displays a possible reason for the degraded state so you can resolve it. For more information, see [{{site.data.keyword.block_storage_is_short}} volume health states](/docs/vpc?topic=vpc-block-storage-vpc-monitoring#block-storage-vpc-health-states). |
| Resource group | Resource group defined when you set up your VPC. Resource groups manage access to resources but do not affect billing or monitoring.|
| Attachment type | Data, for a secondary volume attached to an instance, boot when attached as a boot volume, or blank for an unattached volume.|
| Created date | System-generated date when the volume was created.|
| Location | Availability zone in your region.|
| Size | Size of the volume you specified. When the volume is attached to a virtual server instance and the volume is not at maximum capacity for its range, you can click the icon to expand the volume. For more information, see [expanding {{site.data.keyword.block_storage_is_short}} volume capacity (Beta)](/docs/vpc?topic=vpc-expanding-block-storage-volumes). |
| Profile | This field shows the [volume profile](/docs/vpc?topic=vpc-block-storage-profiles): [sdp](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#defined-performance-profile), [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom), or one of the [IOPS tier](/docs/vpc?topic=vpc-block-storage-profiles#tiers) profiles.|
| Max IOPS | The maximum IOPS value for a predefined IOPS tier or the value you specified for custom IOPS. |
| Throughput | The field shows the allocated bandwidth of the storage volume, which is measured in Gigabits per second (Gbps). Throughput is calculated as the result of the number of IOPS the volume is provisioned for times the throughput multiplier. Depending on the [volume profile](/docs/vpc?topic=vpc-block-storage-profiles), the throughput multiplier can be 16 KB or 256 KB. For volumes that are created with the `sdp` profile, the throughput value is customizable in the range of 1000-8192 Mbps.|
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also use your own root keys to protect your data. The Encryption field shows the name of the key management service (KMS) you provisioned (for example, {{site.data.keyword.keymanagementserviceshort}}) and **customer-managed**. For more information, see [Creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).|
| Encryption Instance | _Optional._ A link to the provisioned KMS instance for a customer-managed encryption volume. |
| Key | _Optional._ The name and copiable ID of the root key that is used to encrypt the passphrase, which secures a customer-managed encryption volume. |
| Backup policies | The number of backup policies that are associated with the volume. Click the number link to go to the backup policies tab. |
| Snapshots | The number of snapshots that were created of the volume. Click the number link to go to the Snapshots and Backups tab. |
| **Attached virtual server** | Volumes attached to a virtual server instance are listed here. Click **Attach** to select an instance to attach this volume. For more information, see [Attaching a volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage). |
| Status | This field tracks the overall lifecycle state of the volume, which ranges from volume creation to volume deletion. Attachment status, for example, is _attached_ when the volume is attached to an instance and _attaching_ when in progress. |
| Name | Click the name of the virtual server instance to see instance details. |
| Auto delete | When _enabled_, the volume is automatically deleted when you delete the instance. Click the toggle to enable automatic deletion. |
| **Backup policies** | Shows backup policies that are associated with this volume. To associate backup policies, you can add a backup policy's tags for target resources to this volume. Click **Apply** to select a backup policy, then apply its tags for the target resource to the volume. |
{: caption="Volume details" caption-side="bottom"}

The Actions menu selections change depending on whether the volume is a boot volume, an attached data volume, or an unattached data volume.
- **Create a snapshot** - you can create a snapshot from an attached data volume or a boot volume. For more information, see [Create a snapshot in the console](/docs/vpc?topic=vpc-snapshots-vpc-create&interface=ui#snapshots-vpc-create-ui).
- **Create image** - you can create an image from a boot volume. For more information, see [Creating an image from a volume in the console](/docs/vpc?topic=vpc-create-ifv&interface=ui#create-image-from-volume-vpc-ui).
- **Expand volume** - you can [Increase the size](/docs/vpc?topic=vpc-about-increasing-volume-capacity) of an attached volume in increments of 1 GB. For more information, see [Increasing capacity of a data volume](/docs/vpc?topic=vpc-expanding-block-storage-volumes&interface=ui) and [Increasing capacity of a boot volume](/docs/vpc?topic=vpc-resize-boot-volumes&interface=ui).
- **Edit IOPS profile** - you can change the performance characteristics of an attached data volume by increasing or decreasing IOPS by [editing the IOPS profile](/docs/vpc?topic=vpc-adjusting-volume-iops). |
- **Delete** - you can delete an unattached volume. For more information, see [Deleting a data volume in the console](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#delete).

To view a list of snapshots of this volume that were created manually or by a backup policy, click the **Snapshots and Backups** tab.

#### Viewing all snapshots of the {{site.data.keyword.block_storage_is_short}} volume
{: #view-snapshots-for-volume}

If you created snapshots of a {{site.data.keyword.block_storage_is_short}} boot or data volume, you can see the snapshots on the second tab of the volume details page.

Click the **Snapshots and Backups** tab to see the list of snapshots that includes information such as the name, status, size, encryption type, and the creation date of the snapshots. The list also shows whether the snapshot was created by the user or by a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#baas-comparison). The snapshots display in descending order, with the most recently created snapshot in first place.

You can see details for a snapshot, create a snapshot, and manage snapshots from the Volume details page. For example, from the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), you can delete the most recent snapshot. For more information, see one of the following topics:

* [View details of a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).
* [Create a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).
* [Delete all snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-delete-all-ui).

#### Viewing all backup policies associated with a volume
{: #view-backup-policies-for-volume}

View all backup policies associated with a {{site.data.keyword.block_storage_is_short}} volume on the 3rd tab of the volume details page. All policies that have the user tag that is applied to this volume are listed.

Click the **Backup policies** tab. All policies that have the user tag that is applied to this volume are listed. The list provides the following information:

   | Field | Description |
   |-------|-------------|
   | Policy name | Click the policy name to go to that backup policy. |
   | Status | [Status of the backup policy](/docs/vpc?topic=vpc-backup-vpc-monitoring&interface=ui). |
   | Last run time | The last scheduled run of the backup policy that created a backup. |
   {: caption="Backup policies associated with a {{site.data.keyword.block_storage_is_short}} volume." caption-side="bottom"}

You can click **Attach** to apply a new backup policy to this volume. In the side panel, select a backup policy from the menu, and select the policy tags to apply to the volume. You can also view the plan details that can help you decide whether to use that policy. If satisfied, click **Apply policy and tags**.

When you want to add volumes to a policy, [add user tags to the volume](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#add-user-tags-volumes-ui) that are in the backup policy's tags for target resources. When you remove tags from a volume that are in a backup policy, the volume is no longer backed up by that policy.

For more information, see [Applying backup policies to resources](/docs/vpc?topic=vpc-backup-use-policies&interface=ui).

### Viewing attached {{site.data.keyword.block_storage_is_short}} volume details in instance details
{: #view-vol-details-instance-ui}

You can also view information about an attached {{site.data.keyword.block_storage_is_short}} volume from the **Virtual server instance details** page:

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances** and select an instance.

2. Under **Attached Block Storage volumes**, click the name of a volume to go to the volume details page.

## Viewing {{site.data.keyword.block_storage_is_short}} volumes from the CLI
{: #viewing-block-storage-cli}
{: cli}

View details about a {{site.data.keyword.block_storage_is_short}} volume or summary information about all volumes from the CLI.

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

### Viewing all volumes from the CLI
{: #viewall-vol-cli}

Run the following command to list summary information about all volumes:

```sh
ibmcloud is volumes [--json]
```
{: pre}

Specifying the resource group ID or name filters the list to volumes that belong to a resource group. When you omit this argument, it defaults to all resource groups. You can also view all volumes in a particular availability zone. By default, the first 25 volumes are displayed per page.

The following example shows all volumes for all resource groups in your availability zone.


```sh
ibmcloud is volumes
```
{: pre}

```sh
Listing volumes in all resource groups and region us-south under account Test Account as user test.user@ibm.com...

ID                                          Name                                            Status      Capacity   IOPS    Profile           Attachment state   Attachment type   Zone         Resource group   Catalog Offering Version   Catalog Offering Plan   Storage Generation   
r006-1dad641f-3a17-4117-8bb6-e30e09e64dff   my-2nd-data-volume                              available   9600       48000   5iops-tier        unattached         -                 us-south-2   Default          -                          -                       1   
r006-f336511e-ece5-4407-9c8e-ef030c94aaa0   my-3rd-data-volume                              available   4800       48000   10iops-tier       unattached         -                 us-south-2   Default          -                          -                       1   
r006-45ca6ddd-9942-4dd2-9ae9-d3edcb9fa076   my-data-volume-1                                available   300        3000    5iops-tier        unattached         -                 us-south-2   Default          -                          -                       1   
r006-6a1ac889-d232-4beb-84dd-93c63cf9a156   my-data-volume-2                                available   300        3000    5iops-tier        unattached         -                 us-south-2   Default          -                          -                       1   
r006-862ddcd3-59a6-4c69-9a10-275c894195b9   my-new-boot-volume                              available   250        3000    general-purpose   unattached         -                 us-south-2   Default          -                          -                       1   
r006-145fb6c8-8e03-4a9f-bcd7-3bb131d30f05   my-sdp-volume                                   available   250        3000    sdp               attached.          -                 us-south-2   Default          -                          -                       2   
r006-693f555e-fec7-4afb-b1e3-a16bf8b0d744   my-virtual-server-instance-boot-1756849222000   available   100        3000    general-purpose   attached           boot              us-south-2   Default          -                          -                       1   
```
{: screen}

For more information about available command options, see [`ibmcloud is volumes`](/docs/cli?topic=cli-vpc-reference#volumes-list).

### Viewing details about a volume from the CLI
{: #viewvol-cli}

Run the following command to show details about a specific volume. You can use the volume's name or ID to identify the volume.

```sh
ibmcloud is volume VOLUME_ID [--json]
```
{: pre}

See the following example.

```sh
ibmcloud is volume my-sdp-volume
```
{: pre}

```sh
Getting volume my-sdp-volume  under account under account Test Account as user test.user@ibm.com...
ID                                     r006-145fb6c8-8e03-4a9f-bcd7-3bb131d30f05   
Name                                   my-sdp-volume   
CRN                                    crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-145fb6c8-8e03-4a9f-bcd7-3bb131d30f05   
Status                                 available   
Attachment state                       attached   
Capacity                               250   
IOPS                                   3000   
Bandwidth(Mbps)                        1000   
Profile                                sdp   
Encryption key                         -   
Encryption                             provider_managed   
Resource group                         Default   
Created                                2025-06-23T16:35:10+00:00   
Zone                                   us-south-2   
Health State                           ok   
Volume Attachment Instance Reference   Attachment type   Instance ID                                 Instance name                Auto delete   Attachment ID                               Attachment name      
                                       data              0726_6cf2d984-e825-458d-8048-11572b72d040   my-virtual-server-instance   false         0726-beec02ac-7edf-4b59-ab9f-7c004bf4d8fa   outpour-perm-cosmic-gander      
                                          
Active                                 true   
Adjustable IOPS                        true   
Adjustable Capacity States             unattached,attached   
Adjustable IOPS States                 unattached,attached   
Busy                                   false   
Tags                                   -   
Storage Generation                     2
```
{: screen}

In the example, the volume is attached to a virtual server instance, so the names and IDs of the volume attachment and instance are also displayed in the command output. The `Active` property is `true` because the virtual server instance to which the volume is attached is running. The `Busy` property with the value `false` indicates that this volume is not performing an operation that must be serialized.

For more information about available command options, see [`ibmcloud is volume`](/docs/cli?topic=cli-vpc-reference#volume-view).

### Extra CLI properties for boot volumes
{: #viewvol-boot-cli}

When you request to view details of boot volumes, a few extra properties are returned in the response. See the following example:

```sh
ibmcloud is volume r006-77851646-75da-4bd0-8bbd-ed86fdec0b0e
```
{: pre}

```sh
Getting volume r006-77851646-75da-4bd0-8bbd-ed86fdec0b0e under account Test Account as user test.user@ibm.com...

ID                                     r006-77851646-75da-4bd0-8bbd-ed86fdec0b0e
Name                                   vsi-test-boot-1712686424000
CRN                                    crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-77851646-75da-4bd0-8bbd-ed86fdec0b0e
Status                                 available
Attachment state                       attached
Capacity                               100
IOPS                                   3000
Bandwidth(Mbps)                        393
Profile                                general-purpose
Encryption key                         -
Encryption                             provider_managed
Resource group                         defaults
Created                                2025-04-01T18:18:57+00:00
Zone                                   us-south-2
Health State                           ok
Volume Attachment Instance Reference   Attachment type   Instance ID                                 Instance name   Auto delete   Attachment ID                               Attachment name
                                       boot              0727_a9910bc9-3ea0-40d2-b3de-2b03e752f989   vsi-lvm-test    true          0727-7ff0df1b-ba0b-40c7-ad99-122819342f39   prissy-reason-unselect-overcome

Operating system                       CentOS Stream 9 - Minimal Install (amd64)
Source image                           ID                                          Name
                                       r006-0950e619-325e-446e-b895-e0bdd21dd1ea   ibm-centos-stream-9-amd64-9

Active                                 true
Adjustable IOPS                        false
Busy                                   false
Tags                                   -
Storage Generation                     1
Allowed Use API Version                2025-03-31
Allowed Use Bare Metal Server          true
Allowed Use Instance                   true
```
{: screen}

The allowed-use properties are inherited from the source image or snapshot that the parent boot volume was created from. By using the CLI or API, you can override these values when you create an instance or when you update the boot volume. You must have the `is.volume.volume.manage-allowed-use` IAM role to make these updates.

These properties comprise a Boolean [Common Expression Language](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external} expression. When the expression is evaluated to be `true`, the virtual server instance or bare metal server provisioning is allowed with the boot volume or an image that was created from the boot volume. When the expression is evaluated to be `false`, the provisioning is blocked. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=cli).

## Viewing {{site.data.keyword.block_storage_is_short}} volumes with the API
{: #viewing-block-storage-api}
{: api}

View {{site.data.keyword.block_storage_is_short}} volumes programmatically by making calls to the [VPC REST APIs](/apidocs/vpc). You can list all volumes and view details for a specific volume.

Before you begin, make sure that you [set up your API environment](/docs/vpc?topic=vpc-creating-block-storage#block-storage-api-prereqs).

### Viewing all volumes with the API
{: #viewall-vol-api}

Make a `GET /volumes` call to list summary information about all volumes. See the following example.

```sh
curl -X GET "$vpc_api_endpoint/v1/volumes?version=2025-01-212&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response looks like the following example. This example shows three data volumes. The second in the list is attached to an instance.

```json
[
  {
    "active": false,
    "attachment_state": "unattached",
    "bandwidth": 393,
    "busy": false,
    "capacity": 100,
    "created_at": "2025-01-22T00:54:01.000Z",
    "crn": "crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34",
    "encryption": "provider_managed",
    "health_reasons": [],
    "health_state": "ok",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34",
    "id": "r006-5ed4006b-3dac-4c95-8eeb-4aa9a85cbd34",
    "iops": 3000,
    "name": "my-data-volume",
    "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
        "name": "general-purpose"
    },
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/6edefe513d934fdd872e78ee6a8e73ef",
        "id": "6edefe513d934fdd872e78ee6a8e73ef",
        "name": "defaults"
    },
    "source_snapshot": {
        "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-8428038a-a399-4894-8c84-c8d7a4a75fae",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-8428038a-a399-4894-8c84-c8d7a4a75fae",
        "id": "r006-8428038a-a399-4894-8c84-c8d7a4a75fae",
        "name": "wdc-fst-rstore-c6a092f34118-4505",
        "resource_type": "snapshot"
    },
    "status": "available",
    "status_reasons": [],
    "storage_generation": 1,
    "adjustable_capacity_states": [
        "attached"
    ],
    "user_tags": [
        "env:prod",
        "env:test"
    ],
    "volume_attachments": [],
    "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2",
        "name": "us-south-2"
    }
  },
  {
    "active": false,
    "allowed_use": {
      "api_version": "2025-03-31",
      "instance": "true",
      "bare_metal_server": "false"
    },
    "attachment_state": "attached",
    "bandwidth": 393,
    "busy": false,
    "capacity": 100,
    "created_at": "2024-04-01T16:48:16.000Z",
    "crn": "crn:v1:bluemix:public:is:us-south-1:a/a1234567::volume:r006-b5f15a27-ee2b-4d49-9344-9f3c17d42903",
    "encryption": "provider_managed",
    "health_reasons": [],
    "health_state": "ok",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-b5f15a27-ee2b-4d49-9344-9f3c17d42903",
    "id": "r006-b5f15a27-ee2b-4d49-9344-9f3c17d42903",
    "iops": 3000,
    "name": "my-vsi--boot-volume-1711989834000",
    "operating_system": {
      "architecture": "amd64",
      "dedicated_host_only": false,
      "display_name": "Ubuntu Linux&reg; 20.04 LTS Focal Fossa Minimal Install (amd64)",
      "family": "Ubuntu Linux",
      "gpu_supported": [],
      "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/ubuntu-20-04-amd64",
      "name": "ubuntu-20-04-amd64",
      "resource_type": "operating_system",
      "vendor": "Canonical",
      "version": "20.04 LTS Focal Fossa Minimal Install"
    },
    "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
        "name": "general-purpose"
    },
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/6edefe513d934fdd872e78ee6a8e73ef",
        "id": "6edefe513d934fdd872e78ee6a8e73ef",
        "name": "defaults"
    },
    "source_image": {
        "crn": "crn:v1:bluemix:public:is:us-south:a/811f8abfbd32425597dc7ba40da98fa6::image:r006-0e578411-53ab-44ba-a0f8-d003d0011993",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/images/r006-0e578411-53ab-44ba-a0f8-d003d0011993",
        "id": "r006-0e578411-53ab-44ba-a0f8-d003d0011993",
        "name": "ibm-redhat-8-6-minimal-amd64-7",
        "resource_type": "image"
    },
    "status": "available",
    "status_reasons": [],
    "storage_generation": 1,
    "adjustable_capacity_states": [
        "attached"
    ],
    "user_tags": [
        "backup-policy-tag"
    ],
    "volume_attachments": [
        {
            "delete_volume_on_instance_delete": true,
            "device": {
                "id": ""
            },
            "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/0717_70aa00c6-65c9-4523-8a43-893c6fa0d87d/volume_attachments/0717-0fbaf561-6390-48a4-bb28-240b20ded36e",
            "id": "0717-0fbaf561-6390-48a4-bb28-240b20ded36e",
            "instance": {
                "crn": "crn:v1:bluemix:public:is:us-south-1:a/a1234567::instance:0717_70aa00c6-65c9-4523-8a43-893c6fa0d87d",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/0717_70aa00c6-65c9-4523-8a43-893c6fa0d87d",
                "id": "0717_70aa00c6-65c9-4523-8a43-893c6fa0d87d",
                "name": "my-vsi-test",
                "resource_type": null
            },
            "name": "perfectly-parting-humble-skewer",
            "type": "boot"
        }
    ],
    "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
        "name": "us-south-1"
    }
  },
  {
    "active": false,
    "attachment_state": "unattached",
    "bandwidth": 393,
    "busy": false,
    "capacity": 10,
    "created_at": "2021-09-10T15:56:30.000Z",
    "crn": "crn:v1:bluemix:public:is:us-south-3:a/a1234567::volume:r006-ed2b09db-36da-4cd1-b862-fed933465fcc",
    "encryption": "provider_managed",
    "health_reasons": [],
    "health_state": "ok",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-ed2b09db-36da-4cd1-b862-fed933465fcc",
    "id": "r006-ed2b09db-36da-4cd1-b862-fed933465fcc",
    "iops": 3000,
    "name": "my-other-data-volume",
    "profile": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
        "name": "general-purpose"
    },
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/6edefe513d934fdd872e78ee6a8e73ef",
        "id": "6edefe513d934fdd872e78ee6a8e73ef",
        "name": "defaults"
    },
    "status": "available",
    "status_reasons": [],
    "storage_generation": 1,
    "adjustable_capacity_states": [
        "attached"
    ],
    "user_tags": [],
    "volume_attachments": [],
    "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3",
        "name": "us-south-3"
    }
  }
]
```
{: codeblock}

### Viewing volume details with the API
{: #viewvol-details-api}

Make a `GET /volumes/{id}` call to see details of a volume. See the following example.

```sh
curl -X GET "$vpc_api_endpoint/v1/volumes/$volume_id?version=2025-01-21&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

A successful response provides details of the volume, including capacity and IOPS, the volume status, and whether the volume is attached to an instance.

```json
{
  "active": false,
  "adjustable_capacity_states": ["attached"],
  "adjustable_iops_states": ["attached"],
  "allowed_use": {
    "api_version": "2025-03-31",
    "instance": "true",
    "bare_metal_server": "false"
  },
  "attachment_state": "attached",
  "bandwidth": 1000,
  "busy": false,
  "capacity": 100,
  "created_at": "2024-10-07T23:16:53.000Z",
  "crn": "crn:v1:bluemix:public:is:us-south-1:a/a1234567::volume:r006-1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:v1:bluemix:public:kms:us-south:a/a1234567:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd6179"
  },
  "health_reasons": [],
  "health_state": "ok",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
  "id": "r006-1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
  "iops": 1000,
  "name": "my-volume",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/custom",
    "name": "custom"
  },
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/fee82deba12e4c0fb69c3b09d1f12345",
    "id": "fee82deba12e4c0fb69c3b09d1f12345",
    "name": "Default"
  },
  "resource_type": "volume",
  "status": "available",
  "status_reasons": [],
  "storage_generation": 1,
  "user_tags": [],
  "volume_attachments": [
    {
      "delete_volume_on_instance_delete": false,
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/0717_e21b7391-2ca2-4ab5-84a8-b92157a633b0/volume_attachments/0717-82cbf856-9cbb-45fb-b62f-d7bcef32399a",
      "id": "0717-82cbf856-9cbb-45fb-b62f-d7bcef32399a",
      "instance": {
        "crn": "crn:v1:bluemix:public:is:us-south-1:a/a1234567::instance:0717_e21b7391-2ca2-4ab5-84a8-b92157a633b0",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/0717_e21b7391-2ca2-4ab5-84a8-b92157a633b0",
        "id": "0717_e21b7391-2ca2-4ab5-84a8-b92157a633b0",
        "name": "my-instance",
        "resource_type": "instance"
      },
      "name": "my-volume-attachment",
      "type": "data"
    }
  ],
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

### Extra API properties for boot volumes
{: #viewvol-boot}

When you request to view details of boot volumes, a couple of extra properties are returned in the response for `GET /volumes` and `GET /volumes/{id}` requests.

* The `active` property indicates whether the virtual server instance to which a volume is attached is running or stopped. When `active = true`, the instance is running and operations that require a running instance such as creating an [image from the boot volume](/docs/vpc?topic=vpc-create-ifv#image-from-volume-vpc-api) also work.

* The `busy` property indicates whether this volume is performing an operation that must be serialized. If an operation requires serialization, the operation fails unless this property is `false`.

* The allowed-use expressions are inherited from the source image or snapshot that the parent boot volume was created from. You can update allowed-use expressions only on detached boot volumes. Using the CLI or API, you can override these values when you create an instance or by updating the boot volume. You must have the `is.volume.volume.manage-allowed-use` IAM role to make these updates. These properties comprise a Boolean [Common Expression Language](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external} expression. When the expression is evaluated to be `true`, the virtual server instance or bare metal server provisioning is allowed with the boot volume or an image that was created from the boot volume. When the expression is evaluated to be `false`, the provisioning is blocked. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).

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
  "allowed_use": {
    "api_version": "2025-03-31",
    "instance": "true",
    "bare_metal_server": "false"
  },
  .
  .
  .
```
{: codeblock}

## Viewing {{site.data.keyword.block_storage_is_short}} volumes with Terraform
{: #viewing-block-storage-terraform}
{: terraform}

You can use Terraform to view information about your {{site.data.keyword.block_storage_is_short}} volumes.

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

### Viewing all volumes with Terraform
{: #viewall-vol-terraform}

Import the list of volumes that belong to an account as a read-only data source. You can filter by volume name, zone name, attachment state, encryption type, operating system family (applicable for boot volumes) or operating system architecture (applicable for boot volumes).

```terraform
data "ibm_is_volumes" "example" {
}
```
{: codeblock}

The attributes that are exported include the total count of volumes and the list of volumes. The nested attributes include volume ID, name, creation date, size, IOPS, CRN, access and user tags, profile, encryption type and key, lifecycle state, health state and reason, operating system, and other attributes.

For more information, see [ibm_is_volumes](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_volumes){: external}.

### Viewing volume details with Terraform
{: #viewvol-details-terraforn}

Import the details of a volume as a read-only data source. You must identify the volume by ID or name.

```terraform
data "ibm_is_volume" "example1" {
  name = ibm_is_volume.example.name
}
```
{: codeblock}

The attributes that are exported include the total count of volumes and the list of volumes. The nested attributes include volume ID, name, creation date, size, IOPS, CRN, access and user tags, profile, encryption type and key, lifecycle state, health state and reason, operating system, and other attributes.

For more information, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_volume){: external}.

### Extra Terraform properties for boot volumes
{: #viewvol-boot-terraform}

The allowed-use expressions are inherited from the source image or snapshot that the parent boot volume was created from. You can update allowed-use expressions only on detached boot volumes. Using Terraform, you can override these values when you create an instance or by updating the boot volume. You must have the `is.volume.volume.manage-allowed-use` IAM role to make these updates. These properties comprise a Boolean [Common Expression Language](https://github.com/google/cel-spec/blob/master/doc/langdef.md){: external} expression. When the expression is evaluated to be `true`, the virtual server instance or bare metal server provisioning is allowed with the boot volume or an image that was created from the boot volume. When the expression is evaluated to be `false`, the provisioning is blocked. For more information, see [Adding allowed-use expressions to custom images](/docs/vpc?topic=vpc-custom-image-allowed-use-expressions&interface=ui).

## Next steps
{: #next-step-viewing-block-storage}

Create more volumes or manage your existing {{site.data.keyword.block_storage_is_short}} volumes.

* [Creating {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-managing-block-storage)
