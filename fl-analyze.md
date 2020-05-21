---

copyright:
  years: 2020
lastupdated: "2020-05-11"

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

A flow log summarizes the network traffic that is sent and received by a specific virtual network interface card (vNIC), over a specific connection, within a certain time window. A flow log describes either traffic that is accepted by the firewall (relevant security groups or network ACLs), or rejected traffic, but not both.
{:shortdesc}

Currently, flow logs collect Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) traffic, but not Internet Control Message Protocol (ICMP) traffic.
{: note}

Each flow log object contains individual flow logs. To view flow logs, follow these steps.

1. Download the objects from your designated Cloud Object Storage (COS) bucket by using the COS UI, CLI, or API.
1. Extract and view the downloaded flow log objects in JSON format.
1. Use command-line tools, such as **jq** or **grep** to search the downloaded flow log objects.

Flow logs are periodically written to COS about every 5 minutes.
{: note}

## Flow log data format

The traffic summary that is contained in a flow log includes:

- Byte/packet counts - separately for RX (receive) and TX (transmit)
- Whether a connection was initiated or terminated in this time window.

Because a flow log reflects network traffic in a limited window of time, a long-running connection might cause multiple flow logs to be emitted. For every connection processed by a vNIC, a time-ordered sequence of flow logs is emitted (not including failures). These flow logs appear in one or more COS objects.

The `initiator_ip` in a flow log is defined to be the `source_ip` that appears in the first packet of its connection that reached the vNIC. At the implementation level, this packet is typically the one that caused the new connection to be added to the connection table. Similarly, the `target_ip` in the flow log is set to the `dest_ip` field in that same packet.

If both connection initiator vNIC and connection target vNIC have flow logs enabled, the initiator/target IP and ports fields are identical in the flow logs on both vNICs.
{: note}

**Example**: Consider a client sending an HTTP request to a web server. The flow log for this HTTP request on the client-side vNIC has the client’s IP address as `initiator_ip`. The corresponding flow log on the server-side vNIC has the same `initiator_ip`.

The `start_time` and `end_time` in a flow log reflects:

   - Capture time: The time that the data path elements were queried for traffic counters.
   - Data path time: The time as maintained in the data path element itself.

It is possible that not all traffic that occurred (for example, in the data path) between a flow log `start_time` and `end_time` is reflected in that flow log. In other words, it might be that packets sent/received by the vNIC towards the end of the capture window are reflected only in a flow log with the later `start_time` window.

**Flow logs reflect actual traffic on connections**: If traffic does not occur on a connection in a capture window, no flow log appears for it in the COS object for that window. This means that the sequence of flow logs for a connection might be mapped to a sequence of non-consecutive COS objects.

The flow logs array within an object need not be sorted in any specific order.
{: note}

Rejected traffic:

- A specific flow log summarizes either rejected or accepted traffic, never both.
- The sequence of flow logs for a connection might have time-window overlaps between accepted and rejected flow logs.
- Rejected packets not associated with an existing connection (for example, a security group, or network ACL, denies the connection initiation).
- Rejected packets on an existing connection (for example, traffic that does not match TCP state-machine, or a network ACL rule changed mid-connection).
-  Rejected packets are written to separate flow logs based on whether they are incoming or outgoing.

   Not all dropped packets appear in flow logs. For example, some packets are dropped in HW (spoofed SRC MAC, SRC IP).
   {: note}

### Flow log object format

A flow log's COS object contains a single valid JSON object. The COS object is .gz compressed.

The object header fields that are specified in the following table are written to the COS object metadata.

Fields are mandatory unless marked optional.
{: important}

#### Flow logs object header fields (per COS object)

| Field                  | Type   | Description       |
| ---------------------- | ------ | ----------------- |
| `version`                | string | Is the Semantic version. |
| `collector_crn`          | string | Is the Cloud Resource Name (CRN) of the flow log collector. |
| `attached_endpoint_type` | string | Currently, the only value is `vnic`. |
| `network_interface_id`   | string | Optional. Required if `attachedEndpointType` is the `vnic` ID as there is no vNIC CRN. |
| `instance_crn`           | string | Optional. Required if `attachedEndpointType` is `vnic`. |
| `vpc_crn`                | string | Is the CRN of the VPC that the flow log collector is a member of. |
| `capture_start_time`     | string | RFC 3339 Date and Time (Coordinated Universal Time) |
| `capture_end_time`       | string | RFC 3339 Date and Time (Coordinated Universal Time) |
| `state`                  | string |  Indicates the operational state of the flow log collector. `OK` means that data is being collected and shipped without any errors. `skip data` indicates that there was data lost during this collection interval (for example, during periods of COS unavailability). |
| `number_of_flow_logs`    | uin32  | Number of elements in a `flow_logs` array. Since this number highly variable, it is useful because it gives you a quick reference for the number of flow logs contained in a single COS object, without needing to download the object first. |
| `flow_logs`              | array of JSON objects | This might be an empty array, which indicates `no traffic`.|

