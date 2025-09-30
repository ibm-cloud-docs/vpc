---

copyright:
  years: 2021, 2025
lastupdated: "2025-09-30"

keywords: Block Storage, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, bandwidth

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}} 

# Bandwidth allocation for Block Storage volumes
{: #block-storage-bandwidth}

Instance bandwidth is distributed between networking and storage resources. The storage bandwidth is divided between the boot volume and the attached data volumes. You can adjust the storage-networking bandwidth ratio in the console, from the CLI or with the API. After that change, you can adjust the portion of available bandwidth that is allocated to the data volumes by detaching and reattaching them. The maximum bandwidth for your volumes is constrained by both the virtual server instance's and the volume's bandwidth limits. 
{: shortdesc}

## Adjusting Volume bandwidth vs Network bandwidth ratio
{: #storage-network-bandwidth}

When you provision an instance, bandwidth is allocated between storage volumes (boot volume and attached data volumes) and networking. The maximum bandwidth capacity is determined by the instance profile that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a total instance bandwidth of 4 Gbps (4,000 Mbps), while a cx3d-8x20 compute profile has an instance bandwidth cap of 16 Gbps (16,000 Mbps).

If you do not specify the initial volume and network bandwidth allocation, then 25% of total instance bandwidth is allocated to volume bandwidth and 75% is allocated to network bandwidth.

You can adjust the storage-networking bandwidth ratio to be more favorable to storage volumes [in the console](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui){: ui}[from the CLI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=cli#adjusting-bandwidth-allocation-cli){: cli}[with the API](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api#adjusting-bandwidth-allocation-api){: api}, but both volume and network bandwidth must be at least 500 Mbps each.

Before you change the storage-networking bandwidth ratio, evaluate your instance's network bandwidth requirements. Make sure that the new bandwidth allocation does not have negative effects on your instance’s network performance.
{: important}

The allocation does not change unless a volume is detached or attached to the instance. If you change the storage-networking bandwidth ratio, detach and reattach the data volumes for the new bandwidth allocation to be realized.

## Throughput limit value of block volumes
{: #block-vol-throughout-limit}

Each volume has an IOPS and a throughput limit. This throughput limit is either calculated automatically by using a throughput multiplier (traditional profiles) or can be specified explicitly (defined performance profile).

When you create a stand-alone data volume with a first-generation profile, the volume throughput limit is calculated based on volume capacity, IOPS, and the [volume profile](/docs/vpc?topic=vpc-block-storage-profiles). The IOPS limit is always set to the maximum IOPS of the volume. 

The provisioned throughput limit is determined by the total number of IOPS multiplied by the throughput multiplier. The throughput multiplier is 16 KB for 3 IOPS/GB or 5 IOPS/GB tiers, or 256 KB for 10 IOPS/GB or custom IOPS tiers. 

Maximum throughput limit:
- for the general-purpose volume profile, the limit is 670 MBps (5360 Mbps). 
- for the 5iops-tier volume profile, the limit is 768 MBps (6144 Mbps). 
- for the 10iops-tier and custom profiles, throughput can't exceed 1024 MBps (8192 Mbps).

Where can you see what bandwidth or throughput limit is assigned to your volume? The bandwidth or throughput limits assigned to the volume can be seen as **Throughput** on the overview tab of the Block Storage volume details page.{: ui}

Where can you see what bandwidth or throughput limit is assigned to your volume? You can see the bandwidth value in the output of the `ibmcloud is volume` command.{: cli}

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
       "name": "general-purpose"},
     "volume_attachments": []
   } 
   ```
   {: screen}
   {: api}
   
In the following examples, the stand-alone volumes are provisioned with:
- **`volume-a`** has 1,800 GB capacity and the 5 IOPS/GB volume profile, it can handle 9,000 IOPS, which means a maximum throughput limit of 1,179 Mbps.
- **`volume-b`** has 3,000 GB capacity and the 5 IOPS/GB volume profile, it can handle 15,000 IOPS, which means a maximum throughput limit of 1,966 Mbps. 
- **`volume-c`** has 3,000 GB capacity and the general-purpose volume profile, it can handle 9,000 IOPS, which means a maximum throughput limit of 1,179 Mbps.
- **`volume_d`** has 2,000 GB capacity and the general-purpose volume profile, it can handle 6,000 IOPS, which means a maximum throughput limit of 786 Mbps.

Volumes that have the same volume profile can have different IOPS and Throughput limit values based on their capacity. Volumes can have the same IOPS limit, even if their capacity and volume profiles are different.
{: note}

## Bandwidth allocation for attached volumes
{: #attached-block-vol-bandwidth}

To help ensure reasonable boot times, a minimum of 393 Mbps is allocated to the primary boot volume. The remaining volume bandwidth can be divided proportionally between the attached data volumes (weighted bandwidth allocation) or shared between the attached data volumes (pooled bandwidth allocation). If the [instance profile](/docs/vpc?group=profile-details) supports it, you can select the mode of Storage QoS directly when you create or modify the virtual server instance.

### Weighted bandwidth allocation for data volumes
{: #weighted-bandwidth-allocation}

By default, most virtual server instances are provisioned with the `weighted` volume QoS setting. Each volume can use the bandwidth that was allocated to it based on its provisioned throughput limit.

If one of the volumes is detached, the volume bandwidth allocation is updated and the individual volumes' allocated bandwidths are increased. Conversely, if more volumes are attached, the allocated bandwidths of individual volumes decrease. To see what volume bandwidth allocation methods your virtual server supports, see the capabilities section in the [compute profiles](/docs/vpc?group=profile-details) topics.

In the following examples, the cx3d-8x20 instance's total volume bandwidth is 4,000 Mbps. The available bandwidth that can be divided among the data volumes is 3607 Mbps. If you attach _`volume-a`_ and _`volume-b`_, their combined maximum throughput limit is 3145 Mbps. That value is less than the available 3607 Mbps, which means that the volume with more capacity (_`volume-b`_) is allocated 1966 Mbps and the volume with less capacity (_`volume-a`_) is allocated 1179 Mbps. These values are the volumes' provisioned throughput limit, which is the most bandwidth that can be allocated to the volumes, even if more bandwidth is available.

   ![Weighted bandwidth allocation with 2 data volumes](images/VolumeBandwidthAllocation-VSI_BlockVolume2.svg "Weighted bandwidth allocation with 2 data volumes"){: caption="Weighted bandwidth allocation with 2 data volumes" caption-side="bottom"}

   | Volumes | Maximum IOPS | Provisioned max throughput limit | Percentage | Allocated volume bandwidth |
   |---------|-------------:|----------------------------------|-----------:|---------------------------:|
   | `volume-a`|  9,000 | 1,179 Mbps |  37.5% | 1,179 Mbps  |
   | `volume-b`| 15,000 | 1,966 Mbps |  62.5% | 1,966 Mbps  |
   | All data volumes | N/A |  3,145 Mbps | 100% | 3,145 Mbps[^ttext1] |
   {: caption="Volume bandwidth allocation with 2 data volumes." caption-side="bottom"}

   [^ttext1]: The data volume's allocated bandwidth values equal their provisioned throughput limits. The available instance volume bandwidth is 3,607 MBps, which is more than the sum of the provisioned throughput limit of the data volumes.

   When you attach _`volume-c`_ the bandwidth allocation changes. The combined provisioned throughput limit value of the 3 data volumes is now 4324 Mbps. This value is more than the available 3607 Mbps. The 3607 Mbps is divided among the 3 data volumes proportionally.

   ![Weighted bandwidth allocation with 3 data volumes](images/VolumeBandwidthAllocation-VSI_BlockVolume3.svg "Weighted bandwidth allocation with 3 data volumes"){: caption="Weighted bandwidth allocation with 3 data volumes" caption-side="bottom"}

   | Volumes | Maximum IOPS | Provisioned max throughput limit | Percentage | Allocated volume bandwidth |
   |---------|-------------:|----------------------------------|-----------:|---------------------------:|
   | `volume-a`|  9,000 | 1,179 Mbps |  27% | 983.5 Mbps |
   | `volume-b`| 15,000 | 1,966 Mbps |  45% | 1,640 Mbps |
   | `volume-c`|  9,000 | 1,179 Mbps |  27% | 983.5 Mbps |
   | All data volumes | N/A |4,324 Mbps | 100% | 3,607 Mbps |
   {: caption="Volume bandwidth allocation with 3 data volumes." caption-side="bottom"}

   If you attach _volume-d_, the bandwidth allocation changes again. Table 4 shows the 4 attached volumes with their provisioned throughput limits in the 3rd column.

   ![Weighted bandwidth allocation with 4 data volumes](images/VolumeBandwidthAllocation-VSI_BlockVolume4.svg "Weighted bandwidth allocation with 4 data volumes"){: caption="Weighted bandwidth allocation with 4 data volumes" caption-side="bottom"}

   | Volumes | Maximum IOPS | Provisioned max throughput limit | Percentage | Allocated volume bandwidth |
   |---------|-------------:|----------------------------------|-----------:|---------------------------:|
   | `volume-a`|  9,000 | 1,179 Mbps |  23% |   832 Mbps |
   | `volume-b`| 15,000 | 1,966 Mbps |  38% | 1,388 Mbps |
   | `volume-c`|  9,000 | 1,179 Mbps |  23% |   832 Mbps |
   | `volume-d`|  6,000 |   786 Mbps |  15% |   555 Mbps |
   | All data volumes |N/A | 4,324 Mbps | 100% | 3,607 Mbps |
   {: caption="Volume bandwidth allocation with 4 data volumes." caption-side="bottom"}

The volume bandwidth is always apportioned on a per volume basis. The bandwidth is assigned per volume, not shared between volumes. In the examples where 3 or 4 data volumes are attached, the allocated bandwidth is less than the volume's own throughput limit. While the volume is provisioned to be able to handle more, it can use only the bandwidth that was allocated to it. It cannot use any bandwidth that is allocated to another volume, even if the other volume is not using its allocated bandwidth.

Increasing volume size, adjusting IOPS, or throughput limit (of second-generation profiles) does not automatically change the assigned bandwidth. Bandwidth assignment is updated when volumes are detached and attached.
{: important}

In most cases, the unattached provisioned volume bandwidth value is not the same as the bandwidth value that you see after the volume is attached to an instance.

### Pooled bandwidth allocation for data volumes
{: #pooled-vol-bandwidth}

Select [compute profiles](/docs/vpc?group=profile-details) support pooled bandwidth allocation for data volumes. With dynamic bandwidth allocation, a volume that's operating at peak I/O capacity can utilize unused bandwidth from other volume attachments.

Look at the second example again. The instance bandwidth cap is 16,000 Mbps. Initially, 4,000 Mbps is allocated to volume bandwidth and 12,000 Mbps is allocated to network bandwidth. The boot volume minimum allocation is 393 Mbps, and the attached data volumes are allocated portions of the remaining 3,607 Mbps.

You can change the default volume QoS setting [in the console](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#updating-qos-mode-ui), [from the CLI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=cli#updating-qos-mode-cli), or [with the API](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api#updating-qos-mode-api). First, you must stop the virtual server instance. Then, you can change the volume QoS setting to `pooled`, and start the virtual server instance.

The data volumes share the overall 3,607 Mbps bandwidth. The bandwidth that is used by each volume is limited by the volume's IOPS and throughput limits and the overall volume bandwidth limit of the instance. In the example of 3 data volumes, if _`volume-c`_ is not actively used, _`volume-a`_ and _`volume-b`_ can use its free bandwidth and are limited by their own provisioned IOPS and Throughput limits. When all data volumes are accessed for IO, their combined bandwidth can't exceed the 3,607 Mbps volume bandwidth.

![Data volumes share the available volume bandwidth dynamically based on usage.](images/PooledBWDiagramAnimationShort.mp4){: video controls loop autoplay}

Detaching and reattaching volumes does not change the volume QoS behavior.
{: note}   

Generally available second-generation profiles do not support pooled bandwidth allocation.

## Next steps
{: #volume-bandwidth-next-steps}

Think about the type of data volume that your workloads require and select the appropriate instance and volume profile. For more information, see [How virtual server profiles relate to storage profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#vsi-profiles-relate-to-storage).
