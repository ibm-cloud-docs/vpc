---

copyright:
  years: 2019
lastupdated: "2019-07-31"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance, IOPS, FAQ

subcollection: vpc

---
{:faq: data-hd-content-type='faq'}
{:codeblock: .codeblock}
{:screen: .screen}

# FAQs for block storage
{: #block-storage-vpc-faq}

Welcome to the FAQ page for {{site.data.keyword.block_storage_is_short}}. This topic answers many of your questions for creating and managing your block storage volumes. If you have additional questions you'd like to see addressed in this topic, please provide feedback using the **Issues** or **Edit topic** links.
{:shortdesc}

## How do I allocate storage for my instances?
{: faq}

When you create a virtual server instance, you can [create a block storage volume](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi) that will be attached to that instance. You can also [create standalone volumes](/docs/vpc?topic=vpc-creating-block-storage#create-standalone-vol) and later attach them to your instances.

## How many instances can share a provisioned block storage volume?
{: faq}

A block storage volume can be attached to only one instance at a time. Instances cannot share the same volume.

## How many block storage secondary (data) volumes can be attached to an instance?
{: faq}

Instances with less than 4 vCPUs can attach up to 4 block storage secondary volumes.

Instances with 4 vCPUs or more can attach up to 12 block storage secondary volumes.

**Note**: Volume attachment limits are recommended but not strictly enforced in the Early Access release. Depending on your OS environment, you might be able to attach more secondary volumes to an instance.

## When provisioning IOPs, are the allocated IOPS enforced by instance or by volume?
{: faq}

IOPS are enforced at the volume level.

## How are IOPS measured?
{: faq}

IOPS are measured based on a load profile of 16 KB blocks with random 50% read and 50% writes. Workloads that differ from this profile might experience lower performance.

## What are IOPS profiles?
{: faq}

IOPS profiles define IOPS/GB performance for volumes of various capacity. There are three predefined [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) you can select that offer guaranteed IOPS performance to meet your workload requirements. You can also define [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) and specify a range of IOPS for a volume size you choose. Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier.

## What are the maximum IOPS I can expect for my data volumes?
{: faq}

Maximum IOPS for data volumes vary based on volume size and the IOPS tier profile you selected. For example, the max IOPS for a general purpose volume up to 1 TB is 3,000 IOPS. For information, see [Tiered IOPs profiles](/docs/vpc?topic=vpc-block-storage-profiles#tiers). If you choose a [Custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom), also define a minimum and maximum range for a given volume size.

## What happens if I use a smaller block size when measuring performance?
{: faq}

Maximum IOPS can still be obtained when using smaller block sizes, however throughput will be lower. For example, a volume with 6000 IOPS would have the following throughput at various block sizes:

* 16 KB * 6000 IOPS == ~93.75 MB/sec
* 8 KB * 6000 IOPS == ~46.88 MB/sec
* 4 KB * 6000 IOPS == ~23.44 MB/sec

## Are there quota limits?

Certain limits apply.  For more information about quotas and limits for your {{site.data.keyword.cloud}} Virtual Private Cloud and the resources available within it, see [Quotas](/docs/vpc?topic=vpc-quotas#quotas).

## After creating a volume, can I increase its capacity later?
{: faq}

No, you cannot increase the capacity of a volume. We recommend that you estimate sufficient capacity for projected growth before you provision a block storage volume.

## Does the volume need to be pre-warmed to achieve expected throughput?
{: faq}

You do not have to pre-warm a volume. You can see the specified throughput immediately upon provisioning the volume.

## Can I create encrypted volumes?
{: faq}

In the early access release, all block storage volumes are encrypted by IBM-managed encryption.

## How secure is my data?
{: faq}

All block storage volumes are encrypted at rest with IBM-managed encryption. IBM-managed keys are generated and securely stored in a block storage vault backed by Consul, and then maintained by the system.

## What should I do about data backups for disaster recovery?
{: faq}

Block Storage for VPC secures your data across redundant fault zones in your region. We also encourage you to independently backup your data. You might also consider using [{{site.data.keyword.blockstoragefull}}](/docs/BlockStorage?topic=BlockStorage-getting-started), which provides disaster recovery features such as volume cloning, snapshots, and replication.

## How many volumes can I provision?
{: faq}

You can provision up to 1,000 block storage volumes per region.

## What strategy should I use to manage my volumes?
{: faq}

Determine the capacity you need based on anticipated growth. Volume size cannot be expanded after provisioning. However, you can create new volumes as needed. Also, if you have well-defined performance requirements, you might consider choosing [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) that provides a specific range of IOPS per volume size.

## How is the boot disk for a virtual server instance created and how does it relate to the image file?
{: faq}

The boot disk, also called a boot volume, is created when you provision a virtual server instance. The boot disk of an instance is a cloned image of the virtual machine image. The boot volume is deleted when you delete the instance to which it's attached.

## Do firewalls or security groups impact performance?
{: faq}

As best practice, it is recommended to run storage traffic on a VLAN that bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## When can I delete a block storage data volume?
{: faq}

You can delete a block storage data volume only when it's not attached to a virtual server instance. [Detach the volume](/docs/vpc?topic=vpc-managing-block-storage-volumes#detach) before deleting it. Boot volumes are detached and deleted when the instance is deleted.

## What happens to my data when I delete a block storage data volume?
{: faq}

When you delete a block storage data volume, all pointers to the data on that volume are removed and the data becomes completely inaccessible. If you later reprovision the physical storage to another account, a new set of pointers are assigned. There is no way for this new account to access any data that might have been on the physical storage; the new set of pointers shows all 0's. When new data is written to the volume, any inaccessible data is overwritten.

## What performance latency can I expect from my block storage?
{: faq}

Target latency within the storage is less than 1 millisecond. Block storage is connected to compute instances on a shared network, so the exact performance latency will depend on the network traffic within a given timeframe.

## Can I set up shared storage in a multizone cluster?
{: faq}

In the IBM Cloud, storage options are localized to an availability zone. We do not recommend managing shared storage across multiple zones.

Instead, use an IBM Cloud data classic service option such as {{site.data.keyword.cos_full}} or {{site.data.keyword.cloudantfull}} if you must share your data across multiple zones and regions.

## I have volumes on the Classic infrastructure. Can I port them to the VPC?
{: faq}

No.  The VPC provides access to new availability zones in multi-zone regions. Compute, network, and storage resources are specifically designed to function in the VPC.
