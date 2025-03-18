---

copyright:
  years: 2024, 2025
lastupdated: "2025-03-18"

keywords: snapshots, File storage snapshots, manage snapshots, fast restore clone, backup snapshot, remote copy, cross-regional copy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing {{site.data.keyword.filestorage_vpc_short}} snapshots
{: #fs-snapshots-manage}



You can manage existing snapshots in the console, from the CLI, with the API or Terraform. You can update the user tags of the snapshots, and delete snapshots that you no longer need to free up space for new snapshots.
{: shortdesc}

Although you can't create a snapshot on a replica share, the snapshots of the origin share are copied from the source to the replica at the next scheduled sync. These replica snapshots are created by the file service. They do not inherit the tags or the name from the original snapshots. However, they have the same fingerprint value as the source snapshot. They can't be manually deleted from the replica share, but are removed from the replica share at the next replication sync if the source snapshot is deleted on the source share.
{: note}



## Updating user tags of a snapshot in the UI
{: #fs-snapshot-rename-ui}
{: ui}

Use the following steps to update the user tags of a snapshot in the console.

1. Go to the list of snapshots. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
1. The file shares are listed for a specific region. If you want to see resources in another region, click the arrow to expand the list and select a different region. By default, the newest shares are displayed at the beginning of the list.
1. Select the file share that you want to view, and click the **Snapshots** tab. 
1. Click the name of a snapshot to open the Snapshot details panel.
1. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") next to the User tags.
1. You can remove any existing tags and add new tags. Click **Save**.



## Updating user tags of a snapshot from the CLI
{: #fs-snapshot-updatetags-cli}
{: cli}

You can update the user tags that are assigned to a snapshot from the CLI. Issue the `ibmcloud is share-snapshot-update` command and provide the snapshot ID and new tag.

```sh
ibmcloud is share-snapshot-update SHARE SNAPSHOT --tags NEW_TAG1,NEW_TAG2 [--output JSON] [-q, --quiet]
```
{: pre}

See the following example, which adds a user tag.

```sh
ibmcloud is share-snapshot-update my-file-share r134-6ce54f3b-8971-4b5d-95a7-7dfa897ddfb3 --user-tags test:cli
Updating file share snapshot share-snapshot-cli-update for share my-file-share under account Test Account as user test.user@ibm.com...
                        
ID                   r134-6ce54f3b-8971-4b5d-95a7-7dfa897ddfb3   
Name                 share-snapshot-cli-update   
Fingerprint          c25fdce4-6e0a-433f-99c0-9985f127cd54   
Backup Policy Plan   -   
Status               available   
Status reasons       Status code   Status message      
                     -             -  
Created at           2024-12-17T11:19:33+05:30   
Captured At          2024-12-17T11:19:34+05:30   
CRN                  crn:v1:bluemix:public:is:us-south-1:a/a123456::share-snapshot:r134-2ae87eb2-b26c-4126-ab34-e6e64f6f1773/r134-6ce54f3b-8971-4b5d-95a7-7dfa897ddfb3   
LifeCycle State      stable
LifeCycle Reasons    Code   Message   More Info      
                      -      -         
Href                 https://us-south.iaas.cloud.ibm.com/v1/shares/r134-2ae87eb2-b26c-4126-ab34-e6e64f6f1773/snapshots/r134-6ce54f3b-8971-4b5d-95a7-7dfa897ddfb3   
Minimum Size         40   
User Tags            test:cli   
Zone                 ID   Name      
                          us-south-1      
                        
Resource group       ID                                 Name      
                     11caaa983d9c4beb82690daab08717e9   Default      
           
Resource type        share_snapshot 
```
{: screen}



## Updating user tags with the API
{: #fs-snapshots-updatetags-api}
{: api}

You can update user tags of a snapshot by using the API. Make a `PATCH /shares/{share-id}/snapshots/{snapshot-id}` call and specify the snapshot ID and the tags that you want the snapshot to have.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/shares/r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/snapshots/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4?version=2024-12-10&generation=2" \
   -H "Authorization: Bearer ${API_TOKEN}" \
   -d '{
     "user_tags": ["env:test","dev:test"]
    }'
```
{: codeblock}  

## Updating a snapshot with Terraform
{: #fs-snapshots-rename-terraform}
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

To update a snapshot, use the `ibm_is_share_snapshot` resource. You can change the tags of the snapshot by adding them with the `tags` argument.

```terraform
resource "ibm_is_share_snapshot" "example" {
  tags          = "dev:test"
  source_share  = ibm_is_share.example.id
  }
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

## Deleting snapshots in the console
{: #fs-snapshots-delete-snapshot-ui}
{: ui}

You can delete any snapshot of a share. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a share.

An easy way to determine whether you can delete a snapshot is to look in the [console](/docs/vpc?topic=vpc-fs-snapshots-view&interface=ui#fs-snapshots-view-ui) for the list of snapshots and check its status.

### Deleting a single snapshot in the UI
{: #fs-snapshots-delete-single-snapshot-ui}

You can delete a snapshot from the list of snapshots by using the following steps.

1. Go to the list of all {{site.data.keyword.filestorage_vpc_short}} shares. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > File storage shares**.
1. Select a share from the list and click the share name to go to the share details page.
1. Click the **Snapshots** tab to see a list of snapshots taken of this share.
5. Locate the snapshot that you want to delete.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions").
1. Select **Delete**.
1. Confirm the deletion.

## Deleting snapshots from the CLI
{: #fs-snapshots-delete-snapshot-cli}
{: cli}

You can delete any snapshot for a share or all snapshots for a share. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a share.

Use the following steps to delete a single snapshot by using the CLI.

1. List the snapshots that are available for a share to confirm the ID of the snapshot that you want to delete.

    ```sh
    ibmcloud is share-snapshots SHARE [--output JSON] [-q, --quiet]
    ```
    {: pre}

    ```sh
    ibmcloud is share-snapshots my-file-share
    ```
    {: screen}

2. Run the `ibmcloud is share-snapshot-delete` command and specify the ID of the snapshot. To delete multiple snapshots, you must specify all of their IDs in the same command.

    ```sh
    ibmcloud is share-snapshot-delete SHARE (SNAPSHOT1 SNAPSHOT2 ...) [-f, --force] [--output JSON] [-q, --quiet]
    ```
    {: pre}

3. Confirm the deletion of the snapshot. The response message indicates that the snapshot is deleted.

   ```sh
   $ ibmcloud is share-snapshot-delete my-file-share r134-6ce54f3b-8971-4b5d-95a7-7dfa897ddfb3
   This will delete share snapshot r134-6ce54f3b-8971-4b5d-95a7-7dfa897ddfb3 for share ID my-file-share and cannot be undone. Continue [y/N] ?> y
   Deleting share snapshot r134-6ce54f3b-8971-4b5d-95a7-7dfa897ddfb3 for share ID my-file-share under account Test Account as user test.user@ibm.com...
   OK
   Share snapshot r134-6ce54f3b-8971-4b5d-95a7-7dfa897ddfb3 is deleted.
   ```
   {: screen}

For more information about available command options, [`ibmcloud is snaphot-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshot-delete).

## Deleting snapshots with the API
{: #fs-snapshots-delete-snapshot-api}
{: api}

You can delete any snapshot for a share or all snapshots for a share. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a share.

Call the `DELETE /shares/{share-id}/snapshots/{snapshot-id}` method to delete a specific snapshot by ID.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/r006-0fe9e5d8-0a4d-4818-96ec-e99708644a58/snapshots/r006-e13ee54f-baa4-40d3-b35c-b9ec163972b4?version=2024-12-10&generation=2" \
     -H "Authorization: Bearer ${API_TOKEN}"
```
{: codeblock}

## Deleting snapshots with Terraform
{: #fs-snapshots-delete-snapshot-terraform}
{: terraform}

You can delete any snapshot for a share or all snapshots for a share. To be able to delete a snapshot, it must meet to the following prerequisites:

* Be in a `stable` or `pending` state.
* Not be actively restoring a share.

Use the `terraform destroy` command to conveniently delete a remote object such as a single snapshot. The following example deletes `my-snapshot`.

```terraform
terraform destroy --target ibm_is_snapshot.my-snapshot
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

## Activity tracking and auditing
{: #fs-snapshots-at-events}

When you initiate an activity on a snapshot, specific Activity tracking events are generated. These activities include creating, listing, modifying, and deleting snapshots. For more information, see [Snapshot events](/docs/vpc?topic=vpc-at_events#events-fs-snapshots).

## Snapshot lifecycle states
{: #fs-snapshots-status}

Table 2 describes the snapshot states in the snapshot lifecycle.

| Snapshot status | Explanation |
|-----------------|-------------|
| Stable | The snapshot is created and is available for restoring a share. |
| Waiting | Snapshot information is being retrieved. |
| Pending | The snapshot is being replicated to a remote share. While the replication is in process, the percentage completed is displayed. |
| Failed | The snapshot failed to be created, the share can't be restored from a snapshot. |
| Suspended | Snapshot is temporarily unavailable. |
| Updating | You changed the name of the snapshot and it is being updated. |
| Deleting | The snapshot is being deleted. |
| Deleted | The snapshot was deleted and is not available to restore shares. |
{: caption="File share snapshot lifecycle states" caption-side="bottom"}

## Managing security and compliance
{: #fs-snapshots-manage-security}

Because snapshots are created from {{site.data.keyword.filestorage_vpc_short}} shares, they share the encryption key. When a share is created from a Snapshot, your SCC-registered preference of allowing only Customer-Managed encryption is applicable. For more information, see [Getting started with Security and Compliance Center](/docs/security-compliance?topic=security-compliance-getting-started).

## Next steps
{: #fs-snapshots-manage-next-steps}

You can [Restore a share from a snapshot](/docs/vpc?topic=vpc-fs-snapshots-restore).
