---

copyright:
  years: 2021, 2024
lastupdated: "2024-10-23"

keywords: Block Storage, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, bandwidth

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}} 

# Bandwidth allocation for Block Storage volumes
{: #block-storage-bandwidth}

Instance bandwidth is distributed between networking and storage resources. The storage bandwidth is divided between the boot volume and the attached data volumes. You can adjust the storage-networking bandwidth ratio in the console, from the CLI or with the API. After that change, you can adjust the portion of available bandwidth that is allocated to the data volumes by detaching and reattaching them.
{: shortdesc}

## Adjusting Volume bandwidth vs Network bandwidth ratio
{: #storage-network-bandwidth}

When you provision an instance, bandwidth is allocated between storage volumes (boot volume and attached data volumes) and networking. The maximum bandwidth capacity is determined by the instance profile that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a total instance bandwidth of 4 Gbps (4,000 Mbps), while a cx3d-8x20 compute profile has an instance bandwidth cap of 16 Gbps (16,000 Mbps).

The initial storage and network bandwidth allocation depends on the instance profile that you selected, and you can also specify its value when you provision the instance with the API. If you do not specify the initial volume and network bandwidth allocation, then 25% of total instance bandwidth is allocated to volume bandwidth and 75% is allocated to network bandwidth.

For example, with the bx2-2x8 profile you might have the following allocations.
   - Volumes: 1 Gbps.
   - Network: 3 Gbps.

For the cx3d-8x20 profile, you might have the following allocations.
   - Volumes: 4 Gbps.
   - Network: 12 Gbps.

You can change the storage-networking bandwidth ratio [in the console](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui){: ui}[from the CLI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=cli#adjusting-bandwidth-allocation-cli){: cli}[with the API](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api#adjusting-bandwidth-allocation-api){: api}, but both volume and network bandwidth must be at least 500 Mbps each.

Before you change the storage-networking bandwidth ratio, evaluate your instance's network bandwidth requirements. Make sure that the new bandwidth allocation does not have negative effects on your instance’s network performance.
{: important} 

To help ensure reasonable boot times, a minimum of 393 Mbps is allocated to the primary boot volume. The remaining volume bandwidth is proportionally allocated between the attached data volumes. The allocation does not change unless a volume is detached or attached to the instance. If you change the storage-networking bandwidth ratio, detach and reattach the data volumes for the new bandwidth allocation to be realized.
{: important}

## Throughput limit value of unattached volumes
{: #block-vol-bandwidth}

Each volume has an IOPS and a throughput limit. When you create a stand-alone (unattached) data volume, the volume throughput limit is calculated based on volume capacity, IOPS, and [volume profile](/docs/vpc?topic=vpc-block-storage-profiles). The IOPS limit is always set to the maximum IOPS of the volume. 

The provisioned throughput limit is determined by the total number of IOPS multiplied by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB or custom IOPS tiers. The maximum throughput limit for the general-purpose volume profile is 670 MBps (5360 Mbps). The maximum throughput limit for the 5iops-tier volume profile is 768 MBps (6144 Mbps). The remaining volume profiles (10iops-tier and custom) can't exceed the throughput limit of 1024 MBps (8192 Mbps).

See the following examples:
- When you provision a stand-alone volume with 1,800 GB capacity and the 5 IOPS/GB volume profile, it can handle 9,000 IOPS, which means a maximum throughput limit of 1,179 Mbps. In the later example and tables, this volume is called _`volume-a`_.
- When you provision a stand-alone volume with 3000 GB capacity and the 5 IOPS/GB volume profile, it can handle 15,000 IOPS, which means a maximum throughput limit of 1,966 Mbps. In the later example and tables, this volume is called _`volume-b`_.
- When you provision a stand-alone volume with 3000 GB capacity and the general-purpose volume profile, it can handle 9,000 IOPS, which means a maximum throughput limit of 1,179 Mbps. In the later example and tables, this volume is called _`volume-c`_.
- When you provision a stand-alone volume with 2000 GB capacity and the general-purpose volume profile, it can handle 6,000 IOPS, which means a maximum throughput limit of 786 Mbps. In the later example and tables, this volume is called _`volume_d`_.

Where can you see what bandwidth or throughput limit is assigned to your volume? In the UI, the volume bandwidth can be seen as **Throughput** on the overview tab of the Block Storage volume details page.{: ui}

Where can you see what bandwidth or throughput limit is assigned to your volume? In the CLI, you can see the bandwidth in the output of the `ibmcloud is volume` command.{: cli}

   ```sh
   ibmcloud is volume my-test-volume 
   Getting volume my-test-volume under account Test Account as user test.user@ibm.com...
                                          
   ID                                     r006-3869cd62-7676-43e3-8196-dad27b0c0f27
   Name                                   my-test-volume
   CRN                                    crn:v1:bluemix:public:is:us-south-3:a/a1234567::volume:r006-3869cd62-7676-43e3-8196-dad27b0c0f27
   Status                                 available
   Attachment state                       unattached   
   Capacity                               100
   IOPS                                   3000
   Bandwidth(Mbps)                        393
   Profile                                general-purpose
   Encryption key                         -
   Encryption                             provider_managed
   Resource group                         defaults
   Created                                2021-12-09T15:42:11+00:00
   Zone                                   us-south-3
   Health State                           ok
   Volume Attachment Instance Reference   -
   Active                                 false
   Adjustable IOPS                        false
   Busy                                   false
   Tags                                   -
   ```
   {: screen}
   {: cli}

Where can you see what bandwidth or throughput limit is assigned to your volume? The API response for a `GET /volume/{id}` call shows the bandwidth for an unattached volume like the following example snippet.{: api}

   ```json
   {
     "active": true,
     "bandwidth": 393,
     "busy": false,
     "capacity": 100,
     "created_at": "2021-12-09T15:42:11+00:00",
     "crn": "crn:v1:bluemix:public:is:us-south-3:a/a1234567::volume:r006-3869cd62-7676-43e3-8196-dad27b0c0f27",
     "encryption": "provider_managed",
     "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/ccbe6fe1-5680-4865-94d3-687076a38293",
     "id": "r006-3869cd62-7676-43e3-8196-dad27b0c0f273",
     "iops": 3000,
     "name": "my-test-volume",
     "profile": {
       "href": "https://us-south.iaas.cloud.ibm.com/v1/volume/profiles/general-purpose",
       "name": "general-purpose"}
     "volume_attachments": []
   } 
   ```
   {: screen}
   {: api}

## Bandwidth allocation for attached volumes
{: #attached-block-vol-bandwidth}

When you attach a data volume to a virtual server instance, the primary boot volume gets priority IOPS and bandwidth allocation to help ensure reasonable boot times. Boot volume IOPS and bandwidth are never reduced to be less than 3000 IOPS and 393 Mbps.

All attached volumes are assigned instance bandwidth proportional to their maximum throughput limit, where the sum of all volume bandwidth equals the overall volumes bandwidth.

In the first example, the bx2-2x8 instance's total instance bandwidth of the bx2-2x8 profile is 4 Gbps. The storage bandwidth is 1 Gbps (1000 Mbps), and the boot volume is allocated 393 Mbps. The remaining 607 Mbps is divided among to the data volumes that you attach. The bandwidth allocation is proportional to the provisioned throughput limit of each data volume. 

In table 1, you can see the 3 attached data volumes and their provisioned throughput limits. The percentage column shows the proportion of each volume's bandwidth compared to the combined provisioned throughput value. To calculate how the available instance volume bandwidth is allocated to each volume, multiply the available instance volume bandwidth with the volume's percentage. The results are shown in the allocated volume bandwidth column.

In the first example, the combined provisioned value equals 4,324 Mbps, which is 100%. `volume-a` and `volume-c` throughput limit value is 27% of the combined throughput value. To see how much of the available instance volume bandwidth is allocated to them, you must multiply 607 Mbps with 0.27. The result is 166 Mbps.

| Volumes | Maximum IOPS | Provisioned max throughput limit | Percentage | Allocated volume bandwidth |
|---------|-------------:|----------------------------------|-----------:|---------------------------:|
| `volume-a`|  9,000 | 1,179 Mbps |  27% | 166 Mbps |
| `volume-b`| 15,000 | 1,966 Mbps |  45% | 275 Mbps |
| `volume-c`|  9,000 | 1,179 Mbps |  27% | 166 Mbps |
| All data volumes | N/A | 4,324 Mbps | 100% | 607 Mbps |
{: caption="Volume bandwidth allocation with 3 data volumes." caption-side="bottom"}

In the second example, the cx3d-8x20 instance's total volume bandwidth is 4 Gbps (4,000 Mbps). The available bandwidth that can be divided among the data volumes is 3607 Mbps. If you attach _`volume-a`_ and _`volume-b`_, both of which have the same 5 IOPS/GB volume profile, their combined maximum throughput limit is 3145 Mbps. That value is less than the available 3607 Mbps, which means that the volume with more capacity (_`volume-b`_) is allocated 1966 Mbps and the volume with less capacity (_`volume-a`_) is allocated 1179 Mbps. That's their provisioned throughput limit, and that's the most bandwidth that can be allocated to them, even if more bandwidth is available.

| Volumes | Maximum IOPS | Provisioned max throughput limit | Percentage | Allocated volume bandwidth |
|---------|-------------:|----------------------------------|-----------:|---------------------------:|
| `volume-a`|  9,000 | 1,179 Mbps |  37.5% | 1,179 Mbps  |
| `volume-b`| 15,000 | 1,966 Mbps |  62.5% | 1,966 Mbps  |
| All data volumes | N/A |  3,145 Mbps | 100% | 3,145 Mbps[^ttext1] |
{: caption="Volume bandwidth allocation with 2 data volumes." caption-side="bottom"}

[^ttext1]: The data volume's allocated bandwidth values equal their provisioned throughput limits. The available instance volume bandwidth is 3,607 MBps, which is more than the sum of the provisioned throughput limit of the data volumes.

When you attach _`volume-c`_ the bandwidth allocation changes. The combined provisioned throughput limit value of the 3 data volumes is now 4324 Mbps. This value is more than the available 3607 Mbps, so the 3607 Mbps is divided among the 3 data volumes proportionally.

| Volumes | Maximum IOPS | Provisioned max throughput limit | Percentage | Allocated volume bandwidth |
|---------|-------------:|----------------------------------|-----------:|---------------------------:|
| `volume-a`|  9,000 | 1,179 Mbps |  27% | 983.5 Mbps |
| `volume-b`| 15,000 | 1,966 Mbps |  45% | 1,640 Mbps |
| `volume-c`|  9,000 | 1,179 Mbps |  27% | 983.5 Mbps |
| All data volumes | N/A |4,324 Mbps | 100% | 3,607 Mbps |
{: caption="Volume bandwidth allocation with 3 data volumes." caption-side="bottom"}

If you provision and attach a 4th data volume with the general-purpose profile and capacity of 2,000 GB, the bandwidth allocation changes again. Table 4 shows the 4 attached volumes with their provisioned throughput limits in the 3rd column.

| Volumes | Maximum IOPS | Provisioned max throughput limit | Percentage | Allocated volume bandwidth |
|---------|-------------:|----------------------------------|-----------:|---------------------------:|
| `volume-a`|  9,000 | 1,179 Mbps |  23% |   832 Mbps |
| `volume-b`| 15,000 | 1,966 Mbps |  38% | 1,388 Mbps |
| `volume-c`|  9,000 | 1,179 Mbps |  23% |   832 Mbps |
| `volume-d`|  6,000 |   786 Mbps |  15% |   555 Mbps |
| All data volumes |N/A | 4,324 Mbps | 100% | 3,607 Mbps |
{: caption="Volume bandwidth allocation with 4 data volumes." caption-side="bottom"}

The volume bandwidth that is available to the instance is always apportioned on a per volume basis. The bandwidth is assigned per volume, not shared between volumes. In the examples where 3 or 4 data volumes are attached, the allocated bandwidth is less than the volume's own throughput limit. While the volume is provisioned to be able to handle more, it can use only the bandwidth that was allocated to it. It cannot use any bandwidth that is allocated to, but not used by another volume.

In most cases, the unattached provisioned volume bandwidth value is not the same as the bandwidth value that you see after the volume is attached to an instance.

## Estimating volume bandwidth
{: #volume-estimate-bandwidth}

Think about the type of data volume that your workloads require and select the appropriate volume profile. Data intensive workloads might require the higher bandwidth performance of a 10 IOPS/GB profile. For more information, see [How virtual server profiles relate to storage profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#vsi-profiles-relate-to-storage).
