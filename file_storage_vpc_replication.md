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

# About file share replication
{: #file-storage-replication}

You can create replicas of your file shares by setting up a replication relationship between primary file shares in one zone to replica file shares in another zone. Using replication is a good way to recover from incidents at the primary site, when data becomes inaccessible or applications fail.
{: shortdesc}

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, London, Sydney, Sao Paulo, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Replication overview
{: #fs-replication-overview}

With this feature, when you create a new file share you can set up a replication relationship between a primary source file share to a replica file share. You specify a different zone for the replica file share. When the file share is created, so is the replica share in the other zone. When the replication relationship is established, the replica file share begins pulling data from the source file share. 

Should a source file share be compromised, replica shares are a good way to recover. [Failover](/docs/vpc?topic=vpc-file-storage-failover&interface=ui) to a replica share assures no disruption to your services.

You can choose how often you want to sync changes from the source share to the replica. You can specify daily, weekly, or monthly replication conveniently from the UI. Or, specify replication using a `cronspec` (replications must be scheduled at least one hour apart). When a replica share is created, the first replica contains the entire share. Thereafter, the changes that occured since the previous replication are added. 

Data on the replica share is read-only. You can obtain read-write access to the data in two ways:

* [Break the replication relationship](/docs/vpc?topic=vpc-file-storage-manage-replication) - In this case, data is not longer written to the replica share and it becomes read/write accessible. You can then configure a new replica share in the original zone, or choose another zone within the region. In the [API]/docs/vpc?topic=vpc-file-storage-failover&interface=ui#fs-failover-concepts), this operation is called a replica `split` operation.

* [Failover to the replication site](/docs/vpc?topic=vpc-file-storage-failover&interface=ui) - The read/writes from the source file share are paused and a final copy of the file share data is pulled into the replica share. The replica share then becomes read/write accessible, and a reverse replication relationship is established. The original source file share now becomes the replica share and set to read-only. It then begins pulling data from the new source file share. 

    If the original site is unavailable, the replica share site pauses reads/writes and pulls the final copy of the source file share's data, the service assumes this is a disaster recovery situation and breaks the replication process, to bring the replica share online as soon as possible. In this case, you most likely need to manually reconcile the state your application, then set up new replication.

Breaking the replication relationship or failing over to the replica will not occur when another operation is being performed on the source or replica file share (for example, the file share size is being expanded). The split or failover operations will remain pending until the other operation completes.
{: note}
    
## Scenarios for using replication
{: #fs-replication-scenarios}

Replication allows you to provide a way to address disaster recovery concerns of your file shares. Replication addresses these scenarios:

* Disaster Recovery from an application failure.

    In this scenario, an application that you're running has failed. The data is unaffected, but the application is not operational. You can perform a failover, where the data is quiesced and sent to another zone. The virtual server instances in that zone can be configured to take over the operation of the application while the primary servers are repaired. 

* Disaster Recovery due to Cloud infrastructure failure.

    In this scenario, the IBM Cloud availability zone in which your application is running has become unusable. You need to bring up your application as quickly as possible and recover your data. Because the service can't tell the state of the application, the replication is stopped to protect data in the replication relationship. If the source file share zone becomes available again, data will be available from the replica share to reconcile from the time of the incident to the recovery point.

* Facilitate regular maintenance of your applications.

    Use replication to facilitate certain administrative tasks such as upgrades with greater availability. Migrate data between two zones that might be running different levels of application code. Running in two environments independently can allow greater flexibility in your deployment process.

## Replication procedure
{: #fs-replication-procedure}

The general procedure for establishing replication is:

1. [Create a new file share](/docs/vpc?topic=vpc-file-storage-create-replication&interface=ui) and specify a replica file share using the UI, CLI, or API - For example, using the API, you would  make a `POST /shares` call and specify the `replica_share` parameter and define the replica and zone in which it will be created. In the same call, you would specify the `replication_cron_spec`parameter to define the replication schedule. You'd also specify the target zone in which to create the replica file share.
2. Verify replication is working by checking the replication status - After the primary and replica file shares are created, you can check to see if the replication is working. For example, from the API you'd make a `GET /shares/{id}` call. In the response, the `replication_status` parameter would indicate `active` when it's operationa.
3. Use the replica file share - In the case of site failure for the primary file share, you can fail over to the replica file share, which becomes the new primary file share, with read/write capability.
4. Re-establish a replica relationship with the original file share when it's back online - In this case, you can continue using the replica site as primary, or fail over again to the original site.

## Next steps
{: #fs-replication-next-steps}

[Set up replication for new and existing file shares](/docs/vpc?topic=vpc-file-storage-create-replication).
