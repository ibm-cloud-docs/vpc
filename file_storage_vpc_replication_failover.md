---

copyright:
  years: 2022, 2023
lastupdated: "2023-08-08"

keywords: file storage, file share, replication, replica, source share, failover, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Replication failover
{: #file-storage-failover}

Failover to the replica file share keeps your file share available if your source file share becomes unavailable. Failover switches the replication relationship so that the replica file share becomes the source file share and the source share becomes the read-only replica file share.
{: shortdesc}

## Replication failover concepts
{: #fs-failover-concepts}

When you create a replica file share, the replica pulls data from the source file share based on a replication schedule. The data on the replica file share is set to read-only. Failover switches the replication relationship. The read-only replica file share becomes the read/write source file share and the original share becomes read-only. You can now manage the share as a regular file share (for example, you can [adjust IOPS tiers](/docs/vpc?topic=vpc-adjusting-share-iops)).

When you initiate a failover, you can choose what happens if the failover operation fails or times out.

* If you decide to keep the replication relationship, the system "falls back" to the source share, and the replication is in a `failed` status. The system attempts to replicate again at the next scheduled time. This option can be used for [routine maintenance](#fs-failover-maintenance) on the primary site or when the site is having problems. You can fall back to the original share when the maintenance is complete and the site is stable again.

* If you decide to remove the replication relationship, the system splits the two file shares apart, and they become independent read/write file shares. This option can be used for failover in a [disaster recovery](#fs-failover-dr) situation when it's more important to start your application up as quickly as possible on the replica site and the future of the original site is uncertain. 

The default timeout is 5 minutes. If the failover does not complete in 5 minutes, the destination site becomes the primary site, and the file share is made read/write. You must [remount the file share](/docs/vpc?topic=vpc-file-storage-vpc-remount) after failover. 

A failover operation or a replica split cannot occur when another operation is being performed on the source or replica file share (for example, the file share size is being expanded). The split or failover operations remains pending until the other operation completes.
{: important}

The [failover status](/docs/vpc?topic=vpc-file-storage-managing#file-storage-vpc-status) shows `failover_pending` while the operation is underway, or while the service is waiting for another operation to complete, such as file share expansion.

### Failover for routine maintenance
{: #fs-failover-maintenance}

Use failover for routine maintenance on the primary site or when the site is having problems. The process works as follows.

* The file share at the source site begins refusing all read and write operations and the system attempts to pull a final copy of the share's data to the replica file share.
* The data is copied to the replica file share, which becomes read/write and is considered the new source site. (The replication relationship is reversed.)
* The service attempts to replicate data to the original site as scheduled. If the data transfer fails, the system attempts again at the next scheduled replication time.
* You can fall back to the original share when the maintenance is done and the site is stable again. Or, you can keep the replica share as your source share.

### Failover in event of a disaster recovery situation
{: #fs-failover-dr}

Failover is also an option for disaster recovery. If the original site is confirmed to be unavailable and you need your application started as soon as possible on the replica location, choose the remove replication relationship option as the fallback policy when you initiate the failover. Failover for disaster recovery works like this:

* The file share at the source site begins refusing all read and write operations and the system attempts to pull a final copy of the share's data to the replica file share.
* When the last data pull times out and fails, the file service determines that it is a disaster recovery situation. Then, the service breaks the replication relationship. The replica file share becomes read/write and operates as an independent file share.
*  Replication relationship cannot be reestablished. However, you can set up a new replica on the original site if and when the site becomes operational again.

Due to the nature of the disaster recovery failover, you might find that the latest data set did not get copied over. In that case, you probably need to reconcile the state of your application manually when the source file share is available again. If the source file share zone becomes available again, data is available from the replica share to reconcile from the time of the incident to the recovery point.

## Restrictions
{: #fs-failover-restrictions}

These restrictions apply when you perform a failover.

* The default timeout for a successful failover is 5 minutes. You can modify this value when you [initiate the failover](/docs/vpc?topic=vpc-file-storage-failover#fs-failover-procedure-ui) options.

* A failover remains pending when other operations are being performed on the source file share, such as expanding the share size. When the operation completes, the failover resumes.

## Initiating a failover in the UI
{: #fs-failover-procedure-ui}
{: ui}

1. Navigate to the list of all file shares. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

2. Click the name of a replica file share to open its details page.

3. From the **Actions** menu, select **Perform failover**. In the side panel, you can preview the changes. Before performing the failover, a final sync of the files is performed to ensure that the failover share has the latest content. When the failover completes, the replica file share becomes the new source file share. The former source share becomes the new read-only replica share.

4. To set a timeout value and change the default timeout of 5 minutes, check the box under **Timeout (optional)** and specify a time value. This value specifies an absolute time limit for the failover to complete. Set a timeout based on how long you can have your file share offline.

5. Under **Failover policy**, if the failover operation does not succeed or times out, choose to keep the replication relationship or change it:
    * Keep replication relationship - No changes are made to the replica file share or source file share.
    * [Remove replication relationship](/docs/vpc?topic=vpc-file-storage-manage-replication#fs-remove-replication) - This action creates two separate read/write file shares. Because the relationship is broken, changes to one file share do not affect the other.

    After you break the relationship, it can't be reestablished.
    {: note}

6. Click **Perform failover**. Messages display that indicate that the failover was requested and is being performed.

The file share details page is updated, and the replication relationship shows the replica file share as the new source file share.

## Initiating a failover from the CLI
{: #fs-failover-procedure-cli}
{: cli}

Run the `ibmcloud is share-replica-failover` command and specify the `fallback-policy` property. You can specify `fail` or `split` for this property.

This example specifies `fail` for the `fallback-policy` property, which indicates that if the failover operation fails or the timeout is reached, the failover operation is unsuccessful.

```sh
ibmcloud is share-replica-failover dc1e70af-c3cb-44c6-a96d-2ece09b51ae3 --fallback-policy fail
The file share dc1e70af-c3cb-44c6-a96d-2ece09b51ae3 failover request was accepted under account VPC as user myuser@mycompany.com...
The file share failover request was accepted.
```
{: screen}

The following example specifies `split` for the `fallback-policy` property, which indicates that the replica share is split from the source file share whenever a failover operation fails. The result of this operation is two independent read/write file shares.

```sh
ibmcloud is share-replica-failover 8b07129c-e376-4572-9ef3-68c729b315d5 --fallback-policy split
The file share 8b07129c-e376-4572-9ef3-68c729b315d5 failover request was accepted under account VPC as user myuser@mycompany.com...
The file share failover request was accepted.
```
{: screen}

For more information about the command options, see [`ibmcloud is share-replica-failover`](/docs/vpc?topic=vpc-vpc-reference#share-replica-failover).

## Initiating a failover with the API
{: #fs-failover-procedure-api}
{: api}

Make a `POST /shares/{share_id}/failover` request and specify the `timeout` and `fallback_policy` properties. The minimum timeout is 300 seconds and the maximum 3600 seconds. This request fails a source file share over to the replica share, which is specified by the replica file share ID.

The `fallback_policy` property can have the values `split` or `fail`. When `fail` is specified, if the failover operation fails or the timeout is reached, the failover operation is unsuccessful. The replication relationship remains unchanged.

If you specify `split` for the `fallback_policy` property, the replica share is split from the source share whenever a failover operation fails. The result is two independent read/write file shares. In this case, because the final file synchronization could not complete, the replica share might not contain all the data of the source file share. Use this option for disaster recovery, when the source file share is known to be unreachable.

If the `fallback_policy` property is not specified in the request, the system defaults to `split` when the failover operations fails.

This example specifies `fail` for the `fallback_policy` property. The `timeout` property is optional. You can use the default timeout.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares/$replica_id?/failover?version=2023-08-08&generation=2&maturity=beta""\
-H "Authorization: $iam_token"\
-d '{
     "fallback_policy": "fail",
      "timeout": 600
    }'
```
{: pre}

A successful response indicates that the file share failover request was accepted.

You can use the API to verify that replication failover succeeded, is pending, or failed. Make a `GET /shares/{replica_id}` call. Look at the `latest_job` property. For more information, see [Verify replication with the API](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-verify-replica-api).
{: note}

## Initiating a failover with Terraform
{: #fs-failover-procedure-terraform}
{: terraform}

When a failover is performed, replica share becomes the source, and the source share becomes replica. The terraform configuration needs to be modified to match this. The `fallback_policy` defines the action to take if the failover request is accepted but cannot be performed or times out. Accepted values are `split` or `fail`. If you specify `split` and the failover is unsuccessful, the system breaks the replication relationship, and the two file shares become independent of each other.

```Terraform
resource "ibm_is_share_replica_operations" "test" {
  share_replica = ibm_is_share.replica.id
  fallback_policy = "split"
  timeout = 500
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share_replica_operations](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/ibm_is_share_replica_operations){: external}.

## Next steps
{: #fs-failover-next-steps}

[Manage replication](/docs/vpc?topic=vpc-file-storage-manage-replication).
