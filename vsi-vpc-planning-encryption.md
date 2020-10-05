---

Copyright:
  years: 2019, 2020
lastupdated: "2020-10-02"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, Key Protect, encryption, key management, Hyper Protect Crypto Services, HPCS, volume, data storage, virtual server instance, instance, customer-managed encryption, planning, best practices

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Planning data encryption
{: #vpc-encryption-planning}

When you're planning a data encryption strategy for your {{site.data.keyword.block_storage_is_short}} volumes or custom images, you might find this checklist helpful to choose and set up your data encryption service.
{:shortdesc}

## Planning for data encryption
{: #planning-for-data-encryption}

Consider the following prerequisites before you set up data encryption for your VPC resources.

|        Considerations|
|-------------------|
|__ Evaluate the amount of control you want over your data encryption. [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-provider-managed-encryption) is provided by default for boot and data volumes.  [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption) lets you control access to your data. |
|__ Evaluate whether a combination of IBM-managed encrypted volumes and customer-managed encrypted volumes meets your needs.  |
|__ For encrypted custom images, review the image requirements, supported operating systems, and learn about creating and importing QCOW2 custom image files. For additional planning tips for custom images, see [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images). |
|__ Choose the UI, CLI, or API for provisioning customer-managed encryption. |
|__ Evaluate which [key management service](#byok-encryption-prereqs) best meets your needs. Determine the availability of these services in your region and zone. | 
|__ For customer-managed encryption, consider importing or creating multiple root keys and [rotating your keys](/docs/vpc?topic=vpc-vpc-key-rotation) for greater security. |
|__ If you're provisioning an instance with encrypted volumes, make sure that you [created a VPC](/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc). |
|__ Decide how many secondary volumes you require and how many will use customer-managed encryption. |
|__ Choose the secondary volume [profile](/docs/vpc?topic=vpc-block-storage-profiles) that best meets your needs. **IOPS** profiles offer pre-defined performance; **Custom** profiles let you independently choose from a range of capacity and IOPS for your volumes. |
|__ Make sure you have a unique name for your virtual server instances and volumes. For example, if you have a method for naming volumes with customer-managed encryption, it's much easier to filter and search for them later. |
|__ Determine how long you want to retain a volume and whether you might want to [make the data inaccessible](/docs/vpc?topic=vpc-vpc-encryption-managing#instance-byok-inaccessible-data) for any reason. |
{: caption="Table 1. Checklist for planning data encryption" caption-side="top"}

## Prerequisites for setting up customer-managed encryption
{: #byok-encryption-prereqs}

Complete the following prerequisites to set up customer-managed encryption for your VPC resources. 

### Block storage volume prerequisites
{: #byok-volumes-prereqs}

Provision a key management service (KMS) and authorize access between **Cloud Block Storage** and your KMS.

The following steps are specific to {{site.data.keyword.keymanagementserviceshort}}, but the general flow also applies to {{site.data.keyword.hscrypto}}. If you're using {{site.data.keyword.hscrypto}}, see the [{{site.data.keyword.hscrypto}} information](/docs/hs-crypto?topic=hs-crypto-get-started) for corresponding instructions.
{:note}

1. Provision the [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-provision) service.
   
   Provisioning a new {{site.data.keyword.keymanagementserviceshort}} service instance ensures that it includes the most recent updates that are required for customer managed encryption of block storage volumes.
   {: tip}
   
2. [Create](/docs/key-protect?topic=key-protect-create-root-keys) or 
[import](/docs/key-protect?topic=key-protect-import-root-keys) a customer root key (CRK) in
{{site.data.keyword.keymanagementservicelong_notm}}.

   Plan ahead for importing keys by [reviewing your options for creating and encrypting key material](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead). For added security, you can enable the secure import of the key material by using an [import token](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens) to encrypt your key material before you bring it to the cloud.
   {: tip}
   
3. From IBM {{site.data.keyword.iamshort}} (IAM), [authorize access](/docs/account?topic=account-serviceauth#serviceauth) between **Cloud Block Storage** (source service) and **{{site.data.keyword.keymanagementserviceshort}}** (target service) for volumes. Specify _reader_ access for the role.

### Encrypted custom image prerequisites
{: #byok-custom-images-prereqs}

If you plan to take advantage of this functionality to import an encrypted custom image, there are several prerequisites you need to complete in addition to setting up your KMS and authorizations. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs).

## Next Steps
{: #next-steps-planning}

* [Creating virtual server instances with customer-managed encrypted volumes](/docs/vpc?topic=vpc-creating-instances-byok).
* [Create standalone volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).
* [Create custom encrypted images](/docs/vpc?topic=vpc-create-encrypted-custom-image).
