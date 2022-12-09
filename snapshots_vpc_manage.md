---

copyright:
  years: 2021, 2022
lastupdated: "2022-12-09"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing Snapshots
{: #snapshots-vpc-manage}

You can delete snapshots that you no longer need and free space for new snapshots. Rename existing snapshots to make them easier to identify. Add user tags to snapshots for use by the VPC backup service. Verify {{site.data.keyword.iamshort}} access. Verify snapshot statuses.
{: shortdesc}

## Deleting snapshots
{: #snapshots-vpc-delete}

You can delete any snapshot for a volume or all snapshots for a volume. When you delete a snapshot, the it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

An easy way to determine whether you can delete a snapshot is look in the [UI](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-list-ui) list of snapshots and check its status.

You can delete all snapshots for a volume. Deleting all snapshots requires further confirmation in the UI. In the CLI and API, you identify the volume by ID.

## Delete snapshots in the UI
{: #snapshots-vpc-delete-snapshot-ui}
{: ui}

### Delete a single snapshot in the UI
{: #snapshots-vpc-delete-single-snapshot-ui}

You can delete a snapshot from the list of all snapshots.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
2. Click the overflow menu (ellipsis) in the row of the snapshot you want to delete.
3. From the overflow menu, select **Delete**.
4. Confirm the deletion and click **Delete**.

You can also delete a snapshot from the details page of a {{site.data.keyword.block_storage_is_short}} volume.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. Select a volume from the list, and click the volume name to go to the volume details page.
3. Click **Snapshots**. A list of snapshots that are taken of this volume are displayed, and you can take the following actions:
    * Click **Delete all** to delete all snapshots for this volume.
    * Click the overflow menu (ellipsis) to delete a specific snapshot.
4. Select **Delete** from the overflow menu. If the snapshot is actively restoring a volume, the delete operation does not work.
5. Confirm the deletion.

### Delete all snapshots for a volume in the UI
{: #snapshots-vpc-delete-all-ui}

To delete all snapshots for a volume in the UI, follow these steps.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to  the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
2. Click the row to select the snapshot that you want to delete.
3. From the overflow menu (ellipsis), select **Delete all for volume**.
4. Confirm the deletion by typing _delete_ and then click **Delete**.

### Delete snapshots from the {{site.data.keyword.block_storage_is_short}} details page in the UI
{: #snapshots-vpc-delete-from-volume}

You can delete the most recently created snapshot from the list of snapshots from the {{site.data.keyword.block_storage_is_short}} volume details page. Optionally, you can delete all snapshots from this view.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. Select a volume from the list and click the volume name to go to the volume details page.
3. Click **Snapshots** to see a list of snapshots taken of this volume.
4. Click **Delete all** to delete all snapshots for this volume.
5. Alternatively, select a single snapshot in the list for deletion and then:
   1. Click the overflow menu (ellipsis).
   2. Select **Delete** from the overflow menu.
   3. Confirm the deletion.

## Add tags to a snapshot in the UI
{: #snapshots-vpc-add-tags-ui}
{: ui}

You can add [user tags](/docs/vpc?topic=vpc-block-storage-about#storage-about-user-tags) and [access management tags](/docs/vpc?topic=vpc-block-storage-about#storage-about-user-tags) to your {{site.data.keyword.block_storage_is_short}} snapshots.

### Add user tags from the list of snapshots in the UI
{: #snapshots-vpc-add-tags-list-ui}

You can add user tags to an existing snapshot or when you create a snapshot. The tags can be used by a backup policy to create backups of the snapshot.

1. Navigate to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-list-ui).

2. Locate an _available_ snapshot created by _user_. (Snapshots that were created by a backup policy are identified as created by _Backup policy_.)

3. In the **Tags** column, snapshots with tags show a number indicating tags that are already applied. Snapshots without tags have an **Add tags** link. Click **Add tags**.

4. In the new window, type a tag in the User tags text box.

5. Click **Save**.

### Add tags from the snapshot details page using the UI
{: #snapshots-vpc-add-tags-details-ui}

Add user tags and access management tags to a {{site.data.keyword.block_storage_is_short}} snapshot.

1. Navigate to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-list-ui).

2. Click the name of a snapshot in the list.

3. On the snapshot details page, click the **Add tags** link.

4. In the new window, enter a user tag or access management tag in the respective fields.

5. Click **Save**.

## Delete snapshots from the CLI
{: #snapshots-vpc-delete-snapshot-cli}
{: cli}

Before you begin, make sure that you:

1. Download, install, and initialize the following [CLI plug-ins](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference):
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plug-in

2. After you install the VPC infrastructure plug-in, set the target to generation 2 by running the `ibmcloud is target --gen 2` command.

### Delete a single snapshot from the CLI
{: #snapshots-vpc-delete-single-snapshot-cli}

1. Navigate to the list of snapshots for a volume:

    ```sh
    ibmcloud is snapshots --volume VOLUME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
    ```
    {: pre}

2. Run the `snapshot-delete` command and specify the ID of the snapshot.

    ```sh
    ibmcloud is snapshot-delete SNAPSHOT_ID
    ```
    {: pre}

3. Confirm the deletion of the snapshot. The response message indicates that the snapshot is deleted.

### Delete all snapshots from the CLI
{: #snapshots-vpc-delete-all-snapshot-cli}}

1. Navigate to the list of snapshots for a volume:

    ```sh
    ibmcloud is snapshots --volume VOLUME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups]
    ```
    {: pre}

2. Enter the `snapshot-delete` command.

    ```sh
    ibmcloud is snapshot-delete
    ```
    {: pre}

3. Confirm the deletion of the snapshots. The response message indicates when the snapshots are deleted.

## Delete snapshots with the API
{: #snapshots-vpc-delete-snapshot-api}
{: api}

### Delete a single snapshot with the API
{: #snapshots-vpc-delete-single-snapshot-api}

Make a `DELETE/snapshots/{snapshot_ID}` call to delete a specific snapshot by ID.

```curl
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-12-09&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

### Delete all snapshots for a volume with the API
{: #snapshots-vpc-delete-all-api}

Make a `DELETE/snapshots` call and specify the source volume ID for the `source_volume.id` parameter in the request.

```curl
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots?source_volume.id=_volume-id_&version=2022-12-09&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

## Naming snapshots
{: #snapshots-vpc-naming}

Consider naming the snapshot to indicate the volume that you copied. For example, _my-volume_ would be _my-volume-snapshot1_. Also, for quick identification, consider naming boot volumes by adding _boot_ as a prefix, such as _boot_my-volume-snapshot1_. As your list of snapshots grows, you can quickly identify the name and type of volume from which you created the snapshot.

Snapshot names adhere to the same requirements as volume names. Valid names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Snapshot names must begin with a lowercase letter and must be unique across the VPC. The UI provides name checking as a convenience. For example, if you end a snapshot name with a hyphen (-), the UI notifies you of the error. It also checks for duplicate names.

## Rename a snapshot from the UI
{: #snapshots-vpc-rename-ui}
{: ui}

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
2. Click the name of a snapshot from the list.
3. Click the pencil icon.
4. Provide a [new name](#snapshots-vpc-naming) for the snapshot, save, and confirm your changes.

## Rename a snapshot from the CLI:
{: #snapshots-vpc-rename-cli}
{: cli}

Specify a `snapshot-update` command and provide the snapshot ID and new name.

```sh
ibmcloud is snapshot-update SNAPSHOT_ID --name SNAPSHOT_NAME [--output JSON] [-q, --quiet]
```
{: pre}

Example:

```sh
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
Created            2022-12-09T14:11:56+08:00
Captured           2022-12-09T14:31:11+08:00
```
{: screen}

## Add user tags to a snapshot from the CLI
{: #snapshots-vpc-add-tags-cli}
{: cli}

Specify a `snapshot-update` command with the `--tags` parameter to add user tags to a volume.

Use the same parameter to add tags to a volume when you create a snapshot by using `ibmcloud is snapshot-create`.
{: tip}

The following example adds user tags `env:test` and `env:prod` to a volume identified by ID.

```sh
ibmcloud is snapshot-update52129844-d84d-45aa-a811-7bcc941f2172 --name mysnapshot60 --tags env:test,env:prod
Updating snapshot52129844-d84d-45aa-a811-7bcc941f2172 under account VPC1 as user user@mycompany.com...

ID                    52129844-d84d-45aa-a811-7bcc941f2172
Name                   mysnapshot60
CRN                    crn:v1:staging:public:is:us-east:a/80a55b0b-1dc6-4c9d-a08b-d6b8cb4cf905:51204475-7330-40bc-94f6-e288bbe58e9a::
Status                 stable
Source volume          ID   Name
                       -    -

Progress               -
Bootable               false
Deletable              true
Encryption             provider_managed
Encryption key         -
Minimum capacity(GB)   103
Size(GB)               57
Resource group         ID                                 Name
                       5018a8564e8120570150b0764d39ebcc   Default

Created                2022-12-09T11:47:46.365+05:30
Captured               2022-12-09T11:58:16.265+05:30
Tags                   env:test,env:prod
```
{: codeblock}


## Rename a snapshot with the API
{: #snapshots-vpc-rename-api}
{: api}

Make a `PATCH/snapshots` call and specify the snapshot ID and new name of the snapshot.

```curl
curl -X PATCH \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-12-09&generation=2" \
   -H "Authorization: Bearer ${API_TOKEN}" \
   -d '{
     "name": "my-snapshop1-renamed"
   }'
```
{: codeblock}

## Add user tags to a snaphot with the API
{: #snapshots-vpc-add-tags-api}
{: api}

Make a `PATCH/snapshots` call and specify the snapshot ID and user tags. The following example adds user tags `env:test` and `env:prod` to the snapshot. When these tags are matched in a backup policy, a backup is triggered based on the backup plan schedule. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-policy-create).

```curl
curl -X PATCH \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-12-09&generation=2" \
    -H "Authorization: Bearer ${API_TOKEN}" \
    -d `{
       "user_tags": [
         "env:test",
         "env:prod"
       ]
    }'
```
{: codeblock}

## IAM roles for creating and managing snapshots
{: #snapshots-vpc-iam}

Snapshots require {{site.data.keyword.iamlong}} (IAM) permissions for role-based access control. Table 1 describes these roles as they pertain to snapshots actions.

| Snapshot action | IAM role |
|-----------------|----------|
| Create snapshots | Administrator, editor |
| Delete snapshots | Administrator, editor |
| Create volume from a snapshot[^createvol] | Administrator, editor, operator |
| List snapshots | Administrator, editor, operator, viewer |
| View snapshot details | Administrator, editor, operator, viewer |
{: caption="Table 1. {{site.data.keyword.iamlong}} roles for snapshots." caption-side="bottom"}

[^createvol]: Need to have administrator and editor privileges on the volume.

## Snapshot lifecycle states
{: #snapshots-vpc-status}

Table 2 describes the snapshot states in the snapshot lifecycle.

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
{: caption="Table 2. Snapshot lifecycle states" caption-side="bottom"}

## Activity Tracker events for snapshots
{: #snapshots-vpc-at-events}

When you initiate activity on a snapshot, specific Activity Tracker events are generated. These activities include creating, listing, modifying, and deleting snapshots. For more information about the Activity Tracker events, see [Snapshots events](/docs/vpc?topic=vpc-at-events#events-snapshots).

## Activity Tracker JSON examples for snapshot events
{: #snapshots-vpc-at-event-examples}

The following example shows JSON output of an Activity Tracker event that was generated after you successfully created a snapshot. The name that you gave the snapshot appears in the response message and reason code `Created`.

```json
{
    "eventTime": "2022-12-09T17:59:07.57+0000",
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
{: codeblock}

The following example shows an event that was generated when you list snapshot details by ID:

```json
{
    "eventTime": "2022-12-09T17:55:25.60+0000",
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
{: codeblock}

## Performance considerations when restoring a volume from a snapshot
{: #snapshots-perf}

Boot and data volume performance is initially degraded when restoring from a snapshot. During the restoration, your data is copied from {{site.data.keyword.cos_full}} to VPC {{site.data.keyword.block_storage_is_short}}. After the restoration process has completed, you caan realize full IOPS on your new volume.

## Managing security and compliance
{: #snapshots-vpc-manage-security}

Snapshots for VPC is integrated with the {{site.data.keyword.compliance_full}} to help you manage security and compliance for your organization. For snapshots, you can set up a goal that checks whether snapshots are encrypted by using customer-managed keys. By using the {{site.data.keyword.compliance_short}} to validate the snapshot resource configurations in your account against a profile, you can identify potential issues as they arise.

Since snapshots are created from {{site.data.keyword.block_storage_is_short}} volumes, {{site.data.keyword.block_storage_is_short}} goals provide an additional level of security. For information about monitoring security and compliance for VPC, including snapshots and {{site.data.keyword.block_storage_is_short}} volumes, see [Monitoring security and compliance posture with VPC](/docs/vpc?topic=vpc-manage-security-compliance#monitor-vpc). For information about creating security and compliance goals, see [Defining rules](/docs/security-compliance?topic=security-compliance-rules-define&interface=ui) in the Security and Compliance documentation.

## Next Steps
{: #snapshots-vpc-manage-next-steps}

* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).
