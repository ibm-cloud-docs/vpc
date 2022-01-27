---

copyright:
  years: 2021, 2022
lastupdated: "2022-01-28"

keywords: compute, virtual private cloud, virtual server instance, instance, bandwidth

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:preview: .preview}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:note: .note}

# Bandwidth allocation for instance profiles
{: #bandwidth-allocation-profiles}

Instance profiles inform the available instance bandwidth of an instance.
{: shortdesc}

## Bandwidth allocation for resources attached to an instance
{: #attached-resources}

When you provision an instance, the total instance bandwidth is allocated between attached volumes and networking. The maximum bandwidth capacity is determined by the [instance profile](/docs/vpc?topic=vpc-profiles&interface=ui) that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a total instance bandwidth of 4,000 Mbps (4 Gbps). The initial volume and network bandwidth allocation depends on the bandwidth you set by using the API or by the instance profile you selected. You can see bandwidth allocations with the `/instance/profiles` endpoint in the API. You can also see the bandwidth allocations in the profile information during instance creation in the UI.

For example, for the bx2-2x8 profile you might have:

* Volumes: 1,000 Mbps
* Network: 3,000 Mbps

The maximum bandwidth of a volume is the highest potential bandwidth that can be allocated to the volume when attached to an instance. If the total maximum bandwidth of attached volumes exceeds the amount available on the instance, the bandwidth for each attached volume is set proportionally based on each volume's maximum bandwidth.

To ensure reasonable boot times, a minimum of 393 Mbps of volume bandwidth is allocated to the boot volume. In the example where the instance's total volume bandwidth is 1,000 Mbps, the remaining 607 Mbps is allocated to any secondary volumes that you attach, up to the maximum bandwidth of the volume. For example, if you have one data volume with 500 Mbps, you can expect to get that level of performance.

## Adjusting bandwidth allocation

The allocation of the instance's total bandwidth can be adjusted, balancing between network bandwidth and volume bandwidth. Both volume and network bandwidth must be at least 500 Mbps each. Before you make changes to the bandwidth ratio, be sure to evaluate your instance's network bandwidth requirements. Make sure that the new bandwidth allocation does not have negative effects on your instance’s network performance.

For example, to allow more volume bandwidth, you could apportion the above example in equal allocations:

* Volumes: 2,000 Mb/s
* Network: 2,000 Mb/s

The volume bandwidth available to the instance is apportioned on a per-volume basis. The bandwidth is assigned per volume, not shared between volumes. For example, if you have four identical volumes attached to an instance but are only using one volume, then that volume can get only the bandwidth assigned to it. The volume in use can't access extra bandwidth that is assigned to the unused volumes.
{: note}

<!--Customers will have the ability to modify the amount provided to volume bandwidth within the overall instance limits. A default amount of volume bandwidth will be set on each instance profile.
1Gbps-2Gbps per-vCPU with a limit of 80Gbps-->

## Optimizing network bandwidth allocation for profiles
{: #network-perf-notes-for-profiles}

Profiles can have a total maximum bandwidth of up to 80 Gbps. That bandwidth is split between Network and Storage traffic. The network bandwidth allocation is distributed evenly across network interfaces, and each network interface has a cap of 25 Gbps. You might need to attach multiple network interfaces to your virtual server instance to optimize network performance.

For example, if you choose the bx2-32x128 profile, the total bandwidth assigned for the instance is 64 Gbps. The default network cap is 48 Gbps for network and 16 Gbps for storage, but this amount can be adjusted. If using the default bandwidth allocation and a single network interface on the instance, that vNIC has a port speed of 25 Gbps. If there are two network interfaces on the system, each network interface has a port speed of 24 Gbps, for a total aggregate network bandwidth for 48 Gbps. The remaining bandwidth (16 Gbps) is allocated to your storage volumes. The following table illustrates this for three different profile examples.

| Profile names: | bx2-16x64 | bx2-32x128 | bx2-48-192 |
| --- | --- | --- | --- |
| Overall bandwidth | 32 Gbps | 64 Gbps | 80 Gbps |
| Default storage bandwidth allocation (25%) | 8 Gbps | 16 Gbps | 20 Gbps |
| Default total network bandwidth allocation (75%) | 24 Gbps | 48 Gbps | 60 Gbps |
| vNIC speed with 1 vNIC attached | 24 Gbps | 25 Gbps | 25 Gbps |
| vNIC speed with 2 vNICs attached | 2x12 Gbps | 2x24 Gbps | 2x25 Gbps |
| vNIC speed with 3 vNICs attached | 3x8 Gbps | 3x16 Gbps | 3x20 Gbps |
{: caption="Table 11 Example profile bandwidth" caption-side="bottom"}

The network bandwidth cap applies separately to egress (transmitted) and ingress (received) traffic. That is, even if an instance capped at 4 Gbps reaches its transmit cap of 4 Gbps, that does not impact its ability to receive up to its received cap of 4 Gbps.

<!--Customers will have the ability to modify the amount provided to volume bandwidth within the overall instance limits. A default amount of volume bandwidth will be set on each instance profile.
1Gbps-2Gbps per-vCPU with a limit of 80Gbps-->

For more information on instance profiles, including network performance information, see [Instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui).


<!-- Do we want to communicate this?-->
<!-- The 2Gbps per-vCPU is a maximum allocation for an instance. The actual allocation of networking bandwidth to an instance is determined by the number of attached vNICs. The maximum allocation for each vNIC is provided on the instance profile and for all existing profiles is 16Gb/s. This means that if only 1 vNIC is attached at-most 16Gb/s is allocated to the instance. -->

## Next steps
{: bandwidth-next-steps}

For more information, see:
* [Instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui)
* [Adjusting bandwidth allocation using the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui)
