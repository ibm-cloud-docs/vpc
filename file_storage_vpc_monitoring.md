---

copyright:
  years: 2021, 2025
lastupdated: "2025-11-20"

keywords: file share, file storage, rename share, increase size, adjust IOPS, mount target

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring file share health states, lifecycle status, and events
{: #fs-vpc-monitoring}

By using the console, the CLI, or the API, you can check on the status and health states of the file shares.
{: shortdesc}

## File share statuses
{: #file-share-statuses}

When you [view the list of your file shares in the console](/docs/vpc?topic=vpc-file-storage-view&interface=ui), you can see the status of each file share in the second column.
{: ui}

When you [view file share details from the CLI](/docs/vpc?topic=vpc-file-storage-view&interface=cli) by running either `ibmcloud is shares` or the `ibmcloud is share` command, the response contains the `Status` of the displayed file share.
{: cli}

When you make a [`GET /shares` request](/docs/vpc?topic=vpc-file-storage-view&interface=api), the API returns a `lifecycle_state` value as part of the information about the file share.
{: api}

Table 1 shows the lifecycle statuses that the file share can have.

| Status      | Explanation |
|-------------|-------------|
| `stable`    | The file share or mount target is stable and available for use. |
| `pending`   | The file share or mount target is being created. |
| `failed`    | The file share or mount target failed to be created. You can delete the failed share and try creating another one. \n This status is also shown when the access to the origin share is revoked from the accessor share.|
| `deleting`  | The file share or mount target is being deleted. |
| `suspended` | The file share violates [{{site.data.keyword.cloud}} Terms of Use](/docs/overview?topic=overview-terms). A suspended file share cannot be updated or deleted.|
| `updating`  | The file share capacity or IOPS is being updated.|
| `waiting`   |  |
{: caption="File Storage lifecycle states" caption-side="bottom"}

## File share replication status
{: #file-share-replication-status}

When you [view your file share in the console](/docs/vpc?topic=vpc-file-storage-view&interface=ui), you can see its Replication role and Replication status. This information is displayed in the file shares list and the file share details page in the File share replication relationship section.
{: ui}

When you [view the details of a file share from the CLI](/docs/vpc?topic=vpc-file-storage-view&interface=cli#fs-share-details-cli) by running the `ibmcloud is share` command, the response contains the `Replication status` of the specified file share.
{: cli}

When you make a [`GET /shares/{id}` request](/docs/vpc?topic=vpc-file-storage-view&interface=api#fs-single-file-shares-api), the API returns a `replication_status` value as part of the information about the file share.
{: api}

Table 2 shows all the replication status that the file shares can have.

| Status            | Explanation |
|-------------------|-------------|
| `active`          | This share is actively participating in replication, and the replica's data is up to date with the replication schedule. |
| `failover_pending`|  This share is performing a replication failover. Or the service is waiting for another operation to complete, such as file share expansion, before the failover can commence. |
| `initializing`    | This share is initializing replication. |
| `none`            | This share is not participating in replication. |
| `split_pending`   | This share is performing a replication split. |
{: caption="File Storage replication status" caption-side="bottom"}

## Replication sync health
{: #fs-repl-synchealth}

Replication is an asynchronous operation, which is not instantaneous. The system queries the last sync status every 15 minutes. The result shows the data of the last replication that completed, such as start and end date, and the transferred data volume.

You can see information about the last replication operation when you view the details of either the source or the replica share. For more information, see [View details of a file share in the console](/docs/vpc?topic=vpc-file-storage-view&interface=ui#fs-view-single-share-ui).
{: ui}

You can see information about the last replication operation when you list the details of either the source or the replica share. For more information, see [View details of a file share from the CLI](/docs/vpc?topic=vpc-file-storage-view&interface=cli#fs-share-details-cli).
{: cli}

You can programmatically retrieve the last sync details by calling the `/shares` method in the [VPC API](/apidocs/vpc/latest#get-share){: external}. Look for the `latest_sync` section in the API response to see when the replication started (`started_at`), when it ended (`completed_at`), and how much data was transferred (`data_transferred`). For more information, see [View a single file share with the API](/docs/vpc?topic=vpc-file-storage-view&interface=api#fs-single-file-shares-api).
{: api}

When the amount of data to be transferred exceeds the amount of data that can be transferred during the replication window with the normal transfer rate, the replication process can't complete and the replication status becomes `degraded`. If this situation occurs, try adjusting the rate of change on the file share and the replication frequency.

## Activity tracking events
{: #fs-activity-tracker}

You can use {{site.data.keyword.atracker_full}} to configure how to route auditing events. Auditing events are critical data for security operations and a key element for meeting compliance requirements. Such events are triggered when you create, modify, or delete a file share. Activity tracking events are also triggered when you establish and use file share replication. For more information, see [Activity tracking events for IBM Cloud VPC](/docs/vpc?topic=vpc-at_events).

## Logging for file share service
{: #fs-event-la-logs}

After you provision {{site.data.keyword.logs_routing_full_notm}} to add log management capabilities to your {{site.data.keyword.cloud}} architecture, you can enable platform logs to view and analyze logs of the {{site.data.keyword.filestorage_vpc_short}} service. When replication occurs, the file service generates a `regional-file.00002I` log message, which includes information about when the replication occurred, and how much data was transferred. For more information, see [Logging for VPC](/docs/vpc?topic=vpc-logging#logging-file-share-replication).

## Monitoring metrics
{: #fs-sysdig-mon}

You can monitor the read and write throughput, read and write IOPS, number of mount targets, and capacity usage of your share over time in the {{site.data.keyword.cloud_notm}} console. To see these metrics, go to the file share details page, and click the **Monitoring** tab.

Monitoring utilization metrics such as total IOPS and total throughput can help you to determine how much work is done by your application or workload. You can use this information to determine whether the IOPS value needs to be adjusted. Monitoring the available capacity of your share can help you identify the need for more storage before insufficient space can become a problem with writing data to the share or replication. Seeing these metrics can help you anticipate any changes in charges at the end of the billing period. These graphs are available to you at no cost, even without an {{site.data.keyword.mon_full_notm}} instance.

If you have an instance of the {{site.data.keyword.mon_full_notm}} service, click **Launch monitoring** to open the Sysdig web UI to work with the metrics dashboards there. In the Sysdig web UI you can view the throughput, IOPS, and capacity metrics in more detail, customize your dashboards, and set up alerts. For more information, see [Getting started with monitoring](/docs/monitoring?topic=monitoring-getting-started) and [Monitoring metrics for {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-fs-vpc-monitoring-sysdig).
