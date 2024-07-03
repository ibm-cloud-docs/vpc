---

copyright:
  years: 2020, 2024
lastupdated: "2024-07-03"

keywords: flow logs, ordering, logging, log analysis

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Logging for VPC
{: #logging}

{{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.vpc_short}}, generate platform logs that you can use to investigate abnormal activity and critical actions in your account, and troubleshoot problems.
{: shortdesc}

You can use {{site.data.keyword.logs_routing_full_notm}}, a platform service, to route platform logs in your account to a destination of your choice by configuring a tenant that defines where platform logs are sent. For more information, see [About Logs Routing](/docs/logs-router?topic=logs-router-about).

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on platform logs that are generated in your account and routed by {{site.data.keyword.logs_routing_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.



As of 28 March 2024, the {{site.data.keyword.la_full_notm}} service is deprecated and will no longer be supported as of 30 March 2025. Customers will need to migrate to {{site.data.keyword.logs_full_notm}} before 30 March 2025. During the migration period, customers can use {{site.data.keyword.la_full_notm}} along with {{site.data.keyword.logs_full_notm}}. Logging is the same for both services. For information about migrating from {{site.data.keyword.la_full_notm}} to {{site.data.keyword.logs_full_notm}} and running the services in parallel, see [migration planning](/docs/cloud-logs?topic=cloud-logs-migration-intro).
{: important}

## Locations where platform logs are generated
{: #log-locations}



### Locations where logs are sent to {{site.data.keyword.la_full_notm}}
{: #la-legacy-locations}



{{site.data.keyword.vpc_short}} sends platform logs to {{site.data.keyword.la_full_notm}} in the regions indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  | Toronto (`ca-tor`) | Sao Paulo (`br-sao`) |
|---------------------|-------------------------|-------------------|----------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where platform logs are sent in Americas locations" caption-side="top"}
{: #la-table-1}
{: tab-title="Americas"}
{: tab-group="la"}
{: class="simple-tab-table"}
{: row-headers}

| Tokyo (`jp-tok`)    | Sydney (`au-syd`) |  Osaka (`jp-osa`) | 
|---------------------|------------------|------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | 
{: caption="Regions where platform logs are sent in Asia Pacific locations" caption-side="top"}
{: #la-table-2}
{: tab-title="Asia Pacific"}
{: tab-group="la"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) | Madrid (`eu-es`) |
|---------------------------------------------------------------|---------------------|------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where platform logs are sent in Europe locations" caption-side="top"}
{: #la-table-3}
{: tab-title="Europe"}
{: tab-group="la"}
{: class="simple-tab-table"}
{: row-headers}

### Locations where logs are sent by {{site.data.keyword.logs_routing_full_notm}}
{: #lr-locations}



{{site.data.keyword.vpc_short}} sends logs by {{site.data.keyword.logs_routing_full_notm}} in the regions that are indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  | Toronto (`ca-tor`) | Sao Paulo (`br-sao`) |
|---------------------|-------------------------|-------------------|----------------------|
| [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} |
{: caption="Regions where platform logs are sent in Americas locations" caption-side="top"}
{: #lr-table-1}
{: tab-title="Americas"}
{: tab-group="lr"}
{: class="simple-tab-table"}
{: row-headers}

| Tokyo (`jp-tok`)    | Sydney (`au-syd`) |  Osaka (`jp-osa`) | 
|---------------------|------------------|------------------|
| [No]{: tag-red} | [Yes]{: tag-green} | [No]{: tag-red} | 
{: caption="Regions where platform logs are sent in Asia Pacific locations" caption-side="top"}
{: #lr-table-2}
{: tab-title="Asia Pacific"}
{: tab-group="lr"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) | Madrid (`eu-es`) |
|---------------------------------------------------------------|---------------------|------------------|
| [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} |
{: caption="Regions where platform logs are sent in Europe locations" caption-side="top"}
{: #lr-table-3}
{: tab-title="Europe"}
{: tab-group="lr"}
{: class="simple-tab-table"}
{: row-headers}
  
## Viewing logs
{: #log-viewing}

### Launching {{site.data.keyword.logs_full_notm}} from the Observability page
{: #log-launch-standalone}



For more information about launching the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.logs_full_notm}} documentation.](/docs/cloud-logs?topic=cloud-logs-instance-launch) 

## Fields per log type
{: #logging_fields}

Table 1 outlines the fields that are included in each log record:

| Field             | Type       | Description             |
|-------------------|------------|-------------------------|
| `logSourceCRN`    | Required   | Defines the account and flow log instance where the log is published. |
| `saveServiceCopy` | Required   | Defines whether IBM saves a copy of the record for operational purposes. |
| `message`         | Required   | Description of the log that is generated. |
| `messageID`       | Required   | ID of the log that is generated. |
| `msg_timestamp`   | Required   | The timestamps when the log is generated. |
| `resolution`      | Optional   | Guidance on how to proceed if you receive this log record. |
| `documentsURL`    | Optional   | More information on how to proceed if you receive this log record. |
| `generation`      | Required   | Defines the VPC source of the log. Valid options are `1` for VPC Classic, and `2` for `VPC Gen 2`. |
{: caption="Table 1. Log record fields" caption-side="bottom"}

## Log messages
{: #logging_msgs}

The following tables list the message IDs that are generated by VPC services:

### Flow log collector
{: #logging-flow-log-collector_msgs}

Table 2 outlines the message IDs that are generated by the flow log collector service:

| Message ID                     | Type  | Learn More  |
|--------------------------------|-------|---------------------|
| `is.flow-log-collector.00001E` | `err` | [`Failed to write Flow Log file for the past 24 hours. Dropping flow log for Virtual Server <ServerName>`](/docs/vpc?topic=vpc-fl-ts-error-failed-write) |
| `is.flow-log-collector.00002E` | `err` | [`Unauthorized access to Cloud Object Storage bucket <BucketName>`](/docs/vpc?topic=vpc-fl-ts-error-unauth-access-cos) |
| `is.flow-log-collector.00003E` | `err` | [`Cloud Object Storage bucket <BucketName> was not found`](/docs/vpc?topic=vpc-fl-ts-error-cos-bucket) |
{: caption="Table 2. Message IDs that are generated by Flow Log Collector" caption-side="bottom"}

Flow log collector generates hourly logs.
{: note}

### Dedicated Host
{: #logging-dedicated-host}

Table 3 outlines the message IDs that are generated for dedicated hosts:

| Message ID             | Type   | Learn More  |
|------------------------|--------|---------------------|
| `dedicated-host.00001` | `err`  | [`Failed to create dedicated host <Dedicated Host ID> due to insufficient capacity in zone.`](/docs/vpc?topic=vpc-why-did-a-dedicated-host-fail-to-create-) |
| `dedicated-host.00002` | `info` | `Provisioned a virtual server instance on dedicated host <Dedicated Host ID>.`              |
| `dedicated-host.00003` | `info` | `Removed a virtual server instance on dedicated host <Dedicated Host ID>.`              |
{: caption="Table 3. Message IDs that are generated for dedicated hosts" caption-side="bottom"}

A log is generated when each Dedicated Host event occurs.
{: note}

### Resource Quota
{: #logging-resource-quota}

Table 4 outlines the message IDs that are generated for resource quota events:

| Message ID             | Type   | Learn More  |
|------------------------|--------|---------------------|
| `quota-monitoring.00001` | `info`  | `Successfully provisioned resource <Resource ID>.`        |
| `quota-monitoring.00002` | `err` | [`Failed to provision resource <Resource ID> due to resource quota limits.`](/docs/vpc?topic=vpc-rqe-ts-failed-publish)              |
| `quota-monitoring.00003` | `info` | `Successfully updated resource <Resource ID>.`              |
| `quota-monitoring.00004` | `err` | [`Failed to update resource <Resource ID> due to resource quota limits.`](/docs/vpc?topic=vpc-rqe-ts-failed-publish)             |
{: caption="Table 4. Message IDs that are generated for resource quota events" caption-side="bottom"}

A log is generated when a provision or update resource quota event succeeds or fails.
{: note}

### Snapshots for VPC
{: #logging-snapshots}

Table 5 outlines the message IDs that are generated by the Snapshots service:

| Message ID       | Type   | Learn More  |
|------------------|--------|---------------------|
| `snapshot.00001` | `info` | `Snapshot creation requested for volume <Volume ID>.` |
| `snapshot.00002` | `info` | `Snapshot <Snapshot ID> is successfully captured. Volume <Volume ID>` |
| `snapshot.00003` | `info` | `Snapshot <Snapshot ID> is an incremental snapshot. Volume <Volume ID>` |
| `snapshot.00004` | `info` | `Snapshot <Snapshot ID> is a full snapshot. Volume <Volume ID>` |
| `snapshot.00005` | `info` | `Snapshot <Snapshot ID> is available. Volume <Volume ID>` |
| `snapshot.00006` | `info` | `Snapshot <Snapshot ID> is uploaded. Volume <Volume ID>` |
| `snapshot.00007` | `info` | `Snapshot <Snapshot ID> deletion requested.` |
| `snapshot.00008` | `info` | `Snapshot <Snapshot ID> is successfully deleted. Volume <Volume ID> Region <Region>` |
| `snapshot.00009` | `info` | `All snapshots of volume <Volume ID> in the region <Region> are requested to be deleted.` |
| `snapshot.00010` | `info` | `Delete all snapshots request for volume <Volume ID> is completed successfully. Region <Region>` |
| `snapshot.00010` | `info` | `Snapshot copy creation in region <Region> requested for snapshot <Snapshot ID> from region <Source Region>. Volume <Volume ID>` |
{: caption="Table 5. Message IDs that are generated for Snapshot events" caption-side="bottom"}


### File share replication
{: #logging-file-share-replication}

Table 6 outlines the message IDs that are generated for File Storage replication events:

| Message ID             | Type   | Description |
|------------------------|--------|------------|
| `regional-file.00001I` | `info` | The replication status of `{{.shareID}}` is active. |
| `regional-file.00002W` | `warning` | The replication status of `{{.shareID}}` is degraded. |
| `regional-file.00003I` | `info` | Initiated by the cron schedule `{{.cronSpec}}`, `{{.dataTransferredInGiB}}` of data transferred from the source share `{{.shareID.}}` between `{{.startedAt.}}` and `{{.endedAt.}}` with a data transfer rate of `{{.transferRate}}`. |
{: caption="Table 6. Message IDs that are generated for file share replication events" caption-side="bottom"}

A log is generated when a replication event occurs.
{: note}

### File share cross-account access
{: #logging-file-share-accessor}

[New]{: tag-new}

Table 7 outlines the message IDs that are generated for File Storage related events:

| Message ID | Type | Description |
|------------|------|-------------|
| `is.share.00004I` | `info` | The lifecycle_state of accessor share `{{.shareID}}` is stable. |
| `is.share.00005I` | `info` | The lifecycle_state of accessor share `{{.shareID}}` is failed and the reason for failure is `{{.shareLifecycleReason}}`. |
| `is.share.00006I` | `info` | The lifecycle_state of share mount target `{{.targetID}}` for the `{{.shareID}}` at accessor account is stable. |
| `is.share.00007I` | `info` | The lifecycle_state of share mount target `{{.targetID}}` for the `{{.shareID}}` at accessor account is failed. |
{: caption="Table 7. Message IDs that are generated for accessor file share events" caption-side="bottom"}
