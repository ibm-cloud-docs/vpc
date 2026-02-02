---

copyright:
  years: 2019, 2026
lastupdated: "2026-02-02"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# About data encryption for VPC
{: #vpc-encryption-about}

{{site.data.keyword.cloud}} takes security seriously and understands the importance of encrypting data to keep it safe. {{site.data.keyword.block_storage_is_short}} volumes, snapshots, and {{site.data.keyword.filestorage_vpc_short}} file shares are automatically encrypted by using IBM-managed encryption. You can also choose to manage your own encryption for volumes, file shares, and custom images by using customer-managed encryption.
{: shortdesc}

## IBM-managed encryption
{: #vpc-provider-managed-encryption}

By default, VPC volumes and file shares are encrypted at rest with IBM-managed encryption. The service comes with no additional cost.

IBM-managed encryption uses the following industry standard protocols:

* AES-256 encryption.
* Keys are managed in-house with Key Management Interoperability Protocol (KMIP).
* Storage architecture is validated for Federal Information Security Management Act (FISMA), and the Health Insurance Portability and Accountability Act (HIPAA)
* Storage architecture is also validated for Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386), and EU Data Protection Directive 95/46/EC compliance.
* {{site.data.keyword.cloud}} Block and File Storage for VPC uses self-encrypting drives (SEDs) to help ensure data security and compliance with industry standards. 
   * For Gen 1 volume and share profiles, all MZRs use SEDs that are validated for Federal Information Processing Standard (FIPS) Publication 140-2 Level 1 or 140-3 Level 1, depending on the software module, meeting federal security standards.
   * For Gen 2 block volume profiles, such as the `sdp` profile, the storage architecture maintains FIPS 140-3 Level 1 validation across most regions.
      * When you provision volumes in London MZR (`eu-gb`), the volumes are created on Trusted Computing Group (TCG) OPAL-compliant SED drives. 
      * The current configuration in Washington, DC MZR (`us-east`) includes a mix of FIPS 140-2 validated and TCG OPAL-compliant drives. During provisioning, the volumes can be created on either type of drive.
      * When you provision block volumes in any other MZRs, the volumes are created and stored on FIPS 140-3 Level 1 validated SEDs.
   * For Gen 2 file share profiles, such as the `rfs` profile, the storage architecture maintains FIPS 140-3 Level 1 validation across all MZRs where regional file shares are available.

IBM is committed to maintaining high standards of data protection and regulatory compliance. All storage drives meet recognized security benchmarks, and you can be confident in the robustness of the encryption and data protection mechanisms that are in place across all regions.

## Customer-managed encryption
{: #vpc-customer-managed-encryption}

For end-to-end encryption in the {{site.data.keyword.cloud_notm}}, you can use customer-managed encryption. Your data is protected while at rest, and also in transit from the storage to the hypervisor and host. You are responsible for encrypting your data outside the VPC.

With customer-managed encryption, you can bring your own customer root key (CRK) to the cloud or have a [key management service](#kms-for-byok) (KMS) generate a key for you. Root keys are used to encrypt volume, file share, and custom image passphrases with [envelope encryption](#vpc-envelope-encryption-byok), a process that wraps a key with another key.

Supported key management services are {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}}. After the key management service is provisioned, you must authorize access between the source service (for example, Cloud Block Storage, Cloud File Storage) and the KMS. For custom images, authorize between Image Service for VPC (source service) and {{site.data.keyword.cos_full_notm}} (target service). This authorization is required so that the image service has authority to import custom images from the {{site.data.keyword.cos_short}} bucket that contains your custom image.

When you use customer-managed encryption, you can use root keys to encrypt resources across regions. You can encrypt resources with a key that is stored in your regional KMS instance, and you can use root keys from another region. For best performance and security, colocate your KMS instance, root keys, and your encrypted resources in the same region. For more information, see [Root key regional and cross-regional considerations](#byok-cross-region-keys).

You can share root keys across accounts. Root keys in a primary account can be accessed and used to encrypt new volumes and file shares that are created in a secondary account. In {{site.data.keyword.cloud_notm}}, the KMS can be either located in the same or in another account as the service that is using an encryption key. This deployment pattern allows enterprises to centrally manage encryption keys for all corporate accounts. For more information, see [Encryption key management](/docs/solution-tutorials?topic=solution-tutorials-resource-sharing#resource-sharing-security-kms).

Customer-managed encryption is available for custom images, boot volumes, data volumes, snapshots, and file shares. 
- Data in the instance's boot volume is encrypted by using the [custom image encryption](#byok-about-encrypted-images). You can also encrypt the boot volume with a different root key.
- [Data volumes](/docs/vpc?topic=vpc-block-storage-about#secondary-data-volumes) can be encrypted by using their own root keys when you provision a virtual server instance or when you create a stand-alone volume. Or, you can use the same root key that you specified for the boot volume.
- Snapshots that are taken of a source volume inherit the encryption from the volume. 
- [File shares](/docs/vpc?topic=vpc-file-storage-byok-encryption) provide a customer-managed encryption option similar to {{site.data.keyword.block_storage_is_short}} volumes.

### Advantages of customer-managed encryption
{: #byok-advantages}

Customer-managed encryption has several advantages over IBM-managed encryption.

**You control your keys:**

* Because you bring your own keys (BYOK) to the cloud, you control encryption of your {{site.data.keyword.block_storage_is_short}} volumes, file shares, and custom images.
* You grant the IBM VPC service access to use your root keys to encrypt your data. You can revoke access at any time for any reason.
* Your data is protected while in transit from the storage system to the hypervisor and host within the VPC, and at rest in {{site.data.keyword.block_storage_is_short}} and {{site.data.keyword.filestorage_vpc_short}}.

**Encrypt boot and data volumes:**

* Your {{site.data.keyword.block_storage_is_short}} data is always encrypted with your own keys both at rest and in motion.
* Each boot and data volume is encrypted at rest with a unique master encryption key. If the key is compromised, no other {{site.data.keyword.block_storage_is_short}} volume is impacted because the compromised key protects only a single volume.
* Primary boot volumes that are created from Linux or Windows stock images are encrypted by default with IBM-managed encryption. If you create an instance from a stock image and specify customer-managed encryption for data volumes, the data that is written to those volumes is protected by customer-managed encryption.
* You control the number and usage of root keys to use for envelope encryption at the volume level. For example, you can choose to encrypt your boot volume with one root key and data volumes with a different root key.
* Snapshots that are created from boot and data volumes inherit the customer-managed encryption from the source volume.

**Encrypt file shares:**

* File share data is always encrypted with your own keys at rest.

* You control the number and usage of root keys to use for envelope encryption at the file shares level. That is, you can choose to encrypt all your file shares with same root key or each file share with different keys. Alternatively, you can encrypt some file shares with one root key and others with different root keys. You have complete flexibility to implement your root key usage based on your individual security needs.

* Manage the root keys for your file shares by rotating, disabling, or deleting keys. You can restore deleted keys within 30 days after their deletion.

**Encrypt your custom images:**

* You can [manually encrypt](/docs/vpc?topic=vpc-create-encrypted-custom-image#manually-encrypt-image) an image by creating an encrypted copy of it by using QEMU.
* You can create encrypted [custom images](/docs/vpc?topic=vpc-create-encrypted-custom-image) from encrypted boot volumes. For more information, see [About encrypted custom images](#byok-about-encrypted-images).

**Excellent performance:**

* Customer-managed encryption uses hypervisor encryption on the VPC infrastructure, which delivers excellent encrypted {{site.data.keyword.block_storage_is_short}} I/O performance for your virtual server instances.
* With the VPC infrastructure, you can create and start 1,000 instances with customer-managed encrypted boot volumes within minutes.
* Because the hypervisor manages the encryption and decryption, the guest OS does not have to modify the data. The guest OS has no knowledge that encryption is occurring.

**Choice of key management service:**

* You choose the [key management service (KMS)](#kms-for-byok) that you want to use. You can select {{site.data.keyword.keymanagementserviceshort}}, a public multi-tenant KMS that is FIPS 140-2 L3 compliant, or the more secure {{site.data.keyword.hscrypto}}, which is FIPS 140-2 L4 compliant.
* The KMS instance that contains your root key can reside outside of the region where the encrypted volume exists. However, for best performance and security, use keys in the [same region as the {{site.data.keyword.block_storage_is_short}} volumes](#byok-cross-region-keys).

**Key rotation and audit tracking:**

* You can manually rotate your root keys with the API or from the CLI, or set up a rotation policy to automatically rotate your keys. Rotating keys replaces the root key's original cryptographic material and generates new material. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

* Customer-managed encryption provides audit records for root key usage. The events are generated and automatically collected in {{site.data.keyword.logs_full_notm}}.

### {{site.data.keyword.cloud_notm}} VPC encryption technology
{: #byok-technologies}

Encryption is handled by {{site.data.keyword.cloud_notm}} VPC's hypervisor technology for your instances. This feature provides a greater level of security over solutions that provide only storage node encryption-at-rest. Data is always encrypted with envelope encryption within the {{site.data.keyword.cloud_notm}}.

Stock and custom images use QEMU Copy On Write Version 2 (QCOW2) file format. LUKS encryption format secures the QCOW2 format files. {{site.data.keyword.cloud_notm}} uses the AES-256 cipher suite and XTS cipher mode options with LUKS. This combination provides a much greater level of security than AES-CBC, along with better management of passphrases for key rotation, and provides key replacement options if your keys are compromised.

In total, four keys protect your data:

* An **IBM-managed key** encrypts your data in the backend storage system. IBM-managed encryption on the storage system is always applied, even when you use customer-managed encryption. This key protects your data while in transit and while at rest.

    Encryption of the network link between your workload outside of {{site.data.keyword.cloud_notm}} and a workload inside {{site.data.keyword.cloud_notm}} is your responsibility. For more information, see [Encryption in Security and regulation compliance](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance).
    {: note}

* A **data encryption key (DEK)** encrypts data within the QCOW2 file and secures the block data clusters in the virtual disk. The DEK is managed by open source QEMU technology and auto-generated when a QCOW2 file is created.

    For {{site.data.keyword.block_storage_is_short}} volumes that are created from stock images, the DEK is generated by QEMU running on IBM-provided KVM hypervisors. For {{site.data.keyword.block_storage_is_short}} volumes that are created from custom images, it is generated by QEMU that runs on your on-premises node. The DEK (an AES-256 key) is encrypted with a LUKS passphrase and stored encrypted in the QCOW2 file.
    {: note}

* A **LUKS passphrase** (also called a "key encryption key") encrypts and decrypts the DEK. This key is managed by the VPC generation 2 infrastructure and is encrypted by your root key. It is stored as metadata that is associated with the {{site.data.keyword.block_storage_is_short}} volume that contains the QCOW2 file.

* A **customer root key** that encrypts volume, share, and custom image passphrases with envelope encryption, which creates a wrapped DEK or WDEK. Root keys are customer-managed from KMS instances ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}), and stored and managed securely within the KMS instance. The root key also unwraps (decrypts) the WDEK, providing access to your encrypted data.

### Supported key management services
{: #kms-for-byok}

Two IBM key management services (KMS) are available for customer-managed encryption, {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} (available in certain [regions](/docs/hs-crypto?topic=hs-crypto-regions#regions)). These services use a common key provider API to provide a consistent approach for managing your encryption keys.

Table 1 describes these services:

| Key Management Service | HSM Encryption Certification | Description |
|-----|-----|-----|
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-getting-started-tutorial) | FIPS 140-2 _Level 3_ compliance | With A multi-tenant KMS, you can import or create your root keys and securely manage them.  |
| [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started) | FIPS 140-2 _Level 4_ compliance | The highest level of security. A single-tenant KMS and [hardware security module (HSM)](#x6704988){: term} that is controlled by you. Import or create your root keys and securely manage them. Create an HSM master key to encrypt the content of key storage, including root keys. Only you have access to your keys and data. |
{: caption="Available key management service options" caption-side="bottom"}

You might see {{site.data.keyword.keymanagementserviceshort}} being described as _BYOK_, "bring your own key" and {{site.data.keyword.hscrypto}} as _KYOK_, or "keep your own key". {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} are similar services.
{: note}

### Envelope encryption
{: #vpc-envelope-encryption-byok}

Root keys serve as key-wrapping keys and are an important part of envelope encryption. With envelope encryption, root keys encrypt LUKS passphrases (also called _key encryption keys_) which, in turn, secure _data encryption keys_ (DEKs) that encrypt your data on the virtual disk. Figure 1 illustrates this process.

![Figure showing envelope encryption.](/images/envelope-encryption.png "Contextual view of envelope encryption"){: caption="Contextual view of envelope encryption" caption-side="bottom"}

Both {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} provide envelope encryption. Root keys in {{site.data.keyword.hscrypto}} service instances are also protected by a hardware security module (HSM) master key. The KMS stores your key and makes it available during volume and custom image encryption. You also manage your keys in the KMS.

A unique master encryption key is assigned to block storage volumes and file shares. That unique key is generated by the instance's host hypervisor. This key is encrypted by a passphrase and wrapped (encrypted) by the root key, creating a wrapped DEK or WDEK. The WDEK is stored as metadata with the volume or image, and is not available on VPC interfaces.

Custom images are encrypted by your own LUKS passphrase that you create by using QEMU. After the image is encrypted, you wrap the passphrase with your root key that is stored in the KMS.

For more information about envelope encryption, see the following documentation:
* {{site.data.keyword.keymanagementserviceshort}} - [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).
* {{site.data.keyword.hscrypto}} - [Protecting your data with envelope encryption](/docs/hs-crypto?topic=hs-crypto-envelope-encryption).

### Root key regional and cross-regional considerations
{: #byok-cross-region-keys}

When you use customer-managed encryption for volumes, file shares, and custom images, you can keep your root keys in the same region as your resources or in a different region (cross-regional).

Cross-regional keys offer more key availability with the tradeoff of slightly higher latency. You can create and manage root keys in one region and apply them to resources in another region. Cross-regional keys are available in all regions.

For optimal performance and security, use root keys in the same region as your encrypted resources. Regional service uses private endpoints to multiple availability zones. If a specific zone is unavailable, you can continue to access your root keys and encrypted resources from another zone.

When you use root keys regionally or cross-regionally, all network traffic is directed to a private endpoint. In other words, your keys encrypt and decrypt resources over a private network that's inaccessible to the internet. For more information about private endpoints, see [Secure access to services by using service endpoints](/docs/account?topic=account-service-endpoints-overview). You can see a list of private endpoints in the console by going to the Resource List > Security and clicking a {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance. In the left menu, select **Endpoints**.

### General procedure for setting up customer-managed encryption
{: #byok-general-prodedure}

Setting up customer-managed encryption for your {{site.data.keyword.block_storage_is_short}} volumes involves several steps.

1. Generate your own root key. You can use your on-premises HSM. Or you can create your root key by using an {{site.data.keyword.cloud_notm}} HSM or {{site.data.keyword.hscrypto}}.

   {{site.data.keyword.cloud_notm}} data centers provide a dedicated HSM to create and protect your keys. By using {{site.data.keyword.hscrypto}}, you can take control of your cloud data encryption keys and cloud hardware security module.
   {: tip}

1. Provision the key management service (KMS) that best meets your needs.
1. Use your KMS to securely import your root key to the cloud service. For added security, create an import token in your KMS to encrypt and import root keys to the service.
1. From {{site.data.keyword.iamlong}} (IAM), authorize service between Cloud Block Storage (source service) and your KMS (target service). For custom images, also authorize between Image Service for VPC (source service) and {{site.data.keyword.cos_full_notm}} (target service).
1. The {{site.data.keyword.cloud_notm}} VPC infrastructure uses your root key to secure the data by wrapping the passphrases for volumes or custom images. For custom images, the root key wraps the passphrase before you import the custom image file to the cloud. For more information, see [About encrypted custom images](#byok-about-encrypted-images).
1. Manage your keys in your KMS. For example, you can [disable](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys) your root key to suspend its encryption and decryption operations. You can also [rotate your customer-managed root keys](/docs/vpc?topic=vpc-vpc-key-rotation). By rotating your root keys, you can roll out a new key across the resources that were protected by the old key.

#### Flow diagrams for specifying customer-managed encryption for volumes
{: #byok-flow-diagrams-volumes}

You can specify customer-managed encryption when you create {{site.data.keyword.block_storage_is_short}} volumes during instance provisioning. You can also specify it when you create a stand-alone volume.

Figure 1 shows the procedure for creating a data volume with customer-managed encryption during instance provisioning. The volume is automatically attached to the instance.

![Figure shows the procedure for creating an instance with a customer-managed encrypted volume.](/images/vpc_flowchart_instances_color.svg "Figure shows the procedure for creating an instance with a customer-managed encrypted volume"){: caption="Creating an instance with a customer-managed encrypted volume." caption-side="bottom"}

Figure 2 shows the procedure for creating a stand-alone volume and attaching it to an instance later.

![Figure shows the procedure for creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption.](/images/vpc_flowchart_volumes_color.svg "Figure shows the procedure for creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption"){: caption="Creating a {{site.data.keyword.block_storage_is_short}} volume with customer-managed encryption." caption-side="bottom"}

### About encrypted custom images
{: #byok-about-encrypted-images}

You can create a QCOW2 custom image that meets the requirements for {{site.data.keyword.vpc_short}} infrastructure and [encrypt it](/docs/vpc?topic=vpc-create-encrypted-custom-image#manually-encrypt-image) by using your own LUKS passphrase and root key. After you encrypt the custom image with your own passphrase, you upload it to {{site.data.keyword.cos_full_notm}}.

To import an encrypted custom image to {{site.data.keyword.vpc_short}}, begin by setting up a key management service (KMS) instance, either {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. In the KMS instance, create a root key for wrapping (encrypting) the passphrase. Wrapping the passphrase produces a wrapped DEK (WDEK). The WDEK protects the passphrase at import time, keeping your data secure. The passphrase is stored encrypted in the image service and unwrapped only when a virtual server that uses the encrypted image is started.

With the required IAM authorizations in place, you can import the encrypted image from {{site.data.keyword.cos_full_notm}}. You import the image to the VPC and specify the cloud resource name (CRN) for your root key that is stored in the KMS. You also specify the ciphertext for your WDEK, which is the passphrase that you use to encrypt your image, wrapped by the root key.

When you import with the API or UI, you supply a single QCOW2 boot image file that can be 10 GB to 250 GB. This file is private to the account to which it is imported. The region where you import the encrypted image is the region where you create virtual server instances from that image. The encrypted image displays with other custom images that you have permission to use. In the console, a lock icon next to the name indicates that it's an encrypted custom image.

If you import an encrypted image that is not exactly 100 GB virtual disk size and use it to provision an instance, and then try to create a custom image from that instance’s boot volume, the `POST /images` call is going to return an error.
{: note}

When you provision a virtual server instance with the encrypted image, no other encryption information is needed. The WDEK and the CRN of the root key are stored as metadata with the image. The WDEK is used to access the encrypted image when a virtual server instance that uses the encrypted image is started.

At provision time, data in the instance's boot volume is also encrypted by using the same root key that the custom image uses along with a generated passphrase. Alternatively, you can provide a different root key for the boot volume.

After the virtual server instance is created, you can view the custom image and see the root key CRN. You can also [rotate the root key](/docs/vpc?topic=vpc-vpc-key-rotation) and be notified by the KMS when your encrypted image WDEK is automatically rewrapped (reencrypted).

Any secondary volumes that you create and attach from the encrypted custom image use a different passphrase than the base custom image. Optionally, you can provide a different root key for secondary volumes that are created during instance provisioning.

For more information about creating custom images, see [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image). Also, see information for creating a [Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image) and [Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image).

#### Flow diagram for specifying customer-managed encryption for custom images
{: #byok-flow-diagram-images}

Figure 3 shows the procedure for encrypting custom images with your own encryption keys.

![Figure shows the procedure for creating encrypted custom images.](/images/vpc-flowchart-encrypted-images-color.svg "Figure shows the procedure for creating encrypted custom images."){: caption="Creating custom images with customer-managed encryption." caption-side="bottom"}

## Next Steps
{: #byok-about-next-steps}

* [Review the checklist](/docs/vpc?topic=vpc-vpc-encryption-planning#planning-for-data-encryption) for planning data encryption and complete the [prerequisites](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs).
* [Create virtual server instances with customer-managed encryption volumes](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
* [Create a {{site.data.keyword.block_storage_is_short}} volume with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
* [Create file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption).
