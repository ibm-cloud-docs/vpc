---

copyright:
  years: 2021, 2023
lastupdated: "2023-12-06"

keywords: Block Storage, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance, bandwidth

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}} 

# Bandwidth allocation for Block Storage volumes
{: #block-storage-bandwidth}

You can allocate bandwidth for volumes that are attached to a virtual server instance out of total instance bandwidth.
{: shortdesc}

## Bandwidth allocation for volumes that are attached to an instance
{: #attached-block-vol-bandwidth}

When you provision an instance, bandwidth is allocated between Block Storage volumes (boot volume and attached Block Storage volumes) and networking. The maximum bandwidth capacity is determined by the instance profile that you select during instance provisioning. For example, a bx2-2x8 balanced server profile allows a total instance bandwidth of 4,000 Mbps (4 Gbps). The initial storage and network bandwidth allocation depends on what you set by using the API or by the instance profile you selected. 

The maximum bandwidth of a volume is the highest potential bandwidth that can be allocated to the volume when it is attached to an instance. In cases where the total maximum bandwidth of attached volumes exceeds the amount that is available on the instance, the bandwidth for each volume attachment is set proportionally. The bandwidth is allocated based on the corresponding volume's maximum bandwidth.

For example, for the bx2-2x8 profile you might have the following allocations.

* Volumes: 1,000 Mbps.
* Network: 3,000 Mbps.

To ensure reasonable boot times, a minimum of 393 Mbps is allocated to the primary boot volume. In the example, the instance's total volume bandwidth is 1,000 Mbps. The remaining 607 Mbps is allocated to the secondary volumes that you attach, up to the maximum bandwidth of the volume. For example, if you have only one data volume with 500 Mbps, you can expect to get that level of performance.

Each volume has an IOPS and bandwidth limit set. The IOPS limit is always set to the maximum IOPS of the volume. The bandwidth for each attached volume is set proportionally based on the [volume size and profile](/docs/vpc?topic=vpc-block-storage-profiles). 

For optimal bandwidth to be realized, detach and reattach the volume after volume bandwidth allocation.

### Adjusting volume bandwidth
{: #volume-adjust-bandwidth}

You can change the storage-networking bandwidth ratio to meet your needs, but both volume and network bandwidth must be at least 500 Mbps each. For example, to allow more bandwidth for volumes, you can apportion the previous example in equal allocations:

* Storage volumes: 2,000 Mbps.
* Network: 2,000 Mbps.

However, before you change the storage-networking bandwidth ratio, be sure to evaluate your instance's network bandwidth requirements. Make sure that the new bandwidth allocation does not have negative effects on your instance’s network performance.

Extending the example for multiple volumes, the bandwidth allocations look like the following example.

* The total allocation for volumes is 2,000 Mbps.
* Minus the boot volume minimum allocation, 393 Mbps.
* The remaining bandwidth for data volumes is 1,607 Mbps.

The volume bandwidth that is available to the instance is apportioned on a per volume basis. The bandwidth is assigned per volume, not shared between volumes. For example, if you have an instance with four identical attached volumes but you are using only one volume, then that volume can get only the bandwidth that is assigned to it. The volume in use can't access the extra bandwidth that is assigned to the unused volumes.

### Unattached volume bandwidth versus attached volume bandwidth
{: #block-vol-bandwidth}

When you create a stand-alone (unattached) Block Storage data volume, the volume bandwidth is assigned based on volume capacity, IOPS, and volume profile.

* In the UI, the volume bandwidth can be seen as **Throughput** on the overview tab of the Block Storage volume details page.{: ui}

* In the CLI, you can see the bandwidth in the output of the `ibmcloud is volume` command.{: cli}

   ```sh
   ibmcloud is volume my-test-volume 
   Getting volume my-test-volume under account Test Account as user test.user@ibm.com...
                                          
   ID                                     r006-3869cd62-7676-43e3-8196-dad27b0c0f27
   Name                                   my-test-volume
   CRN                                    crn:v1:bluemix:public:is:us-south-3:a/a1234567::volume:r006-3869cd62-7676-43e3-8196-dad27b0c0f27
   Status                                 available
   Attachment state                       unattached   
   Capacity                               30
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

* The API response for a `GET /volume/{id}` call shows the bandwidth for an unattached volume like the following example.{: api}

   ```json
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
   } 
   ```
   {: screen}
   {: api}

When you attach a secondary volume to a virtual server instance, the primary boot volume gets priority IOPS, and bandwidth allocation to ensure reasonable boot times. Boot volume IOPS and bandwidth are never reduced to be less than 3000 IOPS or 393 Mbps.

All volumes are assigned instance bandwidth proportional to their maximum bandwidth, where the sum of all volume bandwidth equals the overall "volumes" bandwidth.

In our [example](#volume-adjust-bandwidth), the remaining bandwidth that is allocated on the instance for data volumes is 1,607 Mbps (2000 Mbps minus 393 Mbps for the boot volume). After the data volume is attached to the instance, the volume optimally requires 640 Mbps. If this volume is the only attached volume, it gets the full bandwidth allocation. If you add two more volumes of greater capacity, the bandwidth allocation is less.

Unattached volume bandwidth might not be the same as the actual bandwidth that you see after the volume is attached to an instance. The difference is due to the amount of bandwidth that is dedicated to the boot volume and all other attached data volumes.

## Estimating volume bandwidth
{: #volume-estimate-bandwidth}

Think about the type of data volume that your workloads require and select the appropriate volume profile. Data intensive workloads might require the higher bandwidth performance of a 10 IOPS/GB profile. For more information, see [How virtual server profiles relate to storage profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#vsi-profiles-relate-to-storage).

## Next steps
{: #vol-bandwidth-next-steps}

You can allocate total volume bandwidth in the UI, from the CLI or with the API. For more information, see [Adjusting bandwidth allocation in the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui){: ui}[Adjusting bandwidth allocation from the CLI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=cli#adjusting-bandwidth-allocation-cli){: cli}[Adjusting total volume bandwidth with the API](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api#adjusting-bandwidth-allocation-api){: api}.

You can also [view volume bandwidth for an unattached volume](/docs/vpc?topic=vpc-viewing-block-storage).
