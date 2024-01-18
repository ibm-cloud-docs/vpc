---

copyright:
  years: 2022, 2024
lastupdated: "2024-01-10"

keywords: VPC File Storage, file for VPC, NSF, replica, file share, replication, schedule

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing replica file shares
{: #file-storage-manage-replication}

Manage replica file shares by removing the replication relationship to create two independent file shares. The replica file share becomes read/write, and you can update and delete the share.
{: shortdesc}

You need Administrator or Editor IAM user roles to create and manage file share replicas and the replication relationship. For a list of these roles and actions, see [IAM roles for creating and managing file shares](/docs/account?topic=account-iam-service-roles-actions#is.share-roles).
{: requirement}

## Removing the replication relationship
{: #fs-remove-replication}

You can end replication by removing the replication relationship between the source file share and the replica file share. The operation is called _splitting_ the file shares. Removing the replication relationship creates two independent, read/write file shares. Data is no longer synced between them. You can separately manage each file share, expand capacity and adjust IOPS, and create more replicas.

You can also specify that the source and replica file shares are split if a [failover](/docs/vpc?topic=vpc-file-storage-failover) operation does not succeed.
{: note}

Removing the replication relationship cannot occur when another operation is being performed on the source or replica file share (for example, the file share size is being expanded). The split operation remains pending until the other operation completes.

When you remove the replication relationship, you can't undo the action. Also, the data on the replica is not synced automatically with the source file before removal of the replication relationship.
{: important}

### Removing the replication relationship in the UI
{: #fs-remove-replication-ui}
{: ui}

To remove the replication relationship in the UI:

1. Go to the list of all file shares. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Storage > File Shares**.

2. Click the name of a file share or replica file share to go to its details page.

3. In the **File share replication relationship** section, click **Remove replication relationship**. Removing the replication relationship creates two independent file shares.

4. In the new window, click **Unlink**. The data on the replica is not synced with the source file share automatically before the replication relationship is removed.

The file share details page indicates no replication relationship.

### Removing the replication relationship from the CLI
{: #fs-remove-replication-cli}
{: cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Locate your share from the CLI by listing your file shares in the region with the `ibmcloud is shares` command.

   ```sh
   $ ibmcloud is shares
   Listing shares in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
   ID                                          Name                    Lifecycle state   Zone         Profile   Size(GB)   Resource group   Replication role   
   r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516   my-replica-file-share   stable            us-south-1   dp2       1500       Default          replica   
   r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a   my-source-file-share    stable            us-south-2   dp2       1500       Default          source   
   r006-6d1719da-f790-45cc-9f68-896fd5673a1a   my-replica-share        stable            us-south-3   dp2       1500       Default          replica   
   r006-925214bc-ded5-4626-9d8e-bc4e2e579232   my-new-file-share       stable            us-south-2   dp2       500        Default          none   
   r006-b1707390-3825-41eb-a5bb-1161f77f8a58   my-vpc-file-share       stable            us-south-2   dp2       1000       Default          none   
   r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   my-file-share           stable            us-south-2   dp2       1500       Default          source 
   ```
   {: screen}

1. View the details of the file share that you want to modify with the `ibmcloud is share` command.
   
   ```sh
   $ ibmcloud is share my-file-share
   Getting file share my-file-share under account Test Account as user test.user@ibm.com...
                                
   ID                           r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
   Name                         my-file-share   
   CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
   Lifecycle state              stable   
   Access control mode          security_group   
   Zone                         us-south-2   
   Profile                      dp2   
   Size(GB)                     1500   
   IOPS                         2000   
   Encryption                   provider_managed   
   Mount Targets                ID                                          Name      
                                r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61   my-cli-share-mount-target-1      
                                 
   Resource group               ID                                 Name      
                                db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
   Created                      2023-10-18T22:15:15+00:00   
   Latest job                   Job status   Job status reasons      
                                succeeded    -      
                                
   Replication share            ID                                          Name               Resource type      
                                r006-6d1719da-f790-45cc-9f68-896fd5673a1a   my-replica-share   share      
                                
   Replication role             source   
   Replication status           active   
   Replication status reasons   Status code   Status message      
                                -             -      
   ```
   {: screen}

1. Run the `ibmcloud is share-replica-split` command and specify the replica file share by its name or ID. 
   
   ```sh
   $ ibmcloud is share-replica-split r006-6d1719da-f790-45cc-9f68-896fd5673a1a
   This will disassociate a replica file share r006-6d1719da-f790-45cc-9f68-896fd5673a1a from its source file share and cannot be undone. Continue [y/N] ?> y
   The request to disassociate a replica file share r006-6d1719da-f790-45cc-9f68-896fd5673a1a from its source file share was accepted, under account Test Accouont as user test.user@ibm.com...
   OK
   Replica File share r006-6d1719da-f790-45cc-9f68-896fd5673a1a is disassociated.
   ```
   {: codeblock}

1. The result of this operation is two independent read/write file shares. When you list the file shares in the region, you can see `none` in the replication column for the two file shares.

   ```sh
   $ ibmcloud is shares
   Listing shares in all resource groups and region us-south under account IBM as user Viktoria.Muirhead@ibm.com...
   ID                                          Name                    Lifecycle state   Zone         Profile   Size(GB)   Resource group   Replication role   
   r006-dc6a644d-c7da-4c91-acf0-d66b47fc8516   my-replica-file-share   stable            us-south-1   dp2       1500       Default          replica   
   r006-e4acfa9b-88b0-4f90-9320-537e6fa3482a   my-source-file-share    stable            us-south-2   dp2       1500       Default          source   
   r006-6d1719da-f790-45cc-9f68-896fd5673a1a   my-replica-share        stable            us-south-3   dp2       1500       Default          none   
   r006-925214bc-ded5-4626-9d8e-bc4e2e579232   my-new-file-share       stable            us-south-2   dp2       500        Default          none   
   r006-b1707390-3825-41eb-a5bb-1161f77f8a58   my-vpc-file-share       stable            us-south-2   dp2       1000       Default          none   
   r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   my-file-share           stable            us-south-2   dp2       1500       Default          none
   ```
   {: screen}
   
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

You can use the [UI](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#delete-file-share-ui){: ui}[CLI](/docs/vpc?topic=vpc-file-storage-managing&interface=cli#delete-file-share-cli){: cli}[API](/docs/vpc?topic=vpc-file-storage-managing&interface=api#delete-file-share-api){: api}[Terraform](/docs/vpc?topic=vpc-file-storage-managing&interface=terraform#delete-file-share-terraform){: terraform} to delete a file share.

## Activity tracker events for replication
{: #fs-at-replication}

Activity tracker events are triggered when you establish and use file share replication. Table 1 describes the actions that generate events for replication. For more information about all file storage events, see [File Storage events](/docs/vpc?topic=vpc-at-events#events-file-storage).

| Action | Description |
|--------|-------------|
| `is.share.share.create` | Create a replica file share. |
| `is.share.share.failover` | Status of the failover operation.|
| `is.share.share.init` | Status of the initialization. |
| `is.share.share.read` | View file share replication relationships. |
| `is.share.share.split`| status of the replication split operation. |
{: caption="Table 1. Actions that generate events for file share replication." caption-side="bottom"}

## Replication statuses
{: #fs-repl-status}

Replication status shows when a replica file share is being created, when failover is underway, and when a split operation is creating independent file shares. For more information, see [File share lifecycle states](/docs/vpc?topic=vpc-file-storage-managing#file-storage-vpc-status).

| Activity                                     | Description |
|----------------------------------------------|-------------|
| Replication initialization initiated         | All replicas start with this activity, a replica can't be mounted read-only until this process finishes. |
| Replication initialization success           | This status shows that the replica is mountable read-only, and that it is initially healthy. |
| Replication initialization failure | This status shows when the initialization fails. This particular event leads to the replica share's `failed` lifecycle state. |
| Replication sync initiated                   | This status is only shown when a synchronization event is called with the API, not through automated scheduling. |
| Replication sync | This status is shown when the system successfully completed a sync within the RPO (schedule).|
| Replication sync is degraded | It is a standard sync event, but signifies an `unhealthy` state when the RPO is not met.|
| Replication failover initiated               | This status is shown when the customer initiates a failover.|
| Replication failover failed                  | The result of a failover requested without the `split` failsafe that also wasn't able to complete within the specified time, or communication was not possible on the backend storage devices |
| Replication failover success                 | This status is shown when the failover process succeeded completely, and the new relationship is healthy|
| Replication split (Delete) initiated         | This status is shown when the customer stops replication.|
| Replication split (Delete) failure           | This status is shown when the Split process fails. The relationship status remains as it was before the split was attempted.|
| Replication split (Delete) success           | This status is shown when the split process completes successfully. As a result of this operation, the replication relationship status is deleted. \n Or it can be the result of a Failover process where the customer specifies that they want to `split` the relationship if the failover does not complete within a specific time.|
{: caption="Table 2. Replication statuses." caption-side="bottom"}

## Replication sync information
{: #fs-repl-syncinfo}

Replication is an asynchronous operation, which is not instantaneous. After each sync operation, the system provides useful information about the last replication process, such as start and end date, and the transferred data volume. By viewing the replication information, you can see how long the last replication took and calculate the transfer rate. Seeing the transferred data values can help you estimate the global transfer charges at the end of the billing period.

You can use the replication sync information to fine-tune your replication schedule. It can help you balance the cost and the frequency that you need the data to be refreshed on the replica to satisfy your [recovery point objective](#x3429911){: term}. It can also help to determine whether the replication process is in danger of degradation. 

When the amount of data to be transferred exceeds the amount of data that can be transferred during the replication window with the normal transfer rate, the replication process can't complete and the replication status becomes `degraded`. If this situation occurs, try adjusting the rate of change on the file share and the replication frequency.

The systems queries the last sync status every 15 minutes. The result shows the data of the last replication that completed. If a replication is in progress when the query runs, it does not show in the response. After the replication is completed, the next query updates the last sync information. You can expect a short delay between the replication completion and the last sync information being updated in the interfaces.

You can see information about the last replication operation when you view the details of either the source or the replica share. For more information, see [View details of a file share in the UI](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-view-single-share-ui).
{: ui}

You can see information about the last replication operation when you list the details of either the source or the replica share. For more information, see [View details of a file share from the CLI](/docs/vpc?topic=vpc-file-storage-view&interface=cli#fs-share-details-cli).
{: cli}

You can programmatically retrieve the last sync details by calling the `/shares` method in the [VPC API](/apidocs/vpc/latest#get-share){: external}. Look for the `latest_sync` section in the API response to see when the replication started (`started_at`), when it ended (`completed_at`), and how much data was transferred (`data_transferred`). For more information, see [View a single file share with the API](/docs/vpc?topic=vpc-file-storage-view&interface=api#fs-single-file-shares-api).
{: api}

In addition, you can view historical information of the recent replication sync when you use {{site.data.keyword.la_full}}. When replication occurs, the file service generates a `regional-file.00002I` log message, which includes information about when the replication occurred, and how much data was transferred. For more information, see [Logging for VPC](/docs/vpc?topic=vpc-logging#logging-file-share-replication).

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
