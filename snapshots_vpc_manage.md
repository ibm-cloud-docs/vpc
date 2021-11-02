---

copyright:
  years: 2021
lastupdated: "2021-11-02"

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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing Snapshots
{: #snapshots-vpc-manage}

You can delete snapshots that you no longer need and free space for new snapshots. Rename existing snapshots to make them easier to identify. Verify IAM access. Verify snapshot statuses.
{: shortdesc}

## Deleting snapshots
{: #snapshots-vpc-delete}

You can delete a snapshot anywhere in the chain of snapshots for a volume, or all snapshots for a volume. When you delete a snapshot, the it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

An easy way to determine whether you can delete a snapshot is look in the [UI](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-list-ui) list of snapshots and check its status.

You can delete all snapshots for a volume. Deleting all snapshots requires further confirmation in the UI. In the CLI and API, you identify the volume by ID.

## Delete snapshots by using the UI
{: #snapshots-vpc-delete-snapshot-ui}
{: ui}

### Deleting a single snapshot by using the UI
{: #snapshots-vpc-delete-snapshot-ui}

You can delete a snapshot from the list of all snapshots.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
2. Click the overflow menu (ellipsis) in the row of the snapshot you want to delete.
3. From the overflow menu, select **Delete**.
4. Confirm the deletion and click **Delete**.

You can also delete a snapshot from the details page for a block storage volume.

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. Select a volume from the list and click the volume name to go to the volume details page.
3. Click **Snapshots**. A list of snapshots that are taken of this volume are displayed, and you can do the following actions:
    * Click **Delete all** to delete all snapshots for this volume.
    * For the most recent snapshot (first in the list), click the overflow menu (ellipsis).
4. Select **Delete** from the overflow menu. This option does not appear if the snapshot is not deletable.
5. Confirm the deletion.

### Deleting all snapshots for a volume by using the UI
{: #snapshots-vpc-delete-all-ui}

To delete all snapshots for a volume by using the UI, follow these steps. 

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
1. Click the row to select the snapshot you want to delete.
1. From the overflow menu (ellipsis), select **Delete all for volume**.
1. Confirm the deletion by typing _delete_ and then click **Delete**.

### Delete snapshots from the block storage details page by using the UI
{: #snapshots-vpc-delete-from-volume}

You can delete the most recently created snapshot from the list of snapshots from the block storage volume details page. Optionally, you can delete all snapshots from this view.

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. Select a volume from the list and click the volume name to go to the volume details page.
3. Click **Snapshots** to see a list of snapshots taken of this volume.
4. Click **Delete all** to delete all snapshots for this volume. 
5. Alternatively, select a single snapshot in the list for deletion and then:
   1. Click the overflow menu (ellipsis).
   2. Select **Delete** from the overflow menu. 
   3. Confirm the deletion.

## Delete snapshots from the CLI
{: #snapshots-vpc-delete-snapshot-cli}
{: cli}

Before you begin, make sure that you:

1. Download, install, and initialize the following [CLI plug-ins](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference):
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plug-in

2. After you install the VPC infrastructure plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

### Deleting a single snapshot by using the CLI
{: #snapshots-vpc-delete-snapshot-cli}

1. Navigate to the list of snapshots for a volume:

    ```
    ibmcloud is snapshots --volume VOLUME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
    ```
    {: pre}

2. Run the `snapshot-delete` command and specify the ID of the snapshot.

    ```
    ibmcloud is snapshot-delete SNAPSHOT_ID
    ```
    {: pre}

3. Confirm deleting the snapshot. The response message indicates that the snapshot is deleted.

### Delete all snapshots from the CLI
{: #snapshots-vpc-delete-all-snapshot-cli}}

1. Navigate to the list of snapshots for a volume:

    ```
    ibmcloud is snapshots --volume VOLUME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups]
    ```
    {: pre}

2. Run the `snapshot-delete` command.

    ```
    ibmcloud is snapshot-delete
    ```
    {: pre}

3. Confirm deleting the snapshots. The response message indicates when the snapshots are deleted.

## Delete snapshots from the API
{: #snapshots-vpc-delete-snapshot-api}
{: api}

### Delete a single snapshot from the API
{: #snapshots-vpc-delete-snapshot-api}

Make a `DELETE/snapshots/{snapshot_ID}` call to delete a specific snapshot by ID.

```
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2021-02-12&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: screen}

### Delete all snapshots for a volume from the API
{: #snapshots-vpc-delete-all-api}

Make a `DELETE/snapshots` call and specify the source volume ID for the `source_volume.id` parameter in the request.

```
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots?source_volume.id=_volume-id_&version=2021-10-12&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: pre}

## Naming snapshots
{: #snapshots-vpc-naming}

Consider naming the snapshot to indicate the volume that you copied. For example, _my-volume_ would be _my-volume-snapshot1_. Also, for quick identification, consider naming boot volumes by adding _boot_ as a prefix, such as _boot_my-volume-snapshot1_. As your list of snapshots grows, you can quickly identify the name and type of volume from which you created the snapshot.

Snapshot names adhere to the same requirements as volume names. Valid names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Snapshot names must begin with a lowercase letter and must be unique across the VPC. The UI provides name checking as a convenience. For example, if you end a snapshot name with a hyphen (-), the UI notifies you of the error. It also checks for duplicate names.

## Rename a snapshot using the UI
{: #snapshots-vpc-rename-ui}
{: ui}

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**. 
2. Click the name of a snapshot from the list.
3. Click the pencil icon. 
4. Provide a [new name](#snapshots-vpc-naming) for the snapshot, save, and confirm your changes.

## Rename a snapshot from the CLI:
{: #snapshots-vpc-rename-cli}
{: cli}

Run the `snapshot-update` command and provide the snapshot ID and new name.

```
ibmcloud is snapshot-update SNAPSHOT_ID --name SNAPSHOT_NAME [--output JSON] [-q, --quiet]
```
{: pre}

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
Encryption         provider_managed   
Encryption key     -   
Minimum Capacity   100   
Size               1   
Source Image       ID                                          Name      
                   c348a188-bc70-4c08-afb7-cbcbde831be3        ibm-centos-7-6-minimal-amd64-2      
                      
Resource group     ID                                          Name      
                   64e81667-75d8-4803-9935-fb0ee5895c04        Default      
                      
Created            2021-10-27T14:11:56+08:00
```
{: screen}

## Rename a snapshot from the API
{: #snapshots-vpc-rename-api}
{: api}

Make a `PATCH/snapshots` call and specify the snapshot ID and new name of the snapshot. 

```
curl -X PATCH \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2021-10-12&generation=2" \
   -H "Authorization: Bearer ${API_TOKEN}" \
   -d '{
     "name": "my-snapshop1-renamed"
   }'
```
{: codeblock}

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

When you initiate activity on a snapshot, specific Activity Tracker events are generated. These activities include creating, listing, modifying, and deleting snapshots. For a list of these Activity Tracker events, see [Snapshots events](/docs/vpc?topic=vpc-at-events#events-snapshots).

## Activity Tracker JSON examples for snapshot events
{: #snapshots-vpc-at-event-examples}

The following example shows JSON output of an Activity Tracker event that was generated after you successfully created a snapshot. The name that you gave the snapshot appears in the response message and reason code `Created`.

```
{
    "eventTime": "2021-10-16T17:59:07.57+0000",
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
{: screen}

The following example shows an event that was generated when you list snapshot details by ID:

```
{
    "eventTime": "2021-10-16T17:55:25.60+0000",
    "action": "is.snapshot.read",
    "outcome": "success",
    "message": "Block Storage Snapshots for VPC: read my-snapshot-2",
    "initiator": {
        "id": "IBMid-50A7R6DVH5",
        "typeURI": "service/security/account/user",
        "name": "myuser@mycompany.com",
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
{: screen}

## Next Steps
{: #snapshots-vpc-manage-next-steps}

* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).
