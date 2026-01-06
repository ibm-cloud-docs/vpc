---

copyright:
  years: 2023, 2026
lastupdated: "2026-01-05"

keywords: data center latency, latency dashboard, network latency, inter-AZ latency, Inter-region latency
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Network latency dashboards
{: #network-latency-dashboard}

The **Inter-region latency** dashboard provides the average network round-trip latency (round-trip time or RTT) for all pairs of regions in {{site.data.keyword.cloud}}. The dashboard shows a snapshot of inter-region RTT expressed in milliseconds. This snapshot is an average of multiple measurements over the previous 30 days. For each measurement, a pair of Linux VMs (of cx2-8x16 profile) is provisioned in the two corresponding regions in {{site.data.keyword.cloud_notm}}. VM-to-VM network connectivity is provided by the Transit Gateway. Netperf TCP RR test is used for measuring the VM-to-VM latency between regions.

The **Inter-AZ latency** dashboard provides the average network RTT latency within a zone (Intra-Zone) and between availability zones (Inter-Zone) in a multi-zone region (MZR) in {{site.data.keyword.cloud_notm}}. The dashboard shows a snapshot of inter-availability zone RTT expressed in milliseconds. This snapshot is an average of multiple measurements over the previous 30 days. For each measurement, a pair of Linux VMs (of cx2-8x16 profile) is provisioned in the two corresponding availability zones in the same VPC. VM-to-VM network connectivity is provided by the private network within the VPC. Netperf TCP RR test is used for measuring the VM-to-VM latency between the availability zones. The latency between storage and compute instances within the same zone is dependent on workload characteristics, and on average, expected target latency is in single-digit ms.

The results reported are as measured. There are no performance guarantees implied by these dashboards. These statistics provide visibility into latency between all regions and zones to help you plan the optimal selection for your cloud deployment and plan for scenarios, such as data residency and performance. These dashboards are not intended for use in troubleshooting.

For information about the mapping between regions and zones, see [Zone mapping per account](/docs/overview?topic=overview-locations#zone-mapping).
{: note}

{{_include-segments/network-latency.md}}
