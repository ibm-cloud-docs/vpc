---

copyright:
  years: 2022, 2023
lastupdated: "2023-08-08"

keywords: VPC File Storage, file for VPC, NSF, replica, file share, replication, schedule

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing replica file shares
{: #file-storage-manage-replication}

Manage replica file shares by removing the replication relationship to create two independent file shares. The replica file share becomes read/write, and you can update and delete the share.
{: shortdesc}

You need Administrator or Editor IAM user roles to create and manage file share replicas and the replication relationship. For a list of these roles and actions, see [IAM roles for creating and managing file shares](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#file-storage-vpc-iam).
{: requirement}

## Removing the replication relationship
{: #fs-remove-replication}

You can remove replication by removing the replication relationship between the source file share and the replica file share. The operation is called _splitting_ the file shares. Data is no longer synced between the file shares. Removing the replication relationship creates two independent, read/write file shares. You can separately manage each file share, expand capacity and adjust IOPS, and create replicas.

Use the [UI](#fs-remove-replication-ui), [CLI](#fs-remove-replication-cli), or [API](#fs-remove-replication-api) to remove the replication relationship. You can also specify that the source and replica file shares are split if a [failover](/docs/vpc?topic=vpc-file-storage-failover) operation does not succeed.

Removing the replication relationship cannot occur when another operation is being performed on the source or replica file share (for example, the file share size is being expanded). The split operation remains pending until the other operation completes.

When you remove the replication relationship, you can't undo the action. Also, the data on the replica is not synced automatically with the source file before removal of the replication relationship.
{: note}

### Removing the replication relationship in the UI
{: #fs-remove-replication-ui}
{: ui}

To remove the replication relationship in the UI:

1. Go to the list of all file shares. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

2. Click the name of a file share or replica file share to go to its details page.

3. In the **File share replication relationship** section, click **Remove replication relationship**. Removing the replication relationship creates two independent file shares.

4. In the new window, click **Unlink**. The data on the replica is not synced with the source file share automatically before the replication relationship is removed.

The file share details page indicates no replication relationship.

### Removing the replication relationship from the CLI
{: #fs-remove-replication-cli}
{: cli}

Run the `ibmcloud is share-replica-split` command and specify the replica file share that you want to split from the source file share. The result of this operation is two independent read/write file shares.

```sh
ibmcloud is share-replica-split <replica-share-ID>
```
{: pre}

Example output:
```sh
ibmcloud is share-replica-split replica-share-3
This will disassociate a replica file share replica-share-3 from its source file share and cannot be undone. Continue [y/N] ?> y
The request to disassociate a replica file share replica-p-share-3 from its source file share was accepted, under account VPC as user myuser@mycompany.com...
OK
Replica File share replica-share-3 is disassociated.
```
{: codeblock}

For more information about the command options, see [`ibmcloud is share-replica-split`](/docs/vpc?topic=vpc-vpc-reference#share-replica-split).

### Removing the replication relationship with the API
{: #fs-remove-replication-api}
{: api}

Make a `DELETE /shares/{replica_id}/source` request to remove the replication relationship. Splitting a file share removes the replication relationship and creates two independent file shares. After you remove the relationship, you can't reestablish the relationship. A file share cannot be split, if the `lifecycle_state` of the file share is `updating` or if replica operations are in progress.

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/shares/{replica_share_id}/source?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token"\
```
{: pre}

A successful response indicates that the request to disassociate a replica file share from its source file share was accepted.

### Removing the replication relationship with Terraform
{: #fs-remove-replication-terraform}
{: terraform}

Use the `ibm_is_share_replica_operations` resource to split the source and replica shares. Splitting a file share removes the replication relationship and creates two independent file shares. After you remove the relationship, you can't reestablish the relationship.

```terraform
resource "ibm_is_share_replica_operations" "test" {
  share_replica = ibm_is_share.replica.id
  split_share = true
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share_replica_operations](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/ibm_is_share_replica_operations){: external}.

## Deleting a replica file share
{: #fs-delete-replicas}

The process for deleting a replica file share is similar to deleting a source file share. For example, you need to [delete mount targets](/docs/vpc?topic=vpc-file-storage-managing&interface=api#delete-mount-target-api) for the share before you delete the share. Because the replica file share is in active replication from the source share, the replica file share must be split from the source before the deletion. You can do split the shares in two ways:

* Perform a manual split, which removes the replication relationship and creates two independent, read/write file shares. Then, you can delete the mount target and the replica file share as a normal file share.

* Delete the replica file share directly after you deleted the mount targets. A `split` process is automatically initiated in the background. After the split operation is finished, the replica file share is deleted.

You can use the [UI](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#delete-file-share-ui), [CLI](/docs/vpc?topic=vpc-file-storage-managing&interface=cli#delete-file-share-cli), [API](/docs/vpc?topic=vpc-file-storage-managing&interface=api#delete-file-share-api), or [Terraform](/docs/vpc?topic=vpc-file-storage-managing&interface=terraform#delete-file-share-terraform) to delete a file share.

## Activity tracker events for replication
{: #fs-at-replication}

Activity tracker events are triggered when you establish and use file share replication. Table 1 describes the actions that generate events for replication. For more information about all file storage events, see [File storage events](/docs/vpc?topic=vpc-at-events#events-file-storage).

| Action | Description |
|--------|-------------|
| is.share.share.create | Create a replica file share. |
| is.share.share.read | View file share replication relationships. |
| is.share.share.replica.split | Separate source and replica shares into two independent read/write file shares. |
| is.share.share.replica.failover | Fail over from the source file share to the replica file share. |
{: caption="Table 1. Actions that generate events for file share replication." caption-side="bottom"}

## Replication statuses
{: #fs-repl-status}

Replication status shows when a replica file share is being created, when failover is underway, and when a split operation is creating independent file shares. For more information, see [File share lifecycle states](/docs/vpc?topic=vpc-file-storage-managing&interface=api#file-storage-vpc-status).

## Verifying replication with the API
{: #fs-verify-replica-api}
{: api}

You can use the API to verify that the replication succeeded, is pending, or failed. You can make the `GET /shares/{share_id}` request to see the status with the share ID of the source or the replica shares.

```sh
curl -X GET \
"$vpc_api_endpoint/v1/shares/$share_id?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token"
```
{: pre}

In the response, look at the `latest_job` property. The example shows the replication failover succeeded:

```json
  "created_at": "2023-08-08T23:31:59Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "$vpc_api_endpoint/v1/shares/199d78ec-b971-4a5c-a904-8f37ae710c63",
  "id": "199d78ec-b971-4a5c-a904-8f37ae710c63",
  "iops": 3000,
  "lifecycle_state": "stable",
  "name": "share-name1",
  .
  .
  .
  "latest_job": {
      "status": "succeeded",
      "status_reason": {
          "code": "",
          "message": "",
          "more_info": ""
      },
      "type": "replication_failover"
  }
}
```
{: codeblock}

For a replication split, when the replica share is being split from the source share, you can see a `running` status for `latest_job` in the response.

```json
"latest_job": {
    "status": "running",
     "status_reason": {
          "code": "",
          "message": "",
         "more_info": ""
    },
    "type": "replication_split"
},
```
{: codeblock}

A replication `failover` or `split` operation cannot happen if any other operation is being performed on the file share, such as expanding size. You can see a 409 error in the response that indicates the issue. See the following example.

```json
"errors": [
    {
        "code": "share_operation_pending",
          "message": "An operation 'replication_failover' is pending on file share, request to 'replication_split' cannot be accepted.",
          "more_info": "Before sending another request wait for the current operation to complete and try again."
     }
],
"trace": "4634eee2-0a9b-43b7-b35e-8885cc258500"
```
{: codeblock}
