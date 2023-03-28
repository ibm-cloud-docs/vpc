---

copyright:
  years: 2023
lastupdated: "2023-03-23"

keywords: data center latency, latency dashboard, network latency
subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Network latency dashboard
{: #network-latency-dashboard}

The Inter-Region Latency dashboard provides the average network round-trip latency (round-trip time or RTT) for all pairs of regions in {{site.data.keyword.cloud}}. The dashboard shows a snapshot of Inter-Region Round-Trip Time expressed in milliseconds. This snapshot is an average of multiple measurements over the previous 30 days. For each measurement, a pair of Linux VMs (of cx2-8x16 profile) are provisioned in the two corresponding regions in {{site.data.keyword.cloud_notm}}. VM to VM network connectivity is provided by the transit gateway. Netperf TCP RR test is used for measuring the VM-to-VM latency between regions. 

The results reported are as measured. There are no performance guarantees implied by this dashboard. These global statistics provide visibility into latency between all regions to help you plan the optimal selection for your cloud deployment and plan for scenarios, such as data residency and performance. This dashboard is not intended for use in troubleshooting.

{{_include-segments/network-latency.md}}
