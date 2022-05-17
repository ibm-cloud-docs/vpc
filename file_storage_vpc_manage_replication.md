---

copyright:
  years: 2022
lastupdated: "2022-05-17"

keywords:

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:preview: .preview}
{:external: target="_blank" .external}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing replica file shares
{: #file-storage-manage-replication}

Manage replica file shares by removing the replication relationship to create two independent file shares. The replica file share becomes read/write, and you can update and delete the share.
{: shortdesc}

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, London, Sydney, Sao Paulo, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Remove the replication relationship
{: #fs-remove-replication}

You can remove replication by removing the replication relationship between the source file share and replica file share. The operation is called _splitting_ the file shares. Data is no longer synced between the file shares. Removing the replication relationship creates two independent, read/write file shares. You can then separately manage each file share, expand capacity and adjust IOPS, and create replicas.

Use the [UI](#fs-remove-replication-ui), [CLI](#fs-remove-replication-cli), or [API](#fs-remove-replication-api) to remove the replication relationship. You can also specify that the source and replica file shares are split if a [failover](/docs/vpc?topic=vpc-file-storage-failover) operation does not succeed.

When you remove the replication relationship, you can't undo the action. Also, the data on the replica will not be synced with the source file before removal of the replication relationship.
{: note}

### Remove the replication relationship in the UI
{: #fs-remove-replication-ui}
{: ui}

To remove the replication replationship from the UI:

1. Navigate to the list of all file shares. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

2. Click the name of a file share or replica file share to go to its details page. 

3. In the **File share replication relationship** section, click **Remove replication relationship**. Removing the replication relationship creates two independent file shares.

4. In the popup window, click **Unlink**. The data on the replica is not synced with the source file share before removing the replication relationship.

The file share details page indicates there is no replication relationship.

### Remove the replication relationship in the CLI
{: #fs-remove-replication-cli}
{: cli}

Run the `ibmcloud is share-replica-split` command and specify the replica file share you want to split from the source file share. The result of this operation is two independent read-write file shares. For example, using the interactive CLI:

```bash
ibmcloud is share-replica-split replica-share-3
This will disassociate a replica file share replica-share-3 from its source file share and cannot be undone. Continue [y/N] ?> y
The request to disassociate a replica file share replica-p-share-3 from its source file share was accepted, under account VPC as user myuser@mycompany.com...
OK
Replica File share replica-share-3 is disassociated.
```
{: codeblock}

### Remove the replication relationship in the API
{: #fs-remove-replication-api}
{: api}

Make a `DELETE /shares/{replica_id}/source` request to remove the replication relationship. Splitting a file share removes the replication relationship and creates two independent file shares. After you remove the relationship, you can't reestablish the relationship.

```curl
curl -X DELETE \ 
"$vpc_api_endpoint/v1/shares/$replica_id/source?version=2022-05-03&generation=2"\
-H "Authorization: $iam_token"\
```
{: pre}

A successful response indicates that the request to disassociate a replica file share from its source file share was accepted.

## Deleting a replica file share
{: #fs-delete-replicas}

The process for deleting a replica file share is similar to deleting a source file share. For example, you  [delete mount targets](https://test.cloud.ibm.com/docs/vpc?topic=vpc-file-storage-managing&interface=api#delete-mount-target-api) for the share prior to deleting the share. Because the replica file share is in active replication from the source share, the relica file share must be split from the source before deletion. You can do this in two ways:

* Perform a manual split, which [removes the replication relationship](#fs-remove-replication-ui) and creates two independent, read-write file shares. You can then delete the mount target and the replica file share as a normal file share. 

* Directly delete the replica file share after deleting the mount targets. A `split` process is automatically executed and run in the background. After the split operation is finished, the replica file share is deleted.

Use the [UI](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#delete-file-share-ui), [CLI](/docs/vpc?topic=vpc-file-storage-managing&interface=cli#delete-file-share-cli) or [API](/docs/vpc?topic=vpc-file-storage-managing&interface=api#delete-file-share-api) to delete a file share.

## Activity tracker events for replication
{: #fs-at-replication}

Activity tracker events are triggered when you establish and use file share replication. Table 1 describes the actions that generate events for replication. For information about all file storage events, see [File storage events](/docs/vpc?topic=vpc-at-events#events-file-storage).

| Action | Description |
|--------|-------------|
| is.share.share.create | Create a replica file share |
| is.share.share.read | View file share replication relationships |
| is.share.share.replica.split | Separate source and replica shares into two independent read/write file shares |
| is.share.share.replica.failover | Fail over from the source file share to replica file share |
{: caption="Table 1. Actions that generate events for file share replication" caption-side="bottom"}

## User roles for managing replication
{: #fs-roles-repl}

You need Administrator or Editor IAM user roles to create and manage file share replicas and the replication relationship. For a list of these roles and actions, see [IAM roles for creating and managing file shares](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#file-storage-vpc-iam).

## Replication statuses
{: #fs-repl-status}

Replication status shows when a replica file share is being created, when failover is underway, and when a split operation is creating independent file shares. For information, see [File share lifecycle states](/docs/vpc?topic=vpc-file-storage-managing&interface=api#file-storage-vpc-status).

## Next steps
{: #fs-failover-next-steps}

[Manage]() file shares and [file share replication]().
