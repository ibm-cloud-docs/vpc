---

copyright:
  years: 2022, 2023
lastupdated: "2023-07-19"

keywords: file share, file storage, replication, replica, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About file share replication
{: #file-storage-replication}

You can create replicas of your file shares by setting up a replication relationship between primary file shares in one zone to replica file shares in another zone. Using replication is a good way to recover from incidents at the primary site, when data becomes inaccessible or applications fail.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Madrid, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Replication overview
{: #fs-replication-overview}

When you create a file share, you can set up replication. You create your file share in one zone of the region and a replica share in another zone in the same region. Based on the replication schedule, the service pulls data from the source file share to the replica file share.

You can choose how often you want to sync changes from the source share to the replica. You can specify hourly, daily, weekly, or monthly replication schedule conveniently in the UI. Or, you can specify replication by using a `cronspec` expression. Replications must be scheduled at least 1 hour apart. When a replica share is created, the first replica contains the data of the entire share. Thereafter, only the changes that occurred after the previous replication are added.

Data on the replica share is read-only. You can obtain read/write access to the data in two ways:

* [Fail over to the replication site](/docs/vpc?topic=vpc-file-storage-failover&interface=ui) - The read/writes from the source file share are paused and a final copy of the file share data is pulled into the replica share. The replica share becomes read/write accessible, and a reverse replication relationship is established. The original source file share now becomes the replica share and set to read-only. The service then begins pulling data from the new source file share.

   If a source file share is compromised, replica shares are a good way to recover operations. A [failover](/docs/vpc?topic=vpc-file-storage-failover) to a replica share assures no disruption to your services.
   {: tip} 

   When you initiate the failover, you can specify what happens to the replication relationship if the failover process times out or fails. This option is commonly used when you have a time requirement for how long your file share can be offline. You must specify what you want to happen if the operation times out or if the replication fails due to the original site, which is degraded or unavailable.

   - If the source site is not available due to a planned maintenance, you can choose to keep the replication relationship. The replication resumes as scheduled when the original source site is operational again.
   - In a disaster recovery situation, you can choose to split the volumes to bring the replica share online as soon as possible. However, in this case, you might not have the latest data set available and you might need to manually reconcile the state in your application. Because the replication relationship is severed, you need to set up replication anew when the original site becomes operational again.

* [Remove the replication relationship](/docs/vpc?topic=vpc-file-storage-manage-replication) - In this case, you split the two shares apart and create two independent file shares. Both shares are read/write accessible and data is no longer synchronized between the two. In the [API](/docs/vpc?topic=vpc-file-storage-failover&interface=ui#fs-failover-concepts), this operation is called a replica `split` operation. Removing the replica relationship is permanent, you cannot reestablish it between the two shares. However, you can create new replicas in the same zone or other zones of the same region.

Removing the replication relationship or failing over to the replica does not occur when another operation is being performed on the source or replica file share. (An example of such an operation is expanding the file share size.) The split or failover operations remains pending until the other operation completes.
{: note}

## Use cases
{: #fs-replication-scenarios}

You can use replication to address disaster recovery concerns. Replication addresses these scenarios:

* Disaster Recovery from an application failure.

   In this scenario, the application that you are running fails. The data is unaffected, but the application is not operational. You can perform a failover, where the data is quiesced and sent to another zone. The virtual server instances in that zone can be configured to take over the operation of the application while the primary servers are repaired.

* Disaster Recovery due to {{site.data.keyword.cloud_notm}} infrastructure failure.

   In this scenario, the {{site.data.keyword.cloud_notm}} availability zone in which your application is running becomes unusable. You need to start your application in the replica location as quickly as possible and use the replicated data from the last replication event. You can initiate the Failover with the `split` option to make the replica volume independent. Replication is stopped.

* Facilitate regular maintenance of your applications.

   Use replication to facilitate certain administrative tasks such as upgrades with greater availability. Migrate data between two zones that might be running different levels of application code. Running in two environments independently can allow greater flexibility in your deployment process.

## Replication procedure
{: #fs-replication-procedure}

The general procedure for establishing replication is:

1. [Create a file share](/docs/vpc?topic=vpc-file-storage-create-replication) and specify a replica file share in the UI, from the CLI, with the API or Terraform.
2. Verify that replication is working by checking the replication status - After the primary and replica file shares are created, you can check to see whether the replication is working.
3. Use the replica file share - If the primary file share fails or becomes unavailable for any reason, you can fail over to the replica file share. When you perform the failover, the replica share becomes the new primary file share, with read and write capability.
4. Restart the replication with the original file share as scheduled when it's back online. In this case, you can continue to use the replica site as primary, or fail back to the original site.

## Next steps
{: #fs-replication-next-steps}

[Set up replication for new and existing file shares](/docs/vpc?topic=vpc-file-storage-create-replication).
