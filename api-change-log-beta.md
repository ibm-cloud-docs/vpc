---

copyright:
  years: 2021, 2023
lastupdated: "2023-07-11"

keywords: api, change log, beta

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Beta VPC API change log
{: #api-change-log-beta}

Read this change log to learn about updates and improvements to the beta {{site.data.keyword.vpc_full}} (VPC) [API](/apidocs/vpc-beta). The change log lists changes that are ordered by the date they were released.
{: shortdesc}

Some beta features are for accounts that have been granted special approval to preview a particular beta feature. Contact your IBM sales representative if you are interested in getting access.
{: beta}

There are no backward-compatibility guarantees as a feature progresses through its beta phase or from the final beta release to its initial GA release. Using non-GA-mature features could introduce the risk of corrupting resources in your account. IBM strongly recommends that you do not use non-GA-mature features on production accounts.
{: important}

To review the change log of generally available API features, see the [VPC API change log](/docs/vpc?topic=vpc-api-change-log).

## 11 July 2023
{: #11-july-2023-beta}

### For version 2023-07-11 or later
{: #version-2023-07-11-beta}

**Data encryption in transit for file shares.** For users with accounts that have access to file shares, you can now enable secure end-to-end encryption of your data in transit between the file share and the authorized client.

When [creating a mount target for a file share](/apidocs/vpc-beta#create-share-mount-target) with a virtual network interface, you can now specify a `transit_encryption` property value of `none` (default) or `user_managed`, which encrypts the data in transit by using IPsec with an instance identity certificate. For more information, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=api#fs-eit) and [Creating instance identity certificates](/docs/vpc?topic=vpc-metadata-beta-api-change-log#11-july-2023-metadata-beta).

**File share access control modes.** For users with accounts that have access to file shares, you can now control the way a share is accessed when [creating](/apidocs/vpc-beta#create-share) and [updating](/apidocs/vpc-beta#update-share) a file share. Specifying `access_control_mode` property value `security_group` now allows the use of security groups to manage which resources can access the file share. By using security groups, access can now be restricted to specific clients. When you specify `access_control_mode` property value `vpc`, all clients in each mount target's VPC will continue to have access to this share.

The default value of `access_control_mode` depends on the `version` query parameter date and the profile selected. When making API requests with a `version` query parameter of `2023-07-11` or later, the default is `security_group`. For requests that are using a `version` query parameter of `2023-07-10` or earlier, the default is `vpc`. File shares must be based on the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#dp2-profile) to use the `security_group` value. See [2023-07-11 API migration (file shares)](/docs/vpc?topic=vpc-2023-07-11-migration-file-shares) for guidance on migrating `access_control_mode` from `vpc` to `security_group`.

When [creating a mount target](/apidocs/vpc-beta#create-share-mount-target) for a file share with `access_control_mode` set to `security_group`, you must also create a virtual network interface by using the `virtual_network_interface` property. For more information, see [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about&interface=api) and [Mount target access modes](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode). You must use an `access_control_mode` of `security_group` to enable [Data encryption in transit for file shares](/docs/vpc?topic=vpc-file-storage-vpc-eit).

## 13 June 2023
{: #13-june-2023-beta}

### For all version dates
{: #13-june-2023-all-version-dates-beta}

**Image lifecycle management property name changes.** For accounts that have been granted special approval to preview the image lifecycle management feature, the `deprecated_at` and `obsoleted_at` properties for [images](/apidocs/vpc-beta#create-image) requests have been renamed `deprecation_at` and `obsolescence_at`, respectively. Original property names `deprecated_at` and `obsoleted_at` will continue to be supported until the feature becomes generally available. Requests that specify the original and revised property names simultaneously will be rejected.

This feature is now generally available. Support for property names `deprecated_at` and `obsoleted_at` has been removed. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#11-july-2023).

## 30 May 2023
{: #23-may-2023-beta}

### For version `2023-05-30` or later
{: #version-2023-05-30-beta}

This release introduces the following features for users with accounts that have access to file shares.

**File shares property and request path name changes.** When making API requests using a `version` query parameter of `2023-05-30` or later, the shares `targets` property has been changed to `mount_targets`. This change applies when [creating](/apidocs/vpc-beta#create-share), [updating](/apidocs/vpc-beta#update-share), [listing](/apidocs/vpc-beta#list-shares), and [retrieving](/apidocs/vpc-beta#get-share) a file share, and when [listing all mount targets for a file share](/apidocs/vpc-beta#list-share-mount-targets).

The name change also applies to the method paths: Requests using a `version` query parameter of `2023-05-30` or later must use `/shares/{share_id}/mount_targets` (instead of `/shares/{share_id}/targets`) in the request URL. This change applies when [creating](/apidocs/vpc-beta#create-share-mount-target), [updating](/apidocs/vpc-beta#update-share-mount-target), [listing](/apidocs/vpc-beta#list-share-mount-targets), [retrieving](/apidocs/vpc-beta#get-share-mount-target), and [deleting](/apidocs/vpc-beta#delete-share-mount-target) share mount targets.

See [`2023-05-30` API migration (file shares)](/docs/vpc?topic=vpc-2023-05-30-migration-file-shares) for guidance on migrating from `targets` to  `mount_targets`.

### For all version dates
{: #30-may-2023-all-version-dates-beta}

**Enforcement of file shares beta API requests.** Starting with API version `2023-05-30`, all requests made for [shares methods](/apidocs/vpc-beta#list-shares) must include the [`maturity=beta`](/apidocs/vpc-beta#maturity-query-parameter) query parameter. Requests that omit the `maturity=beta` query parameter will be regarded as requests against the [VPC GA API](/apidocs/vpc), which does not yet support shares. As a result, those requests will fail.

## 11 April 2023
{: #11-april-2023-beta}

### For all version dates
{: #11-april-2023-all-version-dates-beta}

**Image lifecycle management.** Accounts that have been granted special approval to preview this feature can now [deprecate](/apidocs/vpc-beta#deprecate-image) or [obsolete](/apidocs/vpc-beta#obsolete-image) custom images directly. Alternatively, you can schedule transition at a later date by specifying the `deprecated_at` or `obsoleted_at` properties when [creating](/apidocs/vpc-beta#create-image) or [updating](/apidocs/vpc-beta#update-image) an image. If you need to revert a status change, you can transition `deprecated` or `obsolete` images back to `available`. For more information, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=api#custom-images-list-api).

`deprecated` custom images remain usable, while `obsolete` images cannot be used to provision instances or bare metal servers.
{: note}

**Revised file storage profiles.** For users with accounts that have access to file shares, a new `dp2` profile is now available when [creating](/apidocs/vpc-beta#create-share) and [updating](/apidocs/vpc-beta#update-share) a file share. Profiles in the existing `custom` and `tiered` families have been deprecated and will remain available only to accounts that have already provisioned file shares with those profiles. The deprecated profiles also will not be included in the upcoming general availability release for file shares.

The `dp2` profile belongs to a new `defined_performance` file share profile family, which provides similar functionality to the deprecated `custom` file share profile family. While existing file shares using profiles in the `custom` and `tiered` families will continue to work, you are encouraged to update all your file shares to the new `dp2` profile in preparation for general availability of the file share service. Bulk migration of existing file shares is not supported. For more information, see [dp2 file storage profile](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile).

## 14 February 2023
{: #14-february-2023-beta}

### For all version dates
{: #14-february-2023-all-version-dates-beta}

**Exporting custom images.** Accounts that have been granted special approval to preview this feature can now [export custom images](/apidocs/vpc-beta#create-image-export-job) to an authorized IBM Cloud Object Storage bucket. Specify the target `storage_bucket` to export the image to.   The image will be exported as `qcow2` unless you specify another value using the `format` property. 

For more information, see [Exporting a custom image to IBM Cloud Object Storage](/docs/vpc?topic=vpc-managing-custom-images&interface=api#custom-image-export-to-cos-api), or start using the new [export jobs](/apidocs/vpc-beta#list-image-export-jobs) methods.

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#2-may-2023).

## 20 December 2022
{: #20-december-2022-beta}

### For all version dates
{: #20-december-2022-all-version-dates-beta}

**Backup for VPC.** Backup policy jobs are now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#20-december-2022).

The following updates have been made for [listing backup policy jobs](/apidocs/vpc/latest#list-backup-policy-jobs) and [retrieving a backup policy job](/apidocs/vpc/latest#get-backup-policy-job) since the beta release:

* The `source_volume` property has been replaced by the `source` property.
* The `target_snapshot` property has been replaced by the `target_snapshots` array property.

**Instance provision by volume.** Accounts that have been granted special approval to preview this feature can now reuse an existing boot volume to provision a virtual server instance by specifying the existing volume's `id` or `crn` sub-property of the `boot_volume_attachment` property. The specified volume must be unattached and must have an operating system with the same architecture as the instance profile. Volumes now include an `attachment_state` property and an expanded `operating_system` property you can use to view a volume's eligibility. You can also use the new [list volumes](/apidocs/vpc/latest#list-volumes) filters to list volumes that have specific `attachment_state`, `operating_system`, and `encryption_type` values.

By default, a boot volume attached to a virtual server instance is deleted when the instance is deleted. To preserve the boot volume when deleting a virtual server instance, change the `delete_volume_on_instance_delete` property to `false` by updating the [boot volume attachment](/apidocs/vpc/latest#update-instance-volume-attachment). See [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui), [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli) for more information.

This feature is now generally available. Since the beta release, by default, only a boot volume created as part of provisioning a virtual server instance will be deleted when the instance is deleted. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#21-march-2023).

## 16 August 2022
{: #16-august-2022-beta}

### For all version dates
{: #16-august-2022-all-version-dates-beta}

**Sharing images across an enterprise account.** Accounts that have been granted special approval to preview this feature can now use a [catalog to share custom images](/docs/vpc?topic=vpc-planning-custom-images#custom-image-cloud-private-catalog){: external} with users in other accounts within the same enterprise. When you [create an image](/apidocs/vpc/latest#create-image), a new `catalog_offering` property includes a `published` sub-property that is set to `false` by default. When the custom image is imported to a catalog the `published` sub-property is set to `true`, indicating that the image is added to a catalog offering `version` and is managed from a catalog. If you are authorized to the catalog offering `version`, you can [provision virtual server instances](/apidocs/vpc/latest#create-instance) using that custom image by specifying its `catalog_offering.version.crn`. To use the custom image associated with the latest version in the offering, specify `catalog_offering.offering.crn` instead. The image may not be deleted from your IBM Virtual Private Cloud while it is managed from a catalog.

For more information, see the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial) and the [Import offering](/apidocs/resource-catalog/private-catalog#import-offering){: external} method in the Catalog Management API.

This feature is now generally available. The `catalog_offering.published` property in the beta API definition has been renamed to `catalog_offering.managed`. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#27-september-2022).

## 12 July 2022
{: #12-july-2022-beta}

### For all version dates
{: #12-july-2022-all-version-dates-beta}

**File storage cross-account encryption.**  Accounts that have been granted special approval to preview this feature can now use cross-account customer-managed encryption keys (CRKs) when [creating a file share](/apidocs/vpc-beta#create-share) with [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption). With this feature, the CRK account owner [invites you](/docs/account?topic=account-iamuserinv) and sets the IAM delegated policy to the CRKs. Afterward, specify your IAM token to create a file share with an `encryption_key` CRN from the CRK account. For more information, see [Cross-account encryption for multitenant file storage resources](/docs/vpc?topic=vpc-vpc-byok-cross-acct-key-file).

## 5 July 2022
{: #5-july-2022-beta}

### For all version dates
{: #5-july-2022-all-version-dates-beta}

**Client VPN for VPC.**  Client-to-site connectivity is now  generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#5-july-2022).

The following updates have been made since the [beta release](#24-august-2021):

- You can use IBM Cloud Secrets Manager for server authentication when [creating a VPN server](/apidocs/vpc/latest#create-vpn-server)
- You can specify a VPN server when [adding a security group target](/apidocs/vpc/latest#add-security-group-target)
- You can [update a VPN server](/apidocs/vpc/latest#update-vpn-server) to be highly available, or detach a subnet to downgrade to a stand-alone deployment

## 14 June 2022
{: #14-june-2022-beta}

### For all version dates
{: #14-june-2022-all-version-dates-beta}

**File storage adjustable IOPS.** Accounts that have been granted special approval to preview this feature can now [update the IOPS of an existing file share](/apidocs/vpc-beta#update-share). For a file share using a `custom` profile, specify the `iops` property. For a file share using a profile in the `tiered` profile family, specify another tier within the `tiered` family, which will set the `iops` based on the share's size.

You can also change a share between the `tiered` and `custom` profile families so long as the requested `iops` and `size` are supported by the requested profile. For more information about file share profiles, see [File Storage for VPC profiles](/docs/vpc?topic=vpc-file-storage-profiles). For information about adjusting IOPS with a profile, or changing between `tiered` and `custom` profiles, see [Adjusting file share IOPS](/docs/vpc?topic=vpc-adjusting-share-iops&interface=api).

## 31 May 2022
{: #31-may-2022-beta}

### For all version dates
{: #31-may-2022-all-version-dates-beta}

**AMD support for instances and dedicated hosts.** Accounts that have been granted special approval to preview this feature can select new profiles for [dedicated hosts](/apidocs/vpc-beta#list-dedicated-host-profiles) and [instances](/apidocs/vpc-beta#list-instance-profiles). When provisioning or managing an instance or dedicated host, use the new `vcpu_manufacturer` property to choose between profiles from different processor manufacturers.

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#28-march-2023).

## 24 May 2022
{: #24-may-2022-beta}

### For all version dates
{: #24-may-2022-all-version-dates-beta}

**Backup for VPC.** Accounts with special approval to preview this feature can now [create](/apidocs/vpc-beta#create-backup-policy-plan) up to four plans for backup policies. You can now also [update](/apidocs/vpc-beta#update-backup-policy-plan) and [delete](/apidocs/vpc-beta#delete-backup-policy-plan) existing plans, and [add backup plans](/vpc-beta#create-backup-policy) to existing policies. You can use one of the new `deletion_trigger` sub-properties to specify a custom backup deletion policy. For more information, see [Managing backup policies](/docs/vpc?topic=vpc-backup-service-manage&interface=api).

Backup policies now also include information about [backup policy jobs](/apidocs/vpc-beta#list-backup-policy-jobs). A [backup policy job](/docs/vpc?topic=vpc-backup-view-policy-jobs&interface=api) is automatically created each time a backup has to be created or deleted to meet the backup policy's settings. For more information, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

The backup API is now generally available, with the exception of the backup jobs API, which remains in beta. See the [Change log](/docs/vpc?topic=vpc-api-change-log#21-june-2022).

The backup jobs API is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#20-december-2022)

## 17 May 2022
{: #17-may-2022-beta}

### For all version dates
{: #17-may-2022-all-version-dates-beta}

**File share replication.** Accounts that have been granted special approval to preview this feature can now set up replication between a file share in one zone and a replica file share in another zone in the same region. [Using replication](/docs/vpc?topic=vpc-file-storage-replication) is a good way to recover from an incident at your primary site, if data becomes inaccessible or an applications fails.

- When [creating a new file share](/apidocs/vpc-beta#create-share), you can now configure replication by specifying the new `replica_share` property. To create a replica for an existing file share, create a new share and specify the existing file share as `source_share`, along with the replication schedule, using the `replication_cron_spec` property.
- You can now [retrieve the source file share for a replica file share](/apidocs/vpc-beta#get-share-source). Returned information also includes the replication schedule, role, status, and status reasons.
- You can now [split the source file from a replica share](/apidocs/vpc-beta#delete-share-source), which removes the replication relationship between a source share and its replica share, resulting in two independent read-write file shares. This action cannot be reversed. For more information, see [Remove the replication relationship](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-remove-replication).
- You can now [fail over to the replica file share](/apidocs/vpc-beta#failover-share), which reverses the replication relationship. You can optionally specify `split` for the `fallback_policy` to trigger a split if the failover operation fails or times out. For more information, see [Replication failover](/docs/vpc?topic=vpc-file-storage-failover&interface=api).

**File storage native tagging.** Accounts that have been granted special approval to preview this feature can now specify `user_tags` when [creating a new file share](/apidocs/vpc-beta#create-share) or [updating an existing file share](/apidocs/vpc-beta#update-share). Adding user tags to a file share helps you organize your resources. For details, see [Add user tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=api#fs-add-user-tags).

## 22 March 2022
{: #22-march-2022-beta}

### For all version dates
{: #22-march-2022-all-version-dates-beta}

**VPN client-to-site servers update.** You can now [update the subnets for a VPN server](/apidocs/vpc-beta#update-vpn-server) after the VPN is provisioned. For example, you can upgrade a stand-alone VPN server (one subnet) to a High Availability (HA) VPN server (two subnets in different zones). You can also detach a subnet to downgrade an HA VPN server to a stand-alone deployment, or change an attached subnet after your VPN server is provisioned. For more information, see [Upgrading to an HA VPN server](/docs/vpc?topic=vpc-vpn-client-to-site-change-server-types&interface=api).

## 8 March 2022
{: #8-march-2022-beta}

### For all version dates
{: #8-march-2022-all-version-dates-beta}

**Backup for VPC.** Accounts that have been granted special approval to preview this feature can now create backup policies to automatically back up block storage volumes. Use the new [backup service APIs](/apidocs/vpc-beta#list-backup-policies) to create, list, and manage backup policies. Backup policies control which source volumes are selected for backup by [matching user tags](/docs/vpc?topic=vpc-backup-service-about#backup-service-about-tags) in the volume with tags defined in the backup policy. For this beta release, a backup policy contains one [backup plan](/apidocs/vpc-beta#create-backup-policy-plan) in which a `deletion_trigger` specifies the maximum number of days to keep each backup after creation. For more information, see [About Backup for VPC (Beta)](/docs/vpc?topic=vpc-backup-service-about).

## 14 December 2021
{: #14-december-2021-beta}

### For all version dates
{: #14-december-2021-all-version-dates-beta}

**Concurrent update protection.** To prevent multiple clients from unknowingly overwriting each other's updates, select API methods support entity-tags and conditional requests.

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#22-march-2022).

## 23 November 2021
{: #23-november-2021-beta}

### For all version dates
{: #23-november-2021-all-version-dates-beta}

**Customer-managed encryption for file shares.** Accounts that have been granted special approval to preview this feature can now use customer-managed encryption, also called Bring Your Own Key (BYOK), to [create a file share](/apidocs/vpc-beta#create-share) that is encrypted using your root key. Specify the new `encryption_key` property and `crn` sub-property of the root key that you either imported to {{site.data.keyword.cloud_notm}} or created in Key Protect or Hyper Protect Crypto Services (HPCS). For more information, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption&interface=api).

You can also use the API to rotate the root keys that are protecting your file shares. See [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation&interface=api#vpc-key-rotation-api-procedure) for details.
{: tip}

**Supplemental user/group IDs for file shares.** Accounts that have been granted special approval to preview this feature can now access supplemental user/group IDs for file shares. When a process runs on Unix/Linux, the operating system identifies a user with a user ID (UID) and/or group with a group ID (GID). These IDs determine which system resources a user or group can access. When you [create a file share](/apidocs/vpc-beta#create-share), you can specify the new `initial_owner` property and specify a `uid`, `gid`, or both sub-properties to control access to the share. For more information, see [Add supplemental IDs when you create a file share from the API](/docs/vpc?topic=vpc-file-storage-create&interface=api#fs-add-supplemental-id-api).

## 24 August 2021
{: #24-august-2021-beta}

### For all version dates
{: #24-august-2021-all-version-dates-beta}

**VPN client-to-site servers.** Until now, the {{site.data.keyword.vpn_full}} service supported only site-to-site connectivity, which connects your on-premises network to the {{site.data.keyword.cloud_notm}}. This beta release adds client-to-site connectivity, which allows remote devices to securely connect to the VPC network using an OpenVPN software client. This solution is useful for telecommuters who want to connect to the {{site.data.keyword.cloud_notm}} from a remote location, such as a home office. For more information, see [About VPN servers (client-to-site)](/docs/vpc?topic=vpc-vpn-client-to-site-overview) or check out the new [API methods](/apidocs/vpc-beta#list-vpn-servers).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#5-july-2022).

## 17 August 2021
{: #17-august-2021-beta}

### For all API version dates
{: #17-august-2021-all-version-dates-beta}

**File storage for VPC.** Accounts that have been granted special approval to preview this feature can now increase file share size in gigabyte increments up to 32 TB (depending on the file share's profile). The increase takes effect immediately. For more information, see [Expanding file share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity&interface=api). You can also specify the maximum input/output operations per second (IOPS) when [creating](/apidocs/vpc-beta#create-share) a file share, within the range available for its size. For more information, see [Custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles#custom).

## 10 August 2021
{: #10-august-2021-beta}

### For all version dates
{: #10-august-2021-all-version-dates-beta}

**Beta VPC instance metadata API.** Accounts that have been granted special approval to preview this feature can now preview the new beta {{site.data.keyword.vpc_full}} (VPC) [Instance Metadata API](/apidocs/vpc-metadata-beta). This API provides access to VPC instance metadata, including instance initialization data, network interfaces, volume attachments, public SSH keys, and placement groups. Use the [VPC create instance API](/apidocs/vpc-beta#create-instance) or the [VPC update instance API](/apidocs/vpc-beta#update-instance) to enable or disable the metadata service endpoint for a particular instance. For more information, see [About VPC Instance Metadata](/docs/vpc?topic=vpc-imd-about).

## 27 July 2021
{: #27-july-2021-beta}

### For all API version dates
{: #27-july-2021-all-version-dates-beta}

**GPU instances.** Accounts that have been granted special approval to preview this feature can now view new instance profiles, which include GPUs attached to the instance. These attached GPUs allow for accelerated computing to run workloads with more powerful compute capabilities.

The following API methods have been enhanced:

- [List instances](/apidocs/vpc-beta#list-instances) returns a new `gpu` property with four additional sub-properties: `count`, `manufacturer`, `model`, and `memory`
- [Retrieve an instance profile](/apidocs/vpc-beta#get-instance-profile) returns four new properties: `gpu_count`, `gpu_manufacturer`, `gpu_model`, and `gpu_memory`

For more information, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#19-october-2021).

## 20 July 2021
{: #20-july-2021-beta}

### For all version dates
{: #20-july-2021-all-version-dates-beta}

**Bare metal servers for VPC.** Accounts that have been granted special approval to preview this feature can now create bare metal servers to host VMware clusters in {{site.data.keyword.vpc_short}}. You can set up VMware management applications and create VMware virtual machines on the bare metal servers. As bare metal servers are integrated with the VPC platform, you can take advantage of the network and security capabilities of {{site.data.keyword.vpc_short}}. For more information, see [About Bare Metal Servers for VPC (beta)](/docs/vpc?topic=vpc-about-bare-metal-servers) or dive into the new [API methods](/apidocs/vpc-beta#list-bare-metal-server-profiles).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#1-february-2022).

## 24 June 2021
{: #24-june-2021-beta}

### For all version dates
{: #24-june-2021-all-version-dates-beta}

**Placement groups.** Placement groups for {{site.data.keyword.vpc_full}} are logical groupings of virtual server instances that can be configured to reduce the risk of correlated failures inherent in your physical environment, such as networking issues, power loss, or hardware failure. Define a placement group strategy for high-availability workloads, such as for host or power spread. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc) or dive into the new [API methods](/apidocs/vpc#list-placement-groups).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#17-august-2021).

## 6 April 2021
{: #6-april-2021-beta}

### For all version dates
{: #6-april-2021-all-version-dates-beta}

**File storage for VPC.** Accounts that have been granted special approval to preview this feature can now create NFS-based file shares in a zone in your region. Share file storage over multiple virtual service instances within the same zone across multiple VPCs. Learn about [creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-vpc-about), and explore the new [API methods](/apidocs/vpc-beta#list-share-profiles).

## 19 March 2021
{: #19-march-2021-beta}

### For all version dates
{: #19-march-2021-all-version-dates-beta}

**Instance resize.** You can now resize an instance by providing the `profile` property in the API method `PATCH /instances/{id}` ([Update an instance](/apidocs/vpc-beta#update-instance)). For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance&interface=api).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#30-march-2021).

## 9 March 2021
{: #9-march-2021-beta}

### For all version dates
{: #9-march-2021-all-version-dates-beta}

**Bring your own license.** You can now [bring your own license](/docs/vpc?topic=vpc-byol-vpc-about) (BYOL) for custom images that you create and import to {{site.data.keyword.vpc_short}}. When you import a custom image, you can choose from new `byol` Red Hat Enterprise Linux (RHEL) and Windows operating systems.

A new `dedicated_host_only` property has been added to operating system resources. Any instance with a boot volume created from an image with `operating_system.dedicated_host_only` set to `true` must be placed on a dedicated host (or into a dedicated host group). Since Windows BYOL images have `dedicated_host_only` set to `true`, they must be placed on a dedicated host (or into a dedicated host group). There are no restrictions on placing instances using RHEL BYOL images.

Every operation that returns an `OperatingSystem` resource now includes a `dedicated_host_only` property.

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#19-march-2021).

## 5 March 2021
{: #5-march-2021-beta}

### For all version dates
{: #5-march-2021-all-version-dates-beta}

**Instance resize.** Accounts that have been granted special approval to preview this feature can now resize an instance by providing the `profile` property in the API method `PATCH /instances/{id}` ([Update an instance](/apidocs/vpc-beta#update-instance)). For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance&interface=api).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#30-march-2021).

## 22 February 2021
{: #22-february-2021-beta}

### For all version dates
{: #22-february-2021-all-version-dates-beta}

**Block storage snapshots.** Accounts that have been granted special approval to preview this feature can now create and use [snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) and explore the new [snapshots API methods](/apidocs/vpc-beta#list-snapshots).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#18-may-2021).

**Virtual server instance console.** Accounts that have been granted special approval to preview this feature can now access instances by connecting to a VNC or serial console. Learn about [accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console&interface=api), and explore the new instance console API methods:

- [Create a console access token for an instance](/apidocs/vpc-beta#create-instance-console-access-token)
- [Retrieve the console WebSocket for an instance](/apidocs/vpc-beta#get-instance-console)

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#30-march-2021).
