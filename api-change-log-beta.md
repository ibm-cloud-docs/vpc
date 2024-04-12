---

copyright:
  years: 2021, 2024
lastupdated: "2024-04-12"

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

## 9 April 2024
{: #9-april-2024-beta}

### For all version dates
{: #9-april-2024-all-version-dates-beta}

**Generic operating system custom images and network bootable bare metal servers.** Accounts that have been granted special approval to preview this feature can now create a [generic operating system custom image](/docs/vpc?topic=vpc-create-generic-os-custom-image), which is an image containing an operating system that is not specifically defined in IBM Cloud VPC. Such approved accounts can also use a specific new [stock image](/docs/vpc?topic=vpc-getting-started-images-on-vpc-stock) to create a bare metal server that will [network boot an image](/docs/vpc?topic=vpc-network-boot-bare-metal-servers&interface=api) using iPXE.

To facilitate these features, two new immutable operating system properties, `user_data_format` and `allow_user_image_creation`, and one new immutable image property, `user_data_format`, are provided. The operating system property `user_data_format` (possible values `cloud_init`, `esxi_kickstart`, `ipxe`) populates the image [`user_data_format` property](/docs/vpc?topic=vpc-planning-custom-images#custom-image-user-data-format), which specifies how `user_data` is interpreted and used when [creating a virtual server instance](/apidocs/vpc-beta/initial#create-instance) or [creating a bare metal server](/apidocs/vpc-beta/initial#create-bare-metal-server). The operating system property `allow_user_image_creation` indicates whether an operating system may be used to create a custom image.

For more information, see [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc&interface=api).

Images that are used to create virtual server instances or to [create instance templates](/apidocs/vpc-beta/initial#create-instance-template) must have a `user_data_format` value of `cloud_init`.
{: important}

## 12 March 2024
{: #12-march-2024-beta}

### For all version dates
{: #12-march-2024-all-version-dates-beta}

**Private path network load balancer.** Accounts that have been granted special approval to preview this feature can now create a [private path network load balancer](/docs/vpc?topic=vpc-ppnlb-ui-creating-private-path-network-load-balancer&interface=api) to enable and manage private connectivity for consumers of a hosted service. When [creating a load balancer](/apidocs/vpc-beta/initial#create-load-balancer), you can specify the new `is_private_path` property value as `true` to create a private path network load balancer.

**Load balancer and load balancer profile properties.** The private path load network balancer feature introduces a new load balancer profile `network-private-path`, along with the following new load balancer and load balancer profile properties:
 - `source_ip_session_persistence_supported` indicates whether a load balancer supports source IP session persistence. Source IP session persistence is not supported by private path network load balancer.
- `availability` indicates the availability of a load balancer.  Load balancers with `subnet` availability remain available if at least one of its subnets is in a zone that's available. Load balancers with `region` availability remain available if at least one zone in the region is available. Private path network load balancers have `region` availability. Other load balancers have `subnet` availability.
The `value` for load balancer profiles properties `route_mode_supported`, `security_groups_supported`, `udp_supported`, and `logging_supported` is set to `false` for private path load balancers. Additionally, private path load balancers do not support setting or updating the `dns` property, because a private path network load balancers are accessed using endpoint gateways where DNS is configured.

**Private path service gateway.** Accounts that have been granted special approval to preview this feature can now [create private path service gateways](/apidocs/vpc-beta/initial#create-private-path-service-gateway). Creating, [updating](/apidocs/vpc-beta/initial#update-private-path-service-gateway), and [deleting](/apidocs/vpc-beta/initial#delete-private-path-service-gateway) private path service gateways allows you to configure and manage access to [private path services](/docs/vpc?topic=vpc-private-path-service-about&interface=api). Private path service gateways also have two sub-resources:

- [Account policies](/apidocs/vpc-beta/initial#list-private-path-service-gateway-account-policies) provide per-account access policies that supersede the private path service gateway's default access policy.  You can [create](/apidocs/vpc-beta/initial#create-private-path-service-gateway-account-policy), [update](/apidocs/vpc-beta/initial#update-private-path-service-gateway-account-policy), and [delete](/apidocs/vpc-beta/initial#delete-private-path-service-gateway-account-policy) policies to `permit`, `deny`, or manually `review` requests from any account. You can also [revoke](/apidocs/vpc-beta/initial#revoke-account-for-private-path-service-gateway) current and future access for an account. For more information, see [About account policies](/docs/vpc?topic=vpc-pps-about-account-policies).

- [Endpoint gateway bindings](/apidocs/vpc-beta/initial#list-private-path-service-gateway-endpoint-gateway) represent each request to access to the private path service gateway. The associated account policy is applied to all `pending` endpoint gateway bindings. If an associated account policy doesn't exist, the private path service gateway's `default_access_policy` is used.  If the resulting policy is `review`, you will be able to explicitly approve or deny the request, and optionally set a new policy for future requests from the account.

Learn about [creating private path service gateways](/docs/vpc?topic=vpc-private-path-service-about).

## 19 December 2023
{: #19-december-2023-beta}

### For all version dates
{: #19-december-2023-all-version-dates-beta}

This release introduces the following updates for accounts that have been granted special approval to preview these features:

**Confidential computing capabilities.** On select instance profiles, you can now enable [Intel&reg; Software Guard Extensions](/docs/vpc?topic=vpc-about-sgx-vpc). When [creating](/apidocs/vpc-beta#create-instance) or [updating](/apidocs/vpc-beta#update-instance) an instance or when [creating](/apidocs/vpc-beta#create-instance-template) or [updating](/apidocs/vpc-beta#update-instance-template) an instance template, you can specify the new `confidential_compute_modes` property value (`disabled` or `sgx`) to use for a virtual server instance. The new `confidential_compute_modes` instance profile property indicates which profiles will support which modes. If you do not specify the `confidential_compute_modes` property when creating an instance or instance template, the default confidential compute mode from the profile will be used.

**Secure boot capabilities.** When [creating](/apidocs/vpc-beta#create-instance) or [updating](/apidocs/vpc-beta#update-instance) an instance or when [creating](/apidocs/vpc-beta#create-instance-template) or [updating](/apidocs/vpc-beta#update-instance-template) an instance template, you can set the new `enable_secure_boot` property to `true` to enable secure boot on the virtual server instance. The new `secure_boot_modes` instance profile property indicates the secure boot modes supported by the profile. If you do not specify the `enable_secure_boot` property when creating an instance or instance template, the default secure boot mode from the profile will be used. To use secure boot, the image must support secure boot or the instance will fail to boot.

To update the `enable_secure_boot` and `confidential_compute_mode` properties, the virtual server instance `status` must be `stopping` or `stopped`.
{: note}

## 19 September 2023
{: #19-september-2023-beta}

### For all version dates
{: #19-september-2023-all-version-dates-beta}

**New {{site.data.keyword.block_storage_is_full}} profile.** For accounts that have been granted special approval to preview this feature a new `defined_performance` family is introduced for data and boot [volumes](/apidocs/vpc-beta/initial#create-volume). The `defined_performance` volume profile family contains the `sdp` profile, which provides similar functionality to the `custom` volume profile. The new profile introduces the ability to make capacity increases and IOPS changes to volumes, even when they're not attached to a virtual server instance. The properties `unattached_capacity_update_supported` and `unattached_iops_update_supported` properties have been added to all volumes and volume profiles so you can make use of these capabilities in your automation. For more information, see [About Block Storage for VPC](/docs/vpc?topic=vpc-block-storage-about) and [Viewing available IOPS profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#view-iops-profiles).

## 8 August 2023
{: #8-august-2023-beta}

### For version `2023-08-08` or later
{: #version-2023-08-08-beta}

This release introduces the following behavior changes for users with accounts that have access to file shares.

**Fail over to replica share.** When making API requests using a `version` query parameter of `2023-08-08` or later, the default value for the  `fallback_policy` property has been changed to `fail`, and the replication relationship between the shares is broken.

**Retrieve source share information for a replica share.** When making API requests using a `version` query parameter of `2023-08-08` or later, requests to [retrieve the source file share for a replica file share](/apidocs/vpc-beta/initial#get-share-source) now return a more concise source share reference, instead of a share.

For more information, see [About file share replication](/docs/vpc?topic=vpc-file-storage-replication) and [2023-08-08` API migration (file shares)](/docs/vpc?topic=vpc-2023-08-08-migration-file-shares).

## 11 July 2023
{: #11-july-2023-beta}

### For version 2023-07-11 or later
{: #version-2023-07-11-beta}

**Data encryption in transit for file shares.** For users with accounts that have access to file shares, you can now enable secure end-to-end encryption of your data in transit between the file share and the authorized client.

When [creating a mount target for a file share](/apidocs/vpc-beta/initial#create-share-mount-target) with a virtual network interface, you can now specify a `transit_encryption` property value of `none` (default) or `user_managed`, which encrypts the data in transit by using IPsec with an instance identity certificate. For more information, see [Encryption in transit](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=api#fs-eit) and [Instance identity certificates](/docs/vpc?topic=vpc-metadata-beta-api-change-log#11-july-2023-metadata-beta) in the Beta VPC Instance Metadata API change log.

**File share access control modes.** For users with accounts that have access to file shares, you can now control the way a share is accessed when [creating](/apidocs/vpc-beta/initial#create-share) and [updating](/apidocs/vpc-beta/initial#update-share) a file share. Specifying `access_control_mode` property value `security_group` now allows the use of security groups to manage which resources can access the file share. By using security groups, access can now be restricted to specific clients. When you specify `access_control_mode` property value `vpc`, all clients in each mount target's VPC will continue to have access to this share.

The default value of `access_control_mode` depends on the `version` query parameter date and the profile selected. When making API requests with a `version` query parameter of `2023-07-11` or later, the default is `security_group`. For requests that are using a `version` query parameter of `2023-07-10` or earlier, the default is `vpc`. File shares must be based on the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#dp2-profile) to use the `security_group` value. See [2023-07-11 API migration (file shares)](/docs/vpc?topic=vpc-2023-07-11-migration-file-shares) for guidance on migrating `access_control_mode` from `vpc` to `security_group`.

When [creating a mount target](/apidocs/vpc-beta/initial#create-share-mount-target) for a file share with `access_control_mode` set to `security_group`, you must also create a virtual network interface by using the `virtual_network_interface` property. For more information, see [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about&interface=api) and [Mount target access modes](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode). You must use an `access_control_mode` of `security_group` to enable [Data encryption in transit for file shares](/docs/vpc?topic=vpc-file-storage-vpc-eit).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 13 June 2023
{: #13-june-2023-beta}

### For all version dates
{: #13-june-2023-all-version-dates-beta}

**Image lifecycle management property name changes.** For accounts that have been granted special approval to preview the image lifecycle management feature, the `deprecated_at` and `obsoleted_at` properties for [images](/apidocs/vpc-beta/initial#create-image) requests have been renamed `deprecation_at` and `obsolescence_at`, respectively. Original property names `deprecated_at` and `obsoleted_at` will continue to be supported until the feature becomes generally available. Requests that specify the original and revised property names simultaneously will be rejected.

This feature is now generally available. Support for property names `deprecated_at` and `obsoleted_at` has been removed. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#11-july-2023).

## 30 May 2023
{: #23-may-2023-beta}

### For version `2023-05-30` or later
{: #version-2023-05-30-beta}

This release introduces the following features for users with accounts that have access to file shares.

**File shares property and request path name changes.** When making API requests using a `version` query parameter of `2023-05-30` or later, the shares `targets` property has been changed to `mount_targets`. This change applies when [creating](/apidocs/vpc-beta/initial#create-share), [updating](/apidocs/vpc-beta/initial#update-share), [listing](/apidocs/vpc-beta/initial#list-shares), and [retrieving](/apidocs/vpc-beta/initial#get-share) a file share, and when [listing all mount targets for a file share](/apidocs/vpc-beta/initial#list-share-mount-targets).

The name change also applies to the method paths: Requests using a `version` query parameter of `2023-05-30` or later must use `/shares/{share_id}/mount_targets` (instead of `/shares/{share_id}/targets`) in the request URL. This change applies when [creating](/apidocs/vpc-beta/initial#create-share-mount-target), [updating](/apidocs/vpc-beta/initial#update-share-mount-target), [listing](/apidocs/vpc-beta/initial#list-share-mount-targets), [retrieving](/apidocs/vpc-beta/initial#get-share-mount-target), and [deleting](/apidocs/vpc-beta/initial#delete-share-mount-target) share mount targets.

See [Updating to the `2023-05-30` version (file shares, mount targets)](/docs/vpc?topic=vpc-2023-05-30-migration-file-shares) for guidance on migrating from `targets` to  `mount_targets`.

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023). Support for the `targets` property has been removed.

### For all version dates
{: #30-may-2023-all-version-dates-beta}

**Enforcement of file shares beta API requests.** Starting with API version `2023-05-30`, all requests made for [shares methods](/apidocs/vpc-beta/initial#list-shares) must include the [`maturity=beta`](/apidocs/vpc-beta/initial#maturity-query-parameter-beta) query parameter. Requests that omit the `maturity=beta` query parameter will be regarded as requests against the [VPC GA API](/apidocs/vpc), which does not yet support shares. As a result, those requests will fail.

## 11 April 2023
{: #11-april-2023-beta}

### For all version dates
{: #11-april-2023-all-version-dates-beta}

**Revised file share profiles.** For users with accounts that have access to file shares, a new `dp2` profile is now available when [creating](/apidocs/vpc-beta/initial#create-share) and [updating](/apidocs/vpc-beta/initial#update-share) a file share. Profiles in the existing `custom` and `tiered` families have been deprecated and will remain available only to accounts that have already provisioned file shares with those profiles. The deprecated profiles also will not be included in the upcoming general availability release for file shares.

The `dp2` profile belongs to a new `defined_performance` file share profile family, which provides similar functionality to the deprecated `custom` file share profile family. While existing file shares using profiles in the `custom` and `tiered` families will continue to work, you are encouraged to update all your file shares to the new `dp2` profile in preparation for general availability of the file share service. Bulk migration of existing file shares is not supported. For more information, see [dp2 file storage profile](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile).

**IOPS and size configuration for file share profiles.** New file share properties `size` and `iops` are also provided in the API responses when [retrieving a file share profile](/apidocs/vpc-beta/initial#get-share-profile). The `size` property shows the permitted capacity range (in gigabytes) for a share with the profile. The `iops` property shows the permitted IOPS range for a share with the profile. The maximum IO operations each client that accesses the file can perform is 48,000 IOPS. When multiple clients access the file share, the share can handle a maximum of 96,000 IO operations per second.

This API feature was released to production on `2023-04-11`, but this announcement was not included at the time.
{: note}

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

**Image lifecycle management.** Accounts that have been granted special approval to preview this feature can now [deprecate](/apidocs/vpc-beta/initial#deprecate-image) or [obsolete](/apidocs/vpc-beta/initial#obsolete-image) custom images directly. Alternatively, you can schedule transition at a later date by specifying the `deprecated_at` or `obsoleted_at` properties when [creating](/apidocs/vpc-beta/initial#create-image) or [updating](/apidocs/vpc-beta/initial#update-image) an image. If you need to revert a status change, you can transition `deprecated` or `obsolete` images back to `available`. For more information, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=api).

`deprecated` custom images remain usable, while `obsolete` images cannot be used to provision instances or bare metal servers.
{: note}

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#11-july-2023).

## 14 February 2023
{: #14-february-2023-beta}

### For all version dates
{: #14-february-2023-all-version-dates-beta}

**Exporting custom images.** Accounts that have been granted special approval to preview this feature can now [export custom images](/apidocs/vpc-beta/initial#create-image-export-job) to an authorized IBM Cloud Object Storage bucket. Specify the target `storage_bucket` to export the image to. The image will be exported as `qcow2` unless you specify another value using the `format` property.

For more information, see [Exporting a custom image to IBM Cloud Object Storage](/docs/vpc?topic=vpc-managing-custom-images&interface=api#custom-image-export-to-cos-api), or start using the new [export jobs](/apidocs/vpc-beta/initial#list-image-export-jobs) methods.

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#2-may-2023).

## 20 December 2022
{: #20-december-2022-beta}

### For all version dates
{: #20-december-2022-all-version-dates-beta}

**Backup for VPC.** Backup policy jobs are now generally available. The following updates have been made for [listing backup policy jobs](/apidocs/vpc-beta/initial#list-backup-policy-jobs) and [retrieving a backup policy job](/apidocs/vpc-beta/initial#get-backup-policy-job) since the beta release:

* The `source_volume` property has been replaced by the `source` property.
* The `target_snapshot` property has been replaced by the `target_snapshots` array property.

See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#20-december-2022).

**Instance provision by volume.** Accounts that have been granted special approval to preview this feature can now reuse an existing boot volume to provision a virtual server instance by specifying the existing volume's `id` or `crn` sub-property of the `boot_volume_attachment` property. The specified volume must be unattached and must have an operating system with the same architecture as the instance profile. Volumes now include an `attachment_state` property and an expanded `operating_system` property you can use to view a volume's eligibility. You can also use the new [list volumes](/apidocs/vpc-beta/initial#list-volumes) filters to list volumes that have specific `attachment_state`, `operating_system`, and `encryption_type` values.

By default, a boot volume attached to a virtual server instance is deleted when the instance is deleted. To preserve the boot volume when deleting a virtual server instance, change the `delete_volume_on_instance_delete` property to `false` by updating the [boot volume attachment](/apidocs/vpc-beta/initial#update-instance-volume-attachment). See [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui), [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli) for more information.

This feature is now generally available. Since the beta release, by default, only a boot volume created as part of provisioning a virtual server instance will be deleted when the instance is deleted. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#21-march-2023).

## 16 August 2022
{: #16-august-2022-beta}

### For all version dates
{: #16-august-2022-all-version-dates-beta}

**Sharing images across an enterprise account.** Accounts that have been granted special approval to preview this feature can now use a [catalog to share custom images](/docs/vpc?topic=vpc-planning-custom-images) with users in other accounts within the same enterprise. When you [create an image](/apidocs/vpc-beta/initial#create-image), a new `catalog_offering` property includes a `published` sub-property that is set to `false` by default. When the custom image is imported to a catalog the `published` sub-property is set to `true`, indicating that the image is added to a catalog offering `version` and is managed from a catalog. If you are authorized to the catalog offering `version`, you can [provision virtual server instances](/apidocs/vpc-beta/initial#create-instance) using that custom image by specifying its `catalog_offering.version.crn`. To use the custom image associated with the latest version in the offering, specify `catalog_offering.offering.crn` instead. The image may not be deleted from your IBM Virtual Private Cloud while it is managed from a catalog.

For more information, see the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial) and the [Import offering](/apidocs/resource-catalog/private-catalog#import-offering){: external} method in the Catalog Management API.

This feature is now generally available. The `catalog_offering.published` property in the beta API definition has been renamed to `catalog_offering.managed`. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#27-september-2022).

## 12 July 2022
{: #12-july-2022-beta}

### For all version dates
{: #12-july-2022-all-version-dates-beta}

**File storage cross-account encryption.**  Accounts that have been granted special approval to preview this feature can now use cross-account customer-managed encryption keys (CRKs) when [creating a file share](/apidocs/vpc-beta/initial#create-share) with [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption). With this feature, the CRK account owner [invites you](/docs/account?topic=account-iamuserinv&interface=api) and sets the IAM delegated policy to the CRKs. Afterward, specify your IAM token to create a file share with an `encryption_key` CRN from the CRK account. For more information, see [Cross-account encryption for file storage resources](/docs/vpc?topic=vpc-vpc-byok-cross-acct-key-file&interface=api).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 5 July 2022
{: #5-july-2022-beta}

### For all version dates
{: #5-july-2022-all-version-dates-beta}

**Client VPN for VPC.**  Client-to-site connectivity is now  generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#5-july-2022).

The following updates have been made since the [24 August 2021 beta release](#24-august-2021-beta):

- You can use IBM Cloud Secrets Manager for server authentication when [creating a VPN server](/apidocs/vpc-beta/initial#create-vpn-server)
- You can specify a VPN server when [adding a target to a security group](/apidocs/vpc-beta/initial#create-security-group-target-binding)
- You can [update a VPN server](/apidocs/vpc-beta/initial#update-vpn-server) to be highly available, or detach a subnet to downgrade to a stand-alone deployment

## 14 June 2022
{: #14-june-2022-beta}

### For all version dates
{: #14-june-2022-all-version-dates-beta}

**File storage adjustable IOPS.** Accounts that have been granted special approval to preview this feature can now [update the IOPS of an existing file share](/apidocs/vpc-beta/initial#update-share). For a file share using a `custom` profile, specify the `iops` property. For a file share using a profile in the `tiered` profile family, specify another tier within the `tiered` family, which will set the `iops` based on the share's size.

You can also change a share between the `tiered` and `custom` profile families so long as the requested `iops` and `size` are supported by the requested profile. For more information about file share profiles, see [File Storage for VPC profiles](/docs/vpc?topic=vpc-file-storage-profiles). For information about adjusting IOPS with a profile, or changing between `tiered` and `custom` profiles, see [Adjusting file share IOPS](/docs/vpc?topic=vpc-adjusting-share-iops&interface=api).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 31 May 2022
{: #31-may-2022-beta}

### For all version dates
{: #31-may-2022-all-version-dates-beta}

**AMD support for instances and dedicated hosts.** Accounts that have been granted special approval to preview this feature can select new profiles for [dedicated hosts](/apidocs/vpc-beta/initial#list-dedicated-host-profiles) and [instances](/apidocs/vpc-beta/initial#list-instance-profiles). When provisioning or managing an instance or dedicated host, use the new `vcpu_manufacturer` property to choose between profiles from different processor manufacturers.

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#28-march-2023).

## 24 May 2022
{: #24-may-2022-beta}

### For all version dates
{: #24-may-2022-all-version-dates-beta}

**Backup for VPC.** Accounts with special approval to preview this feature can now [create](/apidocs/vpc-beta/initial#create-backup-policy-plan) up to four plans for backup policies. You can now also [update](/apidocs/vpc-beta/initial#update-backup-policy-plan) and [delete](/apidocs/vpc-beta/initial#delete-backup-policy-plan) existing plans, and [add backup plans](/vpc-beta/initial#create-backup-policy) to existing policies. You can use one of the new `deletion_trigger` sub-properties to specify a custom backup deletion policy. For more information, see [Managing backup policies](/docs/vpc?topic=vpc-backup-service-manage&interface=api).

Backup policies now also include information about [backup policy jobs](/apidocs/vpc-beta/initial#list-backup-policy-jobs). A [backup policy job](/docs/vpc?topic=vpc-backup-view-policy-jobs&interface=api) is automatically created each time a backup has to be created or deleted to meet the backup policy's settings. For more information, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

The backup API is now generally available, with the exception of the backup jobs API, which remains in beta. See the [Change log](/docs/vpc?topic=vpc-api-change-log#21-june-2022).

The backup jobs API is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#20-december-2022)

## 17 May 2022
{: #17-may-2022-beta}

### For all version dates
{: #17-may-2022-all-version-dates-beta}

**File share replication.** Accounts that have been granted special approval to preview this feature can now set up replication between a file share in one zone and a replica file share in another zone in the same region. [Using replication](/docs/vpc?topic=vpc-file-storage-replication) is a good way to recover from an incident at your primary site, if data becomes inaccessible or an applications fails.

- When [creating a new file share](/apidocs/vpc-beta/initial#create-share), you can now configure replication by specifying the new `replica_share` property. To create a replica for an existing file share, create a new share and specify the existing file share as `source_share`, along with the replication schedule, using the `replication_cron_spec` property.
- You can now [retrieve the source file share for a replica file share](/apidocs/vpc-beta/initial#get-share-source). Returned information also includes the replication schedule, role, status, and status reasons.
- You can now [split the source file from a replica share](/apidocs/vpc-beta/initial#delete-share-source), which removes the replication relationship between a source share and its replica share, resulting in two independent read-write file shares. This action cannot be reversed. For more information, see [Remove the replication relationship](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-remove-replication).
- You can now [fail over to the replica file share](/apidocs/vpc-beta/initial#failover-share), which reverses the replication relationship. You can optionally specify `split` for the `fallback_policy` to trigger a split if the failover operation fails or times out. For more information, see [Replication failover](/docs/vpc?topic=vpc-file-storage-failover&interface=api).

**File storage native tagging.** Accounts that have been granted special approval to preview this feature can now specify `user_tags` when [creating a new file share](/apidocs/vpc-beta/initial#create-share) or [updating an existing file share](/apidocs/vpc-beta/initial#update-share). Adding user tags to a file share helps you organize your resources. For details, see [Add user tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=api#fs-add-user-tags).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 22 March 2022
{: #22-march-2022-beta}

### For all version dates
{: #22-march-2022-all-version-dates-beta}

**VPN client-to-site servers update.** You can now [update the subnets for a VPN server](/apidocs/vpc-beta/initial#update-vpn-server) after the VPN is provisioned. For example, you can upgrade a stand-alone VPN server (one subnet) to a High Availability (HA) VPN server (two subnets in different zones). You can also detach a subnet to downgrade an HA VPN server to a stand-alone deployment, or change an attached subnet after your VPN server is provisioned. For more information, see [Upgrading to an HA VPN server](/docs/vpc?topic=vpc-vpn-client-to-site-change-server-types&interface=api).

## 8 March 2022
{: #8-march-2022-beta}

### For all version dates
{: #8-march-2022-all-version-dates-beta}

**Backup for VPC.** Accounts that have been granted special approval to preview this feature can now create backup policies to automatically back up block storage volumes. Use the new [backup service APIs](/apidocs/vpc-beta/initial#list-backup-policies) to create, list, and manage backup policies. Backup policies control which source volumes are selected for backup by [matching user tags](/docs/vpc?topic=vpc-backup-service-about&interface=api#backup-service-about-tags) in the volume with tags defined in the backup policy. For this beta release, a backup policy contains one [backup plan](/apidocs/vpc-beta/initial#create-backup-policy-plan) in which a `deletion_trigger` specifies the maximum number of days to keep each backup after creation. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about&interface=api).

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

**Customer-managed encryption for file shares.** Accounts that have been granted special approval to preview this feature can now use customer-managed encryption, also called Bring Your Own Key (BYOK), to [create a file share](/apidocs/vpc-beta/initial#create-share) that is encrypted using your root key. Specify the new `encryption_key` property and `crn` sub-property of the root key that you either imported to {{site.data.keyword.cloud_notm}} or created in Key Protect or Hyper Protect Crypto Services (HPCS). For more information, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption&interface=api).

You can also use the API to rotate the root keys that are protecting your file shares. See [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation&interface=api#vpc-key-rotation-api-procedure) for details.
{: tip}

**Supplemental user/group IDs for file shares.** Accounts that have been granted special approval to preview this feature can now access supplemental user/group IDs for file shares. When a process runs on Unix/Linux, the operating system identifies a user with a user ID (UID) and/or group with a group ID (GID). These IDs determine which system resources a user or group can access. When you [create a file share](/apidocs/vpc-beta/initial#create-share), you can specify the new `initial_owner` property and specify a `uid`, `gid`, or both sub-properties to control access to the share. For more information, see [Adding supplemental IDs when you create a file share from the API](/docs/vpc?topic=vpc-file-storage-create&interface=api#fs-add-supplemental-id-api).

## 24 August 2021
{: #24-august-2021-beta}

### For all version dates
{: #24-august-2021-all-version-dates-beta}

**VPN client-to-site servers.** Until now, the {{site.data.keyword.vpn_full}} service supported only site-to-site connectivity, which connects your on-premises network to the {{site.data.keyword.cloud_notm}}. This beta release adds client-to-site connectivity, which allows remote devices to securely connect to the VPC network using an OpenVPN software client. This solution is useful for telecommuters who want to connect to the {{site.data.keyword.cloud_notm}} from a remote location, such as a home office. For more information, see [About VPN servers (client-to-site)](/docs/vpc?topic=vpc-vpn-client-to-site-overview) or check out the new [API methods](/apidocs/vpc-beta/initial#list-vpn-servers).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#5-july-2022).

## 17 August 2021
{: #17-august-2021-beta}

### For all API version dates
{: #17-august-2021-all-version-dates-beta}

**File storage for VPC.** Accounts that have been granted special approval to preview this feature can now increase file share size in gigabyte increments up to 32 TB (depending on the file share's profile). The increase takes effect immediately. For more information, see [Expanding file share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity&interface=api). You can also specify the maximum input/output operations per second (IOPS) when [creating](/apidocs/vpc-beta/initial#create-share) a file share, within the range available for its size. For more information, see [Custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=api#fs-custom).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 10 August 2021
{: #10-august-2021-beta}

### For all version dates
{: #10-august-2021-all-version-dates-beta}

**Beta VPC instance metadata API.** Accounts that have been granted special approval to preview this feature can now preview the new beta {{site.data.keyword.vpc_full}} (VPC) [Instance Metadata API](/apidocs/vpc-metadata-beta/initial). This API provides access to VPC instance metadata, including instance initialization data, network interfaces, volume attachments, public SSH keys, and placement groups. Use the [VPC create instance](/apidocs/vpc-beta/initial#create-instance) or [VPC update instance](/apidocs/vpc-beta/initial#update-instance) methods to enable or disable the metadata service endpoint for a particular instance. For more information, see [About VPC Instance Metadata](/docs/vpc?topic=vpc-imd-about).

## 27 July 2021
{: #27-july-2021-beta}

### For all API version dates
{: #27-july-2021-all-version-dates-beta}

**GPU instances.** Accounts that have been granted special approval to preview this feature can now view new instance profiles, which include GPUs attached to the instance. These attached GPUs allow for accelerated computing to run workloads with more powerful compute capabilities.

The following API methods have been enhanced:

- [List instances](/apidocs/vpc-beta/initial#list-instances) returns a new `gpu` property with four additional sub-properties: `count`, `manufacturer`, `model`, and `memory`
- [Retrieve an instance profile](/apidocs/vpc-beta/initial#get-instance-profile) returns four new properties: `gpu_count`, `gpu_manufacturer`, `gpu_model`, and `gpu_memory`

For more information, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#19-october-2021).

## 20 July 2021
{: #20-july-2021-beta}

### For all version dates
{: #20-july-2021-all-version-dates-beta}

**Bare metal servers for VPC.** Accounts that have been granted special approval to preview this feature can now create bare metal servers to host VMware clusters in {{site.data.keyword.vpc_short}}. You can set up VMware management applications and create VMware virtual machines on the bare metal servers. As bare metal servers are integrated with the VPC platform, you can take advantage of the network and security capabilities of {{site.data.keyword.vpc_short}}. For more information, see [About Bare Metal Servers for VPC (beta)](/docs/vpc?topic=vpc-about-bare-metal-servers) or dive into the new [API methods](/apidocs/vpc-beta/initial#list-bare-metal-server-profiles).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#1-february-2022).

## 24 June 2021
{: #24-june-2021-beta}

### For all version dates
{: #24-june-2021-all-version-dates-beta}

**Placement groups.** Placement groups for {{site.data.keyword.vpc_full}} are logical groupings of virtual server instances that can be configured to reduce the risk of correlated failures inherent in your physical environment, such as networking issues, power loss, or hardware failure. Define a placement group strategy for high-availability workloads, such as for host or power spread. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc) or dive into the new [API methods](/apidocs/vpc-beta/initial#list-placement-groups).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#17-august-2021).

## 6 April 2021
{: #6-april-2021-beta}

### For all version dates
{: #6-april-2021-all-version-dates-beta}

**File storage for VPC.** Accounts that have been granted special approval to preview this feature can now create NFS-based file shares in a zone in your region. Share file storage over multiple virtual service instances within the same zone across multiple VPCs. Learn about [creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-vpc-about), and explore the new [API methods](/apidocs/vpc-beta/initial#list-share-profiles).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 19 March 2021
{: #19-march-2021-beta}

### For all version dates
{: #19-march-2021-all-version-dates-beta}

**Instance resize.** You can now resize an instance by providing the `profile` property in the API method `PATCH /instances/{id}` ([Update an instance](/apidocs/vpc-beta/initial#update-instance)). For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance&interface=api).

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

**Instance resize.** Accounts that have been granted special approval to preview this feature can now resize an instance by providing the `profile` property in the API method `PATCH /instances/{id}` ([Update an instance](/apidocs/vpc-beta/initial#update-instance)). For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance&interface=api).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#30-march-2021).

## 22 February 2021
{: #22-february-2021-beta}

### For all version dates
{: #22-february-2021-all-version-dates-beta}

**Block storage snapshots.** Accounts that have been granted special approval to preview this feature can now create and use [snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) and explore the new [snapshots API methods](/apidocs/vpc-beta/initial#list-snapshots).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#18-may-2021).

**Virtual server instance console.** Accounts that have been granted special approval to preview this feature can now access instances by connecting to a VNC or serial console. Learn about [accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console&interface=api), and explore the new instance console API methods:

- [Create a console access token for an instance](/apidocs/vpc-beta/initial#create-instance-console-access-token)
- [Retrieve the console WebSocket for an instance](/apidocs/vpc-beta/initial#get-instance-console)

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#30-march-2021).
