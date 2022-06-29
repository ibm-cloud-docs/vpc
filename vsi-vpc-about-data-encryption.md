---

Copyright:
  years: 2019, 2022
lastupdated: "2022-06-23"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# About data encryption for VPC
{: #vpc-encryption-about}

{{site.data.keyword.cloud_notm}} takes security seriously and understands the importance of encrypting data to keep it safe. Block storage volumes, snapshots, and file shares are automatically encrypted using IBM-managed encryption. You can also choose to manage your own encryption for volumes, file shares, and custom images by using customer-managed encryption.
{: shortdesc}

## IBM-managed encryption
{: #vpc-provider-managed-encryption}

By default, VPC volumes and file shares are encrypted at rest with IBM-managed encryption. There is no additional cost for this service.

For end-to-end encryption in the IBM Cloud, you can use customer-managed encryption. Your data is protected while in transit from block and file storage to the host/hypervisor and while at rest in volumes and shares.
{: tip}

IBM-managed encryption uses the following industry standard protocols:

* AES-256 encryption
* Keys are managed in-house with Key Management Interoperability Protocol (KMIP)
* Storage is validated for Federal Information Processing Standard (FIPS) Publication 140-2, Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA). Storage is also validated for Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386), and EU Data Protection Directive 95/46/EC compliance.

