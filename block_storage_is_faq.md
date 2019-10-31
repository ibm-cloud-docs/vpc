---

copyright:
  years: 2019
lastupdated: "2019-10-30"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance, IOPS, FAQ

subcollection: vpc

---
{:faq: data-hd-content-type='faq'}
{:codeblock: .codeblock}
{:screen: .screen}
{:support: data-reuse='support'}

# FAQs for block storage
{: #block-storage-vpc-faq}

This topic answers many of your questions for creating and managing your block storage volumes. If you have other questions you'd like to see addressed here, provide feedback by using the **Open Issue** or **Edit Topic** links.
{:shortdesc}

## How do I create and attach volumes to my instances?
{: faq}
{: #faq-block-storage-0}
{: support}

When you create a virtual server instance, you can [create a block storage volume](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi) that will be attached to that instance. You can also [create stand-alone volumes](/docs/vpc?topic=vpc-creating-block-storage#create-standalone-vol) and later attach them to your instances.

## How many instances can share a provisioned block storage volume?
{: faq}
{: #faq-block-storage-1}

A block storage volume can be attached to only one instance at a time. Instances cannot share the same volume.

## How many block storage secondary (data) volumes can be attached to an instance?
{: faq}
{: #faq-block-storage-2}

Instances with less than 4 vCPUs can attach up to 4 block storage secondary volumes.

Instances with 4 vCPUs or more can attach up to 12 block storage secondary volumes.

## When provisioning IOPS, are the allocated IOPS enforced by instance or by volume?
{: faq}
{: #faq-block-storage-3}

IOPS is enforced at the volume level.

## What are IOPS profiles and how do they affect volume performance?
{: faq}
{: #faq-block-storage-4}
{: support}

IOPS profiles define IOPS/GB performance for volumes of various capacities. There are three predefined [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) you can select that offer guaranteed IOPS performance to meet your workload requirements. You can also define [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) and specify a range of IOPS for a volume size you choose. Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier. If you choose a custom IOPS profile, also define a minimum and maximum range for the volume size.

Maximum IOPS for data volumes vary based on volume size and the type of profile you select. For example, the max IOPS for a general purpose volume up to 1 TB is 3,000 IOPS.

IOPS are measured based on a load profile of 16 KB blocks with random 50% read and 50% writes. Workloads that differ from this profile might experience reduced performance. If you use a smaller block size, maximum IOPS can be obtained but throughput will be less. For information, see 
[How block size affects performance](/docs/vpc?topic=vpc-block-storage-capacity-performance#how-block-size-affects-performance).

## How will I be charged for usage?
{: faq}
{: #faq-block-storage-5}
{: support}

Block Storage for VPC is calculated hourly, based on the total number of hours that the block storage volume exists on the account, until you delete the volume or you reach the end of a billing cycle, whichever comes first. For information, see [Pricing](/docs/vpc?topic=vpc-pricing-for-vpc#pricing-for-block-storage-for-vpc).

## Are there quota limits?
{: faq}
{: #faq-block-storage-6}
For information about quotas and limits for your {{site.data.keyword.cloud}} Virtual Private Cloud and the resources available within it, see [Quotas](/docs/vpc?topic=vpc-quotas#quotas).

## After I create a volume, can I increase its capacity later?
{: faq}
{: #faq-block-storage-7}
{: support}

No, you cannot increase the capacity of a volume. Estimate sufficient capacity for projected growth before you provision a block storage volume.

## Does the volume need to be pre-warmed to achieve expected throughput?
{: faq}
{: #faq-block-storage-8}

You do not have to pre-warm a volume. You can see the specified throughput immediately upon provisioning the volume.

## Can I create encrypted volumes?
{: faq}
{: #faq-block-storage-9}

In this release, all block storage volumes are encrypted by IBM-managed encryption.

## How secure is my data?
{: faq}
{: #faq-block-storage-10}
{: support}

All block storage volumes are encrypted at rest with IBM-managed encryption. IBM-managed keys are generated and securely stored in a block storage vault backed by Consul and then maintained by the system.

For additional security, when you delete a volume, all pointers to the data on that volume are removed and the data becomes completely inaccessible.

## What can I do about data backups for disaster recovery?
{: faq}
{: #faq-block-storage-11}

Block Storage for VPC secures your data across redundant fault zones in your region. You are encouraged to independently backup your data. You might also consider using [{{site.data.keyword.blockstoragefull}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-getting-started), which provides disaster recovery features such as volume cloning, snapshots, and replication.

## How many volumes can I provision?
{: faq}
{: #faq-block-storage-12}

You can provision up to 750 block storage volumes per account in a region.

## What strategy can I use to manage my volumes?
{: faq}
{: #faq-block-storage-13}

Determine the capacity that you need based on anticipated growth. Volume size cannot be expanded after provisioning. However, you can create new volumes as needed. Also, if you have well-defined performance requirements, you might consider choosing [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) that provides a specific range of IOPS per volume size.

## How is the boot disk for a virtual server instance created and how does it relate to the image file?
{: faq}
{: #faq-block-storage-14}

The boot disk, also called a boot volume, is created when you provision a virtual server instance. The boot disk of an instance is a cloned image of the virtual machine image. The boot volume is deleted when you delete the instance to which it is attached.

## When can I delete a block storage data volume?
{: faq}
{: #faq-block-storage-15}

You can delete a block storage data volume only when it isn't attached to a virtual server instance. [Detach the volume](/docs/vpc?topic=vpc-managing-block-storage#detach) before you delete it. Boot volumes are detached and deleted when the instance is deleted.

## What happens to my data when I delete a block storage data volume?
{: faq}
{: #faq-block-storage-16}

When you delete a block storage data volume, all pointers to the data on that volume are removed and the data becomes inaccessible. If you later reprovision the physical storage to another account, a new set of pointers is assigned. There is no way for this new account to access any data that was on the physical storage because the new set of pointers shows all 0's. When new data is written to the volume, any inaccessible data is overwritten.

## What performance latency can I expect from my block storage?
{: faq}
{: #faq-block-storage-17}

Target latency within the storage is less than 1 millisecond. Block storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic within a specific timeframe.

## Can I set up shared storage in a multizone cluster?
{: faq}
{: #faq-block-storage-18}
{: support}

In the IBM Cloud, storage options are limited to an availability zone. Do not manage shared storage across multiple zones.

Instead, use an IBM Cloud data classic service option such as {{site.data.keyword.cos_full}} or {{site.data.keyword.cloudantfull}} if you must share your data across multiple zones and regions.

## I have volumes on the Classic infrastructure. Can I port them to the VPC?
{: faq}
{: #faq-block-storage-19}
{: support}

No.  The VPC provides access to new availability zones in multi-zone regions. Compute, network, and storage resources are designed to function in the VPC.
