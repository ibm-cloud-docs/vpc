---

copyright:
  years: 2019, 2022
lastupdated: "2022-11-04"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# Planning data encryption
{: #vpc-encryption-planning}

When you're planning a data encryption strategy for your {{site.data.keyword.block_storage_is_short}} volumes, file shares, or custom images, you might find this checklist helpful to choose and set up your data encryption service.
{: shortdesc}

## Planning for data encryption
{: #planning-for-data-encryption}

Consider the following prerequisites before you set up data encryption for your VPC resources.

|        Considerations|
|-------------------|
|__ Evaluate the amount of control you want over your data encryption. [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-provider-managed-encryption) is provided by default for boot volumes, data volumes, and file shares.  [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption) lets you control access to your data. |
|__ For encrypted custom images, review the image requirements, supported operating systems, and learn about creating and importing QCOW2 custom image files. For additional planning tips for custom images, see [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images). |
|__ Choose the UI, CLI, or API for provisioning customer-managed encryption. |
|__ Evaluate which [key management service](#byok-encryption-prereqs) best meets your needs. Determine the availability of these services in your region and zone. |
|__ When setting up authorization to the KMS, determine whether your account lets you authorize access:  \n For Cloud Block Storage as the [source service](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-volumes-prereqs). Lite accounts must [upgrade to a Pay-As-You-Go](/docs/account?topic=account-upgrading-account#upgrade-paygo) account or a Subscription account. For more information, see IBM Cloud [account types](/docs/account?topic=account-accounts).  \n  \n For File Storage for VPC, specify VPC Infrastructure Services under (source service), check the box (Resource type), and choose File Storage for VPC and and Key Protect (target service).  \n  \n For custom images, authorize between Image Service for VPC (source service) and IBM Cloud Object Storage (target service). Specify reader access for the role.  \n  \n For all VPC Source services, do not filter by resource group. Do not select the resource group check box.|
|__ For customer-managed encryption, consider importing or creating multiple root keys and [rotating your keys](/docs/vpc?topic=vpc-vpc-key-rotation) for greater security. |
|__ If you're provisioning an instance with encrypted volumes, make sure that you [created a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc). |
|__ Decide how many secondary volumes you require and how many will use customer-managed encryption. |
|__ Choose the secondary volume [profile](/docs/vpc?topic=vpc-block-storage-profiles) that best meets your needs. **IOPS** profiles offer pre-defined performance; **Custom** profiles let you independently choose from a range of capacity and IOPS for your volumes. For [file shares](/docs/vpc?topic=vpc-file-storage-profiles), you have similar profile options. |
|__ Make sure you have a unique name for your virtual server instances, volumes, and file shares. For example, if you have a method for naming volumes with customer-managed encryption, it's much easier to filter and search for them later. |
|__ Determine how long you want to retain the resource and whether you might want to [make the data inaccessible](/docs/vpc?topic=vpc-vpc-encryption-managing#instance-byok-inaccessible-data) for any reason. |
{: caption="Table 1. Checklist for planning data encryption" caption-side="top"}

## Prerequisites for setting up customer-managed encryption
{: #byok-encryption-prereqs}

Complete the following prerequisites to set up customer-managed encryption for your VPC resources. 

### Block and file storage prerequisites
{: #byok-volumes-prereqs}

Provision a key management service (KMS) and authorize access between your VPC resource and KMS.

The following steps are specific to {{site.data.keyword.keymanagementserviceshort}}, but the general flow also applies to {{site.data.keyword.hscrypto}}. If you're using {{site.data.keyword.hscrypto}}, see the [{{site.data.keyword.hscrypto}} information](/docs/hs-crypto?topic=hs-crypto-get-started) for corresponding instructions.
{: note}

1. Provision the [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-provision) service.

   Provisioning a new {{site.data.keyword.keymanagementserviceshort}} service instance ensures that it includes the most recent updates that are required for customer managed encryption of your resources.
   {: tip}

1. [Create](/docs/key-protect?topic=key-protect-create-root-keys) or [import](/docs/key-protect?topic=key-protect-import-root-keys) a customer root key (CRK) in {{site.data.keyword.keymanagementservicelong_notm}}.

   Plan ahead for importing keys by [reviewing your options for creating and encrypting key material](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead). For added security, you can enable the secure import of the key material by using an [import token](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens) to encrypt your key material before you bring it to the cloud.
   {: tip}

1. From IBM {{site.data.keyword.iamshort}} (IAM), [authorize access](/docs/account?topic=account-serviceauth#serviceauth) between Cloud Block Storage or Cloud File Storage (source service) and {{site.data.keyword.keymanagementserviceshort}}(target service) for volumes. For custom images, authorize between Image Service for VPC (source service) and IBM Cloud Object Storage (target service). Specify _reader_ access for the role.

    You might need to upgrade your account to a Pay-as-you-go account to complete this set. For more information, see[Upgrading to a Pay-As-You-Go account](/docs/account?topic=account-upgrading-account#upgrade-paygo).
    {: tip}

### Encrypted custom image prerequisites
{: #byok-custom-images-prereqs}

If you plan to import an encrypted custom image, there are several prerequisites you need to complete in addition to setting up your KMS and authorizations. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs).

## Next Steps
{: #next-steps-planning}

* [Create virtual server instances with customer-managed encrypted volumes](/docs/vpc?topic=vpc-creating-instances-byok).
* [Create standalone volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
* [Create custom encrypted images](/docs/vpc?topic=vpc-create-encrypted-custom-image).
