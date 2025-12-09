---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-09"

keywords: compute, virtual private cloud, virtual server instance, instance, bandwidth

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About bandwidth allocation for instance profiles
{: #bandwidth-allocation-profiles}

Instance profiles inform the available instance bandwidth of an instance. The number of vCPUs determine the available instance bandwidth. Depending on the profile, the bandwidth multiplier for instance bandwidth is 1 or 2 Gbps per vCPU.
{: shortdesc}

## Bandwidth allocation for resources that are attached to an instance
{: #attached-resources}

When you provision an instance, the total instance bandwidth is allocated between attached storage and networking. The maximum bandwidth capacity is determined by the [x86-64 instance profile](/docs/vpc?topic=vpc-profiles&interface=ui) that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a total instance bandwidth of 4 Gbps.

The initial volume and network bandwidth allocation depends on the bandwidth that you set by using the API or by the instance profile you selected. If you don't specify the initial volume and network bandwidth allocation, then 25% of total instance bandwidth is allocated to volume bandwidth and 75% is allocated to network bandwidth. See the following example:

* Instance bandwidth: 4 Gbps
* Volumes bandwidth: 1 Gbps
* Network bandwidth: 3 Gbps

The maximum bandwidth of a volume is the highest potential bandwidth that can be allocated to the volume when attached to an instance. If the total maximum bandwidth of attached volumes exceeds the amount available on the instance, the bandwidth for each attached volume is set proportionally based on each volume's maximum bandwidth.

To help ensure reasonable boot times, a minimum of 393 Mbps of volume bandwidth is allocated to the boot volume. In the example that has the instance's total volume bandwidth of 1,000 Mbps, the remaining 607 Mbps is allocated to any data volumes that you attach, up to the maximum bandwidth of the volume. For example, if you attach one data volume with 500 Mbps bandwidth, you can expect 500 Mbps of throughput.

You can see bandwidth allocations with the `/instance/profiles` endpoint in the API. You can also see the bandwidth allocations in the profile information during instance creation in the console.

## Adjusting bandwidth allocation
{: #adjusting-bandwidth-allocation}

You can adjust the allocation of the instance's total bandwidth, balancing between network bandwidth and volume bandwidth. Both volume and network bandwidth must be at least 500 Mbps each. Before you change the bandwidth ratio, make sure that you evaluate your instance's network bandwidth requirements. Make sure that the new bandwidth allocation doesn't have negative effects on your instance’s network performance.

For example, to allow more volume bandwidth, you might apportion the previous example in equal allocations:

* Volumes: 2,000 MBps
* Network: 2,000 MBps

By default, most virtual server instances are provisioned with the _weighted_ QoS setting. The volume bandwidth that is available to the instance is apportioned per volume. The bandwidth is assigned to each volume, not shared between volumes. For example, if four identical volumes are attached to an instance, then each volume is allocated one fourth of the overall volume bandwidth. A volume can use the bandwidth that is allocated to it, even if it is the only volume that is used. The volume that is in use can't access the bandwidth that is assigned to the unused volumes.

Select [compute profiles](/docs/vpc?group=profile-details) support dynamic bandwidth allocation for data volumes. By using dynamic bandwidth allocation, a volume that's maximizing its I/O capability can use unused bandwidth from the other volume attachments. You can change the _weighted_ storage QoS setting to _pooled_ [in the console](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#updating-qos-mode-ui), [from the CLI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=cli#updating-qos-mode-cli), or [with the API](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api#updating-qos-mode-api). For more information, see [Pooled volume bandwidth allocation](/docs/vpc?topic=vpc-block-storage-bandwidth#pooled-vol-bandwidth).
To see what volume bandwidth allocation methods your virtual server supports, see the capabilities section in the [compute profiles](/docs/vpc?group=profile-details) topics.

## Optimizing network bandwidth allocation for profiles
{: #network-perf-notes-for-profiles}

Profiles can have a total maximum bandwidth of up to 80 Gbps. That bandwidth is split between network and storage traffic. The network bandwidth allocation is distributed evenly across network interfaces. Each network interface has a capacity of 25 Gbps. You might need to attach multiple network interfaces to your virtual server instance to optimize network performance.

For example, if you choose the bx2-32x128 profile, the total bandwidth that is assigned for the instance is 64 Gbps. The default network capacity is 48 Gbps for network and 16 Gbps for storage, but this amount can be adjusted. If you use the default bandwidth allocation and a single network interface on the instance, that vNIC has a port speed of 25 Gbps. If two network interfaces are on the system, each network interface has a port speed of 24 Gbps, for a total aggregate network bandwidth for 48 Gbps. The remaining bandwidth (16 Gbps) is allocated to your storage volumes.

The following table illustrates this allocation for three different Gen 2 profile examples. If you use the same default values that were used for these calculations, you can match other instance profiles to the following table by matching the bandwidth capacity value to the overall bandwidth in the table. For more information about instance profiles, including network performance information, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui).

| Profile names | bx2-16x64 | bx2-32x128 | bx2-48-192 |
|-----|-----|-----|-----|
| Overall bandwidth | 32 Gbps | 64 Gbps | 80 Gbps |
| Default storage bandwidth allocation (25%) | 8 Gbps | 16 Gbps | 20 Gbps |
| Default total network bandwidth allocation (75%) | 24 Gbps | 48 Gbps | 60 Gbps |
| vNIC speed with 1 vNIC attached | 24 Gbps | 25 Gbps | 25 Gbps |
| vNIC speed with 2 vNICs attached | 2x12 Gbps | 2x24 Gbps | 2x25 Gbps |
| vNIC speed with 3 vNICs attached | 3x8 Gbps | 3x16 Gbps | 3x20 Gbps |
{: caption="Example profile bandwidth for Gen 2 profiles" caption-side="bottom"}

The network bandwidth cap applies separately to egress (transmitted) and ingress (received) traffic. That is, even if an instance that is capped at 4 Gbps reaches its transmit cap of 4 Gbps it can still receive up to its ingress cap of 4 Gbps.



## Next steps
{: #bandwidth-next-steps}

For more information, see the following topics:
* [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui)
* [Adjusting bandwidth allocation in the console](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui)