#### Per flow low fields

| Field                  | Type   | Description       |
| ---------------------- | ------ | ----------------- |
| `start_time`             | string | When the first byte in a flow log was captured and seen in the data path (RFC 3339 Date and Time - Coordinated Universal Time) |
| `end_time`               | string | When the last byte in a flow log was captured and seen in the data path (RFC 3339 Date and Time - Coordinated Universal Time) |
| `connection_start_time`  | string | When the first byte in a flow log’s _connection_ was captured and seen in the data path (RFC 3339 Date and Time - Coordinated Universal Time) |
| `direction`              | string | Values are `inbound` or `outbound`. If the first packet on the connection was _received_ by the vNIC, the direction is `inbound`. If the first packet was _sent_ by the vNIC, the direction is `outbound`. |
| `vpc_internal`           | Bool   | Optional. Whether connection is between entities, both of which are in the VPC. |
| `action`                 | string | Values are `accepted` (traffic summariezed by this flow was accepted) or `rejected` (traffic was rejected). |
| `initiator_ip`           | string | (IPv4 address) Source-IP as it appears in the first packet processed by the vNIC on this connection. If  `direction=="outbound"`, this is a private IP associated with the vNIC. |
| `target_ip`              | string | (IPv4 address) Dest-IP as it appears in the first packet processed by the vNIC on this connection. If  `direction=="inbound"`, this is a private IP associated with the vNIC. |
| `initiator_port`         | uint16 | TCP/UDP source-port as it appears in first packet processed by this vNIC on this connection |
| `target_port`            | uint16 | TCP/UDP dest-port as it appears in first packet processed by this vNIC on this connection |
| `transport_protocol`     | uint8  | Internet Assigned Numbers Authority (IANA) protocol number (TCP or UDP) |
| `ether_type`            | string | Currently, `IPv4` is the only value. |
| `was_initiated`          | bool   | Was the connection initiated in this flow log. |
| `was_terminated`         | bool   | Was the connection terminated (for example, `timeout/RST/Final-FIN`). |
| `bytes_from_initiator`   | uint64 | Count of bytes on connection in this flow log’s time-window, in the direction Initiator to Target. |
| `packets_from_initiator` | uint64 | Optional. Count of packets on the connection in this flow log’s time-window, in the direction Initiator to Target. |
| `bytes_from_target`      | uint64 | Count of bytes on the connection in this flow log’s time-window, in the direction Target to Initiator. |
| `packets_from_target`    | uint64 | Optional.  Count of packets on the connection in this flow log’s time-window, in the direction Target to Initiator. |
| `cumulative_bytes_from_initiator` | uint64 | Count of bytes since the connection was initiated, in direction Initiator to Target. |
| `cumulative_packets_from_initiator` | uint64 | Optional. Count of packets since the connection was initiated, in direction Initiator to Target. |
| `cumulative_bytes_from_target` | uint64 | Count of bytes since the connection was initiated, in direction Target to Initiator. |
| `cumulative_packets_from_target` | uint64 | Optional. Count of packets since the connection was initiated, in direction Target to Initiator. |

In most cases, you can find the direction field by comparing the vNIC’s private IP with the source and destination IPs. However, the field is convenient for queries.
{: note}

#### Example flow logs object

```

    {
        "version": "0.0.1",
        "attached_endpoint_type": "vnic",
        "capture_end_time": "2008-09-15T15:53:00Z",
        "capture_start_time": "2008-09-15T15:00:00Z",
        "network_interface_id": "crn",
        "collector_crn": "crn",
        "instance_crn": "crn",
        "vpc_crn": "crn"
        "state": "ok",
        "flow_logs": [
            {
                "action": "accepted",
                "start_time": "2008-09-15T15:53:00.000000Z",
                "end_time": "2008-09-15T15:40:00.000000Z",
                "connection_start_time": "2008-09-15T15:00:00Z",
                "direction": "outbound",
                "was_terminated": false,
                "was_initiated": true,
                "ether_type": "IPv4",
                "initiator_ip": "1.2.3.4",
                "initiator_port": 20033,
                "target_ip": "5.6.7.8",
                "target_port": 80,
                "transport_protocol": 6,
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
