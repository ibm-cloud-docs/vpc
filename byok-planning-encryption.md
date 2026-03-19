---

copyright:
  years: 2019, 2026
lastupdated: "2026-03-19"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning data encryption
{: #vpc-encryption-planning}

When you're planning a data encryption strategy for your {{site.data.keyword.block_storage_is_short}} volumes, snapshots, {{site.data.keyword.filestorage_vpc_short}} shares, or custom images, you might find this checklist helpful.
{: shortdesc}

## Planning for data encryption
{: #planning-for-data-encryption}

Consider the following prerequisites before you set up data encryption for your VPC resources.

|     Considerations|
|-------------------|
|__ Evaluate the amount of control that you want over your data encryption. [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-provider-managed-encryption) is provided by default for boot volumes, data volumes, and file shares. With [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), you own the encryption keys and control the encryption process. |
|__ For encrypted custom images, review the image requirements, supported operating systems, and learn about creating and importing QCOW2 custom image files. For more information, see [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images). |
|__ Evaluate which [key management service](#byok-encryption-prereqs) best meets your needs. Determine the availability of these services in your region and zone. For {{site.data.keyword.keymanagementserviceshort}}, choose between **Standard** (multi-tenant with FIPS 140-2 Level 3) or **Dedicated** (single-tenant with FIPS 140-3 Level 4). Consider regulatory requirements, data sensitivity, budget, and whether you need complete control over the root of trust. For more information, see [About Standard and Dedicated {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about). |
|__ Determine whether your account can authorize access:  \n For Cloud Block Storage as the [source service](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-volumes-prereqs), Lite accounts must [upgrade to a Pay-As-You-Go](/docs/account?topic=account-upgrading-account#upgrade-paygo) account or a Subscription account. For more information, see IBM Cloud [account types](/docs/account?topic=account-accounts). \n \n For {{site.data.keyword.filestorage_vpc_short}}, specify VPC Infrastructure Services under (source service), check the box (Resource type), and choose {{site.data.keyword.filestorage_vpc_short}} and {{site.data.keyword.keymanagementserviceshort}} (target service). \n \n For custom images, authorize access between Image Service for VPC (source service) and {{site.data.keyword.cos_full_notm}} (target service). Specify reader access for the role. \n \n For all VPC Source services, do not filter by resource group. Do not select the resource group checkbox.|
|__ For customer-managed encryption, consider importing or creating multiple root keys and [rotating your keys](/docs/vpc?topic=vpc-vpc-key-rotation) for greater security.|
|__ Make sure you have a unique name for your virtual server instances, volumes, and file shares. For example, if you have a method for naming volumes with customer-managed encryption, it's much easier to filter and search for them later.|
|__ Determine how long you want to retain the resource and whether you might want to [make the data inaccessible](/docs/vpc?topic=vpc-vpc-encryption-managing#instance-byok-inaccessible-data) for any reason.|
{: caption="Checklist for planning data encryption" caption-side="top"}

## Prerequisites for setting up customer-managed encryption
{: #byok-encryption-prereqs}

Complete the following prerequisites to configure customer-managed encryption for your VPC resources.

### General setup procedure
{: #byok-setup-procedure}

Setting up customer-managed encryption for your VPC resources involves the following steps:

1. **Generate or create your root key** - You can use your on-premises HSM to generate a root key, or create one by using an {{site.data.keyword.cloud_notm}} key management service.

   {{site.data.keyword.cloud_notm}} data centers provide a dedicated HSM to create and protect your keys. By using {{site.data.keyword.hscrypto}}, you can take control of your cloud data encryption keys and cloud hardware security module.
   {: tip}

2. **Provision a key management service (KMS)** - Choose the KMS that best meets your needs and provision an instance.

3. **Import your root key** - Use your KMS to securely import your root key to the cloud service. For added security, create an import token in your KMS to encrypt and import root keys to the service.

4. **Authorize service access** - From {{site.data.keyword.iamlong}} (IAM), authorize service between your VPC resource service (such as Cloud Block Storage or Cloud File Storage) and your KMS. For custom images, also authorize between Image Service for VPC (source service) and {{site.data.keyword.cos_full_notm}} (target service).

5. **Create encrypted resources** - When you create volumes, file shares, or import custom images, specify your root key. The {{site.data.keyword.cloud_notm}} VPC infrastructure uses your root key to secure the data by wrapping the passphrases.

6. **Manage your keys** - After setup, you can manage your keys by disabling, enabling, rotating, or deleting them. For more information, see [Managing data encryption](/docs/vpc?topic=vpc-vpc-encryption-managing).

### Block and file storage prerequisites
{: #byok-volumes-prereqs}

Provision a key management service (KMS), and authorize access between your VPC resource and KMS.

1. When you provision a KMS, you can choose between [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-getting-started-tutorial) Single Tenant and Multi Tenant instances, and [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started). Follow the linked tutorials to provision a service instance, and create or import a customer root key.

   For {{site.data.keyword.keymanagementserviceshort}}, choose your deployment option:
   - **Standard**: Multi-tenant service with FIPS 140-2 Level 3 compliance. Follow the [provisioning tutorial](/docs/key-protect?topic=key-protect-provision) to create an instance and manage root keys.
   - **Dedicated**: Single-tenant service with FIPS 140-3 Level 4 compliance. It requires CLI-based initialization. Follow the [Dedicated initialization guide](/docs/key-protect?topic=key-protect-st-init-cli) to create an instance, generate admin credentials, and create your master key.

   For guidance on choosing between Standard and Dedicated, see [When to use Standard vs Dedicated {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about#comparison-when-standard).


1. From IBM {{site.data.keyword.iamshort}} (IAM), [authorize access](/docs/account?topic=account-serviceauth#serviceauth) between Cloud Block Storage or Cloud File Storage (source service) and the target KMS service ({{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}). For more information, see [Establish service-to-service authorizations for File Storage for VPC](/docs/vpc?topic=vpc-file-s2s-auth) and [Establishing service-to-service authorizations for {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-s2s-auth).

    You might need to upgrade your account to a Pay-as-you-go account to complete this set. For more information, see [Upgrading to a Pay-As-You-Go account](/docs/account?topic=account-upgrading-account#upgrade-paygo).
    {: tip}

### Encrypted custom image prerequisites
{: #byok-custom-images-prereqs}

If you plan to import an encrypted custom image, follow the instructions in [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs).

## Next Steps
{: #next-steps-planning}

* [Creating Block Storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
* [Creating file shares with customer-managed encryptions](/docs/vpc?topic=vpc-file-storage-byok-encryption).
* [Create custom encrypted images](/docs/vpc?topic=vpc-create-encrypted-custom-image).
