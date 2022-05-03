---

copyright:
  years: 2019, 2022
lastupdated: "2022-05-02"

keywords:

subcollection: vpc

---
{:faq: data-hd-content-type='faq'}
{:codeblock: .codeblock}
{:screen: .screen}
{:support: data-reuse='support'}
{:external: target="_blank" .external}
{:note: .note}

# FAQs for block storage
{: #block-storage-vpc-faq}

The following questions often arise about the {{site.data.keyword.block_storage_is_short}} service offering including performance and security. If you have other questions you'd like to see answered here, provide feedback by using the **Open Issue** or **Edit Topic** links.
{: shortdesc}

## Offering questions
{: #block-storage-vpc-offering-questions}

### How does Block Storage for VPC prevent a single point of failure? What mechanism assures data durability?
{: faq}
{: #faq-block-storage-durability}

{{site.data.keyword.block_storage_is_short}} volume data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

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

You can create 12 block storage secondary volumes per instance, plus the boot volume.

### How am I charged for usage?
{: faq}
{: #faq-block-storage-6}
{: support}

Cost for block storage is calculated based on GB capacity stored per month, unless the duration is less than one month. The volume exists on the account until you delete the volume or you reach the end of a billing cycle, whichever comes first. 

Pricing is also affected when you [expand volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) or [adjust IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) by specifying a different IOPS profile. For example, expanding volume capacity increases costs while changing an IOPS profile from a 5 IOPS/GB tier to a 3 IOPS/GB tier decreases the monthly and hourly rate.  Billing for an updated volume is automatically updated to add the prorated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

Pricing for block storage volumes is also set by region. For more information, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).

### Are there limits on the number of volumes I can create?
{: faq}
{: #faq-block-storage-7}

You can create up to 300 total block storage volumes (data and boot) per account in a region. To increase this [quota](/docs/vpc?topic=vpc-quotas#block-storage-quotas), open a [support case](/docs/vpc?topic=vpc-getting-help) and specifying in which zone you need more volumes.

### After creating a data volume with specific capacity, can the capacity later be increased?
{: faq}
{: #faq-block-storage-8}
{: support}

You can increase the capacity of data volumes attached to a virtual server instance. You can indicate capacity in GB increments up to 16,000 GB capacity, depending on your volume profile. For more information, see [Increasing block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

### Can I increase capacity of a boot volume?
{: faq}
{: #faq-block-storge-rbv}

Boot volume capacity can be increased during instance provisioning or later, by directly modifying the boot volume.
This feature applies to instances created from stock or custom images. You can also specify a larger boot volume capacity when creating an instance template. The boot volume can't be unattached from an instance (that is, stored as standalone data volume). For more information and for UI, CLI, or API procedures, see [Increasing boot volume volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes).

### Can I change boot volume capacity for an existing instance?
{: faq}
{: #faq-block-storge-rbv-instance}

Yes, boot volume capacity can be increased for an existing instance. However, you increase capacity on the storage side. That is, by modifying the boot volume details. For example, in the UI, you'd select a boot volume from the list of block storage volumes and then resize the volume from the volume details page. For more information, see [Increase boot volume capacity from the list of block storage volumes in the UI](/docs/vpc?topic=vpc-resize-boot-volumes&interface=ui#resize-boot-vol-list-ui). You can also use the [CLI](/docs/vpc?topic=vpc-resize-boot-volumes&interface=cli#expand-existing-boot-vol-cli) and For the [API](/docs/vpc?topic=vpc-resize-boot-volumes&interface=api#expand-existing-boot-vol-api).

### How many volumes can I provision on my account?
{: faq}
{: #faq-block-storage-12}

You can provision up to 300 block storage volumes per account in a region. You can request your quota to be increased by opening a [support case](https://cloud.ibm.com/unifiedsupport/cases/form){: external} and specifying in which region you need more volumes. For more information about preparing a support ticket when you're ordering block storage volumes or requesting an increase to your volume or capacity limits, see [Managing volume count and capacity limits](/docs/vpc?topic=vpc-manage-storage-limit).

### Can I set up shared storage in a multizone cluster?
{: faq}
{: #faq-block-storage-18}
{: support}

In the IBM Cloud, storage options are limited to an availability zone. Do not manage shared storage across multiple zones.

Instead, use an IBM Cloud data classic service options outside a VPC such as {{site.data.keyword.cos_full}} or {{site.data.keyword.cloudantfull}} if you must share your data across multiple zones and regions. 

### I have volumes on the Classic infrastructure. Can I port them to the VPC?
{: faq}
{: #faq-block-storage-19}
{: support}

No. The VPC provides access to new availability zones in multi-zone regions. Compute, network, and storage resources are designed to function in the VPC.

### What is _image from volume_ and how does it relate to block storage volumes?
{: faq}
{: #faq-block-storage-ifv}

Image from volume lets you create a custom image directly from a block storage boot volume. You then use the custom image to provision new virtual server instances. For more information, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc).


## Volume management questions
{: #block-storage-vpc-volume-questions}

### How is the boot disk created for an instance and how does it relate to the virtual machine image?
{: faq}
{: #faq-block-storage-14}

The boot disk, also called a boot volume, is created when you provision a virtual server instance. The boot disk of an instance is a cloned image of the virtual machine image. For stock images, the boot volume capacity is 100 GB. If you are importing a custom image, the boot volume capacity can be 10 GB to 250GB, depending on what the image requires. Images smaller than 10 GB are rounded up to 10 GB. The boot volume is deleted when you delete the instance to which it is attached.

### When can I delete a block storage data volume?
{: faq}
{: #faq-block-storage-15}

You can delete a block storage data volume only when it isn't attached to a virtual server instance. [Detach the volume](/docs/vpc?topic=vpc-managing-block-storage#detach) before you delete it. Boot volumes are detached and deleted when the instance is deleted.

### What happens to my data when I delete a block storage data volume?
{: faq}
{: #faq-block-storage-16}

When you delete a block storage volume, your data immediately becomes inaccessible. All pointers to the data on that volume are removed. The inaccessible data is eventually overwritten as new data is written to the data block. IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten. For more information, see [Block storage data eradication](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication).

### I have compliance requirements. What can I do to ensure my data is inaccessible?
{: faq}
{: #faq-block-storage-nist}

IBM guarantees that your data is completely inaccessible on the physical disk and is eventually [eradicated](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication). If you have additional compliance requirements such as NIST 800-88 Guidelines for Media Sanitization, you must perform data sanitation procedures before you delete your volumes. For information, see the [NIST 800-88 Guidelines for Media Sanitation](https://csrc.nist.gov/publications/detail/sp/800-88/rev-1/final){: external}.

### What rules apply to volume names and can I rename a volume later on?
{: faq}
{: #faq-block-storage-9}

Valid volume names can include a combination of lowercase alphanumeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter and be unique across the entire VPC infrastructure.

You can change the name of an existing volume in the UI. See [this information](/docs/vpc?topic=vpc-managing-block-storage#rename) for details.

### Does the volume need to be pre-warmed to achieve expected throughput?
{: faq}
{: #faq-block-storage-10}

You do not have to pre-warm a volume. You can see the specified throughput immediately upon provisioning the volume.

### What is a block storage snapshot?
{: faq}
{: #faq-block-storage-snapshot}

Snapshots are a point-in-time copy of your block storage boot or data volume that you manually create. The first snapshot is a full backup of the volume. Subsequent snapshots of the same volume record only the changes since the last snapshot. For more information, see [About Block Storage Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about).

### What is a backup snapshot?
{: faq}
{: #faq-block-storage-backup-snapshot}

Backup snapshots, simply called "backups", are automatically-created snapshots using the Backup for VPC service. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

## Performance questions
{: #block-storage-vpc-performance-questions}

### What are IOPS and how do they relate to my block storage volume performance?
{: faq}
{: #faq-block-storage-4a}

Input/output operations per second (IOPS) are used to measure the performance of your block storage volumes. A number of variables impact IOPS values, such as the balance of read/write operations, queue depth, and data block sizes. In general, the higher the IOPS of your block storage volumes, the better the performance. For information on expected IOPS for block storage profiles, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles). For information about how block size affects performance, see [this topic](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

### Are the allocated IOPS enforced by instance or by volume?
{: faq}
{: #faq-block-storage-4}

IOPS is enforced at the volume level.

### What are IOPS profiles and how do they affect volume performance?
{: faq}
{: #faq-block-storage-5}
{: support}

IOPS profiles define IOPS/GB performance for volumes of various capacities. There are three predefined [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) that you can select that offer reliable IOPS performance for your workload requirements. You can also define [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) and specify a range of IOPS for a volume size you choose. Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier. If you choose a custom IOPS profile, also define a minimum and maximum range for the volume size.

Maximum IOPS for data volumes varies based on volume size and the type of profile you select.

IOPS is measured based on a load profile of 16 KB blocks with random 50% read and 50% writes. Workloads that differ from this profile might experience reduced performance. If you use a smaller block size, maximum IOPS can be obtained but throughput is less. For information, see 
[How block size affects performance](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

### What typical network performance might I expect between my compute instances and the block storage service?
{: faq}
{: #faq-block-storage-17}

Block storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic within a specific timeframe. Target latency for a 16KB block size volume is under 1 millisecond for random reads and under 2 milliseconds for writes. For more performance metrics you can expect for your storage volumes and compute instances, see [Storage-compute performance metrics](/docs/vpc?topic=vpc-capacity-performance#storage-performance-metrics). 

### What mechanisms are used to avoid data storage contention?
{: faq}
{: #faq-block-storage-17a}

Data storage contention is a common issue when multiple instances compete for access to the same block storage volume. {{site.data.keyword.block_storage_is_short}} uses rate limiting at the hypervisor for optimal bandwidth between the hypervisor and block storage service. As a result, latency is guaranteed to be less than 1 millisecond for random reads and under 2 milliseconds for writes for a typical 16K block size. Latency outside these metrics might indicate a problem on the client side. For additional typical performance benchmarks, see [Storage-compute performance metrics](/docs/vpc?topic=vpc-capacity-performance#storage-performance-metrics).


## Data security and encryption questions
{: #block-storage-vpc-security-questions}

### How secure is data in a {{site.data.keyword.block_storage_is_short}} volume?
{: faq}
{: #faq-block-storage-20}
{: support}

All block storage volumes are encrypted at rest with IBM-managed encryption. IBM-managed keys are generated and securely stored in a block storage vault that is backed by Consul and maintained by IBM Cloud operations. 

For more security, you can protect your data by using your own customer root keys (CRKs). You import your root keys to, or create them in, a supported key management service (KMS). Your root keys are safely managed by the supported KMS, either {{site.data.keyword.keymanagementserviceshort}} (FIPS 140-2 Level 3 compliance) or {{site.data.keyword.hscrypto}}, which offer the highest level of security (FIPS 140-2 Level 4 compliance). Your key material is protected in transit and at rest. 

For more information, see [Supported key management services for customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok). To learn how to set up customer-managed encryption, see 
[Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

You control access to your root keys stored in KMS instances within IBM Cloud by using IBM Access Management (IAM). You grant access to the IBM Block Storage Service to use your keys. You can also revoke access at any time, for example, if you suspect your keys might are compromised. You can also disable or delete a root key, or temporarily revoke access to the key's associated data on the cloud. For more information, see [Managing root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-manage-root-keys).

### What are the advantages of using customer-managed encryption over provider-managed encryption?
{: faq}
{: #faq-block-storage-21}

Customer-managed encryption encrypts your block storage volumes by using your own root keys. You have complete control over your data security and grant IBM access to use your root keys.  For more information, see [Advantages of customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#byok-advantages).

### What encryption technology is used for customer-managed encryption?
{: faq}
{: #faq-block-storage-22}

Virtual disk images for VPC use QEMU Copy On Write Version 2 (QCOW2) file format. LUKS encryption format secures the QCOW2 format files. IBM currently uses the AES-256 cipher suite and XTS cipher mode options with LUKS. This combination provides you a much greater level of security than AES-CBC, along with better management of passphrases for key rotation, and provides key replacement options in case your keys are compromised.

### What are master encryption keys and how are they assigned to my block storage volumes?  
{: faq}
{: #faq-block-storage-23}

Each volume is assigned a unique master encryption key, called a data encryption key or DEK, that is generated by the instance's host hypervisor. The master key for each block storage volume is encrypted with a unique KMS-generated LUKS passphrase, which is then encrypted by your customer root key (CRK) and stored in the KMS. Passphrases are AES-256 cipher keys, which means they are 32 bytes long and not limited to printable characters. You can view the cloud resource name (CRN) for the CRK used to encrypt a volume. However, the CRK, LUKS passphrase, and the volume's master encryption key are never exposed. For more information about all the keys IBM VPC uses to secure your data, see [IBM's encryption technology - How your data is secured](/docs/vpc?topic=vpc-vpc-encryption-about#byok-technologies).

### I use customer-managed encryption for my volumes. What happens when I disable or delete my root key?
{: faq}
{: #faq-block-storage-24}

These actions are two separate actions. Disabling a root key in your KMS suspends its encryption and decryption operations, placing the key in a suspended state. Workloads remain running in virtual server instances and boot volumes remain encrypted. Data volumes remain attached. However, if you power down the VM and then power it back on, any instances with encrypted boot volumes do not start. You can enable a root key in a suspended state and resume normal operations. For more information, see [Disabling root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys).

Deleting a root key has greater consequences. Deleting a root key purges usage of the key for all resources in the VPC. By default, the KMS prevents you from deleting a root key that's actively protecting a resource. However, you can still force the deletion of a root key. You have limited time to [restore a deleted root key](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-restore-root-key) that you imported to the KMS. For more information, see [Deleting root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys).

### Can I remove IAM authorization from Cloud Block Storage to the KMS and still delete my block storage volumes with customer-managed encryption?
{: faq}
{: #faq_block_storage_remove_iam}

If you remove IAM authorization before deleting your BYOK volume (or image), the delete operation completes but the root keys protecting these resources will not deregister in the KMS instance. This means the root key remains registered for a resource that doesn't exist. Always delete a BYOK resource before you remove IAM authorization. For more information about safely removing service authorization, see [Removing service authorization to a root key](/docs/vpc?topic=vpc-vpc-encryption-managing#instance-byok-inaccessible-data).

### What should I do if my root key is compromised?
{: faq}
{: #faq-block-storage-25}

Independently back up your data. Then, delete the compromised root key and power down the instance with volumes encrypted with that key. 

Also, consider setting up a key rotation policy that automatically rotates your keys based on a schedule. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

### What is key rotation?
{: faq}
{: #faq-block-storage-25a}

For {{site.data.keyword.vpc_short}} resources such as block storage volumes that are protected by your customer root key (CRK), you can rotate the root keys for additional security. When you rotate a root key by a schedule or on demand, the original key material is replaced. The old key remains active to decrypt existing volumes but can't be used to encrypt new volumes. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

### How does key rotation work?
{: faq}
{: #faq-block-storage-25b}

Customer-managed encrypted resources such as block storage volumes use your root key (CRK) as the root-of-trust key that encrypts a LUKS passphrase that encrypts a master key protecting the volume. You can import your CRK to a key management service (KMS) instance or instruct the KMS to generate one for you. Root keys are rotated in your KMS instance.

When you rotate a root key, a new version of the key is created by generating or importing new cryptographic key material. The old root key is retired, which means its key material remains available for decrypting existing volumes, but not available for encrypting new ones. New resources are protected by the latest key. For more information, see [How key rotation works](https://test.cloud.ibm.com/docs/vpc?topic=vpc-vpc-key-rotation#vpc-key-rotation-function).

### Am I charged for using customer-managed encryption?
{: faq}
{: #faq-block-storage-27}

You are not charged extra for creating volumes with customer-managed encryption.  However, there is a charge for setting up a {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance to import, create, and manage your root keys. Contact your IBM customer service representative for details.

### What's the difference between using Key Protect as my KMS compared to HPCS?  When would I use one over the other?
{: faq}
{: #faq-block-storage-28}

Both key management systems provide you complete control over your data, managed by your root keys. {{site.data.keyword.keymanagementserviceshort}} is a multi-tenant KMS that where you can import or create your root keys and securely manage them. {{site.data.keyword.hscrypto}} is a single-tenant KMS and hardware security module (HSM) that is controlled by you, which offers the highest level of security. For more information and links to documentation about these key management services, see [Supported key management services for customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok).

### Can I convert my volume from provider-managed encryption to customer-managed encryption? 
{: faq}
{: #faq-block-storage-29}

No, after you provision a volume and specify the encryption type, you can't change it.


