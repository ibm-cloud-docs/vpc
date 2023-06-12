---

copyright:
  years: 2021, 2023
lastupdated: "2023-02-17"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing snapshots
{: #snapshots-vpc-manage}

You can manage existing snapshots in several ways. Rename existing snapshots to make them easier to identify. Add user tags to snapshots for use by the VPC backup service. Enable of disable fast restore copies of a snapshot. Delete snapshots that you no longer need and free up space for new snapshots. Verify {{site.data.keyword.iamshort}} access. Verify snapshot statuses.
{: shortdesc}

## Naming snapshots
{: #snapshots-vpc-naming}

Consider naming the snapshot to indicate the volume that you copied. For example, _my-volume_ would be _my-volume-snapshot1_. Also, for quick identification, consider naming boot volumes by adding _boot_ as a prefix, such as _boot_my-volume-snapshot1_. As your list of snapshots grows, you can quickly identify the name and type of volume from which you created the snapshot.

Snapshot names adhere to the same requirements as volume names. Valid names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Snapshot names must begin with a lowercase letter and must be unique across the VPC. The UI provides name checking as a convenience. For example, if you end a snapshot name with a hyphen (-), the UI notifies you of the error. It also checks for duplicate names.

## Rename a snapshot in the UI
{: #snapshots-vpc-rename-ui}
{: ui}

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
2. Click the name of a snapshot from the list.
3. Click the pencil icon.
4. Provide a [new name](#snapshots-vpc-naming) for the snapshot, save, and confirm your changes.

## Rename a snapshot from the CLI
{: #snapshots-vpc-rename-cli}
{: cli}

### Prerequisites for issuing commands from the CLI
{: #snapshots-vpc-rename-cli-requirement}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

2. Select the current generation of VPC. 
   ```sh
   ibmcloud is target --gen 2
   ```
   {: pre}

3. Set the `IBMCLOUD_IS_FEATURE_SNAPSHOT` environment variable to `true`.
   ```zsh
   export IBMCLOUD_IS_FEATURE_SNAPSHOT=true
   ```
   {: pre}

### Renaming a snapshot from the CLI
{: #vpc-rename-snapshot-cli}

To rename a snapshot, issue the `ibmcloud is snapshot-update` command and provide the snapshot ID and new name.

```sh
ibmcloud is snapshot-update SNAPSHOT_ID --name SNAPSHOT_NAME
```
{: pre}

See the following example.

```sh
cloudshell:~$ ibmcloud is snapshot-update r138-e6664842-b370-496a-9ae7-da3fb647707c --name snappy-snap-snap
Updating snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c under account Test Account as user test.user@ibm.com...
                          
ID                     r138-e6664842-b370-496a-9ae7-da3fb647707c   
Name                   snappy-snap-snap   
CRN                    crn:v1:bluemix:public:is:eu-de:a/a10d63fa66daffc9b9b5286ce1533080::snapshot:r138-e6664842-b370-496a-9ae7-da3fb647707c   
Status                 stable   
Clones                 Zone      Available   Created      
                       eu-de-3   true        2023-02-17T20:28:53+00:00      
                       eu-de-1   true        2023-02-17T18:53:57+00:00      
                          
Source volume          ID                                          Name      
                       r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   vicky-block-test1      
                          
Bootable               false   
Encryption             provider_managed   
Encryption key         -   
Minimum capacity(GB)   20   
Size(GB)               1   
Resource group         ID                                 Name      
                       a0eb5d9062af485fa5bb2c6999c74eac   test-snap      
                          
Created                2023-02-17T18:53:57+00:00   
Captured at            2023-02-17T18:53:57+00:00   
Tags                   -   
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshots`](/docs/vpc?topic=vpc-vpc-reference#snapshots).

## Rename a snapshot with the API
{: #snapshots-vpc-rename-api}
{: api}

Make a `PATCH /snapshots` call and specify the snapshot ID and new name of the snapshot.

```curl
curl -X PATCH \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-01-12&generation=2" \
   -H "Authorization: Bearer ${API_TOKEN}" \
   -d '{
     "name": "my-snapshop1-renamed"
   }'
```
{: codeblock}


## Add tags to a snapshot in the UI
{: #snapshots-vpc-add-tags-ui}
{: ui}

You can add [user tags](/docs/vpc?topic=vpc-block-storage-about#storage-about-user-tags) and [access management tags](/docs/vpc?topic=vpc-block-storage-about#storage-about-user-tags) to your {{site.data.keyword.block_storage_is_short}} snapshots. You can add user tags to an existing snapshot or when you create a snapshot. The tags can be used by a backup policy to create backups of the snapshot.

### Add user tags from the snapshots list screen
{: #snapshots-vpc-add-tags-list-ui}

1. Go to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-list-ui).
2. Locate an _available_ snapshot created by _user_. (Snapshots that were created by a backup policy are identified as created by _Backup policy_.)
3. In the **Tags** column, snapshots with tags show a number that indicates the tags that are already applied. Snapshots without tags have an **Add tags** link. Click **Add tags**.
4. In the new window, type a tag in the User tags text box.
5. Click **Save**.

### Add tags from the snapshot details page
{: #snapshots-vpc-add-tags-details-ui}

1. Go to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-list-ui).
2. Click the name of a snapshot in the list.
3. On the snapshot details page, click the **Add tags** link.
4. In the new window, enter a user tag or access management tag in the respective fields.
5. Click **Save**.

When the user tags are matched with a backup policy, a backup is triggered based on the backup plan schedule. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-policy-create).

## Add tags to a snapshot from the CLI
{: #snapshots-vpc-add-tags-cli}
{: cli}

Specify a `snapshot-update` command with the `--tags` option to add user tags to a volume.

Use the same option to add tags to a volume when you create a snapshot by using `ibmcloud is snapshot-create`.
{: tip}

The following example adds user tags `env:test` and `env:prod` to a volume that is identified by its ID.

```sh
cloudshell:~$ ibmcloud is snapshot-update r138-e6664842-b370-496a-9ae7-da3fb647707c --name snappy-snap-snap --tags env:test,env:prod
Updating snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c under account Test Account as user test.user@ibm.com...
                          
ID                     r138-e6664842-b370-496a-9ae7-da3fb647707c   
Name                   snappy-snap-snap   
CRN                    crn:v1:bluemix:public:is:eu-de:a/a10d63fa66daffc9b9b5286ce1533080::snapshot:r138-e6664842-b370-496a-9ae7-da3fb647707c   
Status                 stable   
Clones                 Zone      Available   Created      
                       eu-de-3   true        2023-02-17T20:28:53+00:00      
                       eu-de-1   true        2023-02-17T18:53:57+00:00      
                          
Source volume          ID                                          Name      
                       r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   vicky-block-test1      
                          
Bootable               false   
Encryption             provider_managed   
Encryption key         -   
Minimum capacity(GB)   20   
Size(GB)               1   
Resource group         ID                                 Name      
                       a0eb5d9062af485fa5bb2c6999c74eac   test-snap      
                          
Created                2023-02-17T18:53:57+00:00   
Captured at            2023-02-17T18:53:57+00:00   
Tags                   env:test,env:prod   
```
{: screen}

When the user tags are matched with a backup policy, a backup is triggered based on the backup plan schedule. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-policy-create).

## Add user tags to a snapshot with the API
{: #snapshots-vpc-add-tags-api}
{: api}

Make a `PATCH /snapshots` call and specify the snapshot ID and user tags. The following example adds user tags `env:test` and `env:prod` to the snapshot. 

```curl
curl -X PATCH \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-01-12&generation=2" \
    -H "Authorization: Bearer ${API_TOKEN}" \
    -d `{
       "user_tags": [
         "env:test",
         "env:prod"
       ]
    }'
```
{: codeblock}

When the user tags are matched with a backup policy, a backup is triggered based on the backup plan schedule. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-policy-create).

## Edit fast restore zones in the UI
{: #snapshots-edit-fast-restore}
{: ui}

You can edit the zones where fast restore clones are stored in the UI. You can add or remove zones as needed.

1. Select a snapshot from the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-list-ui).
2. From the overflow menu, click **Edit fast restore**.
3. From the side panel, select or deselect the zones for fast restore in your region. Review the billing update based on your selection.
4. Click **Save**. You're returned to the [snapshots details page](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-snapshot-ui). The Fast restore section shows the new zone initially with a _pending_ status.

Fast restore information is updated when you refresh. Zone information is updated to show enabled or disabled, depending on your changes.

## Create a snapshot clone for fast restore from the CLI
{: #snapshots-fast-restore-cli}
{: cli}

To create a zonal copy of a snapshot, issue the `ibmcloud is snapshot-clone-create` command with the snapshot ID and the zone or zones where you want to create copies. The following command example creates the fast restore clone of `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146` in the `eu-de-3` zone.

```sh
cloudshell:~$ ibmcloud is snapshot-clone-create r138-4463eb2c-4913-43b1-b9bf-62a94f74c146  --zone eu-de-3
Creating zonal clone of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
               
Zone        eu-de-3   
Available   false   
Created     2023-02-17T20:29:21+00:00   
Href        https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-3 
```
{: screen}

The snapshot clone appears to be unavailable while the snapshot clone is created. It takes only a few seconds. Issue the `ibmcloud is snapshot-cl` command with the snapshot ID and the clone target zone to see the new snapshot clone as available.

```sh
cloudshell:~$ ibmcloud is snapshot-cl r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 eu-de-3
Getting zonal clone eu-de-3 of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
               
Zone        eu-de-3   
Available   true   
Created     2023-02-17T20:29:21+00:00   
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-clone-create`](/docs/vpc?topic=vpc-vpc-reference#snapshots-cli).

## Delete a snapshot clone from the CLI
{: #snapshots-delete-clone-cli}
{: cli}

To delete a snapshot clone, issue the `ibmcloud is snapshot-clone-delete` command with the Snapshot ID and the zone where you want the snapshot copy removed. The following command example deletes the fast restore copy of the snapshot `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146` in the `eu-de-3` zone.

```sh
@cloudshell:~$ ibmcloud is snapshot-clone-delete r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 eu-de-3
This will delete zonal clone eu-de-3 for snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 and cannot be undone. Continue [y/N] ?> y
Deleting zonal clone eu-de-3 for snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
OK
Deletion request for zonal snapshot clone eu-de-3 has been accepted.
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-clone-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshots-cli).

## Create a snapshot clone for fast restore with the API
{: #snapshots-fast-restore-api}
{: api}

To create [fast restore snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=api#snapshots_vpc_fast_restore), you create a clone of the snapshot in a different zone. You then [restore a volume](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-API) from that clone.

In the API, you create a clone for an existing snapshot by making a `PUT /snapshots/{id}/clones/{zone_name}` call to clone the snapshot to the specified zone. See the following example.

```curl
curl -X PUT \
"$vpc_api_endpoint/v1/snapshots/5e160469-0837-48a7-8973-e44c8d5fd85a/clones/us-south-1&version=2022-12-22&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "available": true,
  "created_at": "2022-12-22T20:35:38.600Z",
  "zone": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
    "name": "us-south-1"
  }
}
```
{: codeblock}

You can also specify the `clone` property when you create a snapshot of a volume. See the following example.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2022-12-12&generation=2" \
-H "Authorization: $iam_token" \
-d '{
    "clones": [
        {
        "zone": {
            "name": "us-south-1"
        }
        }
    ],
    "name": "my-snapshot2",
    "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"
    },
    "source_volume": {
        "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8"
    },
    "user_tags": [
        "env:test",
        "env:prod"
    ]
}'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "bootable": false,
  "clones": [
      "available": true,
      "created_at": "2022-12-12T20:18:38.600Z",
      "zone": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1",
        "name": "us-south-1"
     }
  ],
  "created_at": "2021-12-12T20:18:18Z",
  "crn": "crn:[...]",
  "deletable": false,
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  .
  .
  .
}  
```
{: codeblock}

## Delete a snapshot clone with the API
{: #snapshots-delete-clone-api}
{: api}

Make a `DELETE /v1/snapshots/{id}/clones/{zone-name}` call to delete a snapshot clone in the specified zone. You can't undo the delete when the snapshot clone is deleted.

See the following example.

```curl
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots/fde0b8d5-2d75-4c28-af7d-12ffc3ae2a55/clones/us-south-1&version=2022-12-22&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

## Delete snapshots in the UI
{: #snapshots-vpc-delete-snapshot-ui}
{: ui}

You can delete any snapshot for a volume or all snapshots for a volume. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

An easy way to determine whether you can delete a snapshot is to look in the [console](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-list-ui) for the list of snapshots and check its status.
You can delete all snapshots for a volume. Deleting all snapshots requires further confirmation in the UI.

### Delete a single snapshot in the UI
{: #snapshots-vpc-delete-single-snapshot-ui}

You can delete a snapshot from the list of all snapshots.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
2. Click the Actions menu (ellipsis) in the row of the snapshot that you want to delete.
3. Select **Delete**.
4. Confirm the deletion and click **Delete**.

You can also delete a snapshot from the details page of a {{site.data.keyword.block_storage_is_short}} volume.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. Select a volume from the list, and click the volume name to go to the volume details page.
3. Click **Snapshots**. A list of snapshots that are taken of this volume are displayed, and you can take the following actions:
    * Click **Delete all** to delete all snapshots for this volume.
    * Click the Actions menu (ellipsis) to delete a specific snapshot.
4. Select **Delete**. If the snapshot is actively restoring a volume, the delete operation does not work.
5. Confirm the deletion.

### Delete all snapshots for a volume in the UI
{: #snapshots-vpc-delete-all-ui}

To delete all snapshots for a volume in the UI, follow these steps.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Snapshots**.
2. Click the row to select the snapshot that you want to delete.
3. From the Actions menu (ellipsis), select **Delete all for volume**.
4. Confirm the deletion by typing _delete_ and then click **Delete**.

### Delete snapshots from the {{site.data.keyword.block_storage_is_short}} details page in the UI
{: #snapshots-vpc-delete-from-volume}

You can delete the most recently created snapshot from the list of snapshots from the {{site.data.keyword.block_storage_is_short}} volume details page. Optionally, you can delete all snapshots from this view.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. Select a volume from the list and click the volume name to go to the volume details page.
3. Click **Snapshots** to see a list of snapshots taken of this volume.
4. Click **Delete all** to delete all snapshots for this volume.
5. Alternatively, select a single snapshot in the list for deletion and then:
   1. Click the Actions menu (ellipsis).
   2. Select **Delete** from the overflow menu.
   3. Confirm the deletion.

## Delete snapshots from the CLI
{: #snapshots-vpc-delete-snapshot-cli}
{: cli}

You can delete any snapshot for a volume or all snapshots for a volume. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

### Delete a single snapshot from the CLI
{: #snapshots-vpc-delete-single-snapshot-cli}

1. List the snapshots that are available for a volume to confirm the ID of the snapshot that you want to delete.

    ```sh
    ibmcloud is snapshots --volume VOLUME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
    ```
    {: pre}

    ```sh
    cloudshell:~$ ibmcloud is snapshots --volume  r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa
    Listing snapshots in all resource groups and region eu-de under account Test Account as user test.user@ibm.com...
    ID                                          Name                             Status   Source volume                               Bootable   Resource group   Created   
    r138-7cac80af-63bb-4a1b-83dd-5f6d550a5db7   bear-peroxide-viewable-oxidant   stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:49:48+00:00   
    r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   cli-snapshot-test                stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      defaults         2023-02-17T20:15:43+00:00   
    r138-e6664842-b370-496a-9ae7-da3fb647707c   snappy-snap-snap                 stable   r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   false      test-snap        2023-02-17T18:53:57+00:00   
    ```
    {: screen}

2. Run the `snapshot-delete` command and specify the ID of the snapshot. To delete multiple snapshots, you must specify all of their IDs in the same command.

    ```sh
    ibmcloud is snapshot-delete SNAPSHOT_ID
    ```
    {: pre}

3. Confirm the deletion of the snapshot. The response message indicates that the snapshot is deleted.

   ```sh
   cloudshell:~$ ibmcloud is snapshot-delete r138-e6664842-b370-496a-9ae7-da3fb647707c
   This will delete snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c and cannot be undone. Continue [y/N] ?> y
   Deleting snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c under account Test Account as user test.user@ibm.com...
   OK
   Snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c is deleted.
   ```
   {: screen}

For more information about available command options, [`ibmcloud is snaphot-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshot-delete).

### Delete all snapshots from the CLI
{: #snapshots-vpc-delete-all-snapshot-cli}}

1. List all snapshots.

    ```sh
    ibmcloud is snapshots --volume VOLUME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups]
    ```
    {: pre}

2. Enter the `snapshots-delete` command and specify the volume ID.

    ```sh
    ibmcloud is snapshots-delete --volume VOLUME_ID
    ```
    {: pre}

3. Confirm the deletion of the snapshots. The response message indicates when the snapshot deletion request is accepted and the snapshots are being deleted.

   ```sh
   cloudshell:~$ ibmcloud is snapshots-delete --volume  r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa
   This will delete snapshot by volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa and cannot be undone. Continue [y/N] ?> y
   Deleting snapshot by volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa under account Test Account as user test.user@ibm.com...
   OK
   Deletion request for snapshots by volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa has been accepted. 
   ```
   {: screen}

For more information about available command options, [`ibmcloud is snaphot-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshot-delete).

## Delete snapshots with the API
{: #snapshots-vpc-delete-snapshot-api}
{: api}

You can delete any snapshot for a volume or all snapshots for a volume. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

### Delete a single snapshot with the API
{: #snapshots-vpc-delete-single-snapshot-api}

Make a `DELETE /snapshots/{snapshot_ID}` call to delete a specific snapshot by ID.

```curl
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-12-22&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

### Delete all snapshots for a volume with the API
{: #snapshots-vpc-delete-all-api}

Make a `DELETE/snapshots` call and specify the source volume ID for the `source_volume.id` parameter in the request.

```curl
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots?source_volume.id=_volume-id_&version=2022-12-22&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

## Activity Tracker events for snapshots
{: #snapshots-vpc-at-events}

When you initiate activity on a snapshot, specific Activity Tracker events are generated. These activities include creating, listing, modifying, and deleting snapshots. For more information about the Activity Tracker events, see [Snapshots events](/docs/vpc?topic=vpc-at-events#events-snapshots).

### Activity Tracker JSON examples for snapshot events
{: #snapshots-vpc-at-event-examples}

The following example shows JSON output of an Activity Tracker event that was generated after you successfully created a snapshot. The name that you gave the snapshot appears in the response message and reason code `Created`.

```json
{
    "eventTime": "2022-02-22T17:59:07.57+0000",
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

```json
{
    "eventTime": "2022-01-16T17:55:25.60+0000",
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
| Updating | You changed something about the snapshot and it is being updated. |
| Deleting | The snapshot is being [deleted](#snapshots-vpc-delete). |
| Deleted | The snapshot was deleted and is not available to restore volumes. |
{: caption="Table 2. Snapshot lifecycle states" caption-side="bottom"}

## Managing security and compliance
{: #snapshots-vpc-manage-security}

The Snapshot for VPC service is integrated with the {{site.data.keyword.compliance_full}} to help you manage security and compliance for your organization. For snapshots, you can set up a goal that checks whether snapshots are encrypted by using customer-managed keys. By using the {{site.data.keyword.compliance_short}} to validate the snapshot resource configurations in your account against a profile, you can identify potential issues as they arise.

Since snapshots are created from {{site.data.keyword.block_storage_is_short}} volumes, {{site.data.keyword.block_storage_is_short}} goals provide an extra level of security. For more information about monitoring security and compliance for VPC, see [Monitoring security and compliance posture with VPC](/docs/vpc?topic=vpc-manage-security-compliance#monitor-vpc). For more information about creating security and compliance goals, see [Defining rules](/docs/security-compliance?topic=security-compliance-rules-define&interface=ui) in the Security and Compliance documentation.

## Next Steps
{: #snapshots-vpc-manage-next-steps}

* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).
