---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-29"

keywords: flow logs, viewing objects, analyze

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Viewing flow log objects
{: #fl-analyze}

A flow log is a summary of the network traffic that is uniquely identified by a connection between two virtual network interface cards (vNICs), within a certain time window. A flow log describes traffic the firewall either accepts (relevant security groups or network ACLs) or rejects, but not both. It contains header information and payload statistics.
{: shortdesc}

Flow logs are periodically written to {{site.data.keyword.cos_full}} about every 5 minutes. Flow logs are published more frequently in cases where the flow log collector reaches 100 KB flows.

Currently, flow logs collect Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) traffic, but not Internet Control Message Protocol (ICMP) traffic.
{: note}

Each flow log object contains individual flow logs. To view or analyze the flow logs, use the IBM Analytics Engine. For more information, see the IBM Analytics Engine [Getting started tutorial](/docs/AnalyticsEngine?topic=AnalyticsEngine-getting-started).

IBM Cloud Data Engine is depreciated and is no longer supported. If you currently use existing instances of Data Engine, it is recommended that you migrate your workloads to [IBM Analytics Engine](/docs/AnalyticsEngine?topic=AnalyticsEngine-getting-started).
{: deprecated}

## Flow log data format
{: #flow-log-data-format}

Flow log traffic summaries contain the following information:

- Byte/packet counts, separately for RX (receive) and TX (transmit).
- Whether a connection starts or stops in the time window.

Because a flow log reflects network traffic in a limited window of time, a long-running connection might cause multiple flow logs to be emitted. For every connection processed by a vNIC, a time-ordered sequence of flow logs emits (not including failures). These flow logs appear in one or more {{site.data.keyword.cos_short}} objects.

The `initiator_ip` in a flow log is defined to be the `source_ip` that appears in the first packet of its connection that reached the vNIC. At the implementation level, this packet is typically the one that causes the new connection to be added to the connection table. Similarly, the `target_ip` in the flow log is set to the `dest_ip` field in that same packet.

If both the connection initiator vNIC and connection target vNIC enable flow logs, the initiator and target IP and ports fields are identical in the flow logs on both vNICs.
{: note}

Example: Consider a client that is sending an HTTP request to a web server. The flow log for this HTTP request on the client-side vNIC has the client’s IP address as `initiator_ip`. The corresponding flow log on the server-side vNIC has the same `initiator_ip`.

The `start_time` and `end_time` in a flow log reflects:

* Capture time - The time that data path elements were queried for traffic counters.
* Data path time - The time as maintained in the data path element itself.

It's possible that the flow log does not reflect all traffic (for example, in the data path) between a flow log `start_time` and `end_time`. In other words, it might be that packets sent and received by the vNIC toward the end of the capture window are reflected only in a flow log with the later `start_time` window.

Flow logs reflect actual traffic on connections: If traffic does not occur on a connection in a capture window, no flow log appears for it in the {{site.data.keyword.cos_short}} object for that window. Meaning that the sequence of flow logs for a connection might be mapped to a sequence of non-consecutive {{site.data.keyword.cos_short}} objects.

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
{: caption="Flow logs object header fields (per {{site.data.keyword.cos_short}} object)" caption-side="bottom"}

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
{: caption="Flow log fields" caption-side="bottom"}

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

### Viewing generated flow log files from the {{site.data.keyword.cos_short}} bucket
{: #alternative-method}

If you use the Lite (free) pricing plan for {{site.data.keyword.cos_full_notm}}, you can access generated flow log object files directly from the {{site.data.keyword.cos_short}} bucket to verify that the files were created successfully and that the objects are being generated.

To view generated flow log files from the {{site.data.keyword.cos_short}} bucket, follow these steps:

1. To ensure that the flow logs are being captured, check the {{site.data.keyword.cos_short}} bucket.
1. Navigate in the folder to find your VPC or the resource where you configured monitoring.
1. Drill-down into the resource to find the network resource that you are monitoring.

   If configured correctly, you should see several gzip files for each log entry.

To ensure that data is being captured, download one of the files to view its contents to ensure that traffic is being captured.
{: note}
