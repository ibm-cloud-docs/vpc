---

copyright:
  years:  2020, 2023
lastupdated: "2023-07-11"

keywords: monitoring metrics, platform metrics, metrics, vpc metrics, vpc monitoring metrics

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC virtual server instances metrics definitions
{: #vpc-monitoring-metrics}

The following tables define basic VPC virtual server instance metrics.

## CPU monitoring metrics
{: #cpu-metrics}

### Average CPU usage percentage
{: #avg-cpu-usage-gen2}

The average percentage of time that elapsed running instructions across all CPUs.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_average_cpu_usage_percentage`|
| `Metric type` | `gauge` |
| `Value type`  | `percent` |
| `Segment by` | `IBM IS Generation 2, resource name` |
{: caption="Table 1: Average CPU usage percentage metric metadata" caption-side="bottom"}

### CPU usage
{: #cpu-usage-cumulative-gen2}

The cumulative total of elapsed time that a CPU is running instructions since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_cpu_usage_nanoseconds`|
| `Metric type` | `gauge` |
| `Value type`  | `second` |
| `Segment by` | `IBM IS Generation 2, resource name, Virtual CPU index` |
{: caption="Table 2: Cumulative CPU usage metric metadata" caption-side="bottom"}

### CPU usage percentage
{: #cpu-usage-percentage-gen2}

The average percentage of time that a CPU is running instructions.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_cpu_usage_percentage`|
| `Metric type` | `gauge` |
| `Value type`  | `percent` |
| `Segment by` | `IBM IS Generation 2, resource name, Virtual CPU index` |
{: caption="Table 3: Average CPU usage metric metadata" caption-side="bottom"}

### Total CPU usage
{: #total-cpu-usage-nanoseconds-gen2}

The cumulative time that is elapsed running instructions across all CPUs since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_total_cpu_usage_nanoseconds`|
| `Metric type` | `gauge` |
| `Value type`  | `second` |
| `Segment by` | `IBM IS Generation 2, resource name` |
{: caption="Table 4: Total CPU usage metric metadata" caption-side="bottom"}

### Total number of CPUs
{: #total-cpus-gen2}

The total number of CPUs.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_cpus`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name` |
{: caption="Table 5: Total number of CPUs metric metadata" caption-side="bottom"}

## Network monitoring metrics
{: #network-metrics}

### Bytes received for a network interface
{: #network-int-bytes-received-gen2}

The cumulative number of bytes received for a network interface since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_in_bytes`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation 2, resource name, MAC address of the network interface` |
{: caption="Table 6: Average CPU usage percentage metric metadata" caption-side="bottom"}

### Bytes sent for a network interface
{: #network-bytes-sent}

The cumulative number of bytes sent for a network interface since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_out_bytes`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation 2, resource name, MAC address of the network interface` |
{: caption="Table 7: Bytes sent for a network interface metric metadata" caption-side="bottom"}

### Number of packets received for a network interface
{: #network-packets-received-gen2}

The cumulative number of packets that were received for a network interface since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_in_packets`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name, MAC address of the network interface` |
{: caption="Table 8: Number of packets received for a network interface metric metadata" caption-side="bottom"}

### Number of packets sent for a network interface
{: #network-packets-sent-gen2}

The cumulative number of packets that are sent for a network interface since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_out_packets`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name, MAC address of the network interface` |
{: caption="Table 9: Number of packets sent for a network interface metric metadata" caption}

### Number of receiving errors for a network interface
{: #network-errors-receiving-gen2}

The cumulative number of receiving errors for a network interface since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_in_errors`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name, MAC address of the network interface` |
{: caption="Table 10: Number of receiving errors for a network interface metric metadata" caption-side="bottom"}

### Number of sending errors for a network interface
{: #network-errors-sending-gen2}

The cumulative number of sending errors for a network interface since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_out_errors`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name, MAC address of the network interface` |
{: caption="Table 11: Number of sending errors for a network interface metric metadata" caption-side="bottom"}

### Number of dropped incoming packets for a network interface
{: #dropped-incoming-packets-gen2}

The cumulative number of dropped incoming packets for a network interface since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_in_dropped_packets`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name, MAC address of the network interface` |
{: caption="Table 12: Number of dropped incoming packets for a network interface metric metadata" caption-side="bottom"}

### Number of dropped outgoing packets for a network interface
{: #dropped-outgoing-packets-gen2}

The cumulative number of dropped outgoing packets for a network interface since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_network_out_dropped_packets`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name, MAC address of the network interface` |
{: caption="Table 13: Number of dropped outgoing packets for a network interface metric metadata" caption-side="bottom"}

## Memory monitoring metrics
{: #memory-metrics}

For {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instances, the memory metrics can't be collected because {{site.data.keyword.hpvs}} for VPC instances are created by using Secure Execution images and the memory of a secure execution instance isn't accessible.
{: note}

### Free memory
{: #free-memory-gen2}

The free memory of the virtual server instance in kibibytes (1024 bytes).

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_memory_free_kib`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation 2, resource name` |
{: caption="Table 14: Free memory metric metadata" caption-side="bottom"}

### Memory usage percentage
{: #memory-usage-percentage-gen2}

The percent of used memory of the virtual server instance.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_memory_usage_percentage`|
| `Metric type` | `gauge` |
| `Value type`  | `percent` |
| `Segment by` | `IBM IS Generation 2, resource name` |
{: caption="Table 15: Memory usage percentage metric metadata" caption-side="bottom"}

## Volume monitoring metrics
{: #volume-metrics}

### Number of bytes read for a volume
{: #bytes-read-for-volume-gen2}

The cumulative number of bytes read for a volume since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_volume_read_bytes`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation 2, resource name, disk name of the volume, ID of the volume` |
{: caption="Table 16: Number of bytes read for a volume metric metadata" caption-side="bottom"}

### Number of bytes written for a volume
{: #bytes-written-for-volume-gen2}

The cumulative number of bytes written for a volume since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_volume_write_bytes`|
| `Metric type` | `gauge` |
| `Value type`  | `byte` |
| `Segment by` | `IBM IS Generation 2, resource name, disk name of the volume, ID of the volume` |
{: caption="Table 17: Number of bytes written for a volume metric metadata" caption-side="bottom"}

### Number of read requests for a volume
{: #volume-read-requests-gen2}

The cumulative number of read requests for a volume since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_volume_read_requests`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name, disk name of the volume, ID of the volume` |
{: caption="Table 18: Number of read requests for a volume metric metadata" caption}

### Number of write requests for a volume
{: #volume-write-requests-gen2}

The cumulative number of write requests for a volume since virtual server instance start.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_volume_write_requests`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name, disk name of the volume, ID of the volume` |
{: caption="Table 19: Number of write requests for a volume metric metadata" caption-side="bottom"}

## Virtual server instance monitoring metrics
{: #virtual-server-metrics}

### Virtual server instance state
{: #virtual-server-state}

The state of the virtual server instance. A value of 1 indicates that the virtual server instance is running. A value of -1 indicates that the virtual server instance is stopped. 0 indicates an unknown value.

| Metadata | Description |
|----------|-------------|
| `Metric name` | `ibm_is_instance_running_state`|
| `Metric type` | `gauge` |
| `Value type`  | `none` |
| `Segment by` | `IBM IS Generation 2, resource name` |
{: caption="Table 20: The state of the virtual server" caption-side="bottom"}

## Attributes for segmentation
{: #segmentation-attributes}

### Global attributes
{: #global-segmentation-attributes}

These attributes are available for segmenting all of the previously listed metrics.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Cloud type` | `ibm_ctype` | Coud type is a value of public, dedicated, or local |
| `Location` | `ibm_location` | Location of the monitored resource - this can be a region, data center, or global |
| `Resource` | `ibm_resource` | Resource being measured by the service - typically an identifying name or GUID |
| `Resource type` | `ibm_resource_type` | Type of resource measured by the service |
| `Scope` | `ibm_scope` | Scope of the account, organization, or space GUID that is associated with this metric |
| `Service name` | `ibm_service_name` | Name of the service generating this metric |
{: caption="Table 21: Available attributes for segmenting" caption-side="bottom"}

### Additional attributes
{: #additional-segmentation-attributes}

These attributes are available to segment one or more attributes as described in the preceding table. See the individual metrics for segmentation options.

| Attribute | Attribute name | Attribute description |
|-----------|----------------|-----------------------|
| `Disk name of the volume` | `ibm_is_disk_name` | Disk name of the volume that is attached to the virtual server instance, corresponds to output of `'ls -l /dev/disk/by-id'` |
| `IBM IS Generation, 1 or 2` | `ibm_is_generation` | IBM IS Generation (`1` for Gen. 1 (Classic) or `2` for Gen. 2 (VPC) ) |
| `MAC address of the network interface` | `ibm_is_mac_address` | MAC address of the corresponding network interface that is attached to the virtual server instance |
| `Network interface index` | `ibm_is_nic_index` | Index of the network interface that is attached to the virtual server, starting from 0 |
| `Resource name` | `ibm_resource_name` | Resource name - for example, the virtual server instance name |
| `ID of the volume` | `ibm_is_volume_id` | UUID of storage volume that is attached to the virtual server instance |
| `Virtual CPU index` | `ibm_is_vcpu_index` | Index of the virtual CPU within the virtual server instance, starting from 0 |
{: caption="Table 22: Available attributes for segmenting one or more attributes" caption-side="bottom"}
