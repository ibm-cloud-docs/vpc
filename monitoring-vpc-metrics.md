---

copyright:
  years:  2020, 2021
lastupdated: "2021-04-01"

keywords: IBM Cloud monitoring, platform metrics, metrics, vpc metrics, vpc monitoring metrics

subcollection: cloud-infrastructure

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}

# VPC virtual server instances metrics definitions
{: #vpc-monitoring-metrics}

The following metrics are available only if you use the {{site.data.keyword.mon_full_notm}} full agent. If you provisioned a 'no driver mode' instance, see [Monitoring 'no driver mode' metrics](/docs/cloud-infrastructure?topic=cloud-infrastructure-enabling-monitoring-light-no-driver#monitoring-light-metrics).
{:important} 

The following tables define basic VPC virtual server instance metrics.

### Average CPU usage percentage
{: #avg-cpu-usage-gen2}

Average percentage of time that elapsed executing instructions across all CPUs

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_average_cpu_usage_percentage`|
| `Metric type` | `gauge` |
| `Value type`  | `percent` |
| `Segment by` | `IBM IS Generation, 2, resource name` |

{: caption="Table 23: Average CPU usage percentage metric metadata" caption-side="top"}

### Bytes received for a network interface
{: #network-int-bytes-received-gen2}

Cumulative number of bytes received for a network interface since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_in_bytes`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation, 2, resource name, MAC address of the network interface` |

{: caption="Table 24: Average CPU usage percentage metric metadata" caption-side="top"}

### Bytes sent for a network interface
{: #network-bytes-sent}

Cumulative number of bytes sent for a network interface since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_out_bytes`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation, 2, resource name, MAC address of the network interface` |

{: caption="Table 25: Bytes sent for a network interface metric metadata" caption-side="top"}

### CPU usage
{: #cpu-usage-cumulative-gen2}

Cumulative elapsed time that a CPU is executing instructions since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_cpu_usage_nanoseconds`|
| `Metric type` | `gauge` |
| `Value type`  | `second` |
| `Segment by` | `IBM IS Generation, 2, resource name, Virtual CPU index` |

{: caption="Table 26: Cumulative CPU usage metric metadata" caption-side="top"}

### CPU usage percentage
{: #cpu-usage-percentage-gen2}

Average percentage of time that a CPU is executing instructions

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_cpu_usage_percentage`|
| `Metric type` | `gauge` |
| `Value type`  | `percent` |
| `Segment by` | `IBM IS Generation, 2, resource name, Virtual CPU index` |

{: caption="Table 27: Average CPU usage metric metadata" caption-side="top"}

### Free memory
{: #free-memory-gen2}

Free memory of the virtual server instance in kibibytes (1024 bytes)

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_memory_free_kib`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation, 2, resource name` |

{: caption="Table 28: Free memory metric metadata" caption-side="top"}

### Memory usage percentage
{: #memory-usage-percentage-gen2}

Percent of used memory of the virtual server instance

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_memory_usage_percentage`|
| `Metric type` | `gauge` |
| `Value type`  | `percent` |
| `Segment by` | `IBM IS Generation, 2, resource name` |

{: caption="Table 29: Memory usage percentage metric metadata" caption-side="top"}

### Number of bytes read for a volume
{: #bytes-read-for-volume-gen2}

Cumulative number of bytes read for a volume since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_volume_read_bytes`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation, 2, resource name, disk name of the volume` |

{: caption="Table 30: Number of bytes read for a volume metric metadata" caption-side="top"}

### Number of bytes written for a volume
{: #bytes-written-for-volume-gen2}

Cumulative number of bytes written for a volume since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_volume_write_bytes`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation, 2, resource name, disk name of the volume` |

{: caption="Table 31: Number of bytes written for a volume metric metadata" caption-side="top"}

### Number of dropped incoming packets for a network interface
{: #dropped-incoming-packets-gen2}

Cumulative number of dropped incoming packets for a network interface since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_in_dropped_packets`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation, 2, resource name, MAC address of the network interface` |

{: caption="Table 32: Number of dropped incoming packets for a network interface metric metadata" caption-side="top"}

### Number of dropped outgoing packets for a network interface
{: #dropped-outgoing-packets-gen2}

Cumulative number of dropped outgoing packets for a network interface since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_out_dropped_packets`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation, 2, resource name, MAC address of the network interface` |

{: caption="Table 33: Number of dropped outgoing packets for a network interface metric metadata" caption-side="top"}

### Number of packets received for a network interface
{: #network-packets-received-gen2}

Cumulative number of packets received for a network interface since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_in_packets`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation, 2, resource name, MAC address of the network interface` |

{: caption="Table 34: Number of packets received for a network interface metric metadata" caption-side="top"}

### Number of packets sent for a network interface
{: #network-packets-sent-gen2}

Cumulative number of packets sent for a network interface since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_out_packets`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation, 2, resource name, MAC address of the network interface` |

{: caption="Table 35: Number of packets sent for a network interface metric metadata" caption}

### Number of read requests for a volume
{: #volume-read-requests-gen2}

Cumulative number of read requests for a volume since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_volume_read_requests`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation, 2, resource name, disk name of the volume` |

{: caption="Table 36: Number of read requests for a volume metric metadata" caption}

### Number of receiving errors for a network interface
{: #network-errors-receiving-gen2}

Cumulative number of receiving errors for a network interface since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_in_errors`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation, 2, resource name, MAC address of the network interface` |

{: caption="Table 37: Number of receiving errors for a network interface metric metadata" caption-side="top"}

### Number of sending errors for a network interface
{: #network-errors-sending-gen2}

Cumulative number of sending errors for a network interface since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_out_errors`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation, 2, resource name, MAC address of the network interface` |

{: caption="Table 38: Number of sending errors for a network interface metric metadata" caption-side="top"}

### Number of write requests for a volume
{: #volume-write-requests-gen2}

Cumulative number of write requests for a volume since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_volume_write_requests`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation, 2, resource name, disk name of the volume` |

{: caption="Table 39: Number of write requests for a volume metric metadata" caption-side="top"}

### Total CPU usage
{: #total-cpu-usage-nanoseconds-gen2}

Cumulative time elapsed executing instructions across all CPUs since virtual server instance start

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_total_cpu_usage_nanoseconds`|
| `Metric type` | `gauge` |
| `Value type`  | `second` |
| `Segment by` | `IBM IS Generation, 2, resource name` |

{: caption="Table 40: Total CPU usage metric metadata" caption-side="top"}

### Total number of CPUs
{: #total-cpus-gen2}

Total Number of CPUs

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_cpus`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation, 2, resource name` |

{: caption="Table 41: Total number of CPUs metric metadata" caption-side="top"}

### Total memory
{: #total-memory-kib-gen2}

Total memory of the virtual server instance in kibibytes (1024 bytes)

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_memory_total_kib`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation, 2, resource name` |

{: caption="Table 42: Total memory in kibibytes metric metadata" caption-side="top"}

### Used memory
{: #used-memory-kib-gen2}

Used memory of the virtual server instance in kibibytes (1024 bytes)

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_memory_used_kib`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation, 2, resource name` |

{: caption="Table 43: Used memory metric metadata" caption-side="top"}

## Attributes for segmentation
{: #segmentation-attributes}

### Global attributes
{: #global-segmentation-attributes}

The following attributes are available for segmenting all of the previously listed metrics. 

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Cloud type` | `ibm_ctype` | Coud type is a value of public, dedicated, or local |
| `Location` | `ibm_location` | Location of the monitored resource - this can be a region, data center, or global |
| `Resource` | `ibm_resource` | Resource being measured by the service - typically an identifying name or GUID |
| `Resource type` | `ibm_resource_type` | Type of resource measured by the service |
| `Resource group` | `ibm_resource_group_name` | Resource group where the service instance was created |
| `Scope` | `ibm_scope` | Scope of the account, organization, or space GUID that is associated with this metric |
| `Service name` | `ibm_service_name` | Name of the service generating this metric |

{: caption="Table 44: Available attributes for segmenting" caption-side="top"}

### Additional Attributes
{: #additional-segmentation-attributes}

The following attributes are available for segmenting one or more attributes as described in the preceding table. See the individual metrics for segmentation options.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Disk name of the volume` | `ibm_is_disk_name` | Disk name of the volume attached to the virtual server instance, corresponds to output of `'ls -l /dev/disk/by-id'` |
| `IBM IS Generation, 1 or 2` | `ibm_is_generation` | IBM IS Generation (`1` for Gen. 1 (Classic) or `2` for Gen. 2 (VPC) ) |
| `MAC address of the network interface` | `ibm_is_mac_address` | MAC address of the corresponding network interface that is attached to the virtual server instance |
| `Network interface index` | `ibm_is_nic_index` | Index of the network interface that is attached to the virtual server instance, starting from 0 |
| `Resource name` | `ibm_resource_name` | Resource name - for example, the virtual server instance name |
| `UUID of storage volume` | `ibm_is_volume_id` | UUID of storage volume that is attached to the virtual server instance |
| `Virtual CPU index` | `ibm_is_vcpu_index` | Index of the virtual CPU within the virtual server instance, starting from 0 |

{: caption="Table 45: Available attributes for segmenting one or more attributes" caption-side="top"}
