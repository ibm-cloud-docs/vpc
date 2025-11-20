---

copyright:
  years: 2021, 2025
lastupdated: "2025-09-23"

keywords: api, change log, beta

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Beta VPC API change log
{: #api-change-log-beta}

Read this change log to learn about updates and improvements to the beta {{site.data.keyword.vpc_full}} (VPC) [API](/apidocs/vpc-beta). Change log announcements are ordered by the date they were released.
{: shortdesc}

Some beta features are for accounts that have special approval to preview a particular beta feature. Contact your IBM sales representative if you are interested in getting access.
{: beta}

There are no backward-compatibility guarantees as a feature progresses through its beta phase or from the final beta release to its initial GA release. Using non-GA-mature features could introduce the risk of corrupting resources in your account. IBM strongly recommends that you do not use non-GA-mature features on production accounts.
{: important}

To review the change log of generally available API features, see the [VPC API change log](/docs/vpc?topic=vpc-api-change-log).

## 26 August 2025
{: #26-august-2025-beta}

### For all version dates
{: #26-august-2025-all-version-dates-beta}

**Burstable (shared core) instances.** Accounts that have special approval to preview this feature can now enable [burstable virtual server instances](/docs/vpc?topic=vpc-burstable-virtual-servers) by setting the `vcpu.tenancy` property to `shared` when [creating an instance](/apidocs/vpc-beta#create-instance) or when [creating an instance template](/apidocs/vpc-beta#create-instance-template). You can also set the `vcpu.tenancy` to `shared` or `dedicated` when [updating an instance](/apidocs/vpc-beta#update-instance). Additionally, [instance profiles](/apidocs/vpc-beta#list-instance-profiles) that support the burstable instances feature will have `shared` included in their `vcpu_tenancy` property as well as the supported percentage shares of the VCPUs in the `vcpu_percentage` property.

## 22 July 2025
{: #22-july-2025-beta}

API changes supporting regional file shares are now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#16-september-2025).

### For all version dates
{: #22-july-2025-all-version-dates-beta}

This release introduces the following updates for accounts that have special approval to preview and use these features. Although usage of these features is restricted, changes to schemas (such as new properties) will be visible to all accounts.

**File shares with regional availability.** You can now [create file shares](/apidocs/vpc-beta#create-share) with regional availability by specifying the `rfs` profile. When creating file shares with regional availability, the `zone` property must not be specified. For more information, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about). See also [Storage known issues](/docs/vpc?topic=vpc-known-issues#storage-vpc-known-issues).

Snapshots, accessor shares, cross-region replication, and encryption at rest for regional file shares are not currently supported.
{: note}

**Enhanced transit encryption support for file shares.** When [creating a file share](/apidocs/vpc-beta#create-share) with `storage_generation` of `2`, you can include the new [`stunnel`](/docs/vpc?topic=vpc-file-storage-vpc-eit-tls) value when specifying the `allowed_transit_encryption_modes` property for this share. Subsequently, you can also specify the `stunnel` value for the `transit_encryption` property when [creating a mount target](/apidocs/vpc-beta#create-share-mount-target) for the file share. The `stunnel` transit encryption mode is supported only for file shares that are created with a `storage_generation` of `2`.

When [retrieving](/apidocs/vpc-beta#get-share-profile) or [listing](/apidocs/vpc-beta#list-share-profiles) file share profiles, a new `allowed_transit_encryption_modes` property is provided in the response. The `allowed_transit_encryption_modes.default` property denotes the allowed transit encryption modes for a share with this profile, which will be used if `allowed_transit_encryption_modes` is not specified when [creating a file share](/apidocs/vpc-beta#create-share). 

**Allowed access protocols for file shares.** When [creating a file share](/apidocs/vpc-beta#create-share), a set of [`allowed_access_protocols`](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=api#fs-allowed-access-protocols) may now be specified to denote which access protocols to use when [creating mount targets for that file share](/apidocs/vpc-beta#create-share-mount-target). The `allowed_access_protocols` properties are also included in the `Share` and `ShareProfile` response schemas.

**Allowed transit encryption modes for files shares.** When [retrieving](/apidocs/vpc-beta#get-share-profile) or [listing](/apidocs/vpc-beta#list-share-profiles) file share profiles, the response now includes an `allowed_transit_encryption_modes` property. This property denotes the allowed [transit encryption modes](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-eit) used when [creating file shares](/apidocs/vpc-beta#create-share) using the specified profile, and subsequently when [creating mount targets](/apidocs/vpc-beta#create-share-mount-target) for those file shares.

**Bandwidth for file shares.** When [creating](/apidocs/vpc-beta#create-share), [retrieving](/apidocs/vpc-beta#get-share), or [listing](/apidocs/vpc-beta#list-shares) file shares, and when [retrieving](/apidocs/vpc-beta#get-share-profile) or [listing](/apidocs/vpc-beta#list-share-profiles) file share profiles, the response now includes a `bandwidth` property that denotes the [available bandwidth](/docs/vpc?topic=vpc-file-storage-profiles&interface=api) that is provided when that profile is specified during file share [creation](/apidocs/vpc-beta#create-share).

For more information, see [About File Storage for VPC](/docs/vpc?topic=vpc-file-storage-vpc-about) and [Securing mount connections between a file share and virtual server instance](/docs/vpc?topic=vpc-file-storage-vpc-eit). See also [Storage known issues](/docs/vpc?topic=vpc-known-issues#storage-vpc-known-issues).

### For version `2025-07-22` or later
{: #version-2025-07-22-beta}

**File share `zone` property no longer always included.** When using a `version` query parameter of `2025-07-22` or later, the response no longer includes `zone` when [creating](/apidocs/vpc-beta#create-share), [updating](/apidocs/vpc-beta#update-share), [retrieving](/apidocs/vpc-beta#get-share), [listing](/apidocs/vpc-beta#list-shares), or [deleting](/apidocs/vpc-beta#delete-share) regional file shares. When using a `version` query parameter of `2025-07-21` or earlier, the `zone` property in the `Share` response for regional file shares will return the first zone from the region.

**File share `user_managed` transit encryption mode renamed.** When using a `version` query parameter of `2025-07-22` or later, the IPsec encryption mode is represented as `ipsec` rather than `user_managed`. This change affects the `allowed_transit_encryption_modes` property in the `Share`, `SharePrototype`, `SharePatch`, and `ShareProfile` schemas, as well as the `transit_encryption` property in the `ShareMountTarget` and `ShareMountTargetPrototype` schemas. Requests using a `version` query parameter of `2025-07-21` or earlier are unchanged.

**File share mount target access protocol and transit encryption.** When using a `version` query parameter of `2025-07-22` or later, [`access_protocol`](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=api#fs-allowed-access-protocols) and [`transit_encryption`](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-allowed-eit-modes) modes must be specified when [creating a mount target for a file share](/apidocs/vpc-beta#create-share-mount-target). The specified value for the `access_protocol` must be included in the share's `allowed_access_protocols` property. The specified value for `transit_encryption` must be included in the share's `allowed_transit_encryption_modes` property. Requests using a `version` query parameter of `2025-07-21` or earlier are unchanged.

For more information, see [About File Storage for VPC snapshots](/docs/vpc?topic=vpc-file-storage-vpc-about). See also [Storage known issues](/docs/vpc?topic=vpc-known-issues#storage-vpc-known-issues).

For migration guidance, see [Updating to the `2025-07-22` version (file shares, file share profiles, and file share mount targets)](/docs/vpc?topic=vpc-2025-07-22-migration-file-shares).

## 15 July 2025
{: #15-july-2025-beta}

### For all version dates
{: #15-july-2025-all-version-dates-beta}

VPC Metadata service support for bare metal servers is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#26-august-2025).

**VPC Metadata service support for bare metal servers.** Accounts that have special approval to preview this feature can now enable access to the VPC Metadata service for bare metal servers when [creating](/apidocs/vpc/latest#create-bare-metal-server) or [updating](/apidocs/vpc/latest#update-bare-metal-server) a bare metal server. You can access the VPC Metadata service on a bare metal server using an [endpoint URL](/apidocs/vpc-identity-beta/initial#endpoint-urls-identity-beta) by specifying the new `metadata_service.protocol` property as `http` or `https`.

Access to the VPC Metadata service is disabled on bare metal servers by default. To enable access to the service, when creating or updating a bare metal server, set the new `metadata_service.enabled` property to `true`. The default communication protocol to the VPC Metadata service from the server is `http` (unencrypted). To change the protocol to secure access, specify the `metadata_service.protocol` as `https`.

For more information, see the [Beta VPC Identity API](/apidocs/vpc-identity-beta).

## 24 June 2025
{: #24-june-2025-beta}

### For all version dates
{: #24-june-2025-all-version-dates-beta}

Flex instance profiles are now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#30-september-2025).

**Flex instance profiles.** Accounts that have special approval to preview this feature can now use [flex instance profiles](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers) when [creating](/apidocs/vpc-beta#create-instance) or [updating](/apidocs/vpc-beta#update-instance) an instance, or when [creating](/apidocs/vpc-beta#create-instance-template) an instance template.

**Default instance profile change.** When [creating](/apidocs/vpc-beta#create-instance) a new instance, the `profile` property will now default to `bxf-2x8` for accounts that have special approval to preview [flex instance profiles](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers).

## 15 April 2025
{: #15-april-2025-beta}

### For all version dates
{: #15-april-2025-all-version-dates-beta}

**Public address ranges.** Accounts that have special approval to preview this feature can now [create a public address range](/apidocs/vpc-beta#create-public-address-range). A public address range is a contiguous block of public IP addresses that can be bound to a zone in a VPC. You can bind a public address range when creating it, or [update](/apidocs/vpc-beta#update-public-address-range) its binding later by setting its `target`. You can update `target.zone` to bind it to anther zone in the VPC, or update `target.vpc` to bind it to another VPC. Traffic that originates from the internet destined for addresses in a public address range must be routed to resources in the bound VPC zone by [creating VPC routes](/apidocs/vpc-beta#create-vpc-routing-table-route) in the VPC's routing table with `route_internet_ingress` set to `true`.

When [retrieving](/apidocs/vpc-beta#get-vpc) or [listing](/apidocs/vpc-beta#list-vpcs) VPCs, the response includes the `public_address_ranges` that are bound to each VPC.

Learn about [public address ranges](/docs/vpc?topic=vpc-about-par), and explore the new [API methods](/apidocs/vpc-beta#list-public-address-ranges).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#23-september-2025).

## 17 December 2024
{: #17-december-2024-beta}

### For all version dates
{: #17-december-2024-all-version-dates-beta}

**Enhanced confidential computing capabilities.** Accounts that have special approval to preview this feature can now enable [Intel&reg; Trust Domain Extensions (TDX)](/docs/vpc?topic=vpc-about-confidential-computing-vpc). [Instance profiles](/apidocs/vpc-beta#list-instance-profiles) that support TDX will have `tdx` included in their `confidential_compute_modes.values` property. To use TDX, you can specify that profile along with the `confidential_compute_mode` value of `tdx` when [creating](/apidocs/vpc-beta#create-instance) or [updating](/apidocs/vpc-beta#update-instance) an instance, or when [creating](/apidocs/vpc-beta#create-instance-template) an instance template. For more information, see [Confidential computing with Intel Trust Domain Extensions (TDX) for Virtual Servers for VPC](/docs/vpc?topic=vpc-about-confidential-computing-vpc).

## 22 October 2024
{: #22-october-2024-beta}

### For all version dates
{: #22-october-2024-all-version-dates-beta}

Reservation automatic attachment support is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#19-november-2024).

**Reservation automatic attachment support.** Accounts that have special approval to preview this feature can now [automatically attach](/docs/vpc?topic=vpc-automatic-reservation-vpc) a reservation when [creating an instance](/apidocs/vpc-beta#create-instance) or [creating a bare metal server](/apidocs/vpc-beta#create-bare-metal-server). Additionally, when [updating an instance](/apidocs/vpc-beta#update-instance) or [updating a bare metal server](/apidocs/vpc-beta#update-bare-metal-server), you can change the `reservation_affinity.policy` to `automatic` for the instance or bare metal server to automatically attach to available reserved capacity.

When [creating a reservation](/apidocs/vpc-beta#create-reservation), you can now specify an `affinity_policy` of `restricted` to prevent the policy from being used for automatic attachments. Similarly, while a reservation's `status` is `inactive`, you can [update a reservation](/apidocs/vpc-beta#update-reservation) to be restricted.

For more information, see [Automatic attachments for reservations](/docs/vpc?topic=vpc-automatic-reservation-vpc).

### For version `2024-10-22` or later
{: #version-2024-10-22-beta}

Reservation affinity policy default is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#19-november-2024).

**Reservation affinity policy default.** When using a `version` query parameter of `2024-10-22` or later, the `reservation_affinity.policy` defaults to `automatic` when [creating a reservation](/apidocs/vpc-beta#create-reservation). Similarly, when using a `version` query parameter of `2024-10-22` or later, the `reservation_affinity.policy` defaults to `automatic` when [creating an instance](/apidocs/vpc-beta#create-instance) or [creating a bare metal server](/apidocs/vpc-beta#create-bare-metal-server). The behavior remains unchanged when using a `version` query parameter of `2024-10-21` or earlier.

## 15 October 2024
{: #15-october-2024-beta}

### For all version dates
{: #15-october-2024-all-version-dates-beta}

This release introduces the following updates for accounts that have special approval to preview and use these features. Although usage of these features is restricted, changes to schemas (such as new properties) will be visible to all accounts.

**NVIDIA Hopper HGX H100 instance profiles.** When [creating an instance](/apidocs/vpc-beta#create-instance), a new `gx3d-160x1792x8h100` instance profile is available in select zones. This profile provides 8 NVIDIA H100 GPUs that are tuned for AI workloads, such as inferencing, fine tuning, and large-scale training. For details, see [Accelerated profile family - Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family#hopper-hgx-profiles).

**Cluster networks.** Cluster networks provide high-bandwidth, low-latency networking for workloads such as AI training and large-scale simulations. You can now [create cluster networks](/apidocs/vpc-beta#create-cluster-network) using a [cluster network profile](/apidocs/vpc-beta#get-cluster-network-profile), which defines the cluster network performance characteristics and capabilities. The [H100 cluster network profile](/docs/vpc?topic=vpc-profiles&interface=api#gpu) is the first cluster network profile being introduced. It provides a specialized network that implements the RoCEv2 protocol to enable remote direct memory access for your workloads that are running on the `gx3d-160x1792x8h100` instance profile. 

When [creating an instance](/apidocs/vpc-beta#create-instance) using a supported cluster profile, you can specify the new `cluster_network_attachments` property to connect the virtual server instance to your cluster network. Alternatively, you can [create cluster network attachments](/apidocs/vpc-beta#create-cluster-network-attachment) on an existing instance that is in a `stopping` or `stopped` state. Additionally, when [creating an instance template](/apidocs/vpc-beta#create-instance-template) you can specify `cluster_network_attachments`.

**Instance profile schema changes.** When [retrieving](/apidocs/vpc-beta#get-instance-profile) or [listing](/apidocs/vpc-beta#list-instance-profiles) instance profiles, the response includes the following new properties:

- `cluster_network_attachment_count` specifies the number of cluster network attachments supported for that instance profile.
- `supported_cluster_network_profiles` indicates the cluster network profiles that are supported for that instance profile.

Learn [about cluster networks](/docs/vpc?topic=vpc-about-cluster-network), cluster network subnets, cluster network interfaces, and explore the new [API methods](/apidocs/vpc-beta#list-cluster-networks).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#10-december-2024).

## 17 September 2024
{: #17-september-2024-beta}

This release introduces the following updates for accounts that have special approval to preview these features.

### For all version dates
{: #17-september-2024-all-version-dates-beta}

**Automatic deletion policy for endpoint gateway bindings.** Endpoint gateway bindings are automatically deleted when they are no longer needed. This policy is now reflected in the new `endpoint_gateway_binding_auto_delete` and `endpoint_gateway_binding_auto_delete_timeout` properties included when [retrieving](/apidocs/vpc-beta#get-private-path-service-gateway) or [listing](/apidocs/vpc-beta#list-private-path-service-gateways) private service gateways, and may be configurable in the future.

**Wildcard support for private path service gateway `service_endpoints`.** When [creating a private path service gateway](/apidocs/vpc-beta#create-private-path-service-gateway), wildcard domains in the `service_endpoints` property are now supported, allowing you to specify service endpoints using patterns such as `*.example.com`.

**Load balancer access modes.** New properties are provided in responses to load balancer and load balancer profile methods, allowing you to view the access mode for load balancers. Possible values are `private`, `private_path`, and `public`. When [retrieving](/apidocs/vpc-beta#get-load-balancer-profile) or [listing](/apidocs/vpc-beta#list-load-balancer-profiles) load balancer profiles, the `access_modes` property denotes the access modes supported by load balancers with this profile. When [retrieving](/apidocs/vpc-beta#get-load-balancer) or [listing](/apidocs/vpc-beta#list-load-balancers) load balancers, the `access_mode` property denotes the access mode for the load balancer.

### For version `2024-09-17` or later
{: #version-2024-09-17-beta}

Private Path service gateways is now available for accounts with special approval to preview this feature. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#1-october-2024).

Support for beta API versions earlier than `2024-09-17` was removed on 5 November 2024.
{: attention}

**Publishing or unpublishing changes for private path service gateways.** When using a version query parameter of `2024-09-17` or later to [update a private path service gateway](/apidocs/vpc-beta#update-private-path-service-gateway), the `published` property is no longer supported. `published` is now read only and defaults to `false` when a private path service gateway is first [created](/apidocs/vpc-beta#create-private-path-service-gateway), indicating that access is restricted to the account that created this private path service gateway. New methods must be used to [publish](/apidocs/vpc-beta#publish-private-path-service-gateway) or [unpublish](/apidocs/vpc-beta#unpublish-private-path-service-gateway) a private path service gateway. 

**HTTP status code changes for private path service gateway methods.** When using a version query parameter of `2024-09-17` or later to [permit](/apidocs/vpc-beta#permit-private-path-service-gateway-endpoint-gatew) or [deny](/apidocs/vpc-beta#deny-private-path-service-gateway-endpoint-gateway) an endpoint gateway binding for a private path service gateway, [revoke access](/apidocs/vpc-beta#revoke-account-for-private-path-service-gateway) to a private path service gateway for an account, or [delete](/apidocs/vpc-beta#delete-private-path-service-gateway) a private path service gateway, the response code is changed from `200` to `204` for successful operations. A response code of `200` will continue to be returned for these API requests when using a version query parameter of `2024-09-16` and earlier.

**Private path service gateway property name change.** When using a version query parameter of `2024-09-17` or later to [create](/apidocs/vpc-beta#create-private-path-service-gateway) or [update](/apidocs/vpc-beta#update-private-path-service-gateway) a private path service gateway, the `endpoint_gateways_count` property is renamed to `endpoint_gateway_count`. The `endpoint_gateways_count` property name will continue to be returned when using a version query parameter of `2024-09-16` and earlier.

This feature is now generally available. See "Block storage schema enhancements for adjustable capacity and IOPS" in the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#24-september-2024).

Support for beta API versions earlier than `2024-09-17` was removed on 5 November 2024.
{: attention}

**Revised block storage capabilities.** When making API requests using a `version`  query parameter of `2024-09-17` or later, the volume profile `unattached_capacity_update_supported` property has been changed to `adjustable_capacity_states`, and the volume profile `unattached_iops_update_supported` property has been changed to `adjustable_iops_states`. This change applies when [retrieving](/apidocs/vpc-beta#get-volume-profile) or [listing](/apidocs/vpc-beta#list-volume-profiles) volume profiles.

Similarly, when making API requests using a `version` query parameter of `2024-09-17` or later, the volume `unattached_capacity_update_supported` property has been changed to `adjustable_capacity_states`, and the volume `unattached_iops_update_supported` property has been changed to `adjustable_iops_states`. This change applies when [creating](/apidocs/vpc-beta#create-volume), [updating](/apidocs/vpc-beta#update-volume), [retrieving](/apidocs/vpc-beta#get-volume), or [listing](/apidocs/vpc-beta#list-volumes) volumes.

## 27 August 2024
{: #27-august-2024-beta}

### For all version dates
{: #27-august-2024-all-version-dates-beta}

Reservations for bare metal servers is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#19-november-2024).

**Reservations for bare metal servers.** Accounts that have special approval to preview this feature can now purchase a [capacity reservation](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc&interface=ui) for a specified bare metal server profile in a specified zone. Reservations provide resources for future deployments and cost savings over the life of the term within the availability zone of your choice.

When [creating](/apidocs/vpc-beta#create-reservation) or [updating](/apidocs/vpc-beta#update-reservation) a reservation, specify the `capacity.total` and `committed_use.term` properties to use for this reservation. Optionally specify the `committed_use.expiration_policy` property to apply when the committed use term expires (default: `release`). Specify the `profile.name` and `profile.resource_type` properties of the profile, and the `zone` property to use for this reservation. After you confirm the reservation is configured the way that you want it, you must [activate the reservation](/apidocs/vpc-beta#activate-reservation). The reservation cannot be deleted until the committed use term expires. To provision a bare metal server using a reservation's capacity, specify the reservation using the `reservation_affinity.pool` property when [creating the bare metal server](/apidocs/vpc-beta#create-bare-metal-server). You can also [update a bare metal server](/apidocs/vpc-beta#update-bare-metal-server) that's been provisioned to associate it with a reservation.

When [retrieving a bare metal server](/apidocs/vpc-beta#get-bare-metal-server), the new `reservation_affinity` property indicates the reservation affinity policy in effect for the bare metal server. The new `health_state` property indicates the bare metal server's overall health state, while an accompanying `health_reasons` property indicates the reason for any unhealthy health states, such as a failed reservation.

For more information, see [Provisioning reserved capacity for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc&interface=api).

## 18 June 2024
{: #18-june-2024-beta}

### For all version dates
{: #18-june-2024-all-version-dates-beta}

**Bare metal server reinitialization.** You can now [reinitialize](/apidocs/vpc-beta#replace-bare-metal-server-initialization) a bare metal server. To reinitialize a bare metal server, specify the `image` to provision, one or more SSH public `keys`, and optionally specify `user_data`. Upon successful reinitialization, the bare metal server starts automatically and retains the same physical node, interfaces, IP addresses, and resource IDs it had before reinitialization.

To reinitialize a bare metal server, the server `status` must be `stopped`, or have `failed` a previous reinitialization. For more information, see [Managing Bare Metal Servers for VPC](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=api).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#23-july-2024).

## 4 June 2024
{: #4-june-2024-beta}

### For all version dates
{: #4-june-2024-all-version-dates-beta}

**Firmware update for bare metal servers.** You can now [update firmware](/apidocs/vpc-beta#update-firmware-for-bare-metal-server) for a stopped bare metal server. This request updates server firmware if newer firmware is available and automatically starts the bare metal server after the firmware update is successfully completed. If you don't want the bare metal server to start after the firmware is updated, set the `auto_start` property value to `false` in the request.

When [retrieving](/apidocs/vpc-beta#get-bare-metal-server) or [listing](/apidocs/vpc-beta#list-bare-metal-servers) bare metal servers, the response includes the new `firmware` property, which in turn has an `update` property that indicates the type of update available (`none`, `optional`, or `required`). For more information, see [Managing Bare Metal Servers for VPC](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=api#update-firmware-bare-metal-servers-API).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#9-july-2024).

## 9 April 2024
{: #9-april-2024-beta}

### For all version dates
{: #9-april-2024-all-version-dates-beta}

**Generic operating system custom images and network bootable bare metal servers.** Accounts that have special approval to preview this feature can now create a [generic operating system custom image](/docs/vpc?topic=vpc-create-generic-os-custom-image), which is an image containing an operating system that is not specifically defined in IBM Cloud VPC. Such approved accounts can also use a specific new [stock image](/docs/vpc?topic=vpc-getting-started-images-on-vpc-stock) to create a bare metal server that will [network boot an image](/docs/vpc?topic=vpc-network-boot-bare-metal-servers&interface=api) using iPXE.

To facilitate these features, two new immutable operating system properties, `user_data_format` and `allow_user_image_creation`, and one new immutable image property, `user_data_format`, are provided. The operating system property `user_data_format` (possible values `cloud_init`, `esxi_kickstart`, `ipxe`) populates the image [`user_data_format` property](/docs/vpc?topic=vpc-planning-custom-images#custom-image-user-data-format), which specifies how `user_data` is interpreted and used when [creating a virtual server instance](/apidocs/vpc-beta#create-instance) or [creating a bare metal server](/apidocs/vpc-beta#create-bare-metal-server). The operating system property `allow_user_image_creation` indicates whether an operating system may be used to create a custom image.

For more information, see [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc&interface=api).

Images that are used to create virtual server instances or to [create instance templates](/apidocs/vpc-beta#create-instance-template) must have a `user_data_format` value of `cloud_init`.
{: important}

## 12 March 2024
{: #12-march-2024-beta}

### For all version dates
{: #12-march-2024-all-version-dates-beta}

**Private path network load balancer.** Accounts that have special approval to preview this feature can now create a [private path network load balancer](/docs/vpc?topic=vpc-ppnlb-ui-creating-private-path-network-load-balancer&interface=api) to enable and manage private connectivity for consumers of a hosted service. When [creating a load balancer](/apidocs/vpc-beta#create-load-balancer), you can specify the new `is_private_path` property value as `true` to create a private path network load balancer.

**Load balancer and load balancer profile properties.** The private path load network balancer feature introduces a new load balancer profile `network-private-path`, along with the following new load balancer and load balancer profile properties:
- `source_ip_session_persistence_supported` indicates whether a load balancer supports source IP session persistence. Source IP session persistence is not supported by private path network load balancer.
- `availability` indicates the availability of a load balancer. Load balancers with `subnet` availability remain available if at least one of its subnets is in a zone that's available. Load balancers with `region` availability remain available if at least one zone in the region is available. Private path network load balancers have `region` availability. Other load balancers have `subnet` availability.

The `value` for load balancer profiles properties `route_mode_supported`, `security_groups_supported`, `udp_supported`, and `logging_supported` is set to `false` for private path load balancers. Additionally, private path load balancers do not support setting or updating the `dns` property, because a private path network load balancers are accessed using endpoint gateways where DNS is configured.

**Private path service gateway.** Accounts that have special approval to preview this feature can now [create private path service gateways](/apidocs/vpc-beta#create-private-path-service-gateway). Creating, [updating](/apidocs/vpc-beta#update-private-path-service-gateway), and [deleting](/apidocs/vpc-beta#delete-private-path-service-gateway) private path service gateways allows you to configure and manage access to [private path services](/docs/vpc?topic=vpc-private-path-service-about&interface=api). Private path service gateways also have two sub-resources:

- [Account policies](/apidocs/vpc-beta#list-private-path-service-gateway-account-policies) provide per-account access policies that supersede the private path service gateway's default access policy.  You can [create](/apidocs/vpc-beta#create-private-path-service-gateway-account-policy), [update](/apidocs/vpc-beta#update-private-path-service-gateway-account-policy), and [delete](/apidocs/vpc-beta#delete-private-path-service-gateway-account-policy) policies to `permit`, `deny`, or manually `review` requests from any account. You can also [revoke](/apidocs/vpc-beta#revoke-account-for-private-path-service-gateway) current and future access for an account. For more information, see [About account policies](/docs/vpc?topic=vpc-pps-about-account-policies).

- [Endpoint gateway bindings](/apidocs/vpc-beta#list-private-path-service-gateway-endpoint-gateway) represent each request to access to the private path service gateway. The associated account policy is applied to all `pending` endpoint gateway bindings. If an associated account policy doesn't exist, the private path service gateway's `default_access_policy` is used.  If the resulting policy is `review`, you will be able to explicitly approve or deny the request, and optionally set a new policy for future requests from the account.

Learn about [creating private path service gateways](/docs/vpc?topic=vpc-private-path-service-about), and explore the new [API methods](/apidocs/vpc-beta#list-private-path-service-gateways).

## 19 December 2023
{: #19-december-2023-beta}

### For all version dates
{: #19-december-2023-all-version-dates-beta}

This release introduces the following updates for accounts that have special approval to preview these features:

**Confidential computing capabilities.** On select instance profiles, you can now enable [Intel&reg; Software Guard Extensions](/docs/vpc?topic=vpc-about-confidential-computing-vpc). When [creating](/apidocs/vpc-beta#create-instance) or [updating](/apidocs/vpc-beta#update-instance) an instance or when [creating](/apidocs/vpc-beta#create-instance-template) or [updating](/apidocs/vpc-beta#update-instance-template) an instance template, you can specify the new `confidential_compute_modes` property value (`disabled` or `sgx`) to use for a virtual server instance. The new `confidential_compute_modes` instance profile property indicates which profiles will support which modes. If you do not specify the `confidential_compute_modes` property when creating an instance or instance template, the default confidential compute mode from the profile will be used.

**Secure boot capabilities.** When [creating](/apidocs/vpc-beta#create-instance) or [updating](/apidocs/vpc-beta#update-instance) an instance or when [creating](/apidocs/vpc-beta#create-instance-template) or [updating](/apidocs/vpc-beta#update-instance-template) an instance template, you can set the new `enable_secure_boot` property to `true` to enable secure boot on the virtual server instance. The new `secure_boot_modes` instance profile property indicates the secure boot modes supported by the profile. If you do not specify the `enable_secure_boot` property when creating an instance or instance template, the default secure boot mode from the profile will be used. To use secure boot, the image must support secure boot or the instance will fail to boot.

To update the `enable_secure_boot` and `confidential_compute_mode` properties, the virtual server instance `status` must be `stopping` or `stopped`.
{: note}

## 19 September 2023
{: #19-september-2023-beta}

### For all version dates
{: #19-september-2023-all-version-dates-beta}

**New {{site.data.keyword.block_storage_is_full}} profile.** For accounts that have special approval to preview this feature a new `defined_performance` family is introduced for data and boot [volumes](/apidocs/vpc-beta#create-volume). The `defined_performance` volume profile family contains the `sdp` profile, which provides similar functionality to the `custom` volume profile. The new profile introduces the ability to make capacity increases and IOPS changes to volumes, even when they're not attached to a virtual server instance. The properties `unattached_capacity_update_supported` and `unattached_iops_update_supported` properties have been added to all volumes and volume profiles so you can make use of these capabilities in your automation. For more information, see [About Block Storage for VPC](/docs/vpc?topic=vpc-block-storage-about) and [Viewing available IOPS profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#view-iops-profiles).

## 8 August 2023
{: #8-august-2023-beta}

### For version `2023-08-08` or later
{: #version-2023-08-08-beta}

This feature is now generally available. See [About file share replication](/docs/vpc?topic=vpc-file-storage-replication).

Support for beta API versions earlier than `2023-08-08` was removed on 22 September 2023.
{: attention}

This release introduced the following behavior changes for users with accounts that have access to file shares.

**Fail over to replica share.** When making API requests using a `version` query parameter of `2023-08-08` or later, the default value for the  `fallback_policy` property has been changed to `fail`, and the replication relationship between the shares is broken.

**Retrieve source share information for a replica share.** When making API requests using a `version` query parameter of `2023-08-08` or later, requests to [retrieve the source file share for a replica file share](/apidocs/vpc-beta#get-share-source) now return a more concise source share reference, instead of a share.

## 11 July 2023
{: #11-july-2023-beta}

### For version 2023-07-11 or later
{: #version-2023-07-11-beta}

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

Support for beta API versions earlier than `2023-07-11` was removed on 25 August 2023.
{: attention}

**Data encryption in transit for file shares.** For users with accounts that have access to file shares, you can now enable secure end-to-end encryption of your data in transit between the file share and the authorized client.

When [creating a mount target for a file share](/apidocs/vpc-beta#create-share-mount-target) with a virtual network interface, you can now specify a `transit_encryption` property value of `none` (default) or `user_managed`, which encrypts the data in transit by using IPsec with an instance identity certificate. For more information, see [Encryption in transit](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=api#fs-eit) and [Instance identity certificates](/docs/vpc?topic=vpc-metadata-beta-api-change-log#11-july-2023-metadata-beta) in the Beta VPC Metadata API change log.

**File share access control modes.** For users with accounts that have access to file shares, you can now control the way a share is accessed when [creating](/apidocs/vpc-beta#create-share) or [updating](/apidocs/vpc-beta#update-share) a file share. Specifying `access_control_mode` property value `security_group` now allows the use of security groups to manage which resources can access the file share. By using security groups, access can now be restricted to specific clients. When you specify `access_control_mode` property value `vpc`, all clients in each mount target's VPC will continue to have access to this share.

The default value of `access_control_mode` depends on the `version` query parameter date and the profile selected. When making API requests with a `version` query parameter of `2023-07-11` or later, the default is `security_group`. For requests that are using a `version` query parameter of `2023-07-10` or earlier, the default is `vpc`. File shares must be based on the [`dp2` profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#dp2-profile) to use the `security_group` value.

When [creating a mount target](/apidocs/vpc-beta#create-share-mount-target) for a file share with `access_control_mode` set to `security_group`, you must also create a virtual network interface by using the `virtual_network_interface` property. For more information, see [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about&interface=api) and [Mount target access modes](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode). You must use an `access_control_mode` of `security_group` to enable [Data encryption in transit for file shares](/docs/vpc?topic=vpc-file-storage-vpc-eit).

## 13 June 2023
{: #13-june-2023-beta}

### For all version dates
{: #13-june-2023-all-version-dates-beta}

**Image lifecycle management property name changes.** For accounts that have special approval to preview the image lifecycle management feature, the `deprecated_at` and `obsoleted_at` properties for [images](/apidocs/vpc-beta#create-image) requests have been renamed `deprecation_at` and `obsolescence_at`, respectively. Original property names `deprecated_at` and `obsoleted_at` will continue to be supported until the feature becomes generally available. Requests that specify the original and revised property names simultaneously will be rejected.

This feature is now generally available. Support for property names `deprecated_at` and `obsoleted_at` has been removed. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#11-july-2023).

## 30 May 2023
{: #23-may-2023-beta}

### For all version dates
{: #30-may-2023-all-version-dates-beta}

**Enforcement of file shares beta API requests.** Starting with API version `2023-05-30`, all requests made for [shares methods](/apidocs/vpc-beta#list-shares) must include the [`maturity=beta`](/apidocs/vpc-beta#maturity-query-parameter-beta) query parameter. Requests that omit the `maturity=beta` query parameter will be regarded as requests against the [VPC GA API](/apidocs/vpc), which does not yet support shares. As a result, those requests will fail.

### For version `2023-05-30` or later
{: #version-2023-05-30-beta}

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

Support for beta API beta versions earlier than `2023-05-30` was removed on 14 July 2023, and the `targets` property was removed.
{: attention}

This release introduced the following features for users with accounts that have access to file shares.

**File shares property and request path name changes.** When making API requests using a `version` query parameter of `2023-05-30` or later, the shares `targets` property has been changed to `mount_targets`. This change applies when [creating](/apidocs/vpc-beta#create-share), [updating](/apidocs/vpc-beta#update-share), [listing](/apidocs/vpc-beta#list-shares), or [retrieving](/apidocs/vpc-beta#get-share) file shares, and when [listing all mount targets for a file share](/apidocs/vpc-beta#list-share-mount-targets).

The name change also applies to the method paths: Requests using a `version` query parameter of `2023-05-30` or later must use `/shares/{share_id}/mount_targets` (instead of `/shares/{share_id}/targets`) in the request URL. This change applies when [creating](/apidocs/vpc-beta#create-share-mount-target), [updating](/apidocs/vpc-beta#update-share-mount-target), [listing](/apidocs/vpc-beta#list-share-mount-targets), [retrieving](/apidocs/vpc-beta#get-share-mount-target), or [deleting](/apidocs/vpc-beta#delete-share-mount-target) share mount targets.

## 11 April 2023
{: #11-april-2023-beta}

### For all version dates
{: #11-april-2023-all-version-dates-beta}

**Revised file share profiles.** For users with accounts that have access to file shares, a new `dp2` profile is now available when [creating](/apidocs/vpc-beta#create-share) or [updating](/apidocs/vpc-beta#update-share) a file share. Profiles in the existing `custom` and `tiered` families have been deprecated and will remain available only to accounts that have already provisioned file shares with those profiles. The deprecated profiles also will not be included in the upcoming general availability release for file shares.

The `dp2` profile belongs to a new `defined_performance` file share profile family, which provides similar functionality to the deprecated `custom` file share profile family. While existing file shares using profiles in the `custom` and `tiered` families will continue to work, you are encouraged to update all your file shares to the new `dp2` profile in preparation for general availability of the file share service. Bulk migration of existing file shares is not supported. For more information, see [dp2 file storage profile](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile).

**IOPS and size configuration for file share profiles.** New file share properties `size` and `iops` are also provided in the API responses when [retrieving a file share profile](/apidocs/vpc-beta#get-share-profile). The `size` property shows the permitted capacity range (in gigabytes) for a share with the profile. The `iops` property shows the permitted IOPS range for a share with the profile. The maximum IO operations each client that accesses the file can perform is 48,000 IOPS. When multiple clients access the file share, the share can handle a maximum of 96,000 IO operations per second.

This API feature was released to production on `2023-04-11`, but this announcement was not included at the time.
{: note}

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

**Image lifecycle management.** Accounts that have special approval to preview this feature can now [deprecate](/apidocs/vpc-beta#deprecate-image) or [obsolete](/apidocs/vpc-beta#obsolete-image) custom images directly. Alternatively, you can schedule transition at a later date by specifying the `deprecated_at` or `obsoleted_at` properties when [creating](/apidocs/vpc-beta#create-image) or [updating](/apidocs/vpc-beta#update-image) an image. If you need to revert a status change, you can transition `deprecated` or `obsolete` images back to `available`. For more information, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=api).

`deprecated` custom images remain usable, while `obsolete` images cannot be used to provision instances or bare metal servers.
{: note}

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#11-july-2023).

## 14 February 2023
{: #14-february-2023-beta}

### For all version dates
{: #14-february-2023-all-version-dates-beta}

**Exporting custom images.** Accounts that have special approval to preview this feature can now [export custom images](/apidocs/vpc-beta#create-image-export-job) to an authorized IBM Cloud Object Storage bucket. Specify the target `storage_bucket` to export the image to. The image will be exported as `qcow2` unless you specify another value using the `format` property.

For more information, see [Exporting a custom image to IBM Cloud Object Storage](/docs/vpc?topic=vpc-managing-custom-images&interface=api#custom-image-export-to-cos-api), or start using the new [export jobs](/apidocs/vpc-beta#list-image-export-jobs) methods.

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#2-may-2023).

## 20 December 2022
{: #20-december-2022-beta}

### For all version dates
{: #20-december-2022-all-version-dates-beta}

**Backup for VPC.** Backup policy jobs are now generally available. The following updates have been made for [retrieving](/apidocs/vpc-beta#get-backup-policy-job) and [listing](/apidocs/vpc-beta#list-backup-policy-jobs) backup policy jobs since the beta release:

* The `source_volume` property has been replaced by the `source` property.
* The `target_snapshot` property has been replaced by the `target_snapshots` array property.

See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#20-december-2022).

**Instance provision by volume.** Accounts that have special approval to preview this feature can now reuse an existing boot volume to provision a virtual server instance by specifying the existing volume's `id` or `crn` sub-property of the `boot_volume_attachment` property. The specified volume must be unattached and must have an operating system with the same architecture as the instance profile. Volumes now include an `attachment_state` property and an expanded `operating_system` property you can use to view a volume's eligibility. You can also use the new [list volumes](/apidocs/vpc-beta#list-volumes) filters to list volumes that have specific `attachment_state`, `operating_system`, and `encryption_type` values.

By default, a boot volume attached to a virtual server instance is deleted when the instance is deleted. To preserve the boot volume when deleting a virtual server instance, change the `delete_volume_on_instance_delete` property to `false` by updating the [boot volume attachment](/apidocs/vpc-beta#update-instance-volume-attachment). See [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui), [Creating VPC resources with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api&interface=cli) for more information.

This feature is now generally available. Since the beta release, by default, only a boot volume created as part of provisioning a virtual server instance will be deleted when the instance is deleted. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#21-march-2023).

## 16 August 2022
{: #16-august-2022-beta}

### For all version dates
{: #16-august-2022-all-version-dates-beta}

**Sharing images across an enterprise account.** Accounts that have special approval to preview this feature can now use a [catalog to share custom images](/docs/vpc?topic=vpc-planning-custom-images) with users in other accounts within the same enterprise. When you [create an image](/apidocs/vpc-beta#create-image), a new `catalog_offering` property includes a `published` sub-property that is set to `false` by default. When the custom image is imported to a catalog the `published` sub-property is set to `true`, indicating that the image is added to a catalog offering `version` and is managed from a catalog. If you are authorized to the catalog offering `version`, you can [provision virtual server instances](/apidocs/vpc-beta#create-instance) using that custom image by specifying its `catalog_offering.version.crn`. To use the custom image associated with the latest version in the offering, specify `catalog_offering.offering.crn` instead. The image may not be deleted from your IBM Virtual Private Cloud while it is managed from a catalog.

For more information, see the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial) and the [Import offering](/apidocs/resource-catalog/private-catalog#import-offering){: external} method in the Catalog Management API.

This feature is now generally available. The `catalog_offering.published` property in the beta API definition has been renamed to `catalog_offering.managed`. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#27-september-2022).

## 12 July 2022
{: #12-july-2022-beta}

### For all version dates
{: #12-july-2022-all-version-dates-beta}

**File storage cross-account encryption.**  Accounts that have special approval to preview this feature can now use cross-account customer-managed encryption keys (CRKs) when [creating a file share](/apidocs/vpc-beta#create-share) with [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption). With this feature, the CRK account owner [invites you](/docs/account?topic=account-iamuserinv&interface=api) and sets the IAM delegated policy to the CRKs. Afterward, specify your IAM token to create a file share with an `encryption_key` CRN from the CRK account. For more information, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption&interface=api).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 5 July 2022
{: #5-july-2022-beta}

### For all version dates
{: #5-july-2022-all-version-dates-beta}

**Client VPN for VPC.**  Client-to-site connectivity is now  generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#5-july-2022).

The following updates have been made since the [24 August 2021 beta release](#24-august-2021-beta):

- You can use IBM Cloud Secrets Manager for server authentication when [creating a VPN server](/apidocs/vpc-beta#create-vpn-server)
- You can specify a VPN server when [adding a target to a security group](/apidocs/vpc-beta#create-security-group-target-binding)
- You can [update a VPN server](/apidocs/vpc-beta#update-vpn-server) to be highly available, or detach a subnet to downgrade to a stand-alone deployment

## 14 June 2022
{: #14-june-2022-beta}

### For all version dates
{: #14-june-2022-all-version-dates-beta}

**File storage adjustable IOPS.** Accounts that have special approval to preview this feature can now [update the IOPS of an existing file share](/apidocs/vpc-beta#update-share). For a file share using a `custom` profile, specify the `iops` property. For a file share using a profile in the `tiered` profile family, specify another tier within the `tiered` family, which will set the `iops` based on the share's size.

You can also change a share between the `tiered` and `custom` profile families so long as the requested `iops` and `size` are supported by the requested profile. For more information about file share profiles, see [File Storage for VPC profiles](/docs/vpc?topic=vpc-file-storage-profiles). For more information about adjusting IOPS with a profile, or changing between `tiered` and `custom` profiles, see [Adjusting file share IOPS](/docs/vpc?topic=vpc-file-storage-adjusting-iops&interface=api).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 31 May 2022
{: #31-may-2022-beta}

### For all version dates
{: #31-may-2022-all-version-dates-beta}

**AMD support for instances and dedicated hosts.** Accounts that have special approval to preview this feature can select new profiles for [dedicated hosts](/apidocs/vpc-beta#list-dedicated-host-profiles) and [instances](/apidocs/vpc-beta#list-instance-profiles). When provisioning or managing an instance or dedicated host, use the new `vcpu_manufacturer` property to choose between profiles from different processor manufacturers.

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#28-march-2023).

## 24 May 2022
{: #24-may-2022-beta}

### For all version dates
{: #24-may-2022-all-version-dates-beta}

**Backup for VPC.** Accounts with special approval to preview this feature can now [create](/apidocs/vpc-beta#create-backup-policy-plan) up to four plans for backup policies. You can now also [update](/apidocs/vpc-beta#update-backup-policy-plan) or [delete](/apidocs/vpc-beta#delete-backup-policy-plan) existing plans, and [add backup plans](/vpc-beta#create-backup-policy) to existing policies. You can use one of the new `deletion_trigger` sub-properties to specify a custom backup deletion policy. For more information, see [Managing backup policies](/docs/vpc?topic=vpc-backup-service-manage&interface=api).

Backup policies now also include information about [backup policy jobs](/apidocs/vpc-beta#list-backup-policy-jobs). A [backup policy job](/docs/vpc?topic=vpc-backup-view-policy-jobs&interface=api) is automatically created each time a backup has to be created or deleted to meet the backup policy's settings. For more information, see [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

The backup API is now generally available, with the exception of the backup jobs API, which remains in beta. See the [Change log](/docs/vpc?topic=vpc-api-change-log#21-june-2022).

The backup jobs API is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#20-december-2022)

## 17 May 2022
{: #17-may-2022-beta}

### For all version dates
{: #17-may-2022-all-version-dates-beta}

**File share replication.** Accounts that have special approval to preview this feature can now set up replication between a file share in one zone and a replica file share in another zone in the same region. [Using replication](/docs/vpc?topic=vpc-file-storage-replication) is a good way to recover from an incident at your primary site, if data becomes inaccessible or an application fails.

- When [creating a new file share](/apidocs/vpc-beta#create-share), you can now configure replication by specifying the new `replica_share` property. To create a replica for an existing file share, create a new share and specify the existing file share as `source_share`, along with the replication schedule, using the `replication_cron_spec` property.
- You can now [retrieve the source file share for a replica file share](/apidocs/vpc-beta#get-share-source). Returned information also includes the replication schedule, role, status, and status reasons.
- You can now [split the source file from a replica share](/apidocs/vpc-beta#delete-share-source), which removes the replication relationship between a source share and its replica share, resulting in two independent read-write file shares. This action cannot be reversed. For more information, see [Remove the replication relationship](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-remove-replication).
- You can now [fail over to the replica file share](/apidocs/vpc-beta#failover-share), which reverses the replication relationship. You can optionally specify `split` for the `fallback_policy` to trigger a split if the failover operation fails or times out. For more information, see [Replication failover](/docs/vpc?topic=vpc-file-storage-failover&interface=api).

**File storage native tagging.** Accounts that have special approval to preview this feature can now specify `user_tags` when [creating a new file share](/apidocs/vpc-beta#create-share) or [updating an existing file share](/apidocs/vpc-beta#update-share). Adding user tags to a file share helps you organize your resources. For details, see [Add user tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=api#fs-add-user-tags).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 22 March 2022
{: #22-march-2022-beta}

### For all version dates
{: #22-march-2022-all-version-dates-beta}

**VPN client-to-site servers update.** You can now [update the subnets for a VPN server](/apidocs/vpc-beta#update-vpn-server) after the VPN is provisioned. For example, you can upgrade a stand-alone VPN server (one subnet) to a High Availability (HA) VPN server (two subnets in different zones). You can also detach a subnet to downgrade an HA VPN server to a stand-alone deployment, or change an attached subnet after your VPN server is provisioned. For more information, see [Upgrading to an HA VPN server](/docs/vpc?topic=vpc-vpn-client-to-site-change-server-types&interface=api).

## 8 March 2022
{: #8-march-2022-beta}

### For all version dates
{: #8-march-2022-all-version-dates-beta}

**Backup for VPC.** Accounts that have special approval to preview this feature can now create backup policies to automatically back up block storage volumes. Use the new [backup service APIs](/apidocs/vpc-beta#list-backup-policies) to create, list, and manage backup policies. Backup policies control which source volumes are selected for backup by [matching user tags](/docs/vpc?topic=vpc-backup-service-about&interface=api#backup-service-about-tags) in the volume with tags defined in the backup policy. For this beta release, a backup policy contains one [backup plan](/apidocs/vpc-beta#create-backup-policy-plan) in which a `deletion_trigger` specifies the maximum number of days to keep each backup after creation. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about&interface=api).

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

**Customer-managed encryption for file shares.** Accounts that have special approval to preview this feature can now use customer-managed encryption, also called Bring Your Own Key (BYOK), to [create a file share](/apidocs/vpc-beta#create-share) that is encrypted using your root key. Specify the new `encryption_key` property and `crn` sub-property of the root key that you either imported to {{site.data.keyword.cloud_notm}} or created in Key Protect or Hyper Protect Crypto Services (HPCS). For more information, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption&interface=api).

You can also use the API to rotate the root keys that are protecting your file shares. See [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation&interface=api) for details.
{: tip}

**Supplemental user/group IDs for file shares.** Accounts that have special approval to preview this feature can now access supplemental user/group IDs for file shares. When a process runs on Unix/Linux, the operating system identifies a user with a user ID (UID) and/or group with a group ID (GID). These IDs determine which system resources a user or group can access. When you [create a file share](/apidocs/vpc-beta#create-share), you can specify the new `initial_owner` property and specify a `uid`, `gid`, or both sub-properties to control access to the share. For more information, see [Adding supplemental IDs when you create a file share from the API](/docs/vpc?topic=vpc-file-storage-create&interface=api#fs-add-supplemental-id-api).

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

**File storage for VPC.** Accounts that have special approval to preview this feature can now increase file share size in gigabyte increments up to 32 TB (depending on the file share's profile). The increase takes effect immediately. For more information, see [Expanding file share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity&interface=api). You can also specify the maximum input/output operations per second (IOPS) when [creating](/apidocs/vpc-beta#create-share) a file share, within the range available for its size. For more information, see [Custom IOPS profile](/docs/vpc?topic=vpc-file-storage-profiles&interface=api#fs-custom).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

## 10 August 2021
{: #10-august-2021-beta}

### For all version dates
{: #10-august-2021-all-version-dates-beta}

**Beta VPC Metadata API.** Accounts that have special approval to preview this feature can now preview the new beta {{site.data.keyword.vpc_full}} (VPC) [Metadata API](/apidocs/vpc-metadata-beta/initial). This API provides access to VPC metadata, including instance initialization data, network interfaces, volume attachments, public SSH keys, and placement groups. Use the [VPC create instance](/apidocs/vpc-beta#create-instance) or [VPC update instance](/apidocs/vpc-beta#update-instance) methods to enable or disable the VPC Metadata service endpoint for a particular instance. For more information, see [About VPC Metadata](/docs/vpc?topic=vpc-imd-about).

## 27 July 2021
{: #27-july-2021-beta}

### For all API version dates
{: #27-july-2021-all-version-dates-beta}

**GPU instances.** Accounts that have special approval to preview this feature can now view new instance profiles, which include GPUs attached to the instance. These attached GPUs allow for accelerated computing to run workloads with more powerful compute capabilities.

The following API methods have been enhanced:

- [List instances](/apidocs/vpc-beta#list-instances) returns a new `gpu` property with four additional sub-properties: `count`, `manufacturer`, `model`, and `memory`
- [Retrieve an instance profile](/apidocs/vpc-beta#get-instance-profile) returns four new properties: `gpu_count`, `gpu_manufacturer`, `gpu_model`, and `gpu_memory`

For more information, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#19-october-2021).

## 20 July 2021
{: #20-july-2021-beta}

### For all version dates
{: #20-july-2021-all-version-dates-beta}

**Bare metal servers for VPC.** Accounts that have special approval to preview this feature can now create bare metal servers to host VMware clusters in {{site.data.keyword.vpc_short}}. You can set up VMware management applications and create VMware virtual machines on the bare metal servers. As bare metal servers are integrated with the VPC platform, you can take advantage of the network and security capabilities of {{site.data.keyword.vpc_short}}. For more information, see [About Bare Metal Servers for VPC (beta)](/docs/vpc?topic=vpc-about-bare-metal-servers) or dive into the new [API methods](/apidocs/vpc-beta#list-bare-metal-server-profiles).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#1-february-2022).

## 24 June 2021
{: #24-june-2021-beta}

### For all version dates
{: #24-june-2021-all-version-dates-beta}

**Placement groups.** Placement groups for {{site.data.keyword.vpc_full}} are logical groupings of virtual server instances that can be configured to reduce the risk of correlated failures inherent in your physical environment, such as networking issues, power loss, or hardware failure. Define a placement group strategy for high-availability workloads, such as for host or power spread. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc) or dive into the new [API methods](/apidocs/vpc-beta#list-placement-groups).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#17-august-2021).

## 6 April 2021
{: #6-april-2021-beta}

### For all version dates
{: #6-april-2021-all-version-dates-beta}

**File storage for VPC.** Accounts that have special approval to preview this feature can now create NFS-based file shares in a zone in your region. Share file storage over multiple virtual service instances within the same zone across multiple VPCs. Learn about [creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-vpc-about), and explore the new [API methods](/apidocs/vpc-beta#list-share-profiles).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#8-august-2023).

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

**Instance resize.** Accounts that have special approval to preview this feature can now resize an instance by providing the `profile` property in the API method `PATCH /instances/{id}` ([Update an instance](/apidocs/vpc-beta#update-instance)). For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance&interface=api).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#30-march-2021).

## 22 February 2021
{: #22-february-2021-beta}

### For all version dates
{: #22-february-2021-all-version-dates-beta}

**Block storage snapshots.** Accounts that have special approval to preview this feature can now create and use [snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) and explore the new [snapshots API methods](/apidocs/vpc-beta#list-snapshots).

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#18-may-2021).

**Virtual server instance console.** Accounts that have special approval to preview this feature can now access instances by connecting to a VNC or serial console. Learn about [accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console&interface=api), and explore the new instance console API methods:

- [Create a console access token for an instance](/apidocs/vpc-beta#create-instance-console-access-token)
- [Retrieve the console WebSocket for an instance](/apidocs/vpc-beta#get-instance-console)

This feature is now generally available. See the [VPC API change log](/docs/vpc?topic=vpc-api-change-log#30-march-2021).  
