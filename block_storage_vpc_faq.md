---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-08"

keywords: faqs, Block Storage for vpc, fast restore, multizone, instance, instance provisioning, volume management, volume deletion.

subcollection: vpc

content-type: faq

---
{{site.data.keyword.attribute-definition-list}}

# FAQ for {{site.data.keyword.block_storage_is_short}}
{: #block-storage-vpc-faq}

The following questions often arise about the {{site.data.keyword.block_storage_is_short}} service. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links.
{: shortdesc}

## Where is defined performance volume family available for provisioning?
{: faq}
{: #faq-sdp-release}

You can provision storage with the `sdp` profile in the Dallas, Frankfurt, London, Madrid, Osaka, Sao Paulo, Sydney, Tokyo, Toronto, and Washington, DC regions. For more information, see [About {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-about).

## What functions are supported in this release of the `sdp` profile?
{: faq}
{: #faq-sdp-functionality}

In this release, you can perform the following actions:

* Create Block Storage volumes, and specify a custom throughput limit for it in addition to capacity and IOPS.
* Add customer-managed encryption for data volumes.
* Expand the capacity of the Block Storage volumes after their creation when they are attached to a virtual server instance or unattached.
* Adjust IOPS after Block Storage volume creation when they are attached to a virtual server instance or unattached.
* Change the maximum throughput limit when the volume is attached to a virtual server instance or unattached.
* Attach Block Storage volumes to virtual service instances.
* Create custom images with provider-managed encryption.
* List Block Storage volumes.
* Delete Block Storage volumes.

## Can I use `sdp` profile for my boot volume if I want to use secure boot?
{: #sdp-no-secure-boot}

No. When second-generation boot volumes with the `sdp` profile are used, secure boot is not supported. 

In the current release of {{site.data.keyword.block_storage_is_short}} offering, only first-generation volumes from the tiered and custom volume profile families can be used as boot volumes for virtual server instances that use a profile from one of the following instance profile families:
- [Accelerated Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family ) - Gaudi, MI-300x, H100/H200 profiles.
- [General purpose Gen 3](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen3-intel) profiles if secure boot option is needed.
- [Confidential Computing - Gen 3](/docs/vpc?topic=vpc-confidential-computing-vsi-profiles-gen3-x86) profiles if secure boot option is needed.

If you want to provision a virtual server instance with either [Intel Gen 3](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen3-intel) or [Confidential Computing](/docs/vpc?topic=vpc-confidential-computing-vsi-profiles-gen3-x86) instance profiles, and an `sdp` boot volume, make sure that you do not enable secure boot.

## How is my data protected in the `sdp` profile-based storage volume?
{: faq}
{: #faq-sdp-encryption}

Your data is protected at rest by using either IBM-managed encryption or customer-managed encryption. Data-in-transit is also encrypted.

## Can a Block Storage volume be copied to a different zone?
{: faq}
{: #faq-sdp-zone-copy}

No. You can't copy the storage volume to a different zone.

## Can I create snapshots for data retention of a defined performance volume?
{: faq}
{: #faq-sdp-backup}

Yes, with some limitations. [Beta]{: tag-cyan} Customers with special access can create cross-regional copies of the second-generation volume backups if the source volume exceeds 10 TB. This feature is not generally available yet. Consistency group snapshots are not supported.

## How does {{site.data.keyword.block_storage_is_short}} prevent a single point of failure? What mechanism assures data durability?
{: faq}
{: #faq-block-storage-durability}

{{site.data.keyword.block_storage_is_short}} volume data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

## How are volumes created and attached to an instance?
{: faq}
{: #faq-block-storage-1}
{: support}

When you create a virtual server instance, you can [create a {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi) that is attached to that instance. You can also [create stand-alone volumes](/docs/vpc?topic=vpc-creating-block-storage#create-standalone-vol) and later attach them to your instances.

## How many instances can share a provisioned {{site.data.keyword.block_storage_is_short}} volume?
{: faq}
{: #faq-block-storage-2}

A {{site.data.keyword.block_storage_is_short}} volume can be attached to only one instance at a time. Instances cannot share a volume.

## How many data volumes can be attached to an instance?
{: faq}
{: #faq-block-storage-3}

You can attach 12 {{site.data.keyword.block_storage_is_short}} data volumes per instance, plus the boot volume.

## How am I charged for usage?
{: faq}
{: #faq-block-storage-6}
{: support}

The cost for {{site.data.keyword.block_storage_is_short}} is calculated based on GiB capacity that is stored per month, unless the duration is less than one month. The volume exists on the account until you delete the volume or you reach the end of a billing cycle, whichever comes first.

Pricing is also affected when you [expand volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) or [adjust IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) by specifying a different volume profile. For example, expanding volume capacity increases costs, and changing a volume profile from a 5-IOPS/GB tier to a 3-IOPS/GB tier decreases the monthly and hourly rate. Billing for an updated volume is automatically updated to add the prorated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

You can use the Cost estimator ![Cost estimator icon](../icons/calculator.svg "Cost estimator") in {{site.data.keyword.cloud_notm}} console to see how changes in capacity and IOPS affect the cost. For more information, see [Estimating your costs](/docs/account?topic=account-cost).

## Where can I find pricing information?
{: faq}
{: #faq-bs-pricing}

In the console, go to the [Block storage volume for VPC provisioning page](/infrastructure/provision/storage) and click the **Pricing** tab. On the **Pricing** tab, you can view details of the pricing plan for each volume profile based on the selected Geography, Region, and Currency. You can also switch between Hourly and Monthly rates.

You can programmatically retrieve the pricing information by calling the [Global catalog API](/apidocs/resource-catalog/global-catalog#get-pricing). For more information, see [Getting dynamic pricing](/docs/account?topic=account-getting-pricing-api).

## Is storage capacity measured in GB or GiB?
{: faq}
{: #faq-storage-units}

One confusing aspect of storage is the units that storage capacity and usage are reported in. Sometime GB is really gigabytes (base-10) and sometimes GB represents gibibytes (base-2) which should be abbreviated as GiB.

Humans usually think and calculate numbers in the decimal (base-10) system. In our documentation, we refer to storage capacity by using the unit GB (Gigabytes) to align with the industry standard terminology. In the console, CLI, API, and Terraform, you see the unit GB used and displayed when you query the capacity. When you want to order a 4-TB volume, you enter 4,000 GB in your provisioning request.

However, computers operate in binary, so it makes more sense to represent some resources like memory address spaces in base-2. Since 1984, computer file systems show sizes in base-2 to go along with the memory. Back then, available storage devices were smaller, and the size difference between the binary and decimal units was negligible. Now that the available storage systems are considerably larger this unit difference is causing confusion.

The difference between GB and GiB lies in their numerical representation:
- GB (Gigabyte) is a decimal unit, where 1 GB equals 1,000,000,000 bytes. When you convert GB to TB, you use 1000 as the multiplier.
- GiB (Gibibyte), is a binary unit, where 1 GiB equals 1,073,741,824 bytes. When you convert GiB to TiB, you use 1024 as the multiplier.

The following table shows the same number of bytes expressed in decimal and binary units.

| Decimal SI (base 10) | Binary (base 2)       |
|----------------------|-----------------------|
| 2,000,000,000,000 B  | 2,000,000,000,000 B   |
|     2,000,000,000 KB |     1,953,125,000 KiB |
|         2,000,000 MB |         1,907,348 MiB |
|             2,000 GB |             1,862 GiB |
|                 2 TB |              1.81 TiB |
{: caption="Decimal vs Binary units" caption-side="bottom"}

The storage system uses base-2 units for volume allocation. So if your volume is provisioned as 4,000 GB, that's really 4,000 GiB or 4,294,967,296,000 bytes of storage space. The provisioned volume size is larger than 4 TB. However, your operating system might display the storage size as 3.9 T because it uses base-2 conversion and the T stands for TiB, not TB.

## Why does the available capacity that I see in my OS not match the capacity that I provisioned?
{: faq}
{: #faq-storage-units-2}

One of the reasons can be that your operating system uses base-2 conversion. For example, when you provision a 4000 GB volume on the UI, the storage system reserves a 4,000 GiB volume or 4,294,967,296,000 bytes of storage space for you. The provisioned volume size is larger than 4 TB. However, your operating system might display the storage size as 3.9 T because it uses base-2 conversion and the T stands for TiB, not TB.

Second, partitioning your Block Storage and creating a file system on it reduces available storage space. The amount by which formatting reduces space varies depending upon the type of formatting and the amount and size of the various files on the system.

Take the volume `docs-block-test3` as an example. We specified 1200 GB during provisioning and when you list the details in the CLI, you can see that it has the capacity of 1200.

```sh
ibmcloud is volume r006-6afe1361-b592-45ab-b23b-6cca9982e371
```
{: pre}

```sh
Getting volume r006-6afe1361-b592-45ab-b23b-6cca9982e371 under account Test Account as user test.user@ibm.com...

ID                                     r006-6afe1361-b592-45ab-b23b-6cca9982e371
Name                                   docs-block-test3
CRN                                    crn:v1:bluemix:public:is:us-south-2:a/1234567::volume:r006-6afe1361-b592-45ab-b23b-6cca9982e371
Status                                 available
Attachment state                       attached
Capacity                               1200
IOPS                                   3600
Bandwidth(Mbps)                        471
Profile                                general-purpose
Encryption key                         -
Encryption                             provider_managed
Resource group                         defaults
Created                                2023-08-24T02:32:40+00:00
Zone                                   us-south-2
Health State                           ok
Volume Attachment Instance Reference   Attachment type   Instance ID                                 Instance name        Auto delete   Attachment ID                               Attachment name
                                       data              0727_e99798c7-9783-4f92-8207-96af48561454   docs-demo-instance   false         0727-bc38ec2b-a566-412f-8f76-8eefe5fc9f2c   untaken-senior-coronary-accurate

Active                                 true
Adjustable IOPS                        false
Busy                                   false
Tags                                   dev:test
```
{: screen}

When you list your storage devices from your server's command line, you can see the same volume as `vdc` with a size of 1.2T. The T stands for tebibyte, a base-2 unit that equals 2^40^.

```sh
[root@docs-demo-instance ~]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    253:0    0  100G  0 disk
├─vda1 253:1    0  200M  0 part /boot/efi
└─vda2 253:2    0 99.8G  0 part /
vdb    253:16   0 69.9G  0 disk
vdc    253:32   0  1.2T  0 disk /myvolumedir
vdd    253:48   0  370K  0 disk
vde    253:64   0   44K  0 disk
```
{: screen}

The same `vdc` drive shows 1181679068 K available capacity when it is formatted with an ext4 file system. It's normal and expected.

```sh
[root@docs-demo-instance ~]# df -hk
Filesystem      1K-blocks    Used  Available Use% Mounted on
devtmpfs          3993976       0    3993976   0% /dev
tmpfs             4004356       0    4004356   0% /dev/shm
tmpfs             4004356   33316    3971040   1% /run
tmpfs             4004356       0    4004356   0% /sys/fs/cgroup
/dev/vda2       102877120 1182048   96446100   2% /
/dev/vda1          204580   11468     193112   6% /boot/efi
/dev/vdc       1238411052   72148 1181679068   1% /myvolumedir
tmpfs              800872       0     800872   0% /run/user/0
```
{: screen}

## Are there limits on the number of volumes I can create?
{: faq}
{: #faq-block-storage-7}

You can create up to 300 total {{site.data.keyword.block_storage_is_short}} volumes (data and boot) per account in a region. To increase this [quota](/docs/vpc?topic=vpc-quotas#block-storage-quotas), open a [support case](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) and specify the zone where you need more volumes.

## After a data volume is created with a specific capacity, can the capacity be increased later?
{: faq}
{: #faq-block-storage-8}
{: support}

You can increase the capacity of data volumes that are attached to a virtual server instance. You can indicate capacity in GB increments up to 16,000 GB capacity, depending on your volume profile. For more information, see [Increasing {{site.data.keyword.block_storage_is_short}} volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

## Can I increase the capacity of a boot volume?
{: faq}
{: #faq-block-storge-rbv}

Boot volume capacity can be increased during instance provisioning or later, by directly modifying the boot volume. This feature applies to instances that are created from stock or custom images. You can also specify a larger boot volume capacity when you create an instance template. For more information, see [Increasing boot volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes).

## Can I change boot volume capacity for an existing instance?
{: faq}
{: #faq-block-storge-rbv-instance}

Yes, boot volume capacity can be increased for an existing instance. For example, in the console, select a boot volume from the list of {{site.data.keyword.block_storage_is_short}} volumes and then resize the volume from the volume details page. For more information, see [Increase boot volume capacity from the list of {{site.data.keyword.block_storage_is_short}} volumes in the console](/docs/vpc?topic=vpc-resize-boot-volumes&interface=ui#resize-boot-vol-list-ui). You can also use the [CLI](/docs/vpc?topic=vpc-resize-boot-volumes&interface=cli#expand-existing-boot-vol-cli) or the [API](/docs/vpc?topic=vpc-resize-boot-volumes&interface=api#expand-existing-boot-vol-api).

## How many volumes can I provision on my account?
{: faq}
{: #faq-block-storage-12}

You can provision up to 300 {{site.data.keyword.block_storage_is_short}} volumes per account in a region. You can request your quota to be increased by opening a [support case](/unifiedsupport/cases/form){: external} and specifying the region where you need more volumes. For more information about preparing a support case when you're ordering {{site.data.keyword.block_storage_is_short}} volumes or requesting an increase to your volume or capacity limits, see [Managing volume count and capacity limits](/docs/vpc?topic=vpc-manage-storage-limit).

## Can I set up shared storage in a multizone cluster?
{: faq}
{: #faq-block-storage-18}
{: support}

In the {{site.data.keyword.cloud}}, storage options are limited to an availability zone. Do not try to manage shared storage across multiple zones.

Instead, use an {{site.data.keyword.cloud}} classic service option outside a VPC such as the regional storage repository or {{site.data.keyword.cloudantfull}} if you must share your data across multiple zones and regions.

## I have volumes on the Classic infrastructure. Can I port them to the VPC?
{: faq}
{: #faq-block-storage-19}
{: support}

No. The VPC provides access to new availability zones in multi-zone regions. Compute, network, and storage resources are designed to function in the VPC.

## Can I create custom images from my existing boot volumes?
{: faq}
{: #faq-block-storage-ifv}

Yes, you can create a custom image directly from a {{site.data.keyword.block_storage_is_short}} boot volume. Then, you can use the custom image to provision other virtual server instances. For more information, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc).

## How is the boot disk created for an instance and how does it relate to the virtual machine image?
{: faq}
{: #faq-block-storage-14}

The boot volume is created when you provision a virtual server instance. The boot disk of an instance is a cloned image of the virtual machine image. For stock images, the boot volume capacity is 100 GB. If you are importing a custom image by using the CLI, API, or Terraform, the boot volume capacity can be 10 GB to 250 GB, depending on what the image requires. Images smaller than 10 GB are rounded up to 10 GB.

## When can I delete a {{site.data.keyword.block_storage_is_short}} volume?
{: faq}
{: #faq-block-storage-15}

You can delete a {{site.data.keyword.block_storage_is_short}} volume only when it isn't attached to a virtual server instance. 

You must [detach the data volume](/docs/vpc?topic=vpc-managing-block-storage#detach) before you can delete it. You can also enable the auto-delete feature in the console, from the CLI, or with the API. When auto-delete is enabled, the data volume is deleted along with the instance.

By default, boot volumes are detached and deleted when the instance is deleted. If you want to retain your boot volume, disable the auto-delete feature. For more information, see [Managing Block Storage for VPC volumes](/docs/vpc?topic=vpc-managing-block-storage).

## What happens to my data when I delete a {{site.data.keyword.block_storage_is_short}} data volume?
{: faq}
{: #faq-block-storage-16}

When you delete a {{site.data.keyword.block_storage_is_short}} volume, your data immediately becomes inaccessible. All pointers to the data on that volume are removed. The inaccessible data is eventually overwritten as new data is written to the data block. IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten. For more information, see [{{site.data.keyword.block_storage_is_short}} data eradication](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication).

## I have compliance requirements for data deletion. What can I do to ensure that my data is inaccessible?
{: faq}
{: #faq-block-storage-nist}

IBM guarantees that your data is inaccessible on the physical disk and is eventually [eradicated](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication). If you have extra compliance requirements such as NIST 800-88 Guidelines for Media Sanitization, you must perform data sanitation procedures before you delete your volumes. For more information, see the [NIST 800-88 Guidelines for Media Sanitation](https://csrc.nist.gov/pubs/sp/800/88/r1/final){: external}.

## What rules apply to volume names and can I rename a volume later on?
{: faq}
{: #faq-block-storage-9}

Valid volume names can include a combination of lowercase alphanumeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter and be unique across the entire VPC infrastructure.

You can change the name of an existing volume in the console. For more information, see [Managing {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-managing-block-storage#rename).

## Does the volume need to be pre-warmed to achieve the expected throughput?
{: faq}
{: #faq-block-storage-10}

You do not have to pre-warm a volume. You can see the specified throughput immediately upon provisioning the volume when you create the volume from an image.
You can experience degraded performance when you provision the volume by restoring a snapshot.

## What is a {{site.data.keyword.block_storage_is_short}} snapshot?
{: faq}
{: #faq-block-storage-snapshot}

Snapshots are a point-in-time copy of your {{site.data.keyword.block_storage_is_short}} boot or data volume that you manually create. The first snapshot is a full backup of the volume. Subsequent snapshots of the same volume capture only the changes since the last snapshot. For more information, see [About {{site.data.keyword.block_storage_is_short}} Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about).

## What is a backup snapshot?
{: faq}
{: #faq-block-storage-backup-snapshot}

Backup snapshots, simply called "backups", are snapshots that are automatically created by the Backup for VPC service. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

## What can I do about data backups for disaster recovery?
{: faq}
{: #faq-block-storage-dr}

{{site.data.keyword.block_storage_is_short}} secures your data across redundant fault zones in your region. By using the [backup service](/docs/vpc?topic=vpc-backup-service-about), you can regularly back up your volume data based on a schedule that you set up. You can create backup snapshots as frequently as 1 hour. However, the backup service does not provide continual backup with automatic failover, and restoring a volume from a backup or snapshot is a manual operation that takes time. If you require a higher level of service for automatic disaster recovery, see IBM's [Cloud disaster recovery solutions](https://www.ibm.com/solutions/disaster-recovery).

## Can I restore a volume from a snapshot?
{: faq}
{: #faq-block-storage-restore-vol}

Restoring from a snapshot creates a new, fully provisioned boot or data volume. You can restore storage volumes during instance creation, instance modification, or when you provision a new stand-alone volume. For data volumes, you can also use the `volumes` API to create a data volume from a snapshot. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

For best performance, you can enable snapshots for fast restore. By using the fast restore feature, you can create a volume from a snapshot that is fully provisioned when the volume is created. For more information, see [Snapshots fast restore](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_fast_restore).

## Can I add tags to a volume?
{: faq}
{: #faq-tags}

Yes, you can add user tags and access management tags to your volumes. User tags are used by the backup service to automatically create backup snapshots of the volume. Access management tags help organize access to your {{site.data.keyword.block_storage_is_short}} volumes. For more information, see [Tags for {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-tags).

## What is IOPS and how does it relate to my {{site.data.keyword.block_storage_is_short}} volume performance?
{: faq}
{: #faq-block-storage-4a}

Input/output operations per second (IOPS) is used to measure the performance of your {{site.data.keyword.block_storage_is_short}} volumes. A number of variables impact IOPS values, such as the balance of read/write operations, queue depth, and data block sizes. In general, the higher the IOPS of your {{site.data.keyword.block_storage_is_short}} volumes, the better the performance. For more information about expected IOPS for {{site.data.keyword.block_storage_is_short}} profiles, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles). For more information about how block size affects performance, see [Block Storage capacity and performance](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

## Is the allocated IOPS enforced by instance or by volume?
{: faq}
{: #faq-block-storage-4}

IOPS is enforced at the volume level.

## What are volume profiles and how do they affect volume performance?
{: faq}
{: #faq-block-storage-5}
{: support}

Volume profiles define IOPS/GB performance for volumes of various capacities. The [`sdp`](/docs/vpc?topic=vpc-block-storage-profiles#defined-performance-profile) profile offers the most flexibility for capacity, custom IOPS and bandwidth values. You can select from three predefined [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) that offer reliable IOPS performance for your workload requirements. You can also select the [custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) profile and specify the IOPS and the capacity. Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier.

Maximum IOPS for data volumes varies based on volume size and the type of profile you select.

IOPS is measured based on a load profile of 16-KB blocks with random 50% read and 50% writes. Workloads that differ from this profile might experience reduced performance. If you use a smaller block size, maximum IOPS can be obtained, but throughput is less. For more information, see
[How block size affects performance](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

## What happens when a volume is in a degraded health state?
{: faq}
{: #faq-block-storage-healthstate}

Volume health state defines whether a volume is performing as expected, given its status. Volume health can be OK, degraded, inapplicable, or faulted, depending on what is happening. For example, a degraded status is displayed when a volume is being restored from a snapshot and the volume is not yet fully restored. For more information about volume health states, see [{{site.data.keyword.block_storage_is_short}} volume health states](/docs/vpc?topic=vpc-block-storage-vpc-monitoring#block-storage-vpc-health-states).

## How secure is data in a {{site.data.keyword.block_storage_is_short}} volume?
{: faq}
{: #faq-block-storage-20}
{: support}

All {{site.data.keyword.block_storage_is_short}} volumes are encrypted at rest with IBM-managed encryption. IBM-managed keys are generated and securely stored in a {{site.data.keyword.block_storage_is_short}} vault that is backed by Consul and maintained by {{site.data.keyword.cloud}} operations. For more information about the industry standard protocols {{site.data.keyword.cloud}} follows for encryption, see [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui&q=IBM-managed+encryption&tags=vpc#vpc-provider-managed-encryption).

For more security, you can protect your data by using your own customer root keys (CRKs). You can import your root keys to, or create them in, a supported key management service (KMS). Your root keys are safely managed by the supported KMS, either {{site.data.keyword.keymanagementserviceshort}} (FIPS 140-2 Level 3 compliance) or {{site.data.keyword.hscrypto}}. Both KMS solutions offer the highest level of security (FIPS 140-2 Level 4 compliance). Your key material is protected in transit and at rest.

For more information, see [Supported key management services for customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok). To learn how to configure customer-managed encryption, see
[Creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

You control access to your root keys stored in KMS instances within {{site.data.keyword.cloud}} by using IBM {{site.data.keyword.iamshort}} (IAM). You grant access to the IBM {{site.data.keyword.block_storage_is_short}} Service to use your keys. With the API, you can link a primary account that holds a root key to a secondary account, then use that key to encrypt new volumes in the secondary account. For more information, see [Cross-account encryption for multitenant storage resources](/docs/vpc?topic=vpc-getting-started).

You can also revoke access at any time, for example, if you suspect your keys might be compromised. You can also disable or delete a root key, or temporarily revoke access to the key's associated data on the cloud. For more information, see [Managing data encryption](/docs/vpc?topic=vpc-vpc-encryption-managing).

## What are the advantages of using customer-managed encryption over provider-managed encryption?
{: faq}
{: #faq-block-storage-21}

Customer-managed encryption creates an envelop encryption for your {{site.data.keyword.block_storage_is_short}} volumes with your own root keys. You have complete control over your data security, managing access to your keys, rotating and revoking keys as you want. For more information, see [Advantages of customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#byok-advantages).

## What encryption technology is used for customer-managed encryption?
{: faq}
{: #faq-block-storage-22}

Virtual disk images for VPC use QEMU Copy On Write Version 2 (QCOW2) file format. LUKS encryption format secures the QCOW2 format files. IBM currently uses the AES-256 cipher suite and XTS cipher mode options with LUKS. This combination provides you a much greater level of security than AES-CBC, along with better management of passphrases for key rotation, and provides key replacement options in case your keys are compromised.

## What are master encryption keys and how are they assigned to my {{site.data.keyword.block_storage_is_short}} volumes?
{: faq}
{: #faq-block-storage-23}

A unique master encryption key is assigned to each volume, called a data encryption key or DEK, which is generated by the instance's host hypervisor. The master key for each {{site.data.keyword.block_storage_is_short}} volume is encrypted with a unique KMS-generated LUKS passphrase, which is then encrypted by your customer root key (CRK) and stored in the KMS. Passphrases are AES-256 cipher keys, which means that they are 32 bytes long and not limited to printable characters. You can view the cloud resource name (CRN) for the CRK that is used to encrypt a volume. However, the CRK, LUKS passphrase, and the volume's master encryption key are never exposed. For more information about all the keys IBM VPC uses to secure your data, see [IBM's encryption technology - How your data is secured](/docs/vpc?topic=vpc-vpc-encryption-about#byok-technologies).

## I use customer-managed encryption for my volumes. What happens when I disable or delete my root key?
{: faq}
{: #faq-block-storage-24}

These actions are two separate actions. Disabling a root key in your KMS suspends its encryption and decryption operations, placing the key in a suspended state. Workloads continue to run in virtual server instances and boot volumes remain encrypted. Data volumes remain attached. However, if you power down the VM and then power it back on, any instances with encrypted boot volumes do not start. You can enable a root key in a suspended state and resume normal operations. For more information, see [Disabling root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys).

Deleting a root key has greater consequences. Deleting a root key purges usage of the key for all resources in the VPC. By default, the KMS prevents you from deleting a root key that's actively protecting a resource. However, you can still force the deletion of a root key. You have limited time to [restore a deleted root key](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-restore-root-key) that you imported to the KMS. For more information, see [Deleting root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys).

## Can I remove IAM authorization from Cloud {{site.data.keyword.block_storage_is_short}} to the KMS and still delete my {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption?
{: faq}
{: #faq_block_storage_remove_iam}

If you remove IAM authorization before you delete your BYOK volume (or image), the delete operation completes without unregistering the root keys in the KMS instance. In other words, the root key remains registered for a resource that doesn't exist. Always delete a BYOK resource before you remove IAM authorization. For more information about safely removing service authorization, see [Removing service authorization to a root key](/docs/vpc?topic=vpc-vpc-encryption-managing#instance-byok-inaccessible-data).

## What can I do if my root key is compromised?
{: faq}
{: #faq-block-storage-25}

Independently back up your data. Then, delete the compromised root key and power down the instance with volumes that are encrypted with that key.

Also, consider setting up a key rotation policy that automatically rotates your keys based on a schedule. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

## What is a key rotation?
{: faq}
{: #faq-block-storage-25a}

For {{site.data.keyword.vpc_short}} resources such as {{site.data.keyword.block_storage_is_short}} volumes that are protected by your customer root key (CRK), you can rotate the root keys for extra security. When you rotate a root key by a schedule or on demand, the original key material is replaced. The old key remains active to decrypt existing volumes but can't be used to encrypt new volumes. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

## How does key rotation work?
{: faq}
{: #faq-block-storage-25b}

Customer-managed encrypted resources such as {{site.data.keyword.block_storage_is_short}} volumes use your root key (CRK) as the root-of-trust key that encrypts a LUKS passphrase that encrypts a master key that's protecting the volume. You can import your CRK to a key management service (KMS) instance or instruct the KMS to generate one for you. Root keys are rotated in your KMS instance.

When you rotate a root key, a new version of the key is created by generating or importing new cryptographic key material. The old root key is retired, which means its key material remains available for decrypting existing volumes, but not available for encrypting new ones. New resources are protected by the latest key. For more information, see [How key rotation works](/docs/vpc?topic=vpc-vpc-key-rotation).

## Am I charged for using customer-managed encryption?
{: faq}
{: #faq-block-storage-27}

You are not charged extra for creating volumes with customer-managed encryption. However, setting up a {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance to import, create, and manage your root keys is not without cost. Contact your IBM customer service representative for details.

## What's the difference between using {{site.data.keyword.keymanagementserviceshort}} as my KMS compared to {{site.data.keyword.hscrypto}}? When would I use one over the other?
{: faq}
{: #faq-block-storage-28}

Both key management systems provide you with complete control over your data, managed by your root keys. {{site.data.keyword.keymanagementserviceshort}} is a multi-tenant KMS where you can import or create your root keys and securely manage them. {{site.data.keyword.hscrypto}} is a single-tenant KMS and [hardware security module (HSM)](#x6704988){: term} that is controlled by you, which offers the highest level of security. For more information about these key management services, see [Supported key management services for customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok).

## Can I convert my volume from provider-managed encryption to customer-managed encryption?
{: faq}
{: #faq-block-storage-29}

No, after you provision a volume and specify the encryption type, you can't change it.
