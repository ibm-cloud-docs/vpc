---

copyright:
  years: 2019, 2020
lastupdated: "2020-11-06"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance, IOPS, HPCS, Key Protect

subcollection: vpc

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:beta: .beta}

# About {{site.data.keyword.block_storage_is_short}}
{: #block-storage-about}
[comment]: # (linked help topic)

{{site.data.keyword.block_storage_is_short}} provides hypervisor-mounted, high-performance data storage for your virtual server instances (instances) that you can provision within an {{site.data.keyword.vpc_full}} (VPC). The VPC infrastructure provides rapid scaling across zones and extra performance and security. For more information, see the [overview](/docs/vpc?topic=vpc-about-vpc) about {{site.data.keyword.vpc_short}}.
{:shortdesc}

{{site.data.keyword.block_storage_is_short}} provides primary boot volumes and secondary data volumes. Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning as well, or as stand-alone volumes that you can later attach to an instance. To protect your data, you can use your own encryption keys or choose IBM-managed encryption. 

You can choose an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers) to specify a pre-defined level of performance for your volumes. Or, you can choose a [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) and define your own volume capacity and IOPS level.

## Block storage for VPC volumes
{: #block-storage-vpc-volumes}

{{site.data.keyword.block_storage_is_short}} offers block-level volumes that are attached to an instance as a boot volume when the instance is created, or attached as secondary data volumes. You can configure up to 750 block storage volumes per account in a region. You can attach several block storage data volumes to a single instance for extra capacity. The number of volumes you can attach to an instance depends on how many vCPUs that instance contains. For more information, see [Volume attachment limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits).

### Boot volumes
{: #block-storage-vpc-boot-volumes}

When you create an instance, a 100 GB boot volume is created and attached to the instance by default. The boot volume has a maximum IOPS of 3,000 IOPS.

By default, boot volumes are encrypted by IBM-managed encryption. Optionally, you can use your own root keys (CRKs) by choosing customer-managed encryption during instance creation (see [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption)). You can't detach and delete the boot volume. Boot volumes are always deleted when you when you delete the instance.

### Data volumes
{: #secondary-data-volumes}

Block storage data volumes are secondary volumes with total capacity range of 10 GB to 2000 GB. Maximum IOPS for data volumes varies based on volume size and the IOPS tier profile that you selected. For example, the max IOPS for a 5 IOPS/GB volume up to 2 TBs is 10,000 IOPS. For more information, see
[IOPs tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers).

Data volumes are also available with capacities greater than 2000 GB. You can also expand the capacity of data volumes attached to an instance in GB increments up to 16 TB, depending on your volume profile. These are beta features available for evaluation and testing purposes. For information about creating volumes with greater capacity, see [Expanding block storage volume capacity (Beta)](/docs/vpc?topic=vpc-expanding-block-storage-volumes).
{:beta}

You can create data volumes as stand-alone volumes or when you provision an instance. Stand-alone volumes exist in an unattached state until you attach the volume to an instance. When you create a data volume as part of instance provisioning, the volume is automatically attached to the instance.

Block storage data volumes can be attached to any available instance in your region, based on your customer account and permissions, and within [certain limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits). These volumes are detached by default when the instance is deleted. Detaching by default allows your data to persist beyond the virtual server instance lifecycle. It only removes the volume's association with the instance. You can delete data volumes manually after they are detached. Also, when you create data volumes, you can specify that they be [automatically deleted](/docs/vpc?topic=vpc-managing-block-storage#auto-delete) when the instance is deleted.

Data volumes are encrypted by default with IBM-managed encryption. You can also encrypt data volumes using your own root keys.

## Block Storage encryption
{: #vpc-storage-encryption}

{{site.data.keyword.cloud_notm}} takes the need for security seriously and understands the importance of being able to encrypt data to keep it safe. When you create a standalone data volume or create a data volume as part of instance creation, you can choose to protect your data by using your own root keys, or use the default IBM provider-managed encryption. Boot volumes created when you provision an instance are encrypted with IBM-managed encryption. You can also edit the boot volume to use your root keys. After you set up ecryption for a boot or data volume, you can't change it.

For more information about data encryption, see [About data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about).

## Next Steps
{: #block-storage-about-next-steps}

Start creating {{site.data.keyword.block_storage_is_short}} volumes.

* For information about creating a new volume during instance provisioning, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi)
* For information about creating a block storage encrypted by your own encryption keys, see [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

## Related information
{: #related-info-storage}

For more information about creating and managing instances in the VPC, see [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers).

{{site.data.keyword.block_storage_is_short}} provides features unique to the VPC and is not compatible with the classic infrastructure storage. If you're interested in {{site.data.keyword.blockstoragefull}} on the classic infrastructure, see [{{site.data.keyword.blockstoragefull}}](/docs/BlockStorage?topic=BlockStorage-getting-started).
{:note}
