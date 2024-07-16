---

copyright:
  years: 2020, 2024
lastupdated: "2024-07-16"

keywords: flow logs, viewing objects, SQL, analyze

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Viewing flow log objects
{: #fl-analyze}

A flow log is a summary of the network traffic that is uniquely identified by a connection between two virtual network interface cards (vNICs), within a certain time window. A flow log describes traffic the firewall either accepts (relevant security groups or network ACLs) or rejects, but not both. It contains header information and
payload statistics.
{: shortdesc}

Currently, flow logs collect Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) traffic, but not Internet Control Message Protocol (ICMP) traffic.
{: note}

Each flow log object contains individual flow logs. To view or analyze the flow logs, follow these steps.

1. Ensure that you created an instance of {{site.data.keyword.sqlquery_full}}. To create a service instance, see [Getting started with IBM Cloud Data Engine](/docs/sql-query).

   {{site.data.keyword.cos_full_notm}} offers Lite (free) and Standard pricing plans. If you are using the Lite plan, you will not be able to perform tasks that are available with the Standard plan, such as creating tables or using SQL queries. Instead, see [Viewing generated flow log object files from the {{site.data.keyword.cos_short}} bucket](/docs/vpc?topic=vpc-fl-analyze&interface=ui#alternative-method) and alternative methods documented in the following procedures.
   {: note}

1. Launch {{site.data.keyword.sqlquery_full}}.
1. Start querying flow logs from the bucket that you specified when [Creating a flow log collector](/docs/vpc?topic=vpc-ordering-flow-log-collector).

   To view the most frequent flow object, run the following query:

   ```json
   SELECT * FROM cos://<region>/<bucket>/ibm_vpc_flowlogs_v1/ STORED AS JSON ORDER BY `stream-id` LIMIT 1 INTO cos://<region>/<target_bucket>/result/ STORED AS JSON
   ```
   {: pre}

   Where:

   * **bucket** - The bucket where your flow logs are stored.
   * **region** - The [region alias](/docs/sql-query#endpoints) of the bucket that holds your flow logs.
   * **target_bucket** - A bucket that is different from the bucket where your flow logs are stored. It is recommended to use the target location that was created by {{site.data.keyword.sqlquery_full}}.

      You can find the **Target location** listed under the query editor field in the IBM SQL Query UI.
      {: note}

1. Use the preview to inspect the content of the object.
1. Refine your query to analyze specific aspects of your flow logs by using [SQL](/docs/sql-query?topic=sql-query-sql-reference "SQL Query Language Reference").

Flow logs are periodically written to {{site.data.keyword.cos_full}} about every 5 minutes. Flow logs are published more frequently in cases where the flow log collector reaches 100 KB flows.
{: note}

## Flow log data format
{: #flow-log-data-format}

Flow log traffic summaries contain the following information:

- Byte/packet counts, separately for RX (receive) and TX (transmit).
- Whether a connection starts or stops in the time window.

Because a flow log reflects network traffic in a limited window of time, a long-running connection might cause multiple flow logs to be emitted. For every connection processed by a vNIC, a time-ordered sequence of flow logs emits (not including failures). These flow logs appear in one or more {{site.data.keyword.cos_short}} objects.

The `initiator_ip` in a flow log is defined to be the `source_ip` that appears in the first packet of its connection that reached the vNIC. At the implementation level, this packet is typically the one that causes the new connection to be added to the connection table. Similarly, the `target_ip` in the flow log is set to the `dest_ip` field in that same packet.

If both the connection initiator vNIC and connection target vNIC enable flow logs, the initiator and target IP and ports fields are identical in the flow logs on both vNICs.
{: note}

**Example**: Consider a client that is sending an HTTP request to a web server. The flow log for this HTTP request on the client-side vNIC has the client’s IP address as `initiator_ip`. The corresponding flow log on the server-side vNIC has the same `initiator_ip`.

The `start_time` and `end_time` in a flow log reflects:

* Capture time - The time that data path elements were queried for traffic counters.
* Data path time - The time as maintained in the data path element itself.

It's possible that the flow log does not reflect all traffic (for example, in the data path) between a flow log `start_time` and `end_time`. In other words, it might be that packets sent and received by the vNIC toward the end of the capture window are reflected only in a flow log with the later `start_time` window.

**Flow logs reflect actual traffic on connections**: If traffic does not occur on a connection in a capture window, no flow log appears for it in the {{site.data.keyword.cos_short}} object for that window. Meaning that the sequence of flow logs for a connection might be mapped to a sequence of non-consecutive {{site.data.keyword.cos_short}}objects.

The flow logs array within an object does not need to be sorted in any specific order.
{: note}

Rejected traffic:

- A specific flow log summarizes either rejected or accepted traffic, never both.
- The sequence of flow logs for a connection might have time-window overlaps between accepted and rejected flow logs.
- Rejected packets that are not associated with an existing connection (for example, a security group, or network ACL, denies the connection initiation).
- Rejected packets on an existing connection (for example, traffic that does not match the TCP state-machine, or a network ACL rule changed mid-connection).
-  Rejected packets are written to separate flow logs based on whether they are incoming or outgoing.

Flows are tagged as rejected if their packets were blocked by security group or network ACL rules.
{: note}

## Flow log object format
{: #flow-log-object-format}

Flow logs are written to the user-specified {{site.data.keyword.cos_short}} bucket in the following naming convention:

```sh
ibm_vpc_flowlogs_v1/account={account}/region={region}/vpc-id={vpc-id}/subnet-id={subnet-id}/endpoint-type=vnics/instance-id={vsi-id}/vnic-id={vnic-id}/record-type={all|ingress|egress}/year={xxxx}/month={yy}/day={zz}/hour={hh}/stream-id={stream-id}/{sequence-number}.gz
```
{: screen}

Where:

* `{stream-id}` is a date and time string in ISO 8601 format `yyyymmddThhmmssZ` (Coordinated Universal Time), defined as the time the first object in the `directory` was created.
* `{sequence-number}` is a running counter of objects within the stream, represented as an eight-character, zero-filled field (`%08d`).
* `{all|ingress|egress}` shows the type of traffic the flow includes.
* `{xxxx}` is the year in four-digit format.
* `{yy}` is the month in two-digit, zero-leading format.
* `{zz}` is the day of the month in two-digit, zero-leading format.
* `{hh}` is the hour in zero-leading format (for example, 00-24).

Any instances of URL-unsafe characters in the path are URL-encoded.
{: note}

A flow log's {{site.data.keyword.cos_short}} object contains a single valid JSON object. The {{site.data.keyword.cos_short}} object is `.gz` compressed.

The object header fields that are specified in the following table are written to the {{site.data.keyword.cos_short}} object metadata.

### Flow logs object header fields (per {{site.data.keyword.cos_short}}S object)
{: #flow-logs-object-header-fields}

| Field                  | Type   | Description       |
| ---------------------- | ------ | ----------------- |
| `version`                | string | The Semantic version. |
| `collector_crn`          | string | The Cloud Resource Name (CRN) of the flow log collector. |
| `attached_endpoint_type` | string | Currently, the only value is `vnic`. |
| `network_interface_id`   | string | The `vnic` ID, as it has no vNIC CRN. |
| `instance_crn`           | string | The CRN of the instance that the network interface is attached to. |
| `vpc_crn`                | string | The CRN of the VPC that the flow log collector is a member of. |
| `capture_start_time`     | string | RFC 3339 Date and Time (Coordinated Universal Time) |
| `capture_end_time`       | string | RFC 3339 Date and Time (Coordinated Universal Time) |
| `state`                  | string |  Indicates the operational state of the flow log collector. `OK` means that data is being collected and shipped without any errors. `skip data` indicates data that was lost during this collection interval (for example, because of high rate of rejected SYN packets). |
| `number_of_flow_logs`    | uin32  | The number of elements in a `flow_logs` array. Since this number is highly variable, it's useful as a quick reference of the number of flow logs contained in a single {{site.data.keyword.cos_short}} object, without needing to download the object first. |
| `flow_logs`              | array of JSON objects | This can be an empty array, which indicates `no traffic`.|
{: caption="Table 1. Flow logs object header fields (per {{site.data.keyword.cos_short}} object)" caption-side="bottom"}

### Flow log fields
{: #flow-log-fields}

| Field                  | Type   | Description       |
| ---------------------- | ------ | ----------------- |
| `start_time`             | string | When the first byte in a flow log was captured and seen in the data path (RFC 3339 Date and Time - Coordinated Universal Time). |
| `end_time`               | string | When the last byte in a flow log was captured and seen in the data path (RFC 3339 Date and Time - Coordinated Universal Time). |
| `connection_start_time`  | string | When the first byte in a flow log’s _connection_ was captured and seen in the data path (RFC 3339 Date and Time - Coordinated Universal Time). |
| `direction`              | string | Values are `I` for inbound or `O` for outbound. If the first packet on the connection was _received_ by the vNIC, the direction is `I`. If the first packet was _sent_ by the vNIC, the direction is `O`. |
| `action`                 | string | Values are `accepted` (traffic summarized by this flow was accepted) or `rejected` (traffic was rejected). |
| `initiator_ip`           | string | (IPv4 address) Source-IP as it appears in the first packet that is processed by the vNIC on this connection. If `direction=="outbound"`, a private IP associated with the vNIC. |
| `target_ip`              | string | (IPv4 address) Dest-IP as it appears in the first packet that is processed by the vNIC on this connection. If `direction=="inbound"`, a private IP associated with the vNIC. |
| `initiator_port`         | uint16 | The TCP/UDP source-port as it appears in first packet that is processed by this vNIC on this connection. |
| `target_port`            | uint16 | The TCP/UDP dest-port as it appears in first packet that is processed by this vNIC on this connection. |
| `transport_protocol`     | uint8  | The Internet Assigned Numbers Authority (IANA) protocol number (TCP or UDP). |
| `ether_type`             | string | Currently, `IPv4` is the only value. |
| `was_initiated`          | bool   | The connection initiated in this flow log. |
| `was_terminated`         | bool   | The connection terminated (for example, `timeout/RST/Final-FIN`). |
| `bytes_from_initiator`   | uint64 | The count of bytes on connection in this flow log’s time-window, from Initiator to Target. |
| `packets_from_initiator` | uint64 | The count of packets on the connection in this flow log’s time-window, from Initiator to Target. |
| `bytes_from_target`      | uint64 | The count of bytes on the connection in this flow log’s time-window, from Target to Initiator. |
| `packets_from_target`    | uint64 | The count of packets on the connection in this flow log’s time-window, from Target to Initiator. |
| `cumulative_bytes_from_initiator` | uint64 | The count of bytes since the connection was initiated, from Initiator to Target. |
| `cumulative_packets_from_initiator` | uint64 | The count of packets since the connection was initiated, from Initiator to Target. |
| `cumulative_bytes_from_target` | uint64 | The count of bytes since the connection was initiated, from Target to Initiator. |
| `cumulative_packets_from_target` | uint64 | The count of packets since the connection was initiated, from Target to Initiator. |
{: caption="Table 2. Flow log fields" caption-side="bottom"}

In most cases, you can find the direction field by comparing the vNIC’s private IP with the source and destination IPs. However, the field is convenient for queries.
{: note}

### Example flow log object
{: #example-flow-logs-object}

```json
    {
        "version": "0.0.1",
        "collector_crn": "crn",
        "attached_endpoint_type": "vnic",
        "network_interface_id": "crn",
        "instance_crn": "crn",
        "vpc_crn": "crn"
        "capture_end_time": "2008-09-15T15:53:00Z",
        "capture_start_time": "2008-09-15T15:00:00Z",
        "state": "ok",
        "number_of_flow_logs" : 1,
        "flow_logs": [
            {
                "start_time": "2008-09-15T15:53:00",
                "end_time": "2008-09-15T15:40:00",
                "direction": "O",
                "action": "accepted",
                "initiator_ip": "1.2.3.4",
                "initiator_port": 20033,
                "target_ip": "5.6.7.8",
                "target_port": 80,
                "transport_protocol": 6,
                "ether_type": "IPv4",
                "was_initiated": true,
                "was_terminated": false,
                "bytes_from_initiator": 12000,
                "packets_from_initiator": 2212,
                "bytes_from_target": 323232,
                "packets_from_target": 3232
                "cumulative_packets_from_initiator": 2212,
                "cumulative_packets_from_target": 3232,
                "cumulative_bytes_from_target": 323232,
                "cumulative_bytes_from_initiator": 12000,
            }
        ],
    }

```
{: screen}

### Analyzing flow logs by using IBM Cloud SQL Query
{: #analyzing-flow-logs-for-vpc-using-sql}

You can analyze flow logs with SQL that uses {{site.data.keyword.sqlquery_full}} by using the path to the objects on {{site.data.keyword.cos_short}} in the FROM clause of your SQL query. (For an example, see Step 3 in [Viewing flow log objects](/docs/vpc?topic=vpc-fl-analyze). However, for a more convenient way of querying, it is recommended that you create table and view definitions that give you a flattened view on your flows. For more information about table catalog functions and benefits, see [Getting started with the catalog](/docs/sql-query?topic=sql-query-getting_started_catalog).

The view flattens the flow log data structure; thus, making it easier to analyze your flows.

For more elaborate and repeatable analysis, such as when you want to collaborate and share your log analysis, it is recommended that you use {{site.data.keyword.sqlquery_full}} through Jupyter Notebooks (for example, through IBM Watson Studio). For a generic starter and demo notebook for {{site.data.keyword.sqlquery_full}} that you can use as basis for your work, see [Using IBM Cloud SQL Query](https://dataplatform.cloud.ibm.com/analytics/notebooks/v2/656c7d43-7ccd-4e50-a3c0-bbc37c001132/view?access_token=baaa77ad715e17a8f823615d45431329fde0fe92fecb85abb9fc55a877939fe8){: external}.
{: note}

**Important**: Replace these variables in the following steps:

* **bucket** - The bucket where your flow logs are stored.
* **region** - The [region alias](/docs/sql-query#endpoints) of the bucket that holds your flow logs.

To analyze flow logs, follow these steps:

1. Define a table for flow logs:

   ```json
   -- Table definition for flow logs
   CREATE TABLE FLOW(
    version string,
    collector_crn string,
    attached_endpoint_type string,
    network_interface_id string,
    instance_crn string,
    capture_start_time timestamp,
    capture_end_time timestamp,
    number_of_flow_logs int,
    flow_logs array<struct<
         start_time: string,
         end_time: string,
         connection_start_time: string,
         direction: string,
         action: string,
         initiator_ip: string,
         target_ip: string,
         initiator_port: int,
         target_port: int,
         transport_protocol: int,
         ether_type: string,
         was_initiated: boolean,
         was_termined: boolean,
         bytes_from_initiator: long,
         packets_from_initiator: long,
         bytes_from_target: long,
         packets_from_target: long,
         cumulative_bytes_from_initiator: long,
         cumulative_packets_from_initiator: long,
         cumulative_bytes_from_target: long,
         cumulative_packets_from_target: long
        >>,
      account string,
      region string,
       `vpc-id` string,
       `subnet-id` string,
       `endpoint-type` string,
       `instance-id` string,
       `vnic-id` string,
       `record-type` string,
       year int,
       month int,
       day int,
       hour int,
       `stream-id` string
   ) USING JSON
   LOCATION cos://<region>/<bucket>/ibm_vpc_flowlogs_v1/
   ```
   {: codeblock}

1. Define a view for convenient flow log queries:

   ```json
   CREATE VIEW FLOW_FLAT AS
   WITH EXPLODED_FLOW as (
       SELECT version,collector_crn,attached_endpoint_type,network_interface_id, instance_crn, capture_start_time, capture_end_time, `vnic-id`, `record-type`, year, month, day, hour, `stream-id`, explode(flow_logs) as flow FROM FLOW)
   SELECT version, collector_crn, attached_endpoint_type, network_interface_id, instance_crn, capture_start_time, capture_end_time, `vnic-id`, `record-type`, year, month, day, hour, `stream-id`, flow.*
   FROM EXPLODED_FLOW
   ```
   {: codeblock}

You can verify that flow log data is being collected by using IBM Cloud Monitoring. For more information, see [Monitoring flow logs for VPC metrics](/docs/vpc?topic=vpc-fl-monitoring-metrics).
{: tip}

#### Optimizing flow logs layout with {{site.data.keyword.sqlquery_full}}
{: #optimizing-flow-logs-layout}

When large amounts of flow logs are analyzed, it is recommended to convert flow logs to a layout optimal for queries. This conversion improves query execution time by at least one order of magnitude. For more information about data optimization on {{site.data.keyword.cos_short}}, see [How to Layout Big Data in {{site.data.keyword.cos_full_notm}} for Spark SQL](https://www.ibm.com/blog/big-data-layout/){: external}.

The following SQL statement is an ETL job addresses two aspects that contribute significantly to query execution time:

* object size - Flow logs have a maximum size of 100 KB. An optimal object size for a query is approximately 128 MB.
* data format - Flow logs are stored as compressed JSON. The optimal format for queries is parquet, which allows to read only the columns that a query needs, not the whole object.

Note the INTO clause, which defines the target location and partitioning layout of the data that is produced by this ETL job.

**Important**: Replace these variables in the following steps:

* **bucket** - The bucket where you want to store the optimized flow logs.
* **region** - The [region alias](/docs/sql-query#endpoints) of the bucket that holds your flow logs.

To optimize flow logs layout with {{site.data.keyword.sqlquery_full}}, follow these steps:

1. Convert all flow logs for a specific date to parquet, writing one parquet object per hour:

   ```json
   SELECT * FROM FLOW_FLAT
   WHERE
       day = day(DATE("2020-07-24")) AND
       month = month(DATE("2020-07-24")) AND
       year = year(DATE("2020-07-24"))
   INTO cos://<region>/<bucket>/ibm_vpc_flowlogs_v1_parquet/ JOBPREFIX NONE STORED AS PARQUET PARTITIONED BY (year,month,day,hour)
   ```
   {: codeblock}

1. Create a table for conveniently working with the optimized data:

   ```json
   CREATE TABLE FLOW_PARQUET
   USING PARQUET
   LOCATION cos://<region>/<bucket>/ibm_vpc_flowlogs_v1_parquet/
   ```
   {: codeblock}

   Replace the following place holders in the following steps:

      * **bucket** - The bucket where you stored the optimized flow logs.
      * **region** - The [region alias](/docs/sql-query#endpoints) of the bucket that holds your flow logs.

1. Use the table `FLOW_PARQUET` instead of `FLOW_FLAT`.

   For more information about how data layout influences query execution times, see [How to Layout Big Data in {{site.data.keyword.cos_full_notm}} for Spark SQL](https://www.ibm.com/blog/big-data-layout/ "data layout"){: external}.

#### Example queries for flow logs with {{site.data.keyword.sqlquery_full}}
{: #example-queries-for-flow-logs-with-sql}

The following example queries a maximum of 100 individual flows for the date specified:

```json
SELECT * FROM FLOW_FLAT WHERE CAST(capture_start_time AS date) = DATE("2020-07-24") LIMIT 100
```
{: pre}

The following query lists all accepted TCP connection for the specified date, extracting the hour that the connection was made:

```json
SELECT hour(capture_start_time) as hour,
    network_interface_id,
    initiator_ip,
    target_port
FROM FLOW_FLAT
WHERE
    transport_protocol = 6 AND
    action = "accepted" AND
    CAST(capture_start_time AS date) = DATE("2020-07-24")
SORT BY hour
LIMIT 100
```
{: codeblock}

To see which of your vNICs received the most HTTPS traffic in the last seven days, use this query. It counts the number of packets that are sent and groups them by `target_ip`, and returns the first 10. You can check which vNIC sent the most traffic by sorting by `packets_sent`.

The `where` condition maps to columns that are part of the object names. By restricting columns that are encoded in the object name, the query can be run quickly because you can prune objects before you read their content.

You need to include a similar clause when you query large amounts of flow logs.
{: note}


```json
SELECT target_ip,
    SUM(packets_from_initiator) AS `packets_received`,
    SUM(packets_from_target) AS `packets_sent`
FROM
    FLOW_FLAT
WHERE
    target_port = 443 AND
    day = day(current_date() - INTERVAL 7 days) AND
    month = month(current_date() - INTERVAL 7 days) AND
    year = year(current_date() - INTERVAL 7 days)
GROUP BY target_ip
ORDER BY `packets_received` DESC LIMIT 10
```
{: codeblock}

To see which vNIC received the most traffic in the last hour, use this query. This query counts the number of bytes sent, groups them by `target_ip`, and returns the first 5.

```json
SELECT target_ip,
    SUM(bytes_from_initiator) AS `bytes`
FROM
    FLOW_FLAT
WHERE
    capture_start_time > current_timestamp() - INTERVAL 1 hours AND
    hour = hour(current_date() - INTERVAL 1 hours) AND
    day = day(current_date() - INTERVAL 1 hours) AND
    month = month(current_date() - INTERVAL 1 hours) AND
    year = year(current_date() - INTERVAL 1 hours)
GROUP BY target_ip
ORDER BY `bytes` DESC LIMIT 5
```
{: codeblock}

### Viewing generated flow log files from the {{site.data.keyword.cos_short}} bucket
{: #alternative-method}

If you use the Lite (free) pricing plan for {{site.data.keyword.cos_full_notm}}, you cannot use an SQL query to confirm the creation of flow log objects in the {{site.data.keyword.cos_short}} bucket without purchasing the Standard pricing plan. As an alternative, you can access generated flow log object files directly from the {{site.data.keyword.cos_short}} bucket to verify that the files were created successfully and that the objects are being generated.

To view generated flow log files from the {{site.data.keyword.cos_short}} bucket, follow these steps:

1. To ensure that the flow logs are being captured, check the {{site.data.keyword.cos_short}} bucket.
1. Navigate in the folder to find your VPC or the resource where you configured monitoring.
1. Drill-down into the resource to find the network resource that you are monitoring.

   If configured correctly, you should see several gzip files for each log entry.

Using SQL queries, you can flatten and view these objects or any other type of view of all these files. In our case, we want to ensure that data is being captured. Download one of the files to view its contents to ensure that traffic is being captured.
{: note}