## Customer-managed encryption
{: #vpc-customer-managed-encryption}

Customer-managed encryption lets you bring your own customer root key (CRK) to the cloud or have a [key management service](#kms-for-byok) (KMS) generate a key for you. Supported key managment services are {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} (HPCS). Root keys encrypt volume, file share, and custom image passphrases with [envelope encryption](#vpc-envelope-encryption-byok), a process that encrypts a key with another key.

Customer-managed encryption for volumes, file shares, and custom images lets you use root keys in the same region as your resources. You can also use root keys from another region to encrypt your resources. For best performance and security, collocate your KMS instance/root keys and your encrypted resources in the same region. For more information, see [Root key regional and cross-regional considerations](#byok-cross-region-keys).

You can share root keys across accounts by linking accounts. Root keys in a primary account can be accessed and used to encrypt new volumes and file shares created in a secondary account. For information, see [Cross-account encryption for multitenant storage resources](/docs/vpc?topic=vpc-vpc-byok-cross-acct-key).

With customer-managed encryption, you can import your own root keys to the cloud. This process is commonly called "bring your own key". You can also have the KMS generate root keys for you. After provisioning the key management service, you must authorize access between the source service (for example, Cloud Block Storage, Cloud File Storage) and the KMS. For custom images, authorize between Image Service for VPC (source service) and IBM Cloud Object Storage (target service). This authorization is required so that the image service has authority to import custom images from the ICOS bucket containing your custom image.

Customer-managed encryption is available for creating custom images, boot volumes, data volumes, and file shares. Data in the instance's boot volume is encrypted using the [custom image encryption](#byok-about-encrypted-images). You can also encrypt the boot volume using a different root key. [Data volumes](/docs/vpc?topic=vpc-block-storage-about#secondary-data-volumes) can be encrypted using their own root keys when you provision a virtual server instance or when you create a standalone volume. Or, you can use the same root key that you specified for the boot volume. Snapshots taken of a source volume inherit the encryption from the volume. [File shares](/docs/vpc?topic=vpc-file-storage-vpc-encryption) provide a custmomer managed encryption option similar to volumes.

When using customer-managed encryption with your root key, your data is protected while in transit from the storage system to the host/hypervisor within the Cloud, and at rest in block storage volumes and file shares. For more information about this technology, see [IBM Cloud VPC encryption technology](#byok-technologies). You are responsible for encrypting your data outside the VPC.

### Advantages of customer-managed encryption
{: #byok-advantages}

Customer-managed encryption gives you several advantages over IBM-managed encryption:

**You control your keys:**

* Because you bring your own keys (BYOK) to the cloud, you control encryption of your block storage volumes, file shares, and custom images.
* You grant the IBM VPC service access to use your root keys to encrypt your data. You can revoke access for any reason.
* Your data is protected while in transit from the storage system to the host/hypervisor within the Cloud, and at rest in {{site.data.keyword.block_storage_is_short}} and File Storage for VPC.

**Encrypt boot and data volumes:**

* Your block storage data is encrypted both at rest and in motion at all times using your own keys.
* Each boot and data volume is encrypted at rest with a unique master encryption key. If the key is compromised, no other block storage volumes are impacted because the compromised key protects only a single volume.
* Primary boot volumes created from a Linux or Windows stock images are encrypted by default with IBM-managed encryption. If you create an instance from a stock image and specify customer-managed encryption for data volumes, data written to those volumes uses customer-managed encryption.
* You control the number and usage of root keys to use for envelope encryption at the volume level. For example, you can choose to encrypt your boot volume with one root key and data volumes with a different root key.
* Snapshots created from boot and data volumes inherit the customer-managed encryption from the source volume.

**Encrypt file shares:**

* File share data is encrypted both at rest at all times using your own keys. 

* You control the number and usage of root keys to use for envelope encryption at the file shares level. That is, you can choose to encrypt all your file shares with same root key or each file share with different keys. Aternatively, you can encrypt some file shares with one root key and others with different root keys. You have complete flexibility to implement your root key usage based on your individual security needs.

* Manage the root keys for your file shares by rotating, disabling, or deleting keys. You can also restore deleted keys before 30 days after deletion.

File Storage for VPC is accessible to accounts authorized to use the service.
{: note}

**Encrypt your custom images:**

* You can encrypt [custom images](/docs/vpc?topic=vpc-create-encrypted-custom-image) by importing a QCOW2 boot image file protected with your root key.
* Data in the instance's boot volume is also encrypted. For more information, see [About encrypted custom images](#byok-about-encrypted-images).

**Excellent performance:**

* BYOK uses hypervisor encryption on the VPC infrastructure, which delivers excellent encrypted block storage I/O performance for your virtual server instances.
* The VPC infrastructure can create and start 1,000 instances with customer-managed encrypted boot volumes within minutes.
* Because the hypervisor manages the encryption and decryption, the guest OS does not have to modify the data. The guest OS has no knowledge that encryption is occurring.

**Choice of key management service:**

* You choose the [key management service (KMS)](#kms-for-byok) you want to use. You can select {{site.data.keyword.keymanagementserviceshort}}, a public multi-tenant KMS that is FIPS 140-2 L3 compliant, or the more secure {{site.data.keyword.hscrypto}}, which is FIPS 140-2 L4 compliant.
* The KMS instance containing your root key can reside outside of the region where the encrypted volume exists. As best practice, and for enhanced performance and security, use keys in the [same region as the block storage volumes](#byok-cross-region-keys).

**Key rotation and audit tracking:**

* You can manually rotate your root keys using the API or CLI, or set up a rotation policy to automatically rotate your keys. Rotating keys replaces the root key's original cryptographic material and generates new material. For more inforation, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

* Customer-managed encryption provides better audit records for root key usage. Your key access is logged in the [Activity Tracker](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-activity-tracker-events) service. The Activity Tracker lets you track interactions with the VPC, such as when a root key is rotated.

### Envelope encryption
{: #vpc-envelope-ecryption-byok}

Root keys serve as key-wrapping keys and are an important part of envelope encryption. With envelope encryption, root keys encrypt LUKS passphrases (also called _key encryption keys_) which, in turn, secure _data encryption keys_ (DEKs) that encrypt your data on the virtual disk. Figure 1 illustrates this process.

![Figure showing envelope encryption.](/images/envelope-encryption.png "Contextual view of envelope encryption"){: caption="Figure 1. Contextual view of envelope encryption" caption-side="bottom"}

Both {{site.data.keyword.keymanagementserviceshort}} and HPCS provide envelope encryption. Root keys in HPCS service instances are also protected by a hardware security module (HSM) master key. The KMS stores your key and makes it available during volume and custom image encryption. You also manage your keys in the KMS.

Block storage volumes and file shares are assigned a unique master encryption key that is generated by the instance's host hypervisor. This key is encrypted by a passphrase and wrapped (encrypted) by the root key, creating a wrapped DEK or WDEK. The WDEK is stored as metadata with the volume or image, and is not available on VPC interfaces.

Custom images are encrypted by your own LUKS passphrase you create by using QEMU. After the image is encrypted, you wrap the passphrase with your root key stored in the KMS.

For more information about envelope encryption, see this information:
* Key Protect - [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption)
* HPCS - [Protecting your data with envelope encryption](/docs/hs-crypto?topic=hs-crypto-envelope-encryption)

For more information about the technology IBM Cloud VPC uses for envelope encryption, see [IBM Cloud VPC encryption technology](#byok-technologies).

### IBM Cloud VPC encryption technology
{: #byok-technologies}

Encryption is handled by IBM Cloud VPC's host/hypervisor technology for your instances. This feature provides greater level of security over solutions that provide only storage node encryption-at-rest. Data is always encrypted with envelope encryption within the cloud. Your data is protected while in transit between the host system and {{site.data.keyword.block_storage_is_short}} and Fiile Storage for VPC, and while at rest in the storage system.

Stock and custom images use QEMU Copy On Write Version 2 (QCOW2) file format. LUKS encryption format secures the QCOW2 format files. IBM uses the AES-256 cipher suite and XTS cipher mode options with LUKS. This combination provides you a much greater level of security than AES-CBC, along with better management of passphrases for key rotation, and provides key replacement options in case your keys are compromised.

In total, four keys protect your data:

* An IBM-managed key encrypts your data in the backend storage system - IBM-managed encryption on the storage system is always applied, even when you use customer-managed encryption. This key protects your data while in transit and while at rest.

    Encryption of the network link between your workload outside of IBM Cloud and a workload inside IBM Cloud is the your responsibility. For more information, see [Encryption in Security and regulation compliance](/docs/vpc?topic=vpc-responsibilities-vpc#security-compliance).
    {: note}

* A data encryption key (DEK) encrypts data within the QCOW2 file and secures the block data clusters in the virtual disk - The DEK is managed by open source QEMU technology and auto-generated when a QCOW2 file is created.

    For block storage volumes created from stock images, the DEK is generated by QEMU running on IBM-provided KVM hypervisors. For block storage volumes created from custom images, it's generated by QEMU running on your on-premises node/workstation. The DEK (an AES-256 key) is encrypted with a LUKS passphrase and stored encrypted in the QCOW2 file.

* A LUKS passphrase (also called a "key encryption key") encrypts and decrypts the DEK - This key is managed by the VPC generation 2 infrastructure and is encrypted by your root key. It's stored as metadata associated with the block storage volumes containing the QCOW2 file.

* A customer root key that encrypts volume, share, and custom image passphrases with envelope encryption, which creates the WDEK - Root keys are customer-managed from KMS instances (Key Protect or HPCS) and stored and managed securely within the KMS instance. The root key also unwraps (decrypts) the WDEK, providing access to your encrypted data.

### Root key regional and cross-regional considerations
{: #byok-cross-region-keys}

Customer-managed encryption for volumes, file shares, and custom images lets you keep your root keys in the same region as your resources or in a different region (cross-regional).

Cross-regional keys offer more key availability with the tradeoff of slightly higher latency. You can create and manage root keys in one region and apply them to resources in another region. Cross-regional keys are available in all regions.

For optimal performance and security, use root keys in the same region as your encrypted resources. Regional service uses private endpoints to multiple availability zones. If a given zone is unavailable, you can continue to access your root keys and encrypted resources from another zone.

When you use root keys regionally or cross-regionally, all network traffic is directed to a private endpoint. This means that your keys encrypt and decrypt resources over a private network that's inaccessible to the Internet. For information about private endpoints, see [Secure access to services using service endpoints](/docs/account?topic=account-service-endpoints-overview). You can see a list of private endpoints in the UI by going to the Resource List and clicking on a Key Protect or HPCS instance. In the left-hand menu, select **Endpoints**.

### About encrypted custom images
{: #byok-about-encrypted-images}

You can create a QCOW2 custom image that meets the requirements for {{site.data.keyword.vpc_short}} infrastructure and [encrypt it](/docs/vpc?topic=vpc-create-encrypted-custom-image#manually-encrypt-image) by using your own LUKS passphrase and root key. After you encrypt the custom image with your own passphrase, you upload it to {{site.data.keyword.cos_full_notm}}.

To import an encrypted custom image to {{site.data.keyword.vpc_short}}, begin by setting up a key management service (KMS) instance, either {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. In the KMS instance, you create a root key for wrapping (encrypting) the passphrase. Wrapping the passphrase produces a wrapped DEK (WDEK). The WDEK protects the passphrase at import time, keeping your data secure. (The passphrase is stored encrypted at all times in the image service and unwrapped only when a virtual server that uses the encrypted image is started.)

With the required IAM authorizations in place, you then import the encrypted image from {{site.data.keyword.cos_full_notm}}. You import the image to the KMS and specify the cloud resource name (CRN) for your root key stored in the KMS. You also specify the ciphertext for your WDEK, which is the passphrase that you use to encrypt your image, wrapped by the root key.

When you import using the API or UI, you supply a single QCOW2 boot image file that can be 10 GB to 250 GB. This file is private to the account to which it's imported. The region where you import the encrypted image is the region where you create virtual server instances from that image. The encrypted image displays with other custom images that you have permission to use. In the UI, a lock icon next to the name indicates it's an encrypted custom image.

If you import an encrypted image that is not exactly 100 GB virtual disk size and use it to provision an instance, and then create  a custom image from that instance’s boot volume (Image from a volume feature), it will be blocked on that instance. (A `POST /images` call returns an error.)
{: note}

When it's time to provision a new virtual server instance with the encrypted image, no other encryption information is needed. The WDEK and the CRN of the root key are stored as metadata with the image. The WDEK is used to access the encrypted image when a virtual server instance that uses the encrypted image is started.

At provision time, data in the instance's boot volume is also encrypted by using the same root key that the custom image uses along with a generated passphrase. Alternatively, you can provide a different root key for the boot volume.

After the virtual server instance is created you can view the custom image and see the root key CRN. You can also [rotate the root key](/docs/vpc?topic=vpc-vpc-key-rotation) and be notified by the KMS when your encrypted image WDEK is automatically re-wrapped (re-encrypted).

Any secondary volumes you create and attach from the encrypted custom image use a different passphrase than the base custom image. Optionally, you can provide a different root key for secondary volumes created during instance provisioning.  

For more information about creating custom images, see [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image). Also see information for creating a [Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image) and [Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image).

## Supported key management services for customer-managed encryption
{: #kms-for-byok}

Two IBM key management services (KMS) are available for customer-managed encryption, {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} (available in certain [regions](/docs/hs-crypto?topic=hs-crypto-regions#regions)). These services use a common key provider API to provide a consistent approach for managing your encryption keys.

Table 1 describes these services:

| Key Management Service | HSM Encryption Certification | Description |
| ----- | ----- | ----- |
| [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect/concepts?topic=key-protect-getting-started-tutorial) | FIPS 140-2 *Level 3* compliance | A multi-tenant KMS that lets you import or create your root keys and securely manage them.  |
| [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started) | FIPS 140-2 *Level 4* compliance | Highest level of security; a single-tenant KMS and hardware security module (HSM) that's controlled by you.  Import or create your root keys and securely manage them. Create a HSM master key to encrypt the content of key storage, including root keys. Only you have access to your keys and data. |
{: caption="Table 1. Available key management service options" caption-side="bottom"}

You might see Key Protect described as _BYOK_, "bring your own key" and HPCS as _KYOK_, or "keep your own key". These are similar services. Because HPCS includes an HSM, it's called KYOK.
{: note}

## General procedure for setting up customer-managed encryption
{: #byok-general-prodedure}

Setting up customer-managed encryption for your block storage volumes involves several steps. Here's the general procedure:

1. Generate your own root key from your on-premises HSM. Optionally, create your root key by using an IBM Cloud HSM or use {{site.data.keyword.hscrypto}}, which includes an HSM.
1. Provision the key management service (KMS) that best meets your needs.
1. Use your KMS to securely import your root key to the cloud service. For added security, create an import token in your KMS to encrypt and import root keys to the service.
1. {{site.data.keyword.cloud_notm}} data centers provide a dedicated HSM to create and protect your keys. By using {{site.data.keyword.hscrypto}}, you can take control of your cloud data encryption keys and cloud hardware security module.
1. From IBM {{site.data.keyword.iamshort}} (IAM), authorize service between Cloud Block Storage (source service) and your KMS (target service). For custom images, also authorize between Image Service for VPC (source service) and IBM Cloud Object Storage (target service).
1. The {{site.data.keyword.cloud_notm}} VPC infrastructure uses your root key to secure the data by wrapping the passphrases for volumes or custom images. For custom images, your root key wraps your passphrase before you import the custom image file to the cloud. For more information, see [About encrypted custom images](#byok-about-encrypted-images).
1. Manage your keys in your KMS. For example, you can [disable](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys) your root key to suspend its encryption and decryption operations. You can also [rotate your customer-managed root keys](/docs/vpc?topic=vpc-vpc-key-rotation). Rotating your root keys lets you roll out a new key across the resources that were protected by the old key. 

## Flow diagrams for specifying customer-managed encryption for volumes
{: #byok-flow-diagrams-volumes}

You can specify customer-managed encryption when you create a new block storage volume during instance provisioning. You can also specify it when you create a volume outside instance provisioning.

Figure 1 shows the procedure for creating a data volume with customer-managed encryption during instance provisioning. The volume is automatically attached to the instance.

![Figure showing the procedure for creating an instance with a customer-managed encrypted volume.](/images/vpc_flowchart_instances_color.png "Figure showing the procedure for creating an instance with a customer-managed encrypted volume"){: caption="Figure 1. Creating an instance with a customer-managed encrypted volume" caption-side="bottom"}

Figure 2 shows the procedure for creating a standalone volume and then attaching it to an instance.

![Figure showing the procedure for creating block storage volumes with customer-managed encryption.](/images/vpc_flowchart_volumes_color.png "Figure showing the procedure for creating block storage volumes with customer-managed encryption"){: caption="Figure 2. Creating a block storage volume with customer-managed encryption" caption-side="bottom"}

## Flow diagram for specifying customer-managed encryption for custom images
{: #byok-flow-diagram-images}

Figure 3 shows the procedure for encrypted custom images using your own encryption keys.

![Figure showing the procedure for creating encrypted custom images.](/images/vpc-flowchart-encrypted-images-color.png "Figure showing the procedure for creating encrypted custom images"){: caption="Figure 3. Creating custom images with customer-managed encryption" caption-side="bottom"}

## Next Steps
{: #byok-about-next-steps}

* [Review the checklist](/docs/vpc?topic=vpc-vpc-encryption-planning#planning-for-data-encryption) for planning data encryption and complete the [prerequisites](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs).
* [Create virtual server instances with customer-managed encryption volumes](/docs/vpc?topic=vpc-creating-instances-byok).
* [Create a block storage volume with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
* [Create file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption).
