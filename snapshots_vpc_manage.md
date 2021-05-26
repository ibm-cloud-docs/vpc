---

copyright:
  years: 2021
lastupdated: "2021-05-26"

keywords: snapshots, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance

subcollection: vpc

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}

# Managing Snapshots
{: #snapshots-vpc-manage}

You can delete snapshots that you no longer need. Rename existing snapshots to make them easier to identify. Verify IAM access. Verify snapshot statuses.
{:shortdesc}

## Deleting snapshots
{: #snapshots-vpc-delete}

You can delete a single snapshot or all snapshots for a volume. When you delete a single snapshot, the snapshot must meet to the following prerequisites:

* Be at the first snapshot in the UI list, with no child references. You can't delete a child snapshot.
* Be in a `stable` state.
* Not be actively restoring a volume.

An easy way to determine whether you can delete a snapshot is look in the [UI](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-list-ui) list of snapshots. The overflow menu has a **Delete** option when a snapshot is deletable.

You can also use the [CLI](#snapshots-vpc-delete-snapshot-cli) or [API](#snapshots-vpc-delete-snapshot-api) to list the snapshot by ID and then check the `deletable` attribute value (true or false).

You can delete all snapshots for a volume. Deleting all snapshots requires further confirmation in the UI. In the CLI and API, you identify the volume by ID.

## Deleting a single snapshot
{: #snapshots-vpc-delete-single-snapshot}

### Deleting a single snapshot by using the UI
{: #snapshots-vpc-delete-snapshot-ui}

You can delete a snapshot from the list of all snapshots.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
1. Click the overflow menu (ellipsis) in the row of the snapshot you want to delete.
1. From the overflow menu, select **Delete**.
1. Confirm the deletion and click **Delete**.

You can also delete a snapshot from the details page for a block storage volume.

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Select a volume from the list and click the volume name to go to the volume details page.
1. Click **Snapshots**. A list of snapshots that are taken of this volume are displayed, and you can do the following actions:
  * Click **Delete all** to delete all snapshots for this volume.
  * For the most recent snapshot (first in the list), click the overflow menu (ellipsis).
1. Select **Delete** from the overflow menu. This option does not appear if the snapshot is not deletable.
1. Confirm the deletion.

### Deleting a single snapshot by using the CLI
{: #snapshots-vpc-delete-snapshot-cli}

1. Downloaded, install, and initialize the following CLI plug-ins:
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plug-in

   For more information, see the [CLI Reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).
   
2. After you install the vpc-infrastructure plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

3. Use the `snapshot-delete` CLI command and specify the ID of the snapshot.

```
is snapshot-delete SNAPSHOT_ID 
```
{:pre}

4. Confirm deleting the snapshot. The response message indicates that the snapshot is deleted.

### Deleting a single snapshot from the API
{: #snapshots-vpc-delete-snapshot-api}

Make a `DELETE/snapshots/{snapshot_ID}` call to delete a specific snapshot by ID.

```
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2021-02-12&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{:screen}

## Deleting all snapshots
{: #snapshots-vpc-delete-all}

### Deleting all snapshots for a volume by using the UI
{: #snapshots-vpc-delete-all-ui}

To delete all snapshots for a volume by using the UI, follow these steps. 

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
1. Click the row to select the snapshot you want to delete.
1. From the overflow menu (ellipsis), select **Delete all for volume**.
1. Confirm the deletion by typing _delete_ and then click **Delete**.

### Deleting all snapshots for a volume from the API
{: #snapshots-vpc-delete-all-api}

Make a `DELETE/snapshots` call and specify the source volume ID for the `source_volume.id` parameter in the request.

```
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots?source_volume.id=_volume-id_&version=2021-02-12&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{:screen}

## Deleting snapshots from the block storage details page by using the UI
{: #snapshots-vpc-delete-from-volume}

You can delete the most recently created snapshot from the list of snapshots from the block storage volume details page. Optionally, you can delete all snapshots from this view.

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Select a volume from the list and click the volume name to go to the volume details page.
1. Click **Snapshots** to see a list of snapshots taken of this volume.
1. Click **Delete all** to delete all snapshots for this volume. 
1. Alternatively, delete the most recently created snapshot (first in the list):
   1. Click the overflow menu (ellipsis).
   1. Select **Delete** from the overflow menu. 
   1. Confirm the deletion.

## Naming snapshots
{: #snapshots-vpc-naming}

Consider naming the snapshot to indicate the volume that you copied. For example, _my-volume_ would be _my-volume-snapshot1_. Also, for quick identification, consider naming boot volumes by adding _boot_ as a prefix, such as _boot_my-volume-snapshot1_. As your list of snapshots grows, you can quickly identify the name and type of volume from which you created the snapshot.

Snapshot names adhere to the same requirements as volume names. Valid names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Snapshot names must begin with a lowercase letter and must be unique across the VPC. The UI provides name checking as a convenience. For example, if you end a snapshot name with a hyphen (-), the UI notifies you of the error. It also checks for duplicate names.

## Change the snapshot name
{: #snapshots-vpc-rename}

From the UI:

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**. 
1. Click the name of a snapshot from the list.
1. Click the pencil icon. 
1. Provide a [new name](#snapshots-vpc-naming) for the snapshot, save, and confirm your changes.

From the CLI:

Specify a `snapshot-update` command and provide the snapshot ID and new name.

```
ibmcloud is snapshot-update SNAPSHOT_ID --name SNAPSHOT_NAME [--output JSON] [-q, --quiet]
```
{:pre}

Example:

```
$ ibmcloud is snapshot-update b40ecbfa-296b-4592-b959-59459868d683 --name my-snapshot1-renamed
Updating snapshot b40ecbfa-296b-4592-b959-59459868d683 in resource group under account VPC 01 as user rtuser1@mycompany.com...

ID                 b40ecbfa-296b-4592-b959-59459868d683   
Name               my-snapshot1-renamed
CRN                crn:v1:public:is:us-south:a/23db6395-3466-4055-ada1-c072b6b749bf::
                   snapshot:b40ecbfa-296b-4592-b959-59459868d683   
Status             stable   
Source Volume      ID                                          Name      
                   c3f9ffa4-6609-4750-ad09-e8caea5d9e5c        demo-volume1      
                      
Progress           -  
Bootable           false   
Deletable          false   
Encryption         provider_managed   
Encryption key     -   
Minimum Capacity   100   
Size               1   
Source Image       ID                                          Name      
                   c348a188-bc70-4c08-afb7-cbcbde831be3        ibm-centos-7-6-minimal-amd64-2      
                      
Resource group     ID                                          Name      
                   64e81667-75d8-4803-9935-fb0ee5895c04        Default      
                      
Created            2021-02-17T14:11:56+08:00
```
{:screen}

From the API:

Make a `PATCH/snapshots` call and specify the snapshot ID and new name of the snapshot. 

```
curl -X PATCH \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2021-02-12&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}" \
     -d $'{
  "name": "my-snapshop1-renamed"
}'
```
{:codeblock}

## IAM roles for creating and managing snapshots
{: #snapshots-vpc-iam}

Snapshots require IAM permissions for role-based access control. Table 1 describes these roles as they pertain to snapshots actions.

| Snapshot action | IAM role |
|-----------------|----------|
| Create snapshots | Administrator, editor |
| Delete snapshots | Administrator, editor |
| Create volume from a snapshot<sup>1</sup> | Administrator, editor, operator |
| List snapshots | Administrator, editor, operator, viewer |
| View snapshot details | Administrator, editor, operator, viewer |
{: caption="Table 1. IAM roles for snapshots" caption-side="top"}
<sup>1</sup>Need to have administrator and editor privileges on the volume.

## Snapshot lifecycle states
{: #snapshots-vpc-status}

Table 1 describes the snapshot states in the snapshot lifecycle.

| Snapshot Status | Explanation |
|-----------------|-------------|
| Stable | The snapshot is available for restoring a volume. |
| Waiting | Snapshot information is being retrieved. |
| Pending | While the snapshot is being created, the percentage completed displays. |
| Failed | The snapshot failed to br created, the volume can't be restored from a snapshot. |
| Suspended | Snapshot is temporarily unavailable. |
| Updating | Information you changed about the snapshot is being updated. |
| Deleting | The snapshot is being [deleted](#snapshots-vpc-delete). |
| Deleted | The snapshot was deleted and is not available to restore volumes. |
{: caption="Table 2. Snapshot lifecycle states" caption-side="top"}

## Activity Tracker events for snapshots
{: #snapshots-vpc-at-events}

When you initiate activity to create a snapshot, specific Activity Tracker events are generated. 

* When you create a snapshot, an `is.snapshot.create` event is generated.
* When you list all snapshots or a snapshot by ID, an `is.snapshot.list` event is generated. 
* When you get details for a snapshot by ID, an `is.snapshot.read` event is generated. The name of the snapshot appears in the message when you list a single snapshot by ID.
* When you modify a snapshot, an `is.snapshot.update` event is generated.
* When you delete a snapshot, an `is.snapshot.delete` event is generated.
* When you restore a volume from a snapshot, an `is.snapshot.restore` event is generated. You see an `is.volume.volume.create` event that indicates that the volume was successfully created.

## Activity Tracker JSON examples for snapshot events
{: #snapshots-vpc-at-event-examples}

The following example shows JSON output of an Activity Tracker event that was generated after you successfully created a snapshot. The name that you gave the snapshot appears in the response message and reason code `Created`.

```
{
    "eventTime": "2021-02-16T17:59:07.57+0000",
    "action": "is.snapshot.create",
    "outcome": "success",
    "message": "Block Storage Snapshots for VPC: create my-snapshot-1",
    "initiator": {
        "id": "ABCid-45B7R6TVH4",
        "typeURI": "service/security/account/user",
        "name": "myname@mycompany.com",
        "host": {
            "address": "192.0.2.0"
        },
        "credential": {
            "type": "token"
        }
    },
    "target": {
        "id": "crn:v1::public:is::a/cf27ad23-60e6-47d8-a4c1-b63ac14488f1::snapshot:09ca2bab-c5c4-4c06-b034-dda9bbeb859c",
        "typeURI": "is.snapshot/snapshot",
        "name": "my-snapshot-1"
    },
    "observer": {
        "name": "ActivityTracker"
    },
    "reason": {
        "reasonCode": 201,
        "reasonType": "Created"
    },
    "severity": "normal",
    "requestData": {
        "generation": "2"
    },
    "responseData": {
        "responseURI": "/v1/snapshots/09ca2bab-c5c4-4c06-b034-dda9bbeb859c"
    },
    "dataEvent": false,
    "logSourceCRN": "crn:v1::public:is::a/cf27ad23-60e6-47d8-a4c1-b63ac14488f1::snapshot:09ca2bab-c5c4-4c06-b034-dda9bbeb859c",
    "saveServiceCopy": true
}
```
{:screen}

The following example shows an event that was generated when you list snapshot details by ID:

```
{
    "eventTime": "2021-02-16T17:55:25.60+0000",
    "action": "is.snapshot.read",
    "outcome": "success",
    "message": "Block Storage Snapshots for VPC: read my-snapshot-2",
    "initiator": {
        "id": "IBMid-50A7R6DVH5",
        "typeURI": "service/security/account/user",
        "name": "skinner2@us.ibm.com",
        "host": {
            "address": "192.0.2.0"
        },
        "credential": {
            "type": "token"
        }
    },
    "target": {
        "id": "crn:v1::public:is::a/ef0574dd126031eba03e554728ab939::snapshot:4e3252d7-cf32-4586-93e9-f7d9a497bed4",
        "typeURI": "is.snapshot/snapshot",
        "name": "my-snapshot-2"
    },
    "observer": {
        "name": "ActivityTracker"
    },
    "reason": {
        "reasonCode": 200,
        "reasonType": "OK"
    },
    "severity": "normal",
    "requestData": {
        "generation": "2"
    },
    "responseData": {
        "responseURI": "/v1/snapshots/4e3252d7-cf32-4586-93e9-f7d9a497bed4"
    },
    "dataEvent": false,
    "logSourceCRN": "crn:v1::public:is::a/ef0574dd126031eba03e554728ab939::snapshot:4e3252d7-cf32-4586-93e9-f7d9a497bed4",
    "saveServiceCopy": true
}
```
{:screen}

## Next Steps
{: #snapshots-vpc-manage-next-steps}

* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).
