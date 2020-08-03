---

copyright:
  years: 2020
lastupdated: "2020-07-23"

keywords: flow logs, configure, viewing records

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Viewing flow log objects
{: #fl-analyze}
A flow log is a summary of the network traffic that is uniquely identified by a connection between two virtual network interface cards (vNICs), within a certain time window. A flow log describes traffic the firewall either accepts (relevant security groups or network ACLs) or rejects, but not both.
{:shortdesc}

Currently, flow logs collect Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) traffic, but not Internet Control Message Protocol (ICMP) traffic.
{: note}

Each flow log object contains individual flow logs. To view the flow logs, follow these steps.

1. Download the objects from your designated Cloud Object Storage (COS) bucket by using the COS UI, CLI, or API.
1. Extract and view the downloaded flow log objects in JSON format.
1. Use command-line tools, such as **jq** or **grep**, to search the downloaded flow log objects.

Flow logs are periodically written to Cloud Object Storage about every 5 minutes. Flow logs are published more frequently in cases where the flow log collector reaches 100 KB flows.
{: note}

## Flow log data format

Flow log traffic summaries contain the following information:

- Byte/packet counts, separately for RX (receive) and TX (transmit).
- Whether a connection initiates or terminates in this time window.

Because a flow log reflects network traffic in a limited window of time, a long-running connection might cause multiple flow logs to be emitted. For every connection processed by a vNIC, a time-ordered sequence of flow logs emits (not including failures). These flow logs appear in one or more COS objects.

The `initiator_ip` in a flow log is defined to be the `source_ip` that appears in the first packet of its connection that reached the vNIC. At the implementation level, this packet is typically the one that causes the new connection to be added to the connection table. Similarly, the `target_ip` in the flow log is set to the `dest_ip` field in that same packet.

If both the connection initiator vNIC and connection target vNIC enable flow logs, the initiator/target IP and ports fields are identical in the flow logs on both vNICs.
{: note}

**Example**: Consider a client sending an HTTP request to a web server. The flow log for this HTTP request on the client-side vNIC has the client’s IP address as `initiator_ip`. The corresponding flow log on the server-side vNIC has the same `initiator_ip`.

The `start_time` and `end_time` in a flow log reflects:

   - Capture time - The time that data path elements were queried for traffic counters.
   - Data path time - The time as maintained in the data path element itself.

It's possible that the flow log does not reflect all traffic (for example, in the data path) between a flow log `start_time` and `end_time`. In other words, it might be that packets sent/received by the vNIC towards the end of the capture window are reflected only in a flow log with the later `start_time` window.

**Flow logs reflect actual traffic on connections**: If traffic does not occur on a connection in a capture window, no flow log appears for it in the COS object for that window. This means that the sequence of flow logs for a connection might be mapped to a sequence of non-consecutive COS objects.

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

Flow logs are written to the user-specified COS bucket in the following path:

```
ibm_vpc_flowlogs_v1/account={account}/region={region}/vpc-id={vpc-id}/subnet-id={subnet-id}/endpoint-type=vnics/instance-id={vsi-id}/vnic-id={vnic-id}/record-type={all|ingress|egress|internal}/year={xxxx}/month={yy}/day={zz}/hour={hh}/stream-id={stream-id}/{sequence-number}.gz
```

Where:
* `{stream_id}` is a date and time string in ISO 8601 format `yyyymmddThhmmssZ` (Coordinated Universal Time), defined as the time the first object in this `directory` was created.
* `{sequence-number}` is a running counter of objects within the stream, represented as an eight-character, zero-filled field (`%08d`).
* `{all|ingress|egress|internal}` shows the type of traffic the flow includes,
* `{xxxx}` is the year in four digit format,
* `{yy}` is the month in two digit, zero-leading format,
* `{zz}` is the day of the month in two digit, zero-leading format, and
* `{hh}` is the hour in zero-leading format (e.g. 00-24)
* any instances of URL-unsafe characters in the path are URL-encoded.

A flow log's COS object contains a single valid JSON object. The COS object is `.gz` compressed.

The object header fields that are specified in the following table are written to the COS object metadata.

### Flow logs object header fields (per COS object)

| Field                  | Type   | Description       |
| ---------------------- | ------ | ----------------- |
| `version`                | string | The Semantic version. |
| `collector_crn`          | string | The Cloud Resource Name (CRN) of the flow log collector. |
| `attached_endpoint_type` | string | Currently, the only value is `vnic`. |
| `network_interface_id`   | string | The `vnic` ID, as there is no vNIC CRN. |
| `instance_crn`           | string | The CRN of the instance that the network interface is attached to. |
| `vpc_crn`                | string | The CRN of the VPC that the flow log collector is a member of. |
| `capture_start_time`     | string | RFC 3339 Date and Time (Coordinated Universal Time) |
| `capture_end_time`       | string | RFC 3339 Date and Time (Coordinated Universal Time) |
| `state`                  | string |  Indicates the operational state of the flow log collector. `OK` means that data is being collected and shipped without any errors. `skip data` indicates that there was data lost during this collection interval (for example, because of high rate of rejected SYN packets). |
| `number_of_flow_logs`    | uin32  | The number of elements in a `flow_logs` array. Since this number is highly variable, it's useful as a quick reference of the number of flow logs contained in a single COS object, without needing to download the object first. |
| `flow_logs`              | array of JSON objects | This may be an empty array, which indicates `no traffic`.|

### Flow log fields

| Field                  | Type   | Description       |
| ---------------------- | ------ | ----------------- |
| `start_time`             | string | When the first byte in a flow log was captured and seen in the data path (RFC 3339 Date and Time - Coordinated Universal Time). |
| `end_time`               | string | When the last byte in a flow log was captured and seen in the data path (RFC 3339 Date and Time - Coordinated Universal Time). |
| `connection_start_time`  | string | When the first byte in a flow log’s _connection_ was captured and seen in the data path (RFC 3339 Date and Time - Coordinated Universal Time). |
| `direction`              | string | Values are `I` for inbound or `O` for outbound. If the first packet on the connection was _received_ by the vNIC, the direction is `I`. If the first packet was _sent_ by the vNIC, the direction is `O`. |
| `action`                 | string | Values are `accepted` (traffic summarized by this flow was accepted) or `rejected` (traffic was rejected). |
| `initiator_ip`           | string | (IPv4 address) Source-IP as it appears in the first packet that is processed by the vNIC on this connection. If `direction=="outbound"`, this is a private IP associated with the vNIC. |
| `target_ip`              | string | (IPv4 address) Dest-IP as it appears in the first packet that is processed by the vNIC on this connection. If `direction=="inbound"`, this is a private IP associated with the vNIC. |
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

In most cases, you can find the direction field by comparing the vNIC’s private IP with the source and destination IPs. However, the field is convenient for queries.
{: note}

### Example flow logs object

```

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

### Example solution: Analyzing flow Logs for VPC using LogDNA
{: #example-analyzing-flow-logs-using-logdna}

You can download an example solution of how to use LogDNA to analyze flow logs from [https://github.com/IBM-Cloud/vpc-flowlogs-logdna](https://github.com/IBM-Cloud/vpc-flowlogs-logdna). This project ([Readme file](https://github.com/IBM-Cloud/vpc-flowlogs-logdna/blob/master/README.md)) shows how to use a trigger function to read a flow log COS object and write it to LogDNA.
