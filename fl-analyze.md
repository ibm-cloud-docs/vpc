---

copyright:
  years: 2019, 2020
lastupdated: "2020-04-22"

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
 
Flow log objects summarize the traffic sent and received by a VNIC/VPE in a given time interval. Each object contains individual flow logs. 

To view these flow logs:

1. Download the objects from your designated Cloud Object Storage (COS) bucket using the COS UI, CLI, or API.
1. Extract and view the downloaded flow log objects in JSON format.
1. Leverage command line tools, such as **jq** or **grep** to search the downloaded flow log objects.


# Flow log data format (WORK-IN-PROGRESS)

A flow-log summarizes the network traffic sent _and_ received by a specific VNIC or VPE, over a specific (TCP/UDP) connection, within a certain time window.
 
A flow-log describes either traffic *accepted* by the firewall (relevant security-groups or network ACLs), or *rejected* traffic, but not both.

The traffic summary contained in a flow-log includes:

- Byte/packet counts (separately for RX and TX)
- Whether a connection was initiated or terminated in this time window. 

Since a flow-log reflects network traffic in a limited window of time, a long-running connection might cause multiple flow-logs to be emitted. For every connection processed by a VNIC, a time-ordered sequence of flow-logs is emitted (not including failures). These flow-logs appear in one or more COS objects.

The initiator-ip in a flow-log is defined to be the source-ip that appears
in the first packet of its connection that reached the VNIC. At the
implementation level, this packet is typically the one that caused the new
connection to be added to the connection-table. Similarly, the target-ip in
the flow-log is set to the dest-ip field in that same packet.

Note that if both connection initiator VNIC and connection target VNIC have
flow-logs enabled, the initiator/target IP and ports fields will be identical
in the flow-logs on both VNIC’s.

Example: Consider a client performing an HTTP request to a webserver. The
flow-log for this HTTP request on the client side VNIC will have the client’s
IP-address as initiator-IP. The corresponding flow-log on the server side VNIC
will have the same initiator-IP.

The start-time and end-time in a flow-log reflects either of:
- the _capture time_: the time the datapath elements were queried for traffic
  counters
- the _datapath time_: the time as maintained in the datapath element itself

It is possible that not all traffic that _actually occurred_ (i.e. in the
datapath) between a flow-log start_time and end_time is actually reflected
in that flow-log. E.g. It may be that packets sent/received by the VNIC
towards the end of the capture window are only reflected in a later flow-log
(I.e. one with a later start_time window).

Flow logs reflect actual traffic on connections: if no traffic occurs on a
connection in some capture window, no flow-log will appear for it in the COS
object for that window. This means that, as already mentioned, the sequence
of flow-logs for a connection may be mapped to a sequence of non-consecutive
COS-objects.

The flow-logs array within an object need not be sorted in any way.

Rejected traffic:
- A specific flow log summarizes either rejected or accepted traffic never both
- The sequence of flow logs for a connection may have time-window overlaps between accepted and rejected flow-logs
- Rejected packets not associated with an existing connection
  - E.g. security group / network-ACL denies the connection initiation
- Rejected packets on an existing connection
  - E.g. Traffic that does not match TCP state-machine
  - E.g. Network ACL rule changed mid-connection
- We do not want to define that a connection-table entry be created for
  rejected packets. Thus, it becomes harder to correlate the two directions of
  packets with a given 5-tuple. It seems simplest to define that the two
  directions of rejected packets can be emitted to two separate flow logs.
- Note: Not all dropped packets will appear in flow-logs
  - E.g. In NG, some packets are dropped in HW, e.g. spoofed SRC MAC, SRC IP

Given the differences between NG and GC, for MVP it is NOT required that the
set of rejected flow-logs be identical across NG and GC for all traffic
patterns.

## Object Format

A flow-logs COS-Object contains a single valid JSON object. The COS object
will be .gz compressed.

The object-header fields specified below will also be written to the COS
object metadata.

Fields are mandatory unless marked optional.

### Flow logs object header fields (per COS-object)

| Field                  | Type   | Description       |
| ---------------------- | ------ | ----------------- |
| version                | string | major.minor.patch |
| collector_crn          | string |                   |
| attached_endpoint_type | string | currently “vnic” (In the future may be “vpe”) |
| network_interface_id   | string | Optional. Required if attachedEndpointType is “vnic” Id as there is no VNIC CRN |
| instance_crn           | string | Optional. Required if attachedEndpointType is “vnic” |
| vpc_crn                | string |                   |
| capture_start_time     | string | RFC 3339 Date and Time (UTC) |
| capture_end_time       | string | RFC 3339 Date and Time (UTC) |
| state                  | string | “ok” or “skip data” there may be flow-log data missing (both since _last_ object AND in _this_ object) |
| number_of_flow_logs    | uin32  | Number of elements in flowLogs array. Useful to have in COS object metadata |
| flow_logs              | array of JSON objects | May be empty array – indicates ‘no traffic’ |

#### Per Flow-Log fields

| Field                  | Type   | Description       |
| ---------------------- | ------ | ----------------- |
| start_time             | string | When first byte in flow-log was captured / seen in datapath (RFC 3339 Date and Time (UTC)) |
| end_time               | string | When last byte in flow-log was captured / seen in datapath (RFC 3339 Date and Time (UTC)) |
| connection_start_time  | string | When first byte in flow-log’s _connection_ was captured / seen in datapath (RFC 3339 Date and Time (UTC)) |
| direction              | string | "inbound" or "outbound" - If first packet on connection was _received_ by the VNIC, direction is "inbound". If first packet was _sent_ by the VNIC, direction is "outbound" |
| vpc_internal           | Bool   | Optional. Whether connection is between entities both of which are in the VPC. |
| action                 | string | "accepted" or "rejected" - Whether the traffic summarized by this flow-log was Accepted or Rejected |
| initiator_ip           | string | (IPv4 address) Source-IP as it appears in first packet processed by VNIC on this connection. If Direction=="outbound", this is a _private IP_ associated with the VNIC |
| target_ip              | string | (IPv4 address) Dest-IP as it appears in first packet processed by VNIC on this connection If Direction=="inbound", this is a _private IP_ associated with the VNIC |
| initiator_port         | uint16 | TCP/UDP source-port as it appears in first packet processed by this VNIC on this connection |
| target_port            | uint16 | TCP/UDP dest-port as it appears in first packet processed by this VNIC on this connection |
| transport_protocol     | uint8  | IANA protocol number (TCP or UDP) |
| ether_type             | string | (“IPv4”) - In the future may be extended to IPv6 |
| was_initiated          | bool   | Was the connection initiated in this flow-log TBD: SYN encountered or actual connection established? |
| was_terminated         | bool   | Was the connection terminated (E.g. timeout/RST/Final-FIN) |
| bytes_from_initiator   | uint64 | Count of bytes on connection in this flow-log’s time-window, in the direction Initiator to Target |
| packets_from_initiator | uint64 | Optional |
| bytes_from_target      | uint64 | Count of bytes on connection in this flow-log’s time-window, in the direction Target to Initiator |
| packets_from_target    | uint64 | Optional |
| cumulative_bytes_from_initiator | uint64 | Count of bytes since connection initiated, in direction Initiator to Target |
| cumulative_packets_from_initiator | uint64 | Optional |
| cumulative_bytes_from_target | uint64 | Count of bytes since connection initiated, in direction Target to Initiator |
| cumulative_packets_from_target | uint64 | Optional |

Note: In most cases the direction field can be deduced by comparing the VNIC’s private IP with the SRC/DST-IP’s. However, the field is convenient for queries.

### Example Flow-Logs Object

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
