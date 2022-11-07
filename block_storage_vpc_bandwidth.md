---

copyright:
  years: 2021, 2022
lastupdated: "2022-11-07"

keywords: block storage, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance, bandwidth

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Bandwidth allocation for block storage volumes
{: #block-storage-bandwidth}

You can allocate bandwidth for volumes attached to a virtual server instance out of total instance bandwidth.
{: shortdesc}

## Bandwidth allocation for volumes attached to an instance
{: #attached-block-vol-bandwidth}

When you provision an instance, bandwidth is allocated between block storage volumes (boot volume and attached block storage volumes)and networking. The maximum bandwidth capacity is determined by the instance profile that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a total instance bandwidth of 4,000 Mbps (4 Gbps). The initial storage and network bandwidth allocation depends on what you set by using the API or by the instance profile you selected. 

The maximum bandwidth of a volume is the highest potential bandwidth that can be allocated to the volume when attached to an instance. In cases where the total maximum bandwidth of attached volumes exceeds the amount available on the instance, the bandwidth for each volume attachment is set proportionally, based on the corresponding volume's maximum bandwidth.

For example, for the bx2-2x8 profile you might have:

* Volumes: 1,000 Mbps
* Network: 3,000 Mbps

To ensure reasonable boot times, a minimum of 393 Mbps is allocated to the primary boot volume. In the example, the instance's total volume bandwidth is 1,000 Mbps and the remaining 607 Mbps is allocated to any secondary volumes that you attach, up to the maximum bandwidth of the volume. For example, if you have one data volume with 500 Mbps, you can expect to get that level of performance.

Each volume will have an IOPS and bandwidth limit set. The IOPS limit is always set to the maximum IOPS of the volume. The bandwidth for each attached volume is set proportionally based on the [volume size and profile](/docs/vpc?topic=vpc-block-storage-profiles). 

For optimal bandwidth to be realized, detach and reattach the volume after volume bandwidth allocation using the UI, CLI, or API.

### Adjusting volume bandwidth
{: #volume-adjust-bandwidth}

You can change the volume/networking bandwidth ratio to meet your needs, but both volume and network bandwidth must be at least 500 Mbps each. For example, to allow more bandwidth for volumes, you could apportion the above example in equal allocations:

* Volumes: 2,000 Mbps
* Network: 2,000 Mbps

However, before you make any change to the volume/networking bandwidth ratio, be sure to evaluate your instance's network bandwidth requirements. Make sure the new bandwidth allocation will not have negative effects on your instance’s network performance.

Extending the example for multiple volumes, the bandwidth looks like this:

* Total allocation for volumes is 2,000 Mbps
* Less boot volume minimum allocation, 393 Mbps
* Remaining bandwidth for data volumes: 1,607 Mbps

The volume bandwidth available to the instance is apportioned on a per-volume basis. The bandwidth is assigned per volume, not shared between volumes. For example, if you have four identical volumes attached to an instance but are only using one volume, then that volume can get only the bandwidth assigned to it. The volume in use can't access extra bandwidth that is assigned to the unused volumes.

### Unattached volume bandwidth compared with attached volume bandwidth
{: #block-vol-bandwidth}

When you create a standalone (unattached) block storage data volume, the volume bandwidth assigned is based on volume capacity, IOPS, and volume profile. For example, the API response for a `GET /volume/{id}` call shows the bandwidth for an unattached volume:

```curl
{
  "active": true,
  "bandwidth": 640,
  "busy": false,
  "capacity": 500,
  "created_at": "2021-08-08T06:26:17Z",
  "crn": "crn:[...]",
  "encryption": "provider_managed",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/ccbe6fe1-5680-4865-94d3-687076a38293",
  "id": "ccbe6fe1-5680-4865-94d3-687076a38293",
  "iops": 5000,
  "name": "my-volume-1",
  "profile": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
    "name": "custom"
 .
 .
 .
  "volume_attachments": []
```
{: codeblock}

When you attach a secondary volume to a virtual server instance, the primary boot volume gets priority IOPS and bandwidth allocation to ensure reasonable boot times. Boot volume IOPS and bandwidth are never reduced below 3000 IOPS or 393 Mbps.

All volumes are assigned instance bandwidth proportional to their maximum bandwidth, where the sum of all volume bandwidth equals the overall "volumes" bandwidth.

In our [example](#volume-adjust-bandwidth), the remaining bandwidth allocated on the instance for data volumes was 1,607 Mbps (2000 Mbps less 393 Mbps for the boot volume). After attaching the data volume to the instance, the volume would optimally require 640 Mbps. If this was the only attached volume, it would get this full bandwidth allocation. If you had two additional volumes of greater capacity, the bandwidth allocation might be less.

Unattached volume bandwidth may not be the same as the actual bandwidth you'll see after the volume is attached to an instance. This is due to the amount of bandwidth dedicated to the boot volume and all other attached data volumes.

## Estimating volume bandwidth
{: #volume-estimate-bandwidth}

You should estimate they type of data volume your workloads require and select the right volume profile. Data intensive workloads might require the higher bandwidth performance of a 10 IOPS/GB profile. For the relationship of volume profiles to compute profiles, see [How virtual server profiles relate to storage profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#vsi-profiles-relate-to-storage). For information on how block size affects performance, see [Block storage capacity and performance](/docs/vpc?topic=vpc-capacity-performance&interface=ui#how-block-size-affects-performance).

## Next steps
{: #vol-bandwidth-next-steps}

Allocate total volume bandwidth by using the API or CLI. For more information, see [Adjusting bandwidth allocation using the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances#adjusting-bandwidth-allocation-ui) and [Adjusting total volume bandwidth from the API](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api#adjusting-bandwidth-allocation-api).

You can also [view volume bandwidth for an unattached volume](/docs/vpc?topic=vpc-viewing-block-storage).
