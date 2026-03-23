---

copyright:
  years: 2020, 2026
lastupdated: "2026-03-23"

keywords: data encryption, data storage, bring your own keys, BYOK, key management, key encryption, personal data, data deletion, data security

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Securing your data in VPC
{: #mng-data}

To securely manage your data with {{site.data.keyword.vpc_full}}, you need to understand what data is stored and encrypted. You must also know how to delete any personal data that is stored. Data encryption with your own root keys is available by using a supported key management service (KMS).
{: shortdesc}

[Deprecated]{: tag-deprecated} The {{site.data.keyword.hscrypto}} are deprecated. Customers can use existing instances until 20 March 2027. For more information, see [Deprecation of IBM Cloud Hyper Protect Crypto Services](/docs/hs-crypto?topic=hs-crypto-faqs-deprecation-of-ibm-cloud-hyper-protect-crypto-services). For continued protection, consider migrating your existing encryption keys to a Dedicated {{site.data.keyword.keymanagementserviceshort}} instance. For more information, see the [Migration guide](/docs/key-protect?topic=key-protect-migrate-st).

{{site.data.keyword.vpn_vpc_short}} does not store any customer data other than what is required to configure VPN gateways, connections, and policies. Data that is transmitted through a VPN gateway is not encrypted by IBM. Data about your specific VPN and policy configurations are encrypted in transit and at rest. VPN configuration data is deleted upon your request through the API or in the console.

## How your data is stored and encrypted in VPC
{: #data-storage}

All Block Storage volumes are encrypted by default with IBM-managed encryption. {{site.data.keyword.IBM}}-managed keys are generated and securely stored in a Block Storage vault that is backed by Consul and maintained by {{site.data.keyword.cloud}} operations.

For more security and control, you can protect your data with your own root keys (also called a customer root key or CRK). This feature is commonly called Bring Your Own Key, or BYOK. Root keys encrypt the keys that safeguard your data. You can import your root keys to {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}, or have either key management service create one for you.

{{site.data.keyword.keymanagementserviceshort}} offers two deployment options to meet different security and compliance requirements:
- **Standard** (multi-tenant): A cost-effective solution with FIPS 140-2 Level 3 compliance and shared HSM infrastructure. IBM manages the HSM master keys.
- **Dedicated** (single-tenant): Enhanced security with FIPS 140-3 Level 4 compliance (submitted for certification), dedicated HSM partitions, and complete workload isolation. You own and manage your own master keys with no IBM administrator access.

For more information about choosing between Standard and Dedicated, see [About Standard and Dedicated {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about).

The KMS stores your key and makes it available during volume and custom image encryption. {{site.data.keyword.keymanagementserviceshort}} provides FIPS 140-2 Level 3 compliance. Hyper Protect Crypto Services offers the highest level of security with FIPS 140-2 Level 4 compliance. Your key material is protected in transit (when it's transported) and at rest (when it is stored).

Customer-managed encryption is available for custom images, boot volumes, and data volumes. When an instance is provisioned from an encrypted custom image, its boot volume is encrypted by using the image’s root key. You can also choose a different root key. Data volumes are encrypted by using root keys when you provision a virtual server instance or when you create a stand-alone volume.

Images and volumes are encrypted by using [envelope encryption](#x9860393){: term}, where your data is protected by multiple layers of encryption keys. For more information about the encryption technology and key hierarchy, see [About data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about).

All your interaction with {{site.data.keyword.vpn_vpc_short}} is encrypted. For example, when you use an API or interact with the service through the console to configure VPN gateways and VPN connections, all such interactions are encrypted end-to-end. Likewise, data elements that are related to your configuration are encrypted in transit and at rest. No personal or sensitive data is stored, processed, or transmitted. Data at rest is stored in an encrypted database.

After the {{site.data.keyword.vpn_vpc_short}} is provisioned and the network connections are created, the encryption of data that you choose to transmit across the network is your responsibility.

### Instance storage data isolation and encryption
{: #instance-storage-isolation}

The instance storage disk or disks, which are attached to the virtual server instance, can't be shared with any other virtual servers. The instance storage disk or disks can't be accessed by any other virtual servers in the future. The disks are one-time use, single-attach, for the virtual server that requested the instance storage.

Instance storage data is secured with on-disk encryption. The physical disks that are used for instance storage are self-encrypting with the strong AES-256 encryption standard. The data is automatically decrypted when your instance accesses the data. When your instance is shut down or deleted, the underlying storage space is erased and unrecoverable. At that point, the data is unrecoverable.

Data is automatically encrypted on the physical media at the drive level. However, customer-managed keys are not supported for instance storage. For sensitive data, it is strongly recommended that users use software-based file system encryption such as LUKS for Linux&reg; or BitLocker for Windows&reg;. This technology allows users to encrypt entirely within the instance, and can provide extra protection for sensitive data in-transit between the instances and the physical drive media. Some operating systems also provide FIPS-certified encryption algorithms that can also be used. For an example of how to encrypt on Red Hat Enterprise Linux&reg;, see [Encrypting block devices using LUKS](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/security_hardening/encrypting-block-devices-using-luks_security-hardening). Refer to the Operating System documentation for specific information on how to encrypt each device.

## Protecting your sensitive data in VPC
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} provide a higher level of protection that is called [envelope encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-envelope-encryption-byok), which encrypts one encryption key with another encryption key. This multi-layered approach helps ensure that your data is protected by multiple keys, and only you control access to the root keys.

You control access to your root keys stored in the KMS instances within {{site.data.keyword.cloud}} by using {{site.data.keyword.iamlong}} (IAM). You grant access to a service to use your keys. You can also revoke access at any time, for example, if you suspect your keys might be compromised. For more information about managing your encryption keys, see [Managing data encryption](/docs/vpc?topic=vpc-vpc-encryption-managing).

{{site.data.keyword.vpn_vpc_short}}: Customer-provided preshared keys are encrypted before they are stored in the database. All other data VPN gateway and VPN policy configuration is encrypted at rest at the database level.

### About customer-managed keys
{: #about-encryption}

With {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}, you can create, import, and manage your root keys. {{site.data.keyword.keymanagementserviceshort}} is available in two deployment options:

- **Standard**: A multi-tenant service with FIPS 140-2 Level 3 compliance, suitable for most workloads that require customer-managed encryption.
- **Dedicated**: A single-tenant service with FIPS 140-3 Level 4 compliance (submitted for certification), providing complete control over encryption keys with no IBM administrator access.

You can assign access policies to the keys, assign users or service IDs to the keys, or give the key access only to a specific service. For pricing information, see [Pricing for Key Protect](/docs/key-protect?topic=key-protect-pricing-plan).

You can rotate your root keys for enhanced security. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

Consider regional and cross-regional implications when you choose to use customer-managed encryption. For more information, see [Regional and cross-regional considerations](/docs/vpc?topic=vpc-vpc-encryption-about#byok-cross-region-keys).

### About customer-managed encrypted volumes and images
{: #about-encryption_volumes-images}

With customer-managed encryption, you provision root keys to protect your encrypted resources in the cloud. Root keys serve as key-wrapping keys that encrypt the encryption keys that protect your data. You decide whether to import your existing root keys, or have a supported KMS create one for you.

A unique data encryption key that is generated by the instance's host hypervisor is assigned to the Block Storage volumes. The data encryption key for each volume is encrypted with a unique KMS-generated LUKS passphrase, which is then encrypted by the root key and stored in the KMS.

Custom images are encrypted by your own LUKS passphrase that you create by using QEMU. After the image is encrypted, you wrap the passphrase with your CRK stored in the KMS. For more information, see [Encrypted custom images](/docs/vpc?topic=vpc-vpc-encryption-about#byok-about-encrypted-images).

### Enabling customer-managed keys for VPC
{: #using-byok}

See the following procedure for creating Block Storage boot and data volumes with customer-managed encryption
* [Creating Block Storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption)
* [Creating File Storage shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption)

If you choose {{site.data.keyword.keymanagementserviceshort}} Dedicated, you must first initialize your instance by using the CLI before you can use it to encrypt VPC resources. For more information, see [Initializing Dedicated {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-st-init-cli).
{: important}

### Working with customer-managed keys for VPC
{: #working-with-keys}

For more information about managing data encryption, see [Managing data encryption](/docs/vpc?topic=vpc-vpc-encryption-managing), and the section on temporarily revoking access by [removing service authorization to a root key](/docs/vpc?topic=vpc-vpc-encryption-managing#instance-byok-inaccessible-data).

Encryption of the link between a customer's workload that's outside of {{site.data.keyword.cloud_notm}}, and a workload that's inside {{site.data.keyword.cloud_notm}}, is the customer’s responsibility. See _Encryption_ in [Security and regulation compliance](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance).
{: note}

## Deleting data in VPC
{: #data-delete}

### Deleting root keys
{: #delete-root-keys}

When you delete a root key, all resources that are protected by it become unusable. You have a 30-day grace period to restore imported root keys. For more information, see [Managing data encryption](/docs/vpc?topic=vpc-vpc-encryption-managing).

### Deleting Block Storage volumes
{: #data-delete-volume}

For more information about deleting Block Storage volumes, see the [FAQs for {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-vpc-faq#faq-block-storage-16).

### Deleting custom images
{: #delete-custom-images}

For more information about deleting custom images from {{site.data.keyword.vpc_short}}, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui). Be aware of any virtual server instances that you provisioned with a custom image. To remove all data associated with a specific custom image, make sure that you also delete any instances provisioned from the custom image, along with associated boot volumes.

{{site.data.content.delete-custom-image-private-catalog}}

### Deleting VPC instances
{: #service-delete}

For more information about deleting a VPC and its associated resources, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting).

The VPC data retention policy describes how long your data is stored after you delete the service. The data retention policy is included in the {{site.data.keyword.vpc_full}} service description, which you can find in the [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview?topic=overview-terms).

The VPN and VPN policy configurations are deleted on request through the API or user interface.

### Restoring deleted data for VPC
{: #data-restore}

You can restore deleted root keys that you imported to the KMS within 30 days of deletion. For more information, see [Restoring deleted root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-restore-root-key).

{{site.data.keyword.vpn_vpc_short}} does not support the restoration of deleted data.

### Deleting all VPC data
{: #delete-all-data}

To delete all persisted data that {{site.data.keyword.vpc_short}} stores, choose one of the following options.

Removing your personal and sensitive information requires all of your {{site.data.keyword.vpc_short}} resources to be deleted as well. Make sure that you back up your data before you proceed.
{: note}

* Open an {{site.data.keyword.cloud_notm}} support case. Contact IBM Support to remove your personal and sensitive information from {{site.data.keyword.vpc_short}}. For more information, see [Using the Support Center](/docs/account?topic=account-using-avatar).
* End your {{site.data.keyword.cloud_notm}} subscription. After you end your {{site.data.keyword.cloud_notm}} subscription, {{site.data.keyword.vpc_short}} deletes all service resources that you created, which includes all persisted data that is associated with those resources.
