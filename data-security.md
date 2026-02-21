---

copyright:
  years: 2020, 2026
lastupdated: "2026-02-21"

keywords: data encryption, data storage, bring your own keys, BYOK, key management, key encryption, personal data, data deletion, data security

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Securing your data in VPC
{: #mng-data}

To ensure that you can securely manage your data when you use {{site.data.keyword.vpc_full}}, it's important to know exactly what data is stored and encrypted and how you can delete any stored personal data. Data encryption with your own root keys is available by using a supported key management service (KMS).
{: shortdesc}

{{site.data.keyword.vpn_vpc_short}} does not store any customer data other than what is required to configure VPN gateways, connections, and policies. Data that is transmitted through a VPN gateway is not encrypted by IBM. Data about your specific VPN and policy configurations are encrypted in transit and at rest. VPN configuration data is deleted upon your request through the API or in the console.

## How your data is stored and encrypted in VPC
{: #data-storage}

All Block Storage volumes are encrypted by default with IBM-managed encryption. {{site.data.keyword.IBM}}-managed keys are generated and securely stored in a Block Storage vault that is backed by Consul and maintained by {{site.data.keyword.cloud}} operations.

For more security and control, you can protect your data with your own root keys (also called a customer root key or CRK). This feature is commonly called Bring Your Own Key, or BYOK. Root keys encrypt the keys that safeguard your data. You can import your root keys to {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}, or have either key management service create one for you.

The KMS stores your key and makes it available during volume and custom image encryption. {{site.data.keyword.keymanagementserviceshort}} provides FIPS 140-2 Level 3 compliance. Hyper Protect Crypto Services offers the highest level of security with FIPS 140-2 Level 4 compliance. Your key material is protected in transit (when it's transported) and at rest (when it is stored).

Customer-managed encryption is available for custom images, boot volumes, and data volumes. When an instance is provisioned from an encrypted custom image, its boot volume is encrypted by using the image’s root key. You can also choose a different root key. Data volumes are encrypted by using root keys when you provision a virtual server instance or when you create a stand-alone volume.

Images and volumes are often referred to as being encrypted with a root key when, in fact, [envelope encryption](#x9860393){: term} is used. Internally, each image or volume is encrypted with a [data encryption key (DEK)](#x4791827){: term}, which is an open source QEMU technology that is used by the {{site.data.keyword.vpc_short}} Generation 2 infrastructure. A LUKS passphrase, also called a _key encryption key_, encrypts the DEK. The LUKS passphrase is then encrypted with a root key, creating what is called a wrapped DEK (WDEK). For more information about {{site.data.keyword.vpc_short}} key encryption technology, see [IBM Cloud VPC Generation 2 encryption technology](/docs/vpc?topic=vpc-vpc-encryption-about#byok-technologies).

For example, if you provision two volumes by using the same root key, unique passphrases are generated for each volume, which are then encrypted with the root key. Envelope encryption provides more protection for your data, and ensures that the root key can be rotated without having to reencrypt the data. For more information about envelope encryption, see [Protecting your sensitive data in VPC](#data-encryption).

All your interaction with {{site.data.keyword.vpn_vpc_short}} is encrypted. For example, when you use an API or interact with the service through the console to configure VPN gateways and VPN connections, all such interactions are encrypted end-to-end. Likewise, data elements that are related to your configuration are encrypted in transit and at rest. No personal or sensitive data is stored, processed, or transmitted. Data at rest is stored in an encrypted database.

After the {{site.data.keyword.vpn_vpc_short}} is provisioned and the network connections are created, the encryption of data that you choose to transmit across the network is your responsibility.

### Instance storage data isolation and encryption
{: #instance-storage-isolation}

The instance storage disk or disks, which are attached to the virtual server instance, cannot be shared with any other virtual servers and cannot be accessed by any other virtual servers in the future. They are one-time use, single-attach, for the virtual server that requested the instance storage.

Instance storage data is secured with on-disk encryption. The physical disks that are used for instance storage are self-encrypting with the strong AES-256 encryption standard. The data is automatically decrypted when your instance accesses the data. When your instance is shut down or deleted, the underlying storage space is erased and unrecoverable. At that point, the data is unrecoverable.

Data is automatically encrypted on the physical media at the drive level. However, customer-managed keys are not supported for instance storage. For sensitive data, it is strongly recommended that users utilize software-based file system encryption such as LUKS for Linux&reg; or BitLocker for Windows&reg;. This technology allows end users to encrypt entirely within the instance, and can provide additional protection for sensitive data in-transit between the instances and the physical drive media. Some operating systems also provide FIPS certified encryption algorithms that may also be used. See [Encrypting block devices using LUKS](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/security_hardening/encrypting-block-devices-using-luks_security-hardening) for an example of how to encrypt on Red Hat Enterprise Linux&reg; however, refer to the Operating System documentation or specific information on how to encrypt each device.

## Protecting your sensitive data in VPC
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} provide a higher level of protection called envelope encryption.

[Envelope encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-envelope-encryption-byok) encrypts one encryption key with another encryption key. A DEK encrypts your actual data. The DEK is never stored. Rather, it's encrypted by a key encryption key. The LUKS passphrase is then encrypted by a root key, which creates a WDEK. To decrypt data, the WDEK is unwrapped so you can access the data that's stored on the volume. This process is possible only by accessing the root key that is stored in your KMS instance. Root keys in {{site.data.keyword.hscrypto}} instances are also protected by a [hardware security module (HSM)](#x6704988){: term} master key.

You control access to your root keys stored in KMS instances within {{site.data.keyword.cloud}} by using {{site.data.keyword.iamlong}} (IAM). You grant access to a service to use your keys. You can also revoke access at any time, for example, if you suspect your keys might be compromised, or [delete your root keys](#delete-root-keys).

{{site.data.keyword.vpn_vpc_short}}: Customer-provided preshared keys are encrypted before they are stored in database. All other data VPN gateway and VPN policy configuration is encrypted at rest at the database level.

### About customer-managed keys
{: #about-encryption}

For Block Storage volumes and encrypted images, you can rotate the root keys for more security. When you rotate a root key by schedule or on demand, the original key material is replaced. The old key remains active to decrypt existing resources but can't be used to encrypt new ones. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

Consider regional and cross-regional implications when you choose to use customer-managed encryption. For more information, see [Regional and cross regional considerations](/docs/vpc?topic=vpc-vpc-encryption-about#byok-cross-region-keys).

With {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} you can create, import, and manage your root keys. You can assign access policies to the keys, assign users or service IDs to the keys, or give the key access only to a specific service. The first 20 keys are without cost.

### About customer-managed encrypted volumes and images
{: #about-encryption_volumes-images}

With customer-managed encryption, you provision root keys to protect your encrypted resources in the cloud. Root keys serve as key-wrapping keys that encrypt the encryption keys that protect your data. You decide whether to import your existing root keys, or have a supported KMS create one for you.

Block Storage volumes are assigned a unique data encryption key that is generated by the instance's host hypervisor. The data encryption key for each volume is encrypted with a unique KMS-generated LUKS passphrase, which is then encrypted by the root key and stored in the KMS.

Custom images are encrypted by your own LUKS passphrase you create by using QEMU. After the image is encrypted, you wrap the passphrase with your CRK stored in the KMS. For more information, see [Encrypted custom images](/docs/vpc?topic=vpc-vpc-encryption-about#byok-about-encrypted-images).

### Enabling customer-managed keys for VPC
{: #using-byok}

See the following procedure for creating Block Storage boot and data volumes with customer-managed encryption
* [Creating Block Storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption)
* [Creating File Storage shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption)

### Working with customer-managed keys for VPC
{: #working-with-keys}

For more information about managing data encryption, see [Managing data encryption](/docs/vpc?topic=vpc-vpc-encryption-managing), and the section on temporarily revoking access by [removing service authorization to a root key](/docs/vpc?topic=vpc-vpc-encryption-managing#instance-byok-inaccessible-data).

Encryption of the link between a customer's workload that's outside of {{site.data.keyword.cloud_notm}}, and a workload that's inside {{site.data.keyword.cloud_notm}}, is the customer’s responsibility. See _Encryption_ in [Security and regulation compliance](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance).
{: note}

## Deleting data in VPC
{: #data-delete}

### Deleting root keys
{: #delete-root-keys}

For more information about deleting root keys, see [Deleting root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys).

### Deleting a Block Storage volume
{: #data-delete-volume}

For more information about deleting Block Storage volumes, see the [FAQs for {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-vpc-faq#faq-block-storage-16).

### Deleting a custom image
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

You can restore deleted root keys that you imported to the KMS within 30 days of deletion. For more information, see
[Restoring deleted root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-restore-root-key).

{{site.data.keyword.vpn_vpc_short}} does not support the restoration of deleted data.

### Deleting all VPC data
{: #delete-all-data}

To delete all persisted data that {{site.data.keyword.vpc_short}} stores, choose one of the following options.

Removing your personal and sensitive information requires all of your {{site.data.keyword.vpc_short}} resources to be deleted as well. Make sure that you back up your data before you proceed.
{: note}

* Open an {{site.data.keyword.cloud_notm}} support case. Contact IBM Support to remove your personal and sensitive information from {{site.data.keyword.vpc_short}}. For more information, see [Using the Support Center](/docs/account?topic=account-using-avatar).
* End your {{site.data.keyword.cloud_notm}} subscription. After you end your {{site.data.keyword.cloud_notm}} subscription, {{site.data.keyword.vpc_short}} deletes all service resources that you created, which includes all persisted data that is associated with those resources.
