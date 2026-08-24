---

copyright:
  years: 2021, 2026
lastupdated: "2026-08-24"

keywords: snapshots, Block Storage snapshots, delete snapshots, remove snapshots

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-vpc-delete}

You can delete snapshots that you no longer need to free up space for new snapshots. You can delete individual snapshots or all snapshots for a volume.
{: shortdesc}

## Prerequisites for deleting snapshots
{: #snapshots-vpc-delete-prereqs}

To be able to delete a snapshot, it must meet the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a volume.

A convenient way to determine whether you can delete a snapshot is to look in the [console](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-list-ui) for the list of snapshots and check its status.

## Deleting snapshots in the console
{: #snapshots-vpc-delete-snapshot-ui}
{: ui}

You can delete any snapshot for a volume or all snapshots for a volume. Deleting all snapshots requires further confirmation in the console.

### Deleting a single snapshot in the console
{: #snapshots-vpc-delete-single-snapshot-ui}

You can delete a snapshot from the list of all snapshots by using the following steps.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") in the row of the snapshot that you want to delete.
3. Select **Delete**.
4. Confirm the deletion and click **Delete**.

You can also delete a snapshot from the details page of a {{site.data.keyword.block_storage_is_short}} volume.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
2. Select a volume from the list, and click the volume name to go to the volume details page.
3. Click **Snapshots**. A list of snapshots that are taken of this volume is displayed, and you can take the following actions:
    * Click **Delete all** to delete all snapshots for this volume.
    * Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") to delete a specific snapshot.
4. Select **Delete**. If the snapshot is actively restoring a volume, the delete operation does not work.
5. Confirm the deletion.

### Deleting all snapshots for a volume in the console
{: #snapshots-vpc-delete-all-ui}

To delete all snapshots for a volume in the console, follow these steps.

1. Go to the list of all snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Snapshots**.
2. Click the row to select the snapshot that you want to delete.
3. From the **Actions** menu ![Actions icon](../icons/action-menu-icon.svg "Actions"), select **Delete all for volume**.
4. Confirm the deletion by typing _delete_ and then click **Delete**.

### Deleting snapshots from the {{site.data.keyword.block_storage_is_short}} details page in the console
{: #snapshots-vpc-delete-from-volume}

You can delete the most recently created snapshot from the list of snapshots from the {{site.data.keyword.block_storage_is_short}} volume details page. Optionally, you can delete all snapshots from this view.

1. Go to the list of all {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
2. Select a volume from the list and click the volume name to go to the volume details page.
3. Click **Snapshots** to see a list of snapshots taken of this volume.
4. Click **Delete all** to delete all snapshots for this volume.
5. Alternatively, select a single snapshot in the list for deletion and then:
   1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions").
   2. Select **Delete**.
   3. Confirm the deletion.

## Deleting snapshots from the CLI
{: #snapshots-vpc-delete-snapshot-cli}
{: cli}

You can delete any snapshot for a volume or all snapshots for a volume from the CLI.

### Deleting a single snapshot from the CLI
{: #snapshots-vpc-delete-single-snapshot-cli}

Use the following steps to delete a single snapshot by using the CLI.

1. List the snapshots that are available for a volume to confirm the ID of the snapshot that you want to delete.

    ```sh
    ibmcloud is snapshots --volume VOLUME [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
    ```
    {: pre}

    ```sh
    $ ibmcloud is snapshots --volume  r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa
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
   $ ibmcloud is snapshot-delete r138-e6664842-b370-496a-9ae7-da3fb647707c
   This will delete snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c and cannot be undone. Continue [y/N] ?> y
   Deleting snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c under account Test Account as user test.user@ibm.com...
   OK
   Snapshot r138-e6664842-b370-496a-9ae7-da3fb647707c is deleted.
   ```
   {: screen}

For more information about available command options, [`ibmcloud is snaphot-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshot-delete).

### Deleting all snapshots from the CLI
{: #snapshots-vpc-delete-all-snapshot-cli}

Use the following steps to delete all snapshots by using the CLI.

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
   $ ibmcloud is snapshots-delete --volume  r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa
   This will delete snapshot by volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa and cannot be undone. Continue [y/N] ?> y
   Deleting snapshot by volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa under account Test Account as user test.user@ibm.com...
   OK
   Deletion request for snapshots by volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa has been accepted.
   ```
   {: screen}

For more information about available command options, [`ibmcloud is snaphot-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshot-delete).

## Deleting snapshots with the API
{: #snapshots-vpc-delete-snapshot-api}
{: api}

You can delete any snapshot for a volume or all snapshots for a volume with the API.

### Deleting a single snapshot with the API
{: #snapshots-vpc-delete-single-snapshot-api}

Make a `DELETE /snapshots/{snapshot_ID}` call to delete a specific snapshot by ID.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots/7528eb61-bc01-4763-a67a-a414a103f96d?version=2022-12-22&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

### Deleting all snapshots for a volume with the API
{: #snapshots-vpc-delete-all-api}

Make a `DELETE/snapshots` call and specify the source volume ID for the `source_volume.id` parameter in the request.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/snapshots?source_volume.id=_volume-id_&version=2022-12-22&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

## Deleting snapshots with Terraform
{: #snapshots-vpc-delete-snapshot-terraform}
{: terraform}

You can delete any snapshot for a volume or all snapshots for a volume with Terraform.

### Deleting a single snapshot with Terraform
{: #snapshots-vpc-delete-terraform}
{: terraform}

Use the `terraform destroy` command to conveniently delete a remote object such as a single snapshot. The following example deletes `my-snapshot`.

```terraform
terraform destroy --target ibm_is_snapshot.my-snapshot
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

### Deleting all snapshots for a volume with Terraform
{: #snapshots-vpc-delete-all-snaphots-terraform}
{: terraform}

To delete all snapshots of a volume with terraform, use the `ibm_is_volume` resource.

```terraform
resource "ibm_is_volume" "storage" {
  name                 = "example-volume"
  profile              = "general-purpose"
  zone                 = "us-south-1"
  delete_all_snapshots = true
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume#delete_all_snapshots){: external}.

## Next steps
{: #snapshots-vpc-delete-next-steps}

* [Monitor snapshot lifecycle states](/docs/vpc?topic=vpc-snapshots-vpc-monitoring)
* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore)
* [View snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view)
