---

copyright:
  years: 2019, 2020
lastupdated: "2020-05-28"

keywords: block storage, IBM Cloud Block Storage for VPC, boot volume, data volume, volume, data storage, IOPS, HPCS, Key Protect

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

{{site.data.keyword.block_storage_is_short}} provides hypervisor-mounted, high-performance data storage for your virtual server instances (instances) that you can provision within an {{site.data.keyword.vpc_full}} (VPC). The VPC infrastructure provides rapid scaling across multiple regions and zones, and extra performance and security. For more information about {{site.data.keyword.vpc_short}}, see [About Virtual Private Cloud](/docs/vpc?topic=vpc-about-vpc).
{:shortdesc}

{{site.data.keyword.block_storage_is_short}} provides primary boot volumes and secondary data volumes. Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning as well, or as stand-alone volumes that you can later attach to an instance. To protect your data, you can use your own encryption keys or choose IBM-managed encryption. 

You can choose [IOPS tier profiles](/docs/vpc?topic=vpc-block-storage-profiles#tiers) to specify a pre-defined level of performance for your volumes. Or, you can choose a [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) and define your own volume capacity and IOPS level.

## Block storage for VPC volumes
{: #block-storage-vpc-volumes}

{{site.data.keyword.block_storage_is_short}} offers block-level volumes that can be attached to an instance as either a boot volume or as a data volume. You can configure up to 300 block storage volumes per account in a region. You can attach several block storage data volumes to a single instance for extra capacity. The number of volumes you can attach to an instance depends on how many vCPUs that instance contains. For more information, see [Volume attachment limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits).

### Boot volumes
{: #block-storage-vpc-boot-volumes}

When you create an instance, a 100 GB boot volume is created and attached to the instance by default. The boot volume has a maximum IOPS of 3,000 IOPS.

By default, boot volumes are encrypted by IBM-managed encryption. Optionally, you can use your own encryption keys by choosing customer-managed encryption during instance creation (see [Customer-managed encryption](#about-vpc-customer-managed-encryption)). You cannot detach and delete the boot volume. Boot volumes are always deleted when you when you delete the instance.

### Data volumes
{: #secondary-data-volumes}

Block storage data volumes are secondary volumes with total capacity range of 10 GB to 2000 GB. Maximum IOPS for data volumes varies based on volume size and the IOPS tier profile that you selected. For example, the max IOPS for a 5 IOPS/GB volume up to 2 TBs is 10,000 IOPS. For more information, see
[Tiered IOPs profiles](/docs/vpc?topic=vpc-block-storage-profiles#tiers).

You create data volumes as stand-alone volumes or when you provision an instance. Stand-alone volumes exist in an unattached state until you attach the volume to an instance. When you create a data volume as part of instance provisioning, the volume is automatically attached to the instance.

Block storage data volumes can be attached to any available instance within your region, based on your customer account and permissions, and within [certain limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits). These volumes are detached by default when the instance is deleted. Detaching by default allows your data to persist beyond the virtual server instance lifecycle. It removes only the volume's association with the instance. You can delete data volumes manually after they are detached. Also, when you create data volumes, you can specify that they be [automatically deleted](/docs/vpc?topic=vpc-managing-block-storage#auto-delete) when the instance is deleted.

Data volumes are encrypted by default with IBM-managed encryption. You can also encrypt data volumes using [your own encryption keys](#about-vpc-customer-managed-encryption).

## Block Storage encryption
{: #vpc-storage-encryption}

{{site.data.keyword.cloud_notm}} takes the need for security seriously and understands the importance of being able to encrypt data to keep it safe. When you create a standalone data volume or create a data volume as part of instance creation, you can choose to protect your data by using your own master encryption keys, or by using IBM provider-managed encryption. Provider-managed encryption is the default encryption type for all volumes. After you set up ecryption for a volume, you can't change it.

### Provider-managed encryption
{: #about-vpc-provider-managed-encryption}

By default, all boot and data volumes are encrypted with IBM provider-managed encryption for data-at-rest. There is no additional cost for this service or impact on performance. Provider-managed encryption uses the following industry standard protocols:

* AES-256 encryption
* Keys are managed by {{site.data.keyword.cloud_notm}} operations
* Storage is validated for Federal Information Processing Standard (FIPS) Publication 140-2, Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA). Storage is also validated for Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386), and EU Data Protection Directive 95/46/EC compliance.

### Customer-managed encryption (Beta)
{: #about-vpc-customer-managed-encryption}

Customer-managed encryption, also called "bring your own key" (BYOK), lets you encrypt your block storage volumes with your own customer root keys (CRKs). Customer-managed encryption protects you data while in transit and while at rest. For added security, you can securely import of your CRKs by using import tokens. 

Customer-managed encryption is a beta feature that is available for evaluation and testing purposes. 
{:note}

#### Advantages
{: #byok-advantages}
Customer-managed encryption gives you the following advantages over provider-managed encryption:

* Encryption keys are controlled by you. You grant IBM access to use your root keys to encrypt your data.
* Encryption is handled by IBM's host/hypervisor technology for your instances. This feature provides greater level of security over solutions that use storage node encryption technology only. 
* Data is encrypted with your root key at all times outside of the host/hypervisor. This means your data is protected while in motion between the host system and IBM's storage systems, and while at rest in IBM's storage system.
* You choose the key management service (KMS) you want to use. You can select {{site.data.keyword.keymanagementserviceshort}}, a public multi-tenant KMS that is FIPS 140-2 L3 compliant, or the more secure {{site.data.keyword.hscrypto}}, which is FIPS 140-2 L4 compliant.
* Customer-managed encryption provides better audit records for key usage. Your key access is logged in the Activity Tracker with LogDNA service. The Activity Tracker lets you track interactions with the VPC.

#### How it works
{: #byok-use}

For data volumes, you specify customer-managed encryption when creating the volume. For boot volumes, you can edit the boot volume properties during instance creation and specify customer-managed encryption. For procedures, see [Creating block storage volumes with customer managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

When you use customer-managed encryption, you import your root key to a key management service (KMS) of your choice, either {{site.data.keyword.keymanagementservicelong_notm}} or {{site.data.keyword.hscrypto}}. You can also create your root key in the KMS. The VPC infrastructure locates the key in the KMS instance and then encrypts the volume. For prerequisites and a one-time set up procedure, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-creating-instances-byok#byok-vsi-prereqs).

After encrypting your volume, you can't remove the encryption or change it to provider-managed encryption.

For information about creating customer-managed encryption for volumes during virtual server instance provisioning, see [Customer managed encryption for block storage](/docs/vpc?topic=vpc-creating-instances-byok).

## Next Steps
{: #block-storage-about-next-steps}

Start creating {{site.data.keyword.block_storage_is_short}} volumes.

* For information about creating a new volume during instance provisioning, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi).
* For information about creating block storage volumes with customer-managed encryption, see [Creating customer-managed encrypted data volumes by using the UI](/docs/vpc?topic=vpc-block-storage-vpc-encryption#data-vol-encryption-ui).

## Related information
{: #related-info-storage}

For more information about creating and managing instances in the VPC, see [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers).

{{site.data.keyword.block_storage_is_short}} provides features unique to the VPC and is not compatible with the classic infrastructure storage. If you're interested in {{site.data.keyword.blockstoragefull}} on the classic infrastructure, see [{{site.data.keyword.blockstoragefull}}](/docs/BlockStorage?topic=BlockStorage-getting-started).
{:note}
