---

copyright:
  years: 2021
lastupdated: "2021-08-18"

keywords: compute, virtual private cloud, virtual server instance, instance, bandwidth

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
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
{:shortdesc}

## Bandwidth allocation for resources attached to an instance
{: #attached-resources}

When you provision an instance, the total instance bandwidth is allocated between storage and networking. The maximum bandwidth capacity is determined by the [virtual server profile](/docs/vpc?topic=vpc-profiles&interface=ui) that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a total instance bandwidth of 4,000 Mbps (4 Gbps). The default allocation is 25% for storage, 75% for networking. In this case:

* Storage: 1,000 Mbps
* Network: 3,000 Mbps

To ensure reasonable boot times, a percentage of storage bandwidth is first allocated for the boot volume. By default, this is 393 Mbps. The remaining 607 Mb/s is allocated to any data volumes that you attach, based on the size of the volume. For example, if you have one data volume that's allocated 500 Mbps, you can expect to get that level of performance because the subtracting the boot volume (393 Mbps) from the total volume bandwidth allocated (1000 Mbps), leaves 607 Mbps. The IOPS limit is always set to the maximum IOPS of the volume. The bandwidth for each attached volume will be set proportionally based on the volume size and profile.

For example, if you have one data volume that is allocated 500 Mb/s, you can expect to get that level of performance because subtracting the boot volume bandwidth (393 Mb/s) from the total volume bandwidth allocated (1000 Mb/s) leaves 607 Mb/s, which is larger than the volume’s 500 Mb/s.

## Adjusting bandwidth allocation

The allocation of the instance's total bandwdth can be adjusted after provisioning, balancing between network bandwidth and storage bandwidth, but both storage and network bandwidth must be at least 500 Mbps each. Before you make any change to the storage/networking bandwidth ratio, be sure to evaluate your instance's network bandwidth requirements. Make sure the new bandwidth allocation will not have negative effects on your instance’s network performance.

For example, to allow more storage bandwidth, you could apportion the above example in equal allocations:

* Storage: 2,000 Mb/s
* Network: 2,000 Mb/s

The storage bandwidth available to the instance is apportioned on a per-volume basis. The bandwidth is assigned per volume, not shared between volumes. For example, if you have four identical volumes attached to an instance but are only using one volume, then that volume can get only the bandwidth assigned to it. The volume in use can't access extra bandwidth that is assigned to the unused volumes.
{: note}

<!--Customers will have the ability to modify the amount provided to storage bandwidth within the overall instance limits. A default amount of storage bandwidth will be set on each instance profile.
1Gbps-2Gbps per-vCPU with a limit of 80Gbps-->

## Example balanced profiles for x86_64 processors
{: #balanced-x86-profiles-example}

The following Balanced profiles are available for x86_64 processors:

| Instance profile | vCPU | GiB RAM | Bandwidth Cap (Gbps) | Instance Storage (GB) |
|---------|---------|---------|---------|---------|
| bx2-2x8 | 2 | 8 | 4 | - |
| bx2d-2x8 | 2 | 8 | 4 | 1x75 |
| bx2-4x16 | 4 | 16 | 8 | - |
| bx2d-4x16 | 4 | 16 | 8 | 1x150 |
| bx2-8x32 | 8 | 32 | 16 | - |
| bx2d-8x32 | 8 | 32 | 16 | 1x300 |
| bx2-16x64 | 16 | 64 | 32 | - |
| bx2d-16x64 | 16 | 64 | 32 | 1x600 |
{: caption="Table 1. Example profiles" caption-side="top"}


Most profiles have a bandwidth cap of 2 Gbps per vCPU and Ultra high memory profiles have a cap of 1 Gbps per vCPU. The total instanc bandwidth cap is 80 Gbps. Bandwidth is distributed evenly across network interfaces, and each network interface has a cap of 16 Gbps that might limit the overall performance. You might need to attach multiple network interfaces to your virtual server instance to optimize network performance.

For example, if you choose a profile with 16 vCPUs, the bandwidth cap for the profile is 32 Gbps. If you have just one network interface, the maximum network performance is 16 Gbps due to the network interface cap. You need to attach two network interfaces (16 Gbps each) to reach the profile cap of 32 Gbps.

For more information, see [Instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui).

<!-- Do we want to communicate this?
<!-- The 2Gbps per-vCPU is a maximum allocation for an instance. The actual allocation of networking bandwidth to an instance is determined by the number of attached vNICs. The maximum allocation for each vNIC is provided on the instance profile and for all existing profiles is 16Gb/s. This means that if only 1 vNIC is attached at-most 16Gb/s is allocated to the instance. -->

## Next steps
{: bandwidth-next-steps}

For more information, see:
* [Instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui)
* [Bandwidth allocation for block storage volumes](/docs/vpc?topic=vpc-block-storage-bandwith)
