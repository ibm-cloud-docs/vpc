---

copyright:
  years: 2022
lastupdated: "2022-05-24"

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

# Replication failover
{: #file-storage-failover}

Failover to the replica file share keeps your file share available should your source file share become unavailable. Failover switches the replication relationship, so that the replica file share becomes the source file share and the source share becomes the read-only replica file share.
{: shortdesc}

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, London, Sydney, Sao Paulo, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Replication failover concepts
{: #fs-failover-concepts}

When you create a replica file share, the replica pulls data from the source file share based on a replication schedule you set up. The data on the replica file share is set to read-only. Failover switches the replication relationship. The read-only replica file share becomes the read/write source file share and the original share becomes read-only. You can now manage share a regular file share (for example, you can [adjust IOPS tiers](/docs/vpc?topic=vpc-adjusting-share-iops)). 

The [failover status](/docs/vpc?topic=vpc-file-storage-managing&interface=api#file-storage-vpc-status) shows `failover_pending` while the operation is underway, or while the service is wating for another operation to complete, such as file share expansion. Use failover for routine maintainance on the primary site or when the site is having problems. You can also fallback to the original share.

If the failover operation fails or times out, it "falls back" to the source share. In the API, for example, you can see the `fallback_policy` property with the value `fail`. You can also set this value to `split`, which creates two independent file shares should the failover operation fail. In this case, because the final file synchronization could not complete, the replica share might not contain all the data of the source file share. Use this option for disaster recovery, when the source file share is known to be unreachable. For more information, see [Failover procedure in the API](#fs-failover-procedure-api).

A replica split or failover operation will not occur when another operation is being performed on the source or replica file share (for example, the file share size is being expanded). The split or failover operations will remain pending until the other operation completes.
{: note}

## Failover for disaster recovery
{: #fs-failover-dr}

Failover is also an option for disaster recovery, should the source data center become unavailable. Failover for disaster recovery works like this:

* The file share at the source site quiesces all read/write I/O and a final copy of the share's data is pulled by the replica file share.
* The replica file shares becomes read/write and the replica file share site becomes the source site. (The replication relationship is reversed.)
* The replica file share is now the source file share. When it determines the original site is not available, it quiesces read/writes and pulls a final copy of the file share's data from the original site.
* The file service determines that this is a disaster recovery situation and breaks the replication relationship, brining the original replica share online as the new source file share.

Failover for disaster recovery means that you'll likely have to reconcile the state of your application manually in case the newest copy of the data was not sent before it quiesed. 

When the original site comes back online, you can restablish a replication relationship and if desired, reverse the relationship to the original source and replica sites.

## Restrictions

Note these restrictions when preforming failover.

* The default timeout for a successful failover is 5 minutes. You can modify this value when [configuring failover](/docs/vpc?topic=vpc-file-storage-failover&interface=ui#fs-failover-procedure-ui) options.

* A failover will remain pending when other operations are being performed on the source file share, such as expanding the share size. When the operation completes, the failover resumes.

## Failover procedure with the UI
{: #fs-failover-procedure-ui}
{: ui}

1. Navigate to the list of all file shares. From the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

2. Click the name of a replica file share to go to its details page. 

3. From the **Actions** menu, select **Perform failover**. In the side panel, the replication relationship is now reversed. Before performing the failover, a final sync of the files is preformed to ensure the failover share has the latest content. When the failover completes, the replica file share becomes the new source file share. The former source share becomes the new read-only replica share.

4. To set a timeout value and change the default timeout of 5 minutes, check the box under **Timeout (optional)** and specify a time value. This value specifies an absolute time limit for the failover to complete. Set a timeout based on how long you can have your file share offline.

5. Under **Failover policy**, if the failover operation does not succeed or times out, choose to keep the replication relationship or change it:
    * Keep replication relationship - No changes are made to the replica file share or source file share.
    * [Remove replication relationship](/docs/vpc?topic=vpc-file-storage-manage-replication#fs-remove-replication) - This action creates two separate read/write file shares. Because the relationship is broken, changes to one file share do not affect the other. 
    
    After you break the relationship, it can't be re-established.
    {: note}

6. Click **Perform failover**. Messages display indicating failover has been requested and is being performed.

The file share details page is updated and the replication relationship now shows the replica file share as the new source file share.

## Failover procedure with the CLI
{: #fs-failover-procedure-cli}
{: cli}

Run the `ibmcloud is share-replica-failover` command and specify the `failback-policy` property. You can specify `fail` or `split` for this property.

This example specifies `fail` for the `failback-policy` property, which indicates that if the failover operation fails or the timeout is reached, the failover operation is unsuccessful.

```bash
ibmcloud is share-replica-failover dc1e70af-c3cb-44c6-a96d-2ece09b51ae3 --fallback-policy fail
The file share dc1e70af-c3cb-44c6-a96d-2ece09b51ae3 failover request was accepted under account VPC as user myuser@mycompany.com...
The file share failover request was accepted.
```
{: screen}

This example specifies `split` for the `failback-policy` property, which indicates that the replica share is split from the source file share whenever a failover operation fails. The result of this operation is two independent read-write file shares. 

```bash
ibmcloud is share-replica-failover 8b07129c-e376-4572-9ef3-68c729b315d5 --fallback-policy split 
The file share 8b07129c-e376-4572-9ef3-68c729b315d5 failover request was accepted under account VPC as user myuser@mycompany.com...
The file share failover request was accepted.
```
{: screen}

## Failover procedure with the API
{: #fs-failover-procedure-api}
{: api}

Make a `POST /shares/{share_id}/failover` request and specify the `timeout` and `fallback_policy` properties. The minimum timeout is 300 seconds and the maximum 3600 seconds. This request will fail over a source file share to the replica share, specified by the replica file share ID. 

By default, the `fallback_policy` property has the value `fail`. In this case, if the failover operation fails or the timeout is reached, the failover operation is unsuccessful.

If you specify `split` for the `failback_policy` property, the replica share is split from the source share whenever a failover operation fails. The result is two independent read-write file shares. In this case, because the final file synchronization could not complete, the replica share might not contain all the data of the source file share. Use this option for disaster recovery, when the source file share is known to be unreachable.

This example specifies `fail` for the `fallback_policy` property, which is the default. It's provided here as an example but you could leave it blank. The `timeout` property is also optional. You could use the default timeout.

```curl
curl -X POST \ 
"$vpc_api_endpoint/v1/shares/$replica_id?/failover?version=2022-05-03&generation=2"\
-H "Authorization: $iam_token"\
-d '{
     "fallback_policy": "fail",
      "timeout": 600
    }'
```
{: pre}

Simplified example:

```curl
curl -X POST \ 
"$vpc_api_endpoint/v1/shares/$replica_id?/failover?version=2022-05-03&generation=2"\
-H "Authorization: $iam_token"
```
{: pre}

A successful response indicates that the file share failover request was accepted.

You can use the API to verify that replication failover succeeded, is pending, or failed. Make a `GET /shares/{replica_id}` call. Look at the `latest_job` property. For more information, see [Verify replication with the API](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-verify-replica-api).
{: note}

## Next steps
{: #fs-failover-next-steps}

[Manage replication](/docs/vpc?topic=vpc-file-storage-manage-replication).
