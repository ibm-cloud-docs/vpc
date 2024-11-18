---

copyright:
  years: 2019, 2024
lastupdated: "2024-11-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# About {{site.data.keyword.block_storage_is_short}}
{: #block-storage-about}

{{site.data.keyword.block_storage_is_full}} provides high-performance data storage for your virtual server instances that you can provision within an {{site.data.keyword.vpc_full}} (VPC). The [VPC infrastructure](/docs/vpc?topic=vpc-about-vpc) provides rapid scaling across zones and extra performance and security.
{: shortdesc}

## Overview
{: #block-storage-overview}

{{site.data.keyword.block_storage_is_short}} supports all [virtual server profiles](/docs/vpc?topic=vpc-block-storage-profiles#vsi-profiles-relate-to-storage).

{{site.data.keyword.block_storage_is_short}} volume data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

{{site.data.keyword.block_storage_is_short}} provides primary boot volumes and secondary data volumes. Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning as well, or as stand-alone volumes that you can later attach to an instance. To protect your data, you can use your own encryption keys or choose IBM-managed encryption.

You pay for only the capacity that you need. {{site.data.keyword.block_storage_is_short}} capacity ranges from 10 GB up to 16,000 GB for all available [profiles](#block-storage-profiles-intro). For data volumes that are attached to a virtual server instance, you can [increase volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) in GB increments up to 16,000 GB capacity, depending on your volume profile. You can also [increase or decrease IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) for a volume that is attached to an instance.

You can also apply user tags and access management tags to your boot and data volumes anytime. Add tags when you create a volume or update an existing volume with the UI, CLI, API, and Terraform. For more information, see [Tags for {{site.data.keyword.block_storage_is_short}} volumes](#storage-about-tags).

When you create, view, or update a {{site.data.keyword.block_storage_is_short}} volume, or [restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore), the volume health state is reported in the UI, CLI, or API. For more information, see [{{site.data.keyword.block_storage_is_short}} volume health states](/docs/vpc?topic=vpc-block-storage-vpc-monitoring#block-storage-vpc-health-states).

## IAM roles for creating and managing volumes
{: #block-sorage-vpc-iam}

{{site.data.keyword.block_storage_is_short}} require IAM permissions for role-based access control. Depending on your assigned role, you can create and manage volumes. For more information, see [IAM roles and actions for Block Storage for VPC](/docs/account?topic=account-iam-service-roles-actions#is.volume-roles).

For more information, see the [best practices for assigning access](/docs/account?topic=account-account_setup#account_setup). For the complete IAM process, which includes inviting users to your account and assigning Cloud IAM access, see the [IAM getting started tutorial](/docs/account?topic=account-iamoverview).
{: tip}

## {{site.data.keyword.block_storage_is_short}} volume types
{: #block-storage-vpc-volumes}

{{site.data.keyword.block_storage_is_short}} offers block-level volumes that are attached to an instance as a boot volume when the instance is created or attached as secondary data volumes. You can configure up to 300 {{site.data.keyword.block_storage_is_short}} volumes per account in a region. You can request to increase this quota by [opening support case](/docs/vpc?topic=vpc-manage-storage-limit) and specifying in which zone you need more volumes.

You can attach only one boot volume to a virtual server instance at a time, but you can attach up to 12 {{site.data.keyword.block_storage_is_short}} data volumes to a single instance. For other limitations, see [Volume attachment limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits).

### Boot volumes
{: #block-storage-vpc-boot-volumes}

When you create an instance from a stock image, a 100 GB, 3,000 IOPS general-purpose boot volume is created and attached to the instance by default. When you create an instance from a custom image, you can specify a boot volume capacity of 10 GB to 250 GB, depending what the image requires. This capacity can be any size between the minimum size that is supported for the selected image and the maximum supported image size. If the custom image is smaller than 10 GB, the boot volume capacity is rounded up to 10 GB. After the boot volume is created, you can expand the boot volume size to the maximum supported size, which is 250 GB.

You cannot create an image from a boot volume that is encrypted with customer-managed keys and is not 100 GB. Such an operation is not supported.
{: note}

By default, boot volumes are encrypted by IBM-managed encryption. Optionally, you can use your own root keys (CRKs) by choosing customer-managed encryption during instance creation (see [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption)).

By default, boot volumes are deleted when you delete an instance. You can toggle this setting on or off in the instance details. A boot volume can be unattached only by deleting the instance that it is attached to. A boot volume cannot be detached from an instance while the instance exists. For more information, see [Viewing instance details](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#viewing-virtual-server-instances-ui).

### Data volumes
{: #secondary-data-volumes}

{{site.data.keyword.block_storage_is_short}} data volumes are secondary volumes with total capacity range of 10 GB to 16,000 GB. Maximum IOPS for data volumes varies based on volume size. For more information, see
[{{site.data.keyword.block_storage_is_short}} profiles](/docs/vpc?topic=vpc-block-storage-profiles).

You can create data volumes as stand-alone volumes or when you provision an instance. Stand-alone volumes exist in an unattached state until you attach the volume to an instance. When you create a data volume as part of instance provisioning, the volume is automatically attached to the instance.

When you create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance, the data volume that is attached to the instance during instance creation is automatically encrypted with the seed or passphrase that you provide.
{: note}

{{site.data.keyword.block_storage_is_short}} data volumes can be attached to any available instance in your zone, based on your customer account and permissions, and within [certain limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits). These volumes are detached by default when the instance is deleted. Detaching by default allows your data to persist beyond the virtual server instance lifecycle. It removes only the volume's association with the instance. You can delete data volumes manually after they are detached. Also, when you create data volumes, you can specify that they be [automatically deleted](/docs/vpc?topic=vpc-managing-block-storage#auto-delete) when the instance is deleted.

Detached volumes can be attached to an available, running instance without reprovisioning the volume or the instance.

When you create and attach a data volume to a virtual server instance, you can later increase the size of that volume. You indicate capacity in GB increments up to 16,000 GB capacity, depending on your volume profile. For more information, see [expanding {{site.data.keyword.block_storage_is_short}} volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

Data volumes are encrypted by default with IBM-managed encryption. You can also encrypt data volumes by using your own root keys.

### {{site.data.keyword.block_storage_is_short}} volume profiles
{: #block-storage-profiles-intro}

When you create a {{site.data.keyword.block_storage_is_short}} volume in your availability zone, you can use 3 different tiered profiles with predefined IOPS levels. Or you can select a custom profile with which you can define your own IOPS level based on the volume capacity. All profiles are backed by solid-state drives (SSDs).

## {{site.data.keyword.block_storage_is_short}} encryption
{: #vpc-storage-encryption}

{{site.data.keyword.cloud_notm}} takes the need for security seriously and understands the importance of being able to encrypt data to keep it safe. All block storage volumes are encrypted at rest with IBM-managed encryption by default.  

You can also choose to protect your volumes by creating an envelop encryption with your own root keys that are stored in one of the approved Key Management Systems (KMS). In {{site.data.keyword.cloud_notm}}, the KMS can be either located in the same or in another account as the service that is using an encryption key. This deployment pattern allows enterprises to centrally manage encryption keys for all corporate accounts.

Your data is protected while at rest, and also in transit from the storage to the hypervisor and host. After you set up the encryption type for a boot or data volume, you can't change it.

For more information about data encryption, see [About data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about).

## Block Storage Snapshots for VPC
{: #vpc-storage-snapshots}

Block Storage Snapshots for VPC are point-in-time copies of your {{site.data.keyword.block_storage_is_short}} boot or data volumes. To protect your data in the unlikely event of a zone or region failure, consider Block Storage Snapshots for VPC. By scheduling snapshots at regular intervals, data can be replicated either to another zone in the same region, or cross-region, so that a copy is available in another region. Snapshots can also be cached for fast restore. With fast restore, you can achieve a recovery time objective that is faster than restoring from a regular snapshot. Snapshots can be shared with other accounts to create volumes in their VPCs. For more information about Block Storage Snapshots for VPC, see [About Block Storage Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about) and [Planning snapshots](/docs/vpc?topic=vpc-snapshots-vpc-planning).

## Tags for {{site.data.keyword.block_storage_is_short}} volumes
{: #storage-about-tags}

{{site.data.keyword.block_storage_is_short}} is enabled for Global Searching and Tagging (GhoST). You can create and apply [user tags](#storage-about-user-tags) and [access management tags](#storage-about-mgt-tags) to volumes to better control and organize your {{site.data.keyword.block_storage_is_short}} resources across the VPC. Each resource can have up to 1000 user tags, and no more than 250 access tags. However, only 100 tags can be attached or detached in the same operation.

### User tags for {{site.data.keyword.block_storage_is_short}} volumes
{: #storage-about-user-tags}

User tags are uniquely identified by a Cloud Resource Name (CRN) identifier. When you create a user tag, you provide a unique name within your billing account. You can define user tags in label or key-value format.

You can [add user tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#add-user-tags-volumes-ui) when you create a {{site.data.keyword.block_storage_is_short}} volume or when you update an existing volume. You can also add tags to a boot or data volume during [instance creation](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-from-vsi). For boot volumes, you edit the boot volume to add tags. For data volumes, you can add tags when you create and attach them to the instance, or add them to existing data volumes. Volume user tags are also used by [backup policies](/docs/vpc?topic=vpc-backup-service-about) to create automatic backup snapshots of the volume. Create, view, and manage user tags from the UI, CLI, API, or Terraform, and remove then at any time.

### Access management tags for {{site.data.keyword.block_storage_is_short}} volumes
{: #storage-about-mgt-tags}

[Access management tags](/docs/vpc?topic=vpc-iam-getting-started&interface=ui) help organize access control by creating flexible resource groupings, enabling your {{site.data.keyword.block_storage_is_short}} resources to grow without requiring updates to IAM policies.

You can create access management tags and then apply them to new or existing volumes. Use the IAM UI or the Global Search and Tagging API to create the access management tag. Then, from the VPC UI or API, add access management when you create a volume or modify an existing volume. You can't add these tags to volumes that are created during instance provisioning. After you add the access management tags, you can manage access to them by using the IAM policies. For more information, see [Apply access management tags to a {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags).

For more information about managing tags for your account, see [Working with tags](/docs/account?topic=account-tag&interface=ui).

## Managing security and compliance
{: #block-storage-vpc-manage-security}

{{site.data.keyword.block_storage_is_short}} is integrated with the {{site.data.keyword.compliance_short}} to help you manage security and compliance for your organization. You can set up goals that check whether volumes are encrypted by using customer-managed keys. By using the {{site.data.keyword.compliance_short}} to validate the {{site.data.keyword.block_storage_is_short}} configurations in your account against a profile, you can identify potential issues as they arise.

For more information about monitoring security and compliance for VPC, see [Getting started with Security and Compliance Center](/docs/security-compliance?topic=security-compliance-getting-started). For more information about creating security and compliance goals, see [Defining rules](/docs/security-compliance?topic=security-compliance-rules-define&interface=ui) in the Security and Compliance documentation.

## Deleting your {{site.data.keyword.block_storage_is_short}}
{: #vpc-cancel-storage}

If you no longer need a volume, you can delete it at any time. IBM [wipes all data](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication) before the storage is reused. If you have extra compliance requirements such as NIST 800-88 Guidelines for Media Sanitization, you must perform data sanitation procedures before you delete your volumes. For more information, see [Sanitize your data before you delete a volume](/docs/vpc?topic=vpc-managing-block-storage#block-storage-sanitization).

## Next steps
{: #block-storage-about-next-steps}

Create your {{site.data.keyword.block_storage_is_short}} volumes.

* For more information about creating a volume during instance provisioning, see [Create and attach a {{site.data.keyword.block_storage_is_short}} volume when you create an instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi).
* For more information about creating a {{site.data.keyword.block_storage_is_short}} encrypted by your own encryption keys, see [Creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

For more information about creating and managing instances in the VPC, see [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers).

{{site.data.keyword.block_storage_is_short}} provides features that are unique to the VPC and are not compatible with the classic infrastructure storage. If you're interested in {{site.data.keyword.blockstoragefull}} on the classic infrastructure, see [{{site.data.keyword.blockstoragefull}}](/docs/BlockStorage?topic=BlockStorage-getting-started).
{: note}
