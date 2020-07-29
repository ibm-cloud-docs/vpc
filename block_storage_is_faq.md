---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-29"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance, IOPS, FAQ

subcollection: vpc

---
{:faq: data-hd-content-type='faq'}
{:codeblock: .codeblock}
{:screen: .screen}
{:support: data-reuse='support'}
{:external: target="_blank" .external}

# FAQs for block storage
{: #block-storage-vpc-faq}

The following questions often arise about the {{site.data.keyword.block_storage_is_short}} service offering including performance and security. If you have other questions you'd like to see answered here, provide feedback by using the **Open Issue** or **Edit Topic** links.
{:shortdesc}

## Offering questions
{: #block-storage-vpc-offering-questions}

### How are volumes created and attached to an instance?
{: faq}
{: #faq-block-storage-1}
{: support}

When you create a virtual server instance, you can [create a block storage volume](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi) that is attached to that instance. You can also [create stand-alone volumes](/docs/vpc?topic=vpc-creating-block-storage#create-standalone-vol) and later attach them to your instances.

### How many instances can share a provisioned block storage volume?
{: faq}
{: #faq-block-storage-2}

A block storage volume can be attached to only one instance at a time. Instances cannot share a volume.

### How many block storage secondary data volumes can be attached to an instance?
{: faq}
{: #faq-block-storage-3}

Instances with fewer than 4 cores can attach up to 4 block storage secondary volumes.

Instances with 4 or more cores can attach up to 12 block storage secondary volumes.

### How am I charged for usage?
{: faq}
{: #faq-block-storage-6}
{: support}

Block Storage for VPC is calculated hourly. The calculation is based on the total number of hours that the block storage volume exists on the account. It exists on the account until you delete the volume or you reach the end of a billing cycle, whichever comes first. For imore information, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).

### Are there quota limits?
{: faq}
{: #faq-block-storage-7}

There are quota limits for your block storage volumes based on the number of cores per instance. For more information about quotas and limits for your {{site.data.keyword.cloud}} Virtual Private Cloud and the resources available within it, see [Quotas](/docs/vpc?topic=vpc-quotas#quotas).

### After creating a data volume with specific capacity, can the capacity later be increased?
{: faq}
{: #faq-block-storage-8}
{: support}

For the current general availability release, you can't increase the capacity of a data volume (GBs) after provisioning. Estimate sufficient capacity for projected growth before you provision a block storage volume.

You might consider participating in the Beta release of larger-size and expandable volumes. The Beta lets you provision volumes up to 16 TB capacity. You can expand volume size as follows:

* Volumes under 250 GB can expand up to 250 GB.
* Volumes 250 GB and greater can expand up to 16 TB, depending on the volume profile.

For more information, see [Expanding block storage volume capacity (Beta)](/docs/vpc?topic=vpc-expanding-block-storage-volumes) and [Expanded capacity IOPs tiers (Beta)](/docs/vpc?topic=vpc-block-storage-profiles#tiers-beta).

### How many volumes can I provision on my account?
{: faq}
{: #faq-block-storage-12}

You can provision up to 300 block storage volumes per account in a region. You can request your quota to be increased by opening a [support case](https://cloud.ibm.com/unifiedsupport/cases/form){: external}.

### Can I set up shared storage in a multizone cluster?
{: faq}
{: #faq-block-storage-18}
{: support}

In the IBM Cloud, storage options are limited to an availability zone. Do not manage shared storage across multiple zones.

Instead, use an IBM Cloud data classic service option such as {{site.data.keyword.cos_full}} or {{site.data.keyword.cloudantfull}} if you must share your data across multiple zones and regions.

### I have volumes on the Classic infrastructure. Can I port them to the VPC?
{: faq}
{: #faq-block-storage-19}
{: support}

No. The VPC provides access to new availability zones in multi-zone regions. Compute, network, and storage resources are designed to function in the VPC.


## Volume management questions
{: #block-storage-vpc-volume-questions}

### How is the boot disk created for an instance and how does it relate to the virtual machine image?
{: faq}
{: #faq-block-storage-14}

The boot disk, also called a boot volume, is created when you provision a virtual server instance. The boot disk of an instance is a cloned image of the virtual machine image. The boot volume is deleted when you delete the instance to which it is attached.

### What strategy can I use to manage {{site.data.keyword.block_storage_is_short}} volumes?
{: faq}
{: #faq-block-storage-13}

Determine the capacity that you need based on anticipated growth. Volume size cannot be expanded after provisioning. However, you can create new volumes as needed. Also, if you have well-defined performance requirements, you might consider choosing a [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile that provides a specific range of IOPS per volume size.

### When can I delete a block storage data volume?
{: faq}
{: #faq-block-storage-15}

You can delete a block storage data volume only when it isn't attached to a virtual server instance. [Detach the volume](/docs/vpc?topic=vpc-managing-block-storage#detach) before you delete it. Boot volumes are detached and deleted when the instance is deleted.

### What happens to my data when I delete a block storage data volume?
{: faq}
{: #faq-block-storage-16}

When you delete a block storage data volume, all pointers to the data on that volume are removed and the data becomes inaccessible. If you later reprovision the physical storage to another account, a new set of pointers is assigned. The new account can't access any data that was on the physical storage because the pointers have been deleted. When new data is written to the volume, any inaccessible data is overwritten.

### What rules apply to volume names and can I rename a volume later on?
{: faq}
{: #faq-block-storage-9}

Valid volume names can include a combination of lowercase alphanumeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter and be unique across the entire VPC infrastructure.

You can change the name of an existing volume using the UI. See [this information](/docs/vpc?topic=vpc-managing-block-storage#rename) for details.

### Does the volume need to be pre-warmed to achieve expected throughput?
{: faq}
{: #faq-block-storage-10}

You do not have to pre-warm a volume. You can see the specified throughput immediately upon provisioning the volume.

### What can I do about data backups for disaster recovery?
{: faq}
{: #faq-block-storage-11}

Block Storage for VPC secures your data across redundant fault zones in your region. You are responsible for independently backing up your data. 

You might also consider using [{{site.data.keyword.blockstoragefull}} - Classic](/docs/BlockStorage?topic=BlockStorage-getting-started), which provides disaster recovery features such as volume cloning, snapshots, and replication. In particular, see the topics under **Replication and Disaster Recovery**.


## Performance questions
{: #block-storage-vpc-performance-questions}

### What are IOPS and how do they relate to my block storage volume performance?
{: faq}
{: #faq-block-storage-4a}

Input/Output Operations Per Second (IOPS) are used to measure the performance of your block storage volumes. A number of variables impact IOPS values, such as the balance of read/write operations, queue depth, and data block sizes. In general, the higher the IOPS of your block storage volumes, the better the performance. For information on expected IOPS for block storage profiles, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles). For information about how block size affects performance, see [this topic](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

### Are the allocated IOPS enforced by instance or by volume?
{: faq}
{: #faq-block-storage-4}

IOPS is enforced at the volume level.

### What are IOPS profiles and how do they affect volume performance?
{: faq}
{: #faq-block-storage-5}
{: support}

IOPS profiles define IOPS/GB performance for volumes of various capacities. There are three predefined [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) that you can select that offer reliable IOPS performance for your workload requirements. You can also define [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) and specify a range of IOPS for a volume size you choose. Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier. If you choose a custom IOPS profile, also define a minimum and maximum range for the volume size.

Maximum IOPS for data volumes varies based on volume size and the type of profile you select. For example, the max IOPS for a general-purpose volume up to 1 TB is 3,000 IOPS.

IOPS is measured based on a load profile of 16 KB blocks with random 50% read and 50% writes. Workloads that differ from this profile might experience reduced performance. If you use a smaller block size, maximum IOPS can be obtained but throughput is less. For information, see
[How block size affects performance](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

### What performance latency can I expect from {{site.data.keyword.block_storage_is_short}}?
{: faq}
{: #faq-block-storage-17}

Target latency within the storage is less than 1 millisecond. Block storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic within a specific timeframe.


## Data security and encryption questions
{: #block-storage-vpc-security-questions}

Customer-managed encryption is a beta feature that is available for evaluation and testing purposes. 
{:note}

### How secure is data in a {{site.data.keyword.block_storage_is_short}} volume?
{: faq}
{: #faq-block-storage-20}
{: support}

All block storage volumes are encrypted at rest with IBM-managed encryption. IBM-managed keys are generated and securely stored in a block storage vault that is backed by Consul and maintained by IBM Cloud operations. 

For more security, you can protect your data using your own customer root keys (CRKs). You import your root keys to, or create them in, a supported key management service (KMS). Your root keys are safely managed by the supported KMS, either {{site.data.keyword.keymanagementserviceshort}} (FIPS 140-2 Level 3 compliance) or {{site.data.keyword.hscrypto}}, which offer the highest level of security (FIPS 140-2 Level 4 compliance). Your key material is protected in transit and at rest. 

For information, see [Supported key management services for customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok). To learn how to set up customer-managed encryption, see 
[Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

You control access to your root keys stored in KMS instances within IBM Cloud by using IBM Access Management (IAM). You grant access to the IBM Block Storage Service to use your keys. You can also revoke access at any time, for example, if you suspect your keys might have been compromised. You can also disable or delete a root key, or take temporarily revoke access to the key's associated data on the cloud. For more information, see [Managing root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-manage-root-keys).

### What are the advantages of using customer managed encryption over provider managed encryption?
{: faq}
{: #faq-block-storage-21}

Customer-managed encryption lets you encrypt your block storage volumes using your own root keys. You have complete control over your data security and grant IBM access to use your root keys.  For more information, see [Advantages of customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#byok-advantages).

### What encryption technology is used for customer-managed encryption?
{: faq}
{: #faq-block-storage-22}

Virtual disk images for VPC Generation 2 use QEMU Copy On Write Version 2 (QCOW2) file format. (VPC Generation 1 uses VHD file format.) LUKS encryption format secures the QCOW2 format files. IBM currently uses the ASE-256 cipher suite and XTS cipher mode options with LUKS. This combination provides you a much greater level of security than AES-CBC and provides key replacement options in case your keys are compromised.

### What are master encryption keys and how are they assigned to my block storage volumes?  
{: faq}
{: #faq-block-storage-23}

Each volume is assigned a unique master encryption key generated by the instance's host hypervisor. The master key for each block storage volume is encrypted with a unique KMS-generated LUKS passphrase, which is then encrypted by your customer root key (CRK) and stored in the KMS. Passphrases are AES-256 cipher keys, which means they are 32 bytes long and not limited to printable characters. You can view the cloud resource name (CRN) for the CRK used to encrypt a volume. However, the CRK, LUKS passphrase, and the volume's master encryption key are never exposed.

### I use customer-managed encryption for my volumes. What happens when I delete my root key?
{: faq}
{: #faq-block-storage-24}

When you delete your root key or suspend it in your key management service, your workloads remain running in your virtual server instance and volumes remain encrypted.  However, if you power down the VM and then power it back on, any instances with encrypted boot volumes will not start. If your boot volume was encrypted with provider-managed encryption, the instance will start but all secondary volumes encrypted by root key you deleted will be inaccessible.  All volume attachments will fail during the boot process. You can cancel the deletion if your key is in a suspended state. Look at the list of resources for the status of the key.

### What should I do if my root key is compromised?
{: faq}
{: #faq-block-storage-25}

Independently back up your data. Then, delete the compromised root key and power down the instance with volumes encrypted with that key. 

### Am I charged for using customer-managed encryption?
{: faq}
{: #faq-block-storage-27}

There is no extra charge for creating volumes with customer-managed encryption.  However, there is a charge for setting up a {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance to import, create, and manage your root keys. Contact your IBM customer service representative for details.

### What's the difference between using Key Protect as my KMS compared to HPCS?  When would I use one over the other?
{: faq}
{: #faq-block-storage-28}

Both key management systems provide you complete control over your data, managed by your root keys. {{site.data.keyword.keymanagementserviceshort}} is a multi-tenant KMS that lets you import or create your root keys and securely manage them. {{site.data.keyword.hscrypto}} is a single-tenant KMS and hardware security module (HSM) that's controlled by you, which offers the highest level of security. For more information and links to documentation about these key managment services, see [Supported key management services for customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok).

### Can I convert my volume from provider-managed encryption to customer-managed encryption? 
{: faq}
{: #faq-block-storage-29}

No, after you provision a volume and specify the encryption type, you can't change it.
