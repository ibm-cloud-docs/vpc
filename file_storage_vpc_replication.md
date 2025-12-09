---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-09"

keywords: file share, file storage, replication, replica, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About file share replication
{: #file-storage-replication}

You can create replicas of your file shares in another zone of the same geography. With the replication feature, you can keep a read-only copy of your file share in another zone. The replica share is updated from the source share on a schedule that you specify. Replication provides a way to recover from an incident at the primary site, when data becomes inaccessible or an application fails. Replication can also be used for geographical expansion.
{: shortdesc}

[Select availability]{: tag-green} Customers with special access to preview the new regional file share offering can use the **rfs** profile to create file shares with regional availability. When you create file shares with regional availability, data is automatically replicated throughout the region, so you don't need to set up replication pairs. Cross-regional replication of regional file shares is not supported in this release.

Currently, cross-regional replication for zonal file shares is not supported in the Chennai region.
{: restriction}

## Replication overview
{: #fs-replication-overview}

After you created a file share, you can set up replication. 

When a replica share is created, the first replica contains the data of the entire share. Thereafter, only the changes that occurred after the previous replication are added.

You can create a replica share in another zone of the same region. You can also create a replica in another region in the same geography, if you have another VPC in the target region. Cross-geography replication is not supported.

| Americas | Europe  | Asia  |
|----------|---------|-------|
| - Dallas, TX / `us-south` \n - Montreal / `ca-mon` \n - Sao Paulo / `br-sao` \n - Toronto / `ca-tor` \n - Washington, DC / `us-east` |  - Frankfurt / `eu-de` \n - London / `eu-gb` \n - Madrid / `eu-es`| - Osaka/ `jp-osa` \n - Sydney / `au-syd`\n - Tokyo / `jp-tok` |
{: caption="This table shows the metro regions that can replicate with each other in each geography. Every geography is a separate column." caption-side="bottom"}

When you create your replica file share in another zone of the same region, the replica share inherits the encryption type and key from the source file share. The encryption can't be changed.

When you replicate your file share to another region, the replica must match the type of encryption that the source share has. However, it does not inherit the encryption from the source. In other words, if the source share is encrypted with provider-managed keys, the replica must have provider-managed encryption, too. If the source share is encrypted with a customer-managed key, the replica must be encrypted with a customer-managed key as well. However, it does not have to be the same key. When you create the replica, provide the CRN of the key that you want to use.

Based on the replication schedule, the service pulls data from the source file share to the replica file share. You can choose how often you want to sync changes from the source share to the replica. You can specify an hourly, daily, weekly, or monthly replication schedule. Replications must be scheduled at least 15 minutes apart.

While you can't create snapshots of a replica share manually or programmatically, the snapshots of the origin share are copied to the replica share at the next scheduled sync. If a source share with snapshots is replicated, the corresponding replica share snapshots are created with system-generated names, rather than inheriting the source share snapshot names. Since replicated snapshots share the source's fingerprint ID, you can use the fingerprint to correlate the snapshots.

When you replicate across regions, the data is crossing VPC boundaries. Both VPCs and file shares must belong to the same account, and you need to [establish service-to-service authorizations](/docs/vpc?topic=vpc-file-s2s-auth) between the file services of the two regions. The data is encrypted in transit while it's moving between file shares. Charges for data transfer between the two file shares are calculated with a flat rate in GB increments. The charges are based on the amount of data that was transferred during the entire billing period.

When you replicate data across regions, consider the local data residency laws because moving data across borders can have legal implications.

Replication is an asynchronous operation and it's not instantaneous. You can use the [replication sync information](/docs/vpc?topic=vpc-file-storage-manage-replication#fs-repl-syncinfo) to see the duration of the replication process and the transfer rate. By reviewing the replication sync information, you can adjust your replication schedule, and balance your costs with how often you need the data to be refreshed on the replica. By viewing the job logs and the transfer rates, you can also determine whether the size of the data that needs to be transferred fits within the replication window.

You need to have sufficient unused capacity in your file share for replication to complete. During the replication process, new data from the source is copied to the replica volume. The old data is not overwritten immediately, but removed after the copy operation completes. For example, if your share is at 95% capacity and the rate of change is 10%, the replica might not have enough space to hold the changes. If the replica does not have enough space to hold the updates, the replication process fails. You can monitor the file share capacity in the console, and configure alerts for utilization. For more information, see [Monitoring metrics for {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-fs-vpc-monitoring-sysdig).

Data on the replica share is read-only. You can obtain read/write access to the data in two ways:

* [Fail over to the replication site](/docs/vpc?topic=vpc-file-storage-failover&interface=ui) - The read/writes from the source file share are paused and a final copy of the file share data is pulled into the replica share. The replica share becomes read/write accessible, and a reverse replication relationship is established. The original source file share now becomes the replica share and set to read-only. The service then begins pulling data from the new source file share.

   If a source file share is compromised, replica shares are a good way to recover operations. A [failover](/docs/vpc?topic=vpc-file-storage-failover) to a replica share ensures no disruption to your services.
   {: tip}

   When you initiate the failover, you can specify what happens to the replication relationship if the failover process times out or fails. This option is commonly used when you have a time requirement for how long your file share can be offline. You must specify what you want to happen if the operation times out or if the replication fails due to the original site, which is degraded or unavailable.

   - If the source site is not available due to a planned maintenance, you can choose to keep the replication relationship. The replication resumes as scheduled when the original source site is operational again.
   - In a disaster recovery situation, you can choose to split the volumes to bring the replica share online as soon as possible. However, in this case, you might not have the most recent data set available and you might need to manually reconcile the state in your application. Because the replication relationship is severed, you need to set up replication anew when the original site becomes operational again.

* [Remove the replication relationship](/docs/vpc?topic=vpc-file-storage-manage-replication) - In this case, you split the two shares apart and create two independent file shares. Both shares are read/write accessible and data is no longer synchronized between the two. In the [API](/docs/vpc?topic=vpc-file-storage-failover&interface=ui#fs-failover-concepts), this operation is called a replica `split` operation. Removing the replica relationship is permanent, you cannot reestablish it between the two shares. However, you can create new replicas in the same zone or other zones of the same region.

Removing the replication relationship or failing over to the replica does not occur when another operation is being performed on the source or replica file share. (An example of such an operation is expanding the file share size.) The split or failover operation remains in pending status until the other operation completes.
{: note}

## Use cases
{: #fs-replication-scenarios}

You can use replication to address disaster recovery concerns. The replication addresses these scenarios:

* Disaster Recovery from an application failure.

   In this scenario, the application that you are running fails. The data is unaffected, but the application is not operational. You can perform a failover, where the data is quiesced and sent to another zone. The virtual server instances in that zone can be configured to take over the operation of the application while the primary servers are repaired.

* Disaster Recovery due to {{site.data.keyword.cloud_notm}} infrastructure failure.

   In this scenario, the {{site.data.keyword.cloud_notm}} availability zone in which your application is running becomes unusable. You need to start your application in the replica location as quickly as possible and use the replicated data from the last replication event. You can initiate the Failover with the `split` option to make the replica volume independent. Replication is stopped.

* Facilitate regular maintenance of your applications.

   Use replication to facilitate certain administrative tasks such as upgrades with greater availability. Migrate data between two zones that might be running different levels of application code. Running in two environments independently can allow greater flexibility in your deployment process.

* Data migration or geographic expansion.
 
   You can use replication to migrate data between two MZR regions easily. After your data is replicated, you can [remove the replication relationship](/docs/vpc?topic=vpc-file-storage-manage-replication#fs-remove-replication) and your replica file share becomes available with your data ready to use independently in the new region.

## Next steps
{: #fs-replication-next-steps}

1. [Create a replica file share](/docs/vpc?topic=vpc-file-storage-create-replication) in the console, from the CLI, with the API or Terraform.

   If you want to set up replication between different regions, you need to [establish service-to-service authorizations](/docs/vpc?topic=vpc-file-s2s-auth) between the file services of the two VPCs first. 
   {: requirement}

   If you want to create a replica share in a different region where you use a different KMS solution, [establish service-to-service authorizations](/docs/vpc?topic=vpc-file-s2s-auth) between the file service and the target KMS.
   {: requirement}

2. Verify that the replication is working by checking the replication status and the [replication sync information](/docs/vpc?topic=vpc-file-storage-manage-replication#fs-repl-syncinfo). The system queries the last sync status every 15 minutes.

3. Use the replica file share - If the primary file share fails or becomes unavailable for any reason, you can fail over to the replica file share. When you perform the failover, the replica share becomes the new primary file share, with read and write capability.
   
4. Restart the replication with the original file share as scheduled when it's back online. In this case, you can continue to use the replica site as primary, or fail back to the original site.
