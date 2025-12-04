---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-04"

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

{{site.data.keyword.block_storage_is_short}} provides primary boot volumes and secondary data volumes. Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning, or as stand-alone volumes that you can later attach to an instance. To protect your data, you can use your own encryption keys or choose IBM-managed encryption.

You pay for only the capacity that you need. When you use the SSD Defined Performance (`sdp`) volume profile, you can increase the size of your data and boot volumes up to 32,000 GB. The maximum IOPS that a volume with the `sdp` profile can support is 64,000. You can also modify the throughput limit in the range of 125-1024 MBps (1000-8192 Mbps). Capacity, IOPS, and throughput values of volumes that are created with the `sdp` profile can be modified even when the volume is not attached to a virtual server instance.

The capacity of volumes that are created with the [traditional profiles](#block-storage-profiles-intro) ranges from 10 GB up to 16,000 GB. For data volumes that are attached to a virtual server instance, you can [increase volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) in GB increments up to 16,000 GB capacity, depending on your volume profile. You can also [increase or decrease IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) for a volume that is attached to an instance.

{{site.data.keyword.block_storage_is_short}} supports all [virtual server profiles](/docs/vpc?topic=vpc-block-storage-profiles#vsi-profiles-relate-to-storage).

{{site.data.keyword.block_storage_is_short}} volume data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

## {{site.data.keyword.block_storage_is_short}} volume types
{: #block-storage-vpc-volumes}

{{site.data.keyword.block_storage_is_short}} offers block-level volumes that are attached to an instance as a boot volume when the instance is created or attached as secondary data volumes. You can configure up to 300 {{site.data.keyword.block_storage_is_short}} volumes per account in a region. You can request to increase this quota by [opening support case](/docs/vpc?topic=vpc-manage-storage-limit) and specifying in which zone you need more volumes.

You can attach only one boot volume to a virtual server instance at a time, but you can attach up to 12 {{site.data.keyword.block_storage_is_short}} data volumes to a single instance. For other limits, see [Volume attachment limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits).

### Boot volumes
{: #block-storage-vpc-boot-volumes}

When you create an instance with a stock image, a 100 GB boot volume is created and attached to the instance by default. When you create an instance from a custom image, you can specify a boot volume capacity of 10 GB to 250 GB, depending what the image requires.

When you create an instance from a custom image, you can specify a boot volume capacity of 10 GB to 250 GB, depending what the image requires. If the custom image is smaller than 10 GB, the boot volume capacity is rounded up to 10 GB. If the boot volume exceeds 250 GB, the virtual server instance fails to successfully boot.

If you use the console to provision the instance, you can select between the `general-purpose` and the `sdp` profile before the instance is created. When you provision a boot volume from the CLI, with the API, or Terraform, you can specify any of the profiles from the `tiered`, `custom`, or `defined performance` volume profile families.

After the boot volume is created, you can expand the boot volume size to the maximum supported size, which is 250 GB for first-generation `tiered` or `custom` volumes and 32,000 GB for the second-generation `sdp` volumes. However, if you increase your boot volume over 250 GB, it can no longer be imaged or used to boot another virtual server instance.
{: important}

By default, boot volumes are encrypted by IBM-managed encryption. Optionally, you can use your own root keys (CRKs) by choosing customer-managed encryption during instance creation (see [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption)).

By default, boot volumes are deleted when you delete an instance. You can change the auto-delete setting in the console, from the CLI, and with the API. A boot volume can be unattached only by deleting the instance that it is attached to. A boot volume cannot be detached from an instance while the instance exists. For more information, see [Managing Block Storage for VPC volumes](/docs/vpc?topic=vpc-managing-block-storage).

### Data volumes
{: #secondary-data-volumes}

{{site.data.keyword.block_storage_is_short}} data volumes are secondary volumes with total capacity range of 10 GB to 16,000 GB for first-generation volumes and 1 GB to 32,000 GB for second-generation volumes. Maximum IOPS for data volumes varies based on volume size and profile. For more information, see
[{{site.data.keyword.block_storage_is_short}} profiles](/docs/vpc?topic=vpc-block-storage-profiles).

You can create data volumes as stand-alone volumes or when you provision an instance. Stand-alone volumes exist in an unattached state until you attach the volume to an instance. When you create a data volume as part of instance provisioning, the volume is automatically attached to the instance.

Data volumes are encrypted by default with IBM-managed encryption. You can also encrypt data volumes by using your own root keys. You can use {{site.data.keyword.hpvs}} or {{site.data.keyword.keymanagementservicefull_notm}} as your key management service.

{{site.data.keyword.block_storage_is_short}} data volumes can be attached to any available instance in your zone, based on your customer account and permissions, and within [certain limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits). 

When the instance is deleted, these volumes are detached by default. Detaching by default allows your data to persist beyond the virtual server instance lifecycle. Only the volume's association with the instance is removed. Detached volumes can be attached to an available, running instance without reprovisioning the volume or the instance. Or you can delete data volumes manually after they are detached.

When you create volumes, you can specify whether you want the data volumes to be deleted when then instance is deleted. You can enable and disable the auto-delete feature in the console, from the CLI or with the API.

You can also increase the size of an attached volume in the console, from the CLI, or with the API. For first-generation volumes, you can increase the capacity in GB increments up to 16,000 GB capacity, depending on your volume profile. For second-generation volumes that use the `sdp` profile, you can increase the capacity up to 32,000 GB. For more information, see [expanding {{site.data.keyword.block_storage_is_short}} volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) and [Managing Block Storage for VPC volumes](/docs/vpc?topic=vpc-managing-block-storage).

## {{site.data.keyword.block_storage_is_short}} volume profiles
{: #block-storage-profiles-intro}

Volume profiles define the capacity and performance characteristics of storage volumes. So you can choose the best option for your specific needs, whether the volume is meant for general use or high-performance workloads.

### The SSD defined performance profile
{: #block-storage-sdp-intro}

The SSD Defined Performance profile is available in the Dallas (`us-south`), Frankfurt (`eu-de`), London (`eu-gb`), Madrid (`eu-es`), Osaka (`jp-osa`), Sao Paulo (`br-sao`), Sydney (`au-syd`), Tokyo (`jp-tok`), Toronto (`ca-tor`), and Washington (`us-east`) regions. You can specify custom capacity, custom throughput limit, and custom IOPS for your second-generations volumes.

The following limitations apply to this release:

* The `sdp` profile is available for use with 2nd and 3rd generation Compute resources only.
* Secure boot from `sdp` boot volume is not supported. If you want to provision a virtual server instance with either [Intel Gen 3](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen3-intel) or [Confidential Computing](/docs/vpc?topic=vpc-confidential-computing-vsi-profiles-gen3-x86) instance profiles, and an `sdp` boot volume, make sure that you do not enable secure boot.
* No support for use as boot volume with [Accelerated instance profiles - Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family).
* No support for the IBM Z platform (s390x architecture) or {{site.data.keyword.bm_is_short}}.
* Importing custom encrypted images are not supported.
* Creating a custom image from a boot volume with customer-managed encryption is not supported.
* Migration between storage volume profile families is not supported.
* Migration of volumes across zones is not supported.

### Traditional volume profiles
{: #block-storage-gen1-profiles}

When you create a {{site.data.keyword.block_storage_is_short}} volume in your availability zone, you can use 3 different tiered profiles with predefined IOPS levels. Or you can select a custom profile and define your own IOPS level based on the volume capacity. All profiles are backed by solid-state drives (SSDs).

For more information, see [{{site.data.keyword.block_storage_is_short}} profiles](/docs/vpc?topic=vpc-block-storage-profiles).

## Securing your data
{: #bs-data-security}

{{site.data.keyword.cloud}} offers security-specific tools and features to help you securely manage your data when you use {{site.data.keyword.vpc_full}}. The following section provides information about access control, data encryption, configuration management, and auditing options that are available for your storage volumes.

### IAM roles for creating and managing volumes
{: #block-storage-vpc-iam}

{{site.data.keyword.block_storage_is_short}} require IAM permissions for role-based access control. Depending on your assigned role, you can create and manage volumes. For more information, see [IAM roles and actions for Block Storage for VPC](/docs/account?topic=account-iam-service-roles-actions#is.volume-roles).

For more information, see the [best practices for assigning access](/docs/account?topic=account-account_setup#account_setup). For the complete IAM process, which includes inviting users to your account and assigning Cloud IAM access, see the [IAM getting started tutorial](/docs/account?topic=account-iamoverview).
{: tip}

### IAM service-to-service authorizations
{: #block-storage-vpc-iam-s2sauth}

You can use the {{site.data.keyword.iamshort}} (IAM) to create or remove an authorization that grants one service access to another service. For {{site.data.keyword.block_storage_is_short}}, you need to create service-to-service authorization for configuring customer-managed encryption, and backups. For more information, see [Establishing service-to-service authorizations for {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-s2s-auth).

### Context-based restrictions
{: #block-storage-vpc-cbr}

You can enable context-based restrictions (CBR) for block volume operations. These restrictions work with traditional IAM policies, which are based on identity, to provide an extra layer of protection. Unlike IAM policies, context-based restrictions don't assign access. Context-based restrictions check that an access request comes from an allowed context that you configure, such as creating a data volume. For more information, see [Protecting Virtual Private Cloud (VPC) Infrastructure Services with context-based restrictions](/docs/vpc?topic=vpc-cbr).

### Encryption at rest and in transit
{: #vpc-storage-encryption}

{{site.data.keyword.cloud_notm}} takes the need for security seriously and understands the importance of being able to encrypt data to keep it safe. All block storage volumes are encrypted at rest with IBM-managed encryption by default. For more information about the industry standard protocols {{site.data.keyword.cloud}} follows for data encryption, see [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui&q=IBM-managed+encryption&tags=vpc#vpc-provider-managed-encryption).

You can also choose to protect your volumes by creating an envelop encryption with your own root keys that are stored in one of the approved Key Management Systems (KMS). In {{site.data.keyword.cloud_notm}}, the KMS can be either located in the same or in another account as the service that is using an encryption key. This deployment pattern allows enterprises to centrally manage encryption keys for all corporate accounts.

Your data is protected while at rest, and also in transit from the storage to the hypervisor and host. After you set up the encryption type for a boot or data volume, you can't change it.

For more information about data encryption, see [About data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about).

### Activity tracking events
{: #bs-activity-tracking-events}

You can use {{site.data.keyword.atracker_full}} to configure how to route auditing events. Auditing events are critical data for security operations and a key element for meeting compliance requirements. Such events are triggered when you create, modify, or delete a block volume. For more information, see [Activity tracking events for IBM Cloud VPC](/docs/vpc?topic=vpc-at_events).

## Tags for {{site.data.keyword.block_storage_is_short}} volumes
{: #storage-about-tags}

{{site.data.keyword.block_storage_is_short}} is enabled for Global Searching and Tagging (GhoST). You can create and apply user tags and access management tags to volumes to better control and organize your {{site.data.keyword.block_storage_is_short}} resources across the VPC. 

You can also apply tags to your boot and data volumes anytime in the console, from the CLI, with the API, and Terraform. Each resource can have up to 1000 user tags, and no more than 250 access tags. However, only 100 tags can be attached or detached in the same operation.

### User tags
{: #storage-about-user-tags}

User tags are uniquely identified by a Cloud Resource Name (CRN) identifier. When you create a user tag, you provide a unique name within your billing account. You can define user tags in label or key-value format.

You can [add user tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#add-user-tags-volumes-ui) when you create a {{site.data.keyword.block_storage_is_short}} volume or when you update an existing volume. You can also add tags to a boot or data volume during [instance creation](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-from-vsi). For boot volumes, you edit the boot volume to add tags. For data volumes, you can add tags when you create and attach them to the instance, or add them to existing data volumes. Volume user tags are also used by [backup policies](/docs/vpc?topic=vpc-backup-service-about) to create automatic backup snapshots of the volume. Create, view, and manage user tags from the UI, CLI, API, or Terraform, and remove then at any time.

### Access management tags
{: #storage-about-mgt-tags}

[Access management tags](/docs/vpc?topic=vpc-iam-getting-started&interface=ui) help organize access control by creating flexible resource groupings, enabling your {{site.data.keyword.block_storage_is_short}} resources to grow without requiring updates to IAM policies.

You can create access management tags and then apply them to new or existing volumes. Use the IAM UI or the Global Search and Tagging API to create the access management tag. Then, from the VPC UI or API, add access management when you create a volume or modify an existing volume. You can't add these tags to volumes that are created during instance provisioning. After you add the access management tags, you can manage access to them by using the IAM policies. For more information, see [Apply access management tags to a {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags).

For more information about managing tags for your account, see [Working with tags](/docs/account?topic=account-tag&interface=ui).

## Block storage snapshots
{: #vpc-storage-snapshots}

Block Storage Snapshots for VPC are point-in-time copies of your {{site.data.keyword.block_storage_is_short}} boot or data volumes. To protect your data in the unlikely event of a zone or region failure, consider Block Storage Snapshots for VPC. By scheduling snapshots at regular intervals, data can be replicated either to another zone in the same region, or cross-region, so that a copy is available in another region. Snapshots can also be cached for fast restore. With fast restore, you can achieve a recovery time objective that is faster than restoring from a regular snapshot. Snapshots can be shared with other accounts to create volumes in their VPCs. For more information about Block Storage Snapshots for VPC, see [About Block Storage Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about) and [Planning snapshots](/docs/vpc?topic=vpc-snapshots-vpc-planning).

## {{site.data.keyword.block_storage_is_short}} data eradication
{: #vpc-cancel-storage}

If you no longer need a volume, you can delete it at any time. {{site.data.keyword.IBM_notm}} guarantees that data that you deleted cannot be accessed, and that deleted data is eventually overwritten and eradicated. 

When you delete a {{site.data.keyword.block_storage_is_short}} volume, those blocks must be overwritten before that {{site.data.keyword.block_storage_is_short}} is made available again, either to you or to another customer.

Further, when {{site.data.keyword.IBM_notm}} decommissions a physical drive, the drive is destroyed before disposal. Decommissioned drives are unusable and any data on them is inaccessible. For more information, see [Deleting a {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication). 

If you have extra compliance requirements such as NIST 800-88 Guidelines for Media Sanitization, you must perform data sanitation procedures before you delete your volumes. For more information, see [Sanitize your data before you delete a volume](/docs/vpc?topic=vpc-managing-block-storage#block-storage-sanitization).

## Next steps
{: #block-storage-about-next-steps}

Create your {{site.data.keyword.block_storage_is_short}} volumes.

* For more information about creating a volume during instance provisioning, see [Create and attach a {{site.data.keyword.block_storage_is_short}} volume when you create an instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi).
* For more information about creating a {{site.data.keyword.block_storage_is_short}} encrypted by your own encryption keys, see [Creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

For more information about creating and managing instances in the VPC, see [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers).

When you create, [view](/docs/vpc?topic=vpc-viewing-block-storage), or [update a {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-managing-block-storage), or [restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore), the volume health state is reported in the console, CLI, and the API. For more information, see [{{site.data.keyword.block_storage_is_short}} volume health states](/docs/vpc?topic=vpc-block-storage-vpc-monitoring#block-storage-vpc-health-states).

{{site.data.keyword.block_storage_is_short}} provides features that are unique to the VPC and are not compatible with the classic infrastructure storage. If you're interested in {{site.data.keyword.blockstoragefull}} on the classic infrastructure, see [{{site.data.keyword.blockstoragefull}}](/docs/BlockStorage?topic=BlockStorage-getting-started).
{: note}
