---

copyright:
  years: 2019, 2022
lastupdated: "2022-02-16"

keywords: block storage, boot volume, data volume, volume, data storage, virtual server instance, instance, IOPS, Hyper Protect, Key Protect

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:note: .note}

# About {{site.data.keyword.block_storage_is_short}}
{: #block-storage-about}
[comment]: # (linked help topic)

{{site.data.keyword.block_storage_is_short}} provides hypervisor-mounted, high-performance data storage for your virtual server instances that you can provision within an {{site.data.keyword.vpc_full}} (VPC). The [VPC](/docs/vpc?topic=vpc-about-vpc) infrastructure provides rapid scaling across zones and extra performance and security. 
{: shortdesc}

## Overview
{: #block-storage-overview}

{{site.data.keyword.block_storage_is_short}} supports all [virtual server profiles](/docs/vpc?topic=vpc-block-storage-profiles#vsi-profiles-relate-to-storage).

{{site.data.keyword.block_storage_is_short}} volume data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

{{site.data.keyword.block_storage_is_short}} provides primary boot volumes and secondary data volumes. Boot volumes are automatically created and attached during instance provisioning. You can increase capacity of the boot volume during instance provisioning. Data volumes can be created and attached during instance provisioning as well, or as stand-alone volumes that you can later attach to an instance. To protect your data, you can use your own encryption keys or choose IBM-managed encryption.

You can choose an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers) to specify a pre-defined level of performance for your volumes. Or, you can choose a [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) and define your own volume capacity and IOPS level. All profiles are backed by solid-state drives (SSDs).

For data volumes attached to a virtual server instance, you can [increase volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) in GB increments up to 16,000 GB capacity, depending on your volume profile. You can also [increase or decrease IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) for a volume attached to an instance by specifying a different IOPS tier or custom IOPS band. 

For optimal performance, you can allocate storage bandwidth on the instance and change the storage/networking bandwidth ratio. For more information, see [Bandwidth allocation for block storage volumes](/docs/vpc?topic=vpc-block-storage-bandwidth).

Block Storage for VPC is integrated with the Security and Compliance Center to help you manage security and compliance for your organization. For more information, see [Managing security and compliance](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#block-storage-vpc-manage-security).

## Block storage for VPC volumes
{: #block-storage-vpc-volumes}

{{site.data.keyword.block_storage_is_short}} offers block-level volumes that are attached to an instance as a boot volume when the instance is created or attached as secondary data volumes. You can configure up to 300 block storage volumes per account in a region. You can request to increase this quota by [opening support case](/docs/vpc?topic=vpc-manage-storage-limit) and specifying in which zone you need more volumes.

You can attach only one block storage volume to a virtual server instance at a time, but you can attach up to 12 block storage data volumes to a single instance. For other limitations, see [Volume attachment limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits).

### Boot volumes
{: #block-storage-vpc-boot-volumes}

When you create an instance from a stock image, a 100 GB, 3,000 IOPS general purpose boot volume is created and attached to the instance by default. You can't detach and delete the boot volume. Boot volumes are always deleted when you delete the instance.

You can increase the capacity of a boot volume up to 250 gigabytes (GB). When you create an instance from an image or an instance template, you can specify a larger capacity than the image's minimum provisioned size. You can also increase the size of an existing boot volume by updating the volume. For more information, see [Increasing boot volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes).

By default, boot volumes are encrypted by IBM-managed encryption. Optionally, you can use your own root keys (CRKs) by choosing customer-managed encryption during instance creation (see [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption)).

You cannot create an image from an encrypted boot volume (Image from a volume feature) that is not 100GB. The operation will be blocked.
{: note}

### Data volumes
{: #secondary-data-volumes}

Block storage data volumes are secondary volumes with total capacity range of 10 GB to 16,000 GB. Maximum IOPS for data volumes varies based on volume size and the IOPS tier profile that you selected. For more information, see
[IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers).

You can create data volumes as stand-alone volumes or when you provision an instance. Stand-alone volumes exist in an unattached state until you attach the volume to an instance. When you create a data volume as part of instance provisioning, the volume is automatically attached to the instance.

Block storage data volumes can be attached to any available instance in your region, based on your customer account and permissions, and within [certain limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits). These volumes are detached by default when the instance is deleted. Detaching by default allows your data to persist beyond the virtual server instance lifecycle. It only removes the volume's association with the instance. You can delete data volumes manually after they are detached. Also, when you create data volumes, you can specify that they be [automatically deleted](/docs/vpc?topic=vpc-managing-block-storage#auto-delete) when the instance is deleted.

Detached volumes can be attached to an available, running instance without reprovisioning the volume or the instance.

When you create and attach a data volume to a virtual server instance, you can later increase the size of that volume. You indicate capacity in GB increments up to 16,000 GB capacity, depending on your volume profile. For more information, see [Expanding block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

Data volumes are encrypted by default with IBM-managed encryption. You can also encrypt data volumes using your own root keys.

## Block Storage encryption
{: #vpc-storage-encryption}

{{site.data.keyword.cloud_notm}} takes the need for security seriously and understands the importance of being able to encrypt data to keep it safe. When you create a stand-alone data volume or create a data volume as part of instance creation, you can choose to protect your data by using your own root keys, or use the default IBM-managed encryption. Boot volumes created when you provision an instance are encrypted with IBM-managed encryption. You can also edit the boot volume to use your root keys. After you set up encryption for a boot or data volume, you can't change it.

For more information about data encryption, see [About data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about).

## Cancelling your block storage
{: #vpc-cancel-storage}

If you no longer need a volume, you can cancel it at any time. IBM [wipes all data](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication) before the storage is reused. If you have additional compliance requirements such as NIST 800-88 Guidelines for Media Sanitization, you must perform data sanitation procedures before you delete your volumes. For more information, see [Sanitizing your data before deleting a volume](/docs/vpc?topic=vpc-managing-block-storage#block-storage-sanitization).

## Next Steps
{: #block-storage-about-next-steps}

Start creating {{site.data.keyword.block_storage_is_short}} volumes.

* For information about creating a new volume during instance provisioning, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi).
* For information about creating a block storage encrypted by your own encryption keys, see [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

## Related information
{: #related-info-storage}

For more information about creating and managing instances in the VPC, see [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers).

{{site.data.keyword.block_storage_is_short}} provides features unique to the VPC and is not compatible with the classic infrastructure storage. If you're interested in {{site.data.keyword.blockstoragefull}} on the classic infrastructure, see [{{site.data.keyword.blockstoragefull}}](/docs/BlockStorage?topic=BlockStorage-getting-started).
{: note}
