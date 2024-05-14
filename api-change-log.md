---

copyright:
  years: 2019, 2024
lastupdated: "2024-05-14"

keywords: vpc, api, change log, new features, restrictions, migration

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC API change log
{: #api-change-log}

Read the VPC API change log to learn about updates and improvements to the {{site.data.keyword.vpc_full}} (VPC) [API](/apidocs/vpc). The change log lists changes that are ordered by the date they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{: shortdesc}

By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code. If backward-incompatible changes require non-trivial client code changes to use an API version, the API change log might provide links to instructions, tips, or best practices for updating client code.
{: note}

Some changes, such as new response properties or new optional request parameters, are considered backward compatible. Other changes, such as new required request parameters, are not considered backward compatible. To avoid disruption from changes to the API, use the following best practices when you call the API:

- Catch and log any `4xx` or `5xx` HTTP status code, along with the included `trace` property
- Follow HTTP redirect rules for any `3xx` HTTP status code
- Consume only the resources and properties your application needs to function
- Avoid depending on behavior that is not explicitly documented

SDK changes are based on API changes. For information about the latest changes to the VPC SDKs, see the change logs in the SDK repositories:

- [Java SDK change log](https://github.com/IBM/vpc-java-sdk/blob/master/CHANGELOG.md)
- [Node SDK change log](https://github.com/IBM/vpc-node-sdk/blob/master/CHANGELOG.md)
- [Python SDK change log](https://github.com/IBM/vpc-python-sdk/blob/master/CHANGELOG.md)
- [Go SDK change log](https://github.com/IBM/vpc-go-sdk/blob/master/CHANGELOG.md)

## Upcoming changes
{: #upcoming-changes}

**`InstanceTemplate` response schema change.** In an upcoming release, future methods of creating instances, and therefore creating instance templates, may not require a primary network interface. To accommodate this, the `primary_network_interface` property is now optional in the instance template response model.

At this time, all instances, and therefore all instance templates, continue to require that a primary network interface be specified. Therefore, existing instance templates are unaffected. Additionally, new instance templates will continue to include a primary network interface until further notice. However, to ensure your clients will not be affected in the future, verify that they are tolerant of the `primary_network_interface` property not being included when consuming `InstanceTemplate` responses.
{: important}

**`Instance` response schema change.** In an upcoming release, volume attachments returned in the `boot_volume_attachment` and `volume_attachments[]` properties of an instance will not include the `volume` sub-property if the volume has not yet been provisioned. Such volumes are currently represented with empty `crn`, `id`, and `href` properties along with an undocumented sentinel value for `name`.

To prepare for this change, verify that your client checks that the `volume` property exists for a volume attachment before attempting to access its `crn`, `id`, `href`, or `name` sub-properties.

**Asynchronous `DELETE` response code change.** In an upcoming release, the response code output for asynchronous `DELETE` operations will change from `204` to `202`. A response code of `204` implies the action is completed, which could be misleading for operations that are still processing. A response code of `202` is more appropriate. This behavior change will occur only for an API version date after its release. A response code of `204` will continue to be returned for API versions up to this version date.

The new response code will be rolled out gradually. Each phase of the rollout will be tied to a dated API version. These changes will be announced in future change log updates.
{: note}

**Security group targets.** In an upcoming release, new resource types will be permitted as security group targets. If you add resources of these new types to a security group, existing client applications will be exposed to the new types when iterating over the security group's targets. To avoid disruption, check that client applications are written to gracefully handle unexpected resource types in a security group's targets.

## 14 May 2024
{: #14-may-2024}

### For all version dates
{: #14-may-2024-all-version-dates}

**Local IP address support for security group rules.** You can now specify local IP addresses or address ranges in security group rules. When [creating](/apidocs/vpc/latest#create-security-group-rule) or [updating](/apidocs/vpc/lates#update-security-group-rule) a security group rule, specify the optional `local` property. The value can be an IP address or a range of IP addresses in CIDR format. If not specified, the default value is `cidr_block: 0.0.0.0/0`, which means the rule allows traffic to all local IP addresses (or from all local IP addresses, for outbound rules). This default value will be applied to all existing security group rules. For more information, see [Applying security group rules to source and destination IP addresses](/docs/vpc?topic=vpc-security-groups-rules&interface=api).

## 30 April 2024
{: #30-april-2024}

The `2024-04-30` release includes incompatible changes. To avoid regressions in client functionality, read and follow [Updating to the `2024-04-30` version (VPN gateway connection)](/docs/vpc?topic=vpc-2024-04-30-migration-vpn-advanced-configuration) before specifying version `2024-04-30` or later in any API requests.
{: important}

### For version `2024-04-30` or later
{: #version-2024-04-30}

**Advanced VPN gateway configuration.** Using a `version` query parameter of `2024-04-30` or later, you can specify `peer.fqdn` instead of `peer.address` when [creating](/apidocs/vpc/latest#create-vpn-gateway-connection) or [updating](/apidocs/vpc/latest#update-vpn-gateway-connection) a VPN gateway connection. You can also now fully control the local and peer IKE identities assigned to a VPN gateway connection by using properties `local.ike_identities` and `peer.ike_identity`. If unspecified, the local IKE identities will default to the public IP addresses of the VPN gateway and member's VPN connection tunnel. The peer identity will default to either the peer's address or FQDN, depending on what was specified.

For migration guidance, see [Updating to the `2024-04-30` version (VPN gateway connection)](/docs/vpc?topic=vpc-2024-04-30-migration-vpn-advanced-configuration). See also the [known issue](/docs/vpc?topic=vpc-known-issues#vpc-vpn-gateway-known-issues) about updating the `peer.address` or `peer.fqdn` of a VPN connection tunnel.

**Migration of VPN gateway connection CIDR paths.** Using a `version` query parameter of `2024-04-30` or later, API paths for [listing](/apidocs/vpc/latest#list-vpn-gateway-connection-local-cidrs), [removing](/apidocs/vpc/latest#remove-vpn-gateway-connection-local-cidr), [checking](/apidocs/vpc/latest#check-vpn-gateway-connection-local-cidr), or [setting](/apidocs/vpc/latest#add-vpn-gateway-connection-local-cidr) a local CIDR, and [listing](/apidocs/vpc/latest#list-vpn-gateway-connection-peer-cidrs), [removing](/apidocs/vpc/latest#remove-vpn-gateway-connection-peer-cidr), [checking](/apidocs/vpc/latest#check-vpn-gateway-connection-peer-cidr), or [setting](/apidocs/vpc/latest#add-vpn-gateway-connection-peer-cidr) a peer CIDR are changed. See [Updating to the `2024-04-30` version (VPN gateway connection)](/docs/vpc?topic=vpc-2024-04-30-migration-vpn-advanced-configuration) for guidance on migration.

**VPN gateway connection schema change.**  As of version `2023-04-30`, the VPN gateway connection `peer_address`, `peer_cidrs`, and `local_cidrs` properties have been replaced by new `peer` and `local` properties. These changes apply to all methods that interact with VPN gateway connections, including [creating](/apidocs/vpc/latest#create-vpn-gateway-connection), [retrieving](/apidocs/vpc/latest#get-vpn-gateway-connection), and [updating](/apidocs/vpc/latest#update-vpn-gateway-connection) a VPN gateway connection.  See [Updating to the `2024-04-30` version (VPN gateway connection)](/docs/vpc?topic=vpc-2024-04-30-migration-vpn-advanced-configuration) for a full list of affected methods and guidance on migration.

### For all version dates
{: #30-april-2024-all-version-dates}

**Establish mode for VPN gateway connections.** When [creating](/apidocs/vpc/latest#create-vpn-gateway-connection) or [updating](/apidocs/vpc/latest#update-vpn-gateway-connection) a VPN gateway connection, you can specify the new `establish_mode` property to control which side of the gateway can initiate the connection by setting the value to `bidirectional` (default) or `peer_only`.

## 26 March 2024
{: #26-march-2024}

### For all version dates
{: #26-march-2024-all-version-dates}

**Reservations for Virtual Servers for VPC.** You can now purchase a [capacity reservation](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc) for a specified instance profile in a specified zone. Reservations provide resources for future deployments and cost savings over the life of the term within the availability zone of your choice.

When [creating](/apidocs/vpc/latest#create-reservation) or [updating](/apidocs/vpc/latest#update-reservation) a reservation, specify the `capacity.total` and `committed_use.term` properties to use for this reservation. Optionally specify the `committed_use.expiration_policy` property to apply when the committed use term expires (default: `release`). Specify the `profile.name` and `profile.resource_type` properties of the profile, and the `zone` property to use for this reservation. After you confirm the reservation is configured the way you want it, you must [activate the reservation](/apidocs/vpc/latest#activate-reservation). The reservation cannot be deleted until the committed use term expires. To provision an instance using a reservation's capacity, specify the reservation using the `reservation_affinity.pool` property when [creating the instance](/apidocs/vpc/latest#create-instance). You can also [update an instance](/apidocs/vpc/latest#update-instance) that's been provisioned to associate it with a reservation.

When [retrieving an instance](/apidocs/vpc/latest#get-instance), the new `reservation_affinity` property indicates the reservation affinity policy in effect for the virtual server instance. The new `health_state` property indicates the instance's overall health state, while an accompanying `health_reasons` property indicates the reason for any unhealthy health states, such as a failed reservation.

For more information, see [Provisioning reserved capacity for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).

**Local IP addresses in security group rules.** Accounts that have been granted special approval to preview this feature can now specify local IP addresses or address ranges in security group rules. When [creating](/apidocs/vpc#create-security-group-rule) or [updating](/apidocs/vpc#update-security-group-rule) a security group rule, specify the optional `local` property. The value can be an IP address or a range of IP addresses in CIDR format, where `0.0.0.0/0` means the rule allows traffic to all local IP addresses (or from all local IP addresses, for an outbound rule). The default value is `cidr_block: 0.0.0.0/0`, which is also used for all existing rules.

## 19 March 2024
{: 19-march-2024}

### For all version dates
{: #19-march-2024-all-version-dates}

**Sharing DNS resolution for endpoint gateways across VPCs.** When multiple VPCs are connected together using Transit Gateway, Direct Link, or other connectivity options, a VPC in the connected topology can now be enabled as a DNS hub to centralize the DNS resolution for endpoint gateways. When [creating](/apidocs/vpc/latest#create-vpc) or [updating](/apidocs/vpc/latest#update-vpc) a VPC, the `dns` property includes new configuration options for DNS. Specify the `dns.enable_hub` property as `true` to enable the VPC as a DNS hub (default is `false`). Specify a DNS hub VPC when [creating a DNS resolution binding](/apidocs/vpc/latest#create-vpc-dns-resolution-binding) on another VPC to share its DNS resolution with that DNS hub VPC. The `dns.resolution_binding_count` response property specifies how many other VPCs a VPC is bound to for DNS resolution sharing. For more information, see [About DNS sharing for VPE gateways](/docs/vpc?topic=vpc-hub-spoke-model).

**Configuring DNS resolvers for a VPC.** You can now use the `dns.resolver` property to configure the DNS resolvers for a VPC. Use a `dns.resolver.type` of `manual` to specify the DNS resolvers by IP address. Use a `dns.resolver.type` of `delegated` to specify another VPC (typically a DNS hub VPC) whose DNS resolvers will be used. Use a `dns.resolver.type` of `system` to restore the system default DNS resolvers. When `dns.resolver.type` is `manual`, [updating](/apidocs/vpc/latest#update-vpc) specifying the VPC's `dns.resolver.manual_servers` requires the [`If-Match` header](/apidocs/vpc/latest#concurrent-update-protection) also be provided with the VPC's current `ETag` value.

## 12 March 2024
{: #12-march-2024}

### For all version dates
{: #12-march-2024-all-version-dates}

**Virtual network interfaces.** Accounts that have not requested a feature deferral through [IBM Support](/unifiedsupport/supportcenter) can use a new feature that expands the support for [virtual network interfaces](https://cloud.ibm.com/docs/vpc?topic=vpc-vni-about).

Your account is affected by these changes if you have API clients (such as custom automations, auditing scripts, or dashboards) that interact with instances, bare metal servers, network interfaces, or file shares. For more information about what changed, along with guidance on avoiding possible failures due to these changes, see [Mitigating behavior changes to virtual network interfaces, instances, bare metal servers, and file shares](/docs/vpc?topic=vpc-vni-api-introduction).
{: important}

- You can now create [instances](/apidocs/vpc/latest#create-instance) and [bare metal servers](/apidocs/vpc/latest#create-bare-metal-server) with virtual network interfaces attached to new child resources called network attachments. You can specify a `primary_network_attachment` (instead of a `primary_network_interface`) and provide either the identity of an already created virtual network interface, or a subnet to create a new virtual network interface for the instance or bare metal server.
- Virtual network interfaces now have lifecycles that are independent of the resources they are attached to. You can [update](/apidocs/vpc/latest#update-virtual-network-interface) the `auto_delete` property to `false` to allow a virtual network interface to persist beyond the lifecycle of its original bare metal server or instance, and be re-attached to another bare metal server or instance.
- Virtual network interfaces now support [secondary IP addresses](/docs/vpc?topic=vpc-vni-about-secondary-ip). You can now [add](/apidocs/vpc/latest#add-virtual-network-interface-ip) and [remove](/apidocs/vpc/latest#remove-virtual-network-interface-ip) reserved IPs to and from a virtual network interface.
- For compatibility with existing clients, instances and bare metal servers with virtual network interfaces now include a read-only representation of their network attachments and virtual network interfaces as old-style network interface child resources. Learn about [support for old API clients](/docs/vpc?topic=vpc-vni-about&interface=ui#vni-old-api-clients).
- For instances and bare metal servers with virtual network interfaces, the IAM permissions for options to allow IP spoofing or disable infrastructure NAT are now managed on their attached virtual network interfaces. When [creating](/apidocs/vpc/latest#create-virtual-network-interface) or [updating](/apidocs/vpc/latest#update-virtual-network-interface) a virtual network interface, you can set non-default values for the `allow_ip_spoofing` and `enable_infrastructure_nat` properties only if you have the `is.virtual-network-interface.virtual-network-interface.manage-ip-spoofing` and `is.virtual-network-interface.virtual-network-interface.manage-infrastructure-nat` IAM permissions respectively.
- You can now use [flow log collectors](/apidocs/vpc/latest#create-flow-log-collector) to target instance network attachments and virtual network interfaces. There is no support for flow logs for bare metal servers and share mount targets.

**Resource suspension for instance groups.** The [list all instance groups](/apidocs/vpc/latest#list-instance-groups) and [retrieve an instance group](/apidocs/vpc/latest#get-instance-group) methods now include `lifecycle_reasons` and `lifecycle_state` properties. An instance group that violates the IBM Cloud [Acceptable Use Policy](https://www.ibm.com/services/us/imc/html/aup1.html){: external} will have its `lifecycle_state` property set to `suspended`. A suspended instance group will not auto scale or self heal, and you cannot enable, update, or delete it.

## 30 January 2024
{: #30-january-2024}

### For all version dates
{: #30-january-2024-all-version-dates}

**Reservations for Virtual Servers for VPC.** Accounts that have been granted special approval to preview this feature can now purchase a [capacity reservation](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc) for a specified instance profile in a specified zone. Reservations provide resources for future deployments and cost savings over the life of the term within the availability zone of your choice.

When [creating](/apidocs/vpc/latest#create-reservation) or [updating](/apidocs/vpc/latest#update-reservation) a reservation, specify the `capacity.total` and `committed_use.term` properties to use for this reservation. Optionally specify the `committed_use.expiration_policy` property to apply when the committed use term expires (default: `release`). Specify the `profile.name` and `profile.resource_type` properties of the profile, and the `zone` property to use for this reservation. After you confirm the reservation is configured the way you want it, you must [activate the reservation](/apidocs/vpc/latest#activate-reservation). The reservation cannot be deleted until the committed use term expires. To provision an instance using a reservation's capacity, specify the reservation using the `reservation_affinity.pool` property when [creating the instance](/apidocs/vpc/latest#create-instance). You can also [update an instance](/apidocs/vpc/latest#update-instance) that's been provisioned to associate it with a reservation.

When [retrieving an instance](/apidocs/vpc/latest#get-instance), the new `reservation_affinity` property indicates the reservation affinity policy in effect for the virtual server instance. The new `health_state` property indicates the instance's overall health state, while an accompanying `health_reasons` property indicates the reason for any unhealthy health states, such as a failed reservation.

For more information, see [Provisioning reserved capacity for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).

This feature is now generally available. See the [26 March 2024](#26-march-2024) announcement.

## 19 December 2023
{: #19-december-2023}

### For all version dates
{: #19-december-2023-all-version-dates}

**VPC route advertisement to Direct Link and Transit Gateway.** When [creating](/apidocs/vpc/latest#create-vpc-routing-table-route) or [updating](/apidocs/vpc/latest#update-vpc-routing-table-route) a route in a routing table, you can set the new `advertise` property to `true` (default is `false`). For the route to be advertised, the route's routing table must be configured for the source or sources to advertise it to:

- When [creating](/apidocs/vpc/latest#create-vpc-routing-table) or [updating](/apidocs/vpc/latest#update-vpc-routing-table) a VPC routing table, you can set the new `advertise_routes_to` array property to include the value `direct_link`. Including this value requires that the routing table's `route_direct_link_ingress` property be set to `true`. Routes in this routing table with the `advertise` property set to `true` will be advertised to Direct Link sources.
- When [creating](/apidocs/vpc/latest#create-vpc-routing-table) or [updating](/apidocs/vpc/latest#update-vpc-routing-table) a VPC routing table, you can set the new `advertise_routes_to` array property to include the value `transit_gateway`. Including this value requires that the routing table's `route_transit_gateway_ingress` property be set to `true`. Routes in this routing table with the `advertise` property set to `true` will be advertised to Transit Gateway sources.

When [creating](/apidocs/vpc/latest#create-vpc-routing-table) a routing table, the default value for the `advertise_routes_to` property is an empty array. When the `advertise_routes_to` property is an empty array, the `advertise` property for routes in the table has no effect.

**Virtual network interface expanded support.** Accounts that have been granted special approval can preview a new feature that expands the support for [virtual network interfaces](/docs/vpc?topic=vpc-vni-about):

- [Instances](/apidocs/vpc/latest#create-instance) and [bare metal servers](/apidocs/vpc/latest#create-bare-metal-server) can now be created with virtual network interfaces attached to new child resources called network attachments. You can specify a `primary_network_attachment` (instead of a `primary_network_interface`) and provide either the identity of an existing virtual network interface, or a subnet to create a new virtual network interface for the instance or bare metal server.
- Virtual network interfaces can now have lifecycles that are independent of the resources they are attached to. When [creating](/apidocs/vpc/latest#create-virtual-network-interface) a virtual network interface, the `auto_delete` property is set to `false`. When automatically creating a new virtual network interface in the context of creating another resource, the `auto_delete` property for the automatically created virtual network interface defaults to `true`. You can override it, or you can later [update](/apidocs/vpc/latest#update-virtual-network-interface) the `auto_delete` property to `false`. A virtual network interface with `auto_delete` set to `false` persists beyond the lifecycle of its current `target` resource. After the target resource is deleted, you can re-attach the virtual network interface to another resource, such as to a bare metal server, an instance, or a share mount target.
- Virtual network interfaces now support [secondary IP addresses](/docs/vpc?topic=vpc-vni-about-secondary-ip). To add a secondary IP, [bind](/apidocs/vpc/latest#add-virtual-network-interface-ip) a reserved IP to a virtual network interface. To remove a secondary IP, [unbind](/apidocs/vpc/latest#remove-virtual-network-interface-ip) a reserved IP from from a virtual network interface.
- For compatibility with existing clients, instances and bare metal servers with virtual network interfaces now include a read-only representation of their network attachments and virtual network interfaces as legacy network interface child resources. Learn about [support for old API clients](/docs/vpc?topic=vpc-vni-about&interface=ui#vni-old-api-clients).
- For instances and bare metal servers with virtual network interfaces, the IAM permissions for options to allow IP spoofing or disable infrastructure NAT are managed on their attached virtual network interfaces. When [creating](/apidocs/vpc/latest#create-virtual-network-interface) or [updating](/apidocs/vpc/latest#update-virtual-network-interface) a virtual network interface, you can set non-default values for the `allow_ip_spoofing` and `enable_infrastructure_nat` properties only if you have the `is.virtual-network-interface.virtual-network-interface.manage-ip-spoofing` and `is.virtual-network-interface.virtual-network-interface.manage-infrastructure-nat` IAM permissions respectively.
- [Flow log collectors](/apidocs/vpc/latest#create-flow-log-collector) can now target instance network attachments and virtual network interfaces. There is currently no support for flow logs for bare metal servers and share mount targets.

## 12 December 2023
{: #12-december-2023}

### For all version dates
{: #12-december-2023-all-version-dates}

**Cross-region replication of file shares.** When [creating a file share](/apidocs/vpc/latest#create-share), you can now specify a zone in an associated partner region to create a replica file share. For more information about cross-region pairings, see [About file share replication](/docs/vpc?topic=vpc-file-storage-replication). A [service-to-service authorization for cross-region replication](/docs/vpc?topic=vpc-file-s2s-auth&interface=api#file-s2s-auth-replication-api) between the regional file services must be created before creating a replica.

An important difference between setting up in-region and cross-region replication is configuring the encryption for the replica share.

- When the replica is created in another zone of the same region, the encryption type and the encryption key are inherited from the source share and can't be changed.
- When the replica is created in another region, only the encryption type is inherited. Therefore, if the source share has `user_managed` encryption, you must specify the root key by using the `encryption_key` property when creating the replica share.
{: note}

When [retrieving a file share](/apidocs/vpc/latest#get-share), the `source_share` property now includes a `remote` sub-property that, if present in the response, indicates that the resource that is associated with this reference is remote and might not be directly retrievable.

**Last replication sync information.** When [retrieving a file share](/apidocs/vpc/latest#get-share), the response now includes the properties `completed_at`, `started_at`, and `data_transferred`. These properties provide information about the replication process that can be used to monitor the health of the replication. For more information, see [Replication sync information](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-repl-syncinfo).

## 5 December 2023
{: #5-december-2023}

### For all version dates
{: #5-december-2023-all-version-dates}

**Multi-volume snapshots and backups.** This release introduces a new way to create snapshots. You can now [create a snapshot consistency group](/apidocs/vpc/latest#create-snapshot-consistency-group) and specify one or more snapshots that are attached to the same virtual server instance. When you create a consistency group, you are implicitly creating one or more snapshots. Snapshots taken simultaneously are data-consistent with one another, which helps to ensure consistent backups of a group of {{site.data.keyword.block_storage_is_short}} volumes attached to the same instance. [Deleting a snapshot consistency group](/apidocs/vpc/latest#delete-snapshot-consistency-group) will delete the snapshots in the group by default. However, you can keep the snapshots and delete the consistency group by specifying the `delete_snapshots_on_delete` property.

You can also automate the creation of snapshots in consistency groups. When [creating a backup policy](/apidocs/vpc/latest#create-backup-policy) you can now specify `instance` as a `match_resource_type` value that this backup policy will apply to. Resources that have both a matching type and a matching user tag will be subject to the backup policy. You can exclude boot volumes from backup policies by specifying the `included_content` property. The default behavior includes boot volumes and data volumes.

For more information, see [Backup service concepts](/docs/vpc?topic=vpc-backup-service-about&interface=api#backup-service-concepts), [Snapshot consistency groups](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=api#multi-volume-snapshots), and explore the [backup policy](/apidocs/vpc/latest#list-backup-policies) and [snapshot consistency group](/apidocs/vpc/latest#list-snapshot-consistency-groups) methods.

### For version `2023-12-05` or later
{: #version-2023-12-05}

When making API requests using a `version` query parameter of `2023-12-05` or later, the backup policy `match_resource_types` property has been changed to `match_resource_type`. This change applies when [creating](/apidocs/vpc/latest#create-backup-policy), [updating](/apidocs/vpc/latest#update-backup-policy), [listing](/apidocs/vpc/latest#list-backup-policies), [retrieving](/apidocs/vpc/latest#get-backup-policy), and [deleting](/apidocs/vpc/latest#delete-backup-policy) a backup policy. See [Updating to the `2023-12-05` version (backup policies)](/docs/vpc?topic=vpc-2023-12-05-migration-backup-policy) for guidance on migrating from `match_resource_types` to `match_resource_type`.

## 24 October 2023
{: #24-october-2023}

### For all version dates
{: #24-october-2023-all-version-dates}

**Network load balancer security group integration.** For enhanced security, you can now associate security groups with [network load balancers](/docs/vpc?topic=vpc-nlb-integration-with-security-groups). When [creating a load balancer](/apidocs/vpc/latest#create-load-balancer), you can now specify the `security_groups` property, which associates those security groups with the load balancer. If you do not specify `security_groups`, the network load balancer will be associated with the VPC's default security group. Before using the default security group, review your default security group rules and, if necessary, edit the rules to accommodate your expected network load balancer traffic.

All existing network load balancers in your account will continue to allow all inbound and outbound traffic. This is indicated by the `security_groups` property being set to an empty array (no security groups configured).
{: note}

When creating a network load balancer, you must now have an appropriate Identity and Access Management (IAM) role with the `is.security-group.security-group.operate` action.
{: note}

You can update security groups for a network load balancer by [adding a network load balancer](/apidocs/vpc/latest#create-security-group-target-binding) to or [removing a network load balancer](/apidocs/vpc/latest#delete-security-group-target-binding) from a security group's targets.

You will not be able to remove the only remaining security group from a network load balancer. As a result, if you add a security group to a network load balancer that had no security groups, you will not be able to revert that network load balancer to have no security groups.

Finally, the security group `targets` property can now refer to a network load balancer, as can the responses for the [get security group target](/apidocs/vpc/latest#get-security-group-target) and [list security group targets](/apidocs/vpc/latest#list-security-group-targets) methods.

## 17 October 2023
{: #17-october-2023}

### For all version dates
{: #17-october-all-version-dates}

**Non-uniform memory access (NUMA) awareness on instances and dedicated hosts.**  When [retrieving](/apidocs/vpc/latest#get-instance) and [listing](/apidocs/vpc/latest#list-instances) an instance, the new `numa_count` property indicates the number of NUMA nodes on which a virtual server instance is provisioned. This property will be absent from the response if the instance's `status` is not `running`. When [retrieving](/apidocs/vpc/latest#get-dedicated-host) and [listing](/apidocs/vpc/latest#list-dedicated-hosts) a dedicated host, the new `numa.count` and `numa.nodes` properties describe the processor topology.

When [retrieving](/apidocs/vpc/latest#get-instance-profile) and [listing](/apidocs/vpc/latest#list-instance-profiles) an instance profile, the `numa_count` property indicates the total number of NUMA nodes for an instance with this profile. When the `type` is `dependent`, the total number of NUMA nodes for an instance with this profile depends on its configuration and the capacity constraints within the zone. Not all instance profiles have a strict NUMA definition within them.

For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=api#next-gen-profiles).

**Status on instance profiles and dedicated host profiles.** When [retrieving](/apidocs/vpc/latest#get-instance-profile) and [listing](/apidocs/vpc/latest#list-instance-profiles) an instance profile or [retrieving](/apidocs/vpc/latest#get-dedicated-host-profile) and [listing](/apidocs/vpc/latest#list-dedicated-host-profiles) a dedicated host profile, a new `status` property indicates the status of the instance profile or dedicated host profile. A `status` value of `previous` indicates an older profile generation that remains provisionable and usable. A `status` value of `current` indicates the latest generation of a given profile. For more information, see [Next generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=api#next-gen-profiles) and [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles).

**Viewing DNS resolver information for VPCs.** When [retrieving](/apidocs/vpc/latest#get-vpc) a VPC, the new `dns.resolver` property contains information about the DNS resolvers provided by the system for DHCP clients in the VPC.

This release introduces the following updates for accounts that have been granted special approval to preview these features:

- **Sharing DNS resolution for endpoint gateways across VPCs.** When multiple VPCs are connected together using Transit Gateway, Direct Link, or other connectivity options, a VPC in the connected topology can now be enabled as a DNS hub to centralize the DNS resolution for endpoint gateways. When [creating](/apidocs/vpc/latest#create-vpc) or [updating](/apidocs/vpc/latest#update-vpc) a VPC, a new `dns` property includes configuration options for DNS. Specify the `dns.enable_hub` property as `true` to enable the VPC as a DNS hub (default is `false`).  Specify a DNS hub VPC when [creating a DNS resolution binding](/apidocs/vpc/latest#create-vpc-dns-resolution-binding) on another VPC to share its DNS resolution with that DNS hub VPC. The new `dns.resolution_binding_count` response property specifies how many other VPCs a VPC is bound to for DNS resolution sharing. For more information, see [About DNS sharing for VPE gateways](/docs/vpc?topic=vpc-hub-spoke-model).

- **Configuring DNS resolvers for a VPC.** You can use the new `dns.resolver` property to configure the DNS resolvers for a VPC. Use a `dns.resolver.type` of `manual` to specify the DNS resolvers by IP address. Use a `dns.resolver.type` of `delegated` to specify another VPC (typically a DNS hub VPC) whose DNS resolvers will be used. Use a `dns.resolver.type` of `system` to restore the system default DNS resolvers. When `dns.resolver.type` is `manual`, [updating](/apidocs/vpc/latest#update-vpc) specifying the VPC's `dns.resolver.manual_servers` requires the [`If-Match` header](/apidocs/vpc/latest#concurrent-update-protection) also be provided with the VPC's current `ETag` value.

## 10 October 2023
{: #10-october-2023}

### For all version dates
{: #10-october-2023-all-version-dates}

**Diagnosing VPN gateway and VPN server issues.** You can now diagnose and resolve issues with your deployed VPN gateways and VPN servers:

- The [list all VPN gateways](/apidocs/vpc/latest#list-vpn-gateways) and [retrieve a VPN gateway](/apidocs/vpc/latest#get-vpn-gateway) methods now include `health_reasons`, `health_state`, `members[].health_reasons`, and `members[].health_state` properties. An unhealthy VPN gateway or VPN gateway member now has its `health_state` property set to `degraded` or `faulted`. The `health_reasons` property includes the reasons for the current VPN gateway or VPN gateway member health state. For more information, see [Diagnosing VPN gateway health](/docs/vpc?topic=vpc-vpn-health).

- The [list all VPN gateway connections](/apidocs/vpc/latest#list-vpn-gateway-connections) and [retrieve a VPN gateway connection](/apidocs/vpc/latest#get-vpn-gateway-connection) methods now include `status_reasons` and `tunnels[].status_reasons` properties for a static-route-mode VPN gateway. A VPN gateway connection or tunnel in a down state now includes the reasons for the current VPN gateway connection or tunnel through the `status_reasons` property. For more information, see [Diagnosing VPN gateway connection health](/docs/vpc?topic=vpc-vpn-health#vpn-connection-health).

- The [list all VPN servers](/apidocs/vpc/latest#list-vpn-servers) and [retrieve a VPN server](/apidocs/vpc/latest#get-vpn-server) methods now include a `health_reasons` property. An unhealthy VPN server now has its `health_state` property set to `degraded` or `faulted`. The `health_reasons` property includes the reasons for the current VPN server health state. For more information, see [Diagnosing VPN server health](/docs/vpc?topic=vpc-vpn-server-health).

- The [list all VPN server routes](/apidocs/vpc/latest#list-vpn-server-routes) and [retrieve a VPN server route](/apidocs/vpc/latest#get-vpn-server-route) methods now include `health_reasons` and `health_state` properties. An unhealthy VPN server route now has its `health_state` property set to `degraded` or `faulted`. The  `health_reasons` property includes the reasons for the current VPN server route health state. For more information, see [Diagnosing VPN server route health](/docs/vpc?topic=vpc-vpn-server-health#vpn-server-route-health).

**Resource suspension for VPNs for VPC.**

- VPN gateway. The [list all VPN gateways](/apidocs/vpc/latest#list-vpn-gateways) and [retrieve a VPN gateway](/apidocs/vpc/latest#get-vpn-gateway) methods now include `lifecycle_reasons` and `lifecycle_state` properties. The same properties are also included for VPN gateway member child resources. A VPN gateway that violates the IBM Cloud [Acceptable Use Policy](https://www.ibm.com/services/us/imc/html/aup1.html){: external} will have its `lifecycle_state` property set to `suspended`, along with the `lifecycle_state` of its members. A suspended VPN gateway is automatically disabled, causing all connections to be brought down, and you cannot enable, update, or delete it or its connections.

- VPN server. The [list all VPN servers](/apidocs/vpc/latest#list-vpn-servers) and [retrieve a VPN server](/apidocs/vpc/latest#get-vpn-server) methods now include `lifecycle_reasons` and `lifecycle_state` properties. The same properties are also included for the VPN server route child resource. A VPN server that violates the IBM Cloud [Acceptable Use Policy](https://www.ibm.com/services/us/imc/html/aup1.html){: external} will have its `lifecycle_state` property set to `suspended`, along with the `lifecycle_state` of its server routes. A suspended VPN server is automatically disabled, and you cannot enable, update, or delete it or its routes.

For more information, see [Resource suspension](/docs/vpc?topic=vpc-resource-suspension).

### For version `2023-10-10` or later
{: #version-2023-10-10}

When [listing all VPN gateways](/apidocs/vpc/latest#list-vpn-gateways) and [retrieving a VPN gateway](/apidocs/vpc/latest#get-vpn-gateway) using a `version` query parameter of `2023-10-10` or later, the response will no longer include `status` and `members[].status` properties. These properties remain supported for API requests using a version query parameter of `2023-10-09` or earlier. To avoid regressions in client functionality, follow the guidance in [Updating to the `2023-10-10` version (VPN)](/docs/vpc?topic=vpc-2023-10-10-migration-vpn) before specifying version `2023-10-10` or later in VPN gateway requests.

## 3 October 2023
{: #3-october-2023}

### For all version dates
{: #3-october-2023-all-version-dates}

**Enterprise Backup as a Service.** As an enterprise account administrator, you can now create backup policies and plans that apply to resources of all accounts within your enterprise. Specify the enterprise CRN in the `scope` property when you [create a backup policy](/apidocs/vpc/latest#create-backup-policy), and the policy will apply to all resources that have tags that match with the policy across all accounts within your enterprise. For more information, see [Scope of the backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=api#backup-service-about-scope). As a prerequisite, ensure that authorizations are in place between services and between the enterprise account and the child accounts. For more information, see [Establishing service to service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth&interface=api).

## 8 August 2023
{: #8-august-2023}

### For all version dates
{: #8-august-2023-all-version-dates}

**File storage for VPC.** You can now create NFS-based file shares in a zone in your region. Share file storage over multiple virtual server instances within the same zone across multiple VPCs. Learn about [file shares and mount targets](/docs/vpc?topic=vpc-file-storage-vpc-about), and explore the new [API methods](/apidocs/vpc/latest#list-share-profiles).

## 11 July 2023
{: #11-july-2023}

### For all version dates
{: #11-july-2023-all-version-dates}

**Image lifecycle management.** You can now [deprecate](/apidocs/vpc/latest#deprecate-image) or [obsolete](/apidocs/vpc/latest#obsolete-image) custom images directly. Alternatively, you can schedule transition at a later date by specifying the `deprecation_at` or `obsolescence_at` properties when [creating](/apidocs/vpc/latest#create-image) or [updating](/apidocs/vpc/latest#update-image) an image. If you need to revert a status change, you can transition `deprecated` or `obsolete` images back to `available`. For more information, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=api#custom-images-list-api).

`deprecated` custom images remain usable, while `obsolete` images cannot be used to provision instances or bare metal servers.
{: note}

## 27 June 2023
{: #27-june-2023}

### For all version dates
{: #27-june-2023-all-version-dates}

**Copying snapshots and backups across regions.** You can now specify an existing snapshot in one region to [create](/apidocs/vpc/latest#create-snapshot) a copy of the snapshot in another region. When you [list all snapshots](/apidocs/vpc/latest#list-snapshots), you can see the direct snapshot copies in the other regions and you can use them to restore volumes in the other regions. You can create copies in multiple regions, but only one copy of the snapshot can exist in each region. The cross-region snapshot copy contains the same data as the source snapshot, but the cross-region snapshot copy has its own, independent lifecycle and is billed independently. For more information, see [Cross-regional snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_crossregion_copy) and [Cross-regional copy array issue](/docs/vpc?topic=vpc-known-issues#snapshots-CRC-known-issue).

You can now [create a backup policy](/apidocs/vpc/latest#create-backup-policy) that creates backup copies in other regions, in addition to creating a backup snapshot in the current region. [Listing all plans for a backup policy](/apidocs/vpc/latest#list-backup-policy-plans) or [retrieving a backup policy plan](/apidocs/vpc/latest#get-backup-policy-plan) shows the `copies` of the snapshot in other regions. For more information, see [Cross-regional backup copies](/docs/vpc?topic=vpc-backup-service-about&interface=api#backup-service-crc).

**Extended SSH key encryption.** When [creating](/apidocs/vpc/latest#create-key) an SSH key, you can now specify a `type` property value of `ed25519` for the crypto-system used by the key. If `type` is not specified during key creation, the default value `rsa` continues to be used. When [listing all keys](/apidocs/vpc/latest#list-keys) and [retrieving a key](/apidocs/vpc/latest#get-key), the response provides the key `type` used. For more information, see [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys&interface=api). See also [Extended SSH key encryption](/docs/vpc?topic=vpc-metadata-api-change-log#27-june-2023-metadata) in the VPC Instance Metadata API change log.

## 20 June 2023
{: #20-june-2023}

### For all version dates
{: #20-june-2023-all-version-dates}

**Instance group integration with Network Load Balancer for VPC.** Network load balancer is now integrated with instance groups to improve pool member scaling. When [creating](/apidocs/vpc/latest#create-instance-group) or [updating](/apidocs/vpc/latest#update-instance-group) an instance group for auto scaling, you can now also specify a network load balancer pool for the `load_balancer_pool` property. As before, if `load_balancer_pool` is set, `load_balancer` and `application_port` must also be set. As with application load balancers, the pool must not be used by another instance group in the VPC. When you configure a listener with a range of ports, the instance group's application port is used only for checking the health status of targets. For more information see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

In the future, load balancer profiles may be introduced that do not support instance groups. To ensure your clients will work reliably in the future, check that the new `instance_groups_supported` property on the [load balancer](/apidocs/vpc/latest#get-load-balancer) is `true` before specifying that load balancer or one of its pools.
{: important}

## 13 June 2023
{: #13-june-2023}

### For all version dates
{: #13-june-2023-all-version-dates}

**VPC routing table authorizations.** You can use the new VPC routing table authorizations to allow users to administer VPC routing tables but not allow them to administer the broader VPC. The VPC API methods that operate on routing tables have been updated to check for these new authorizations, instead of the broader VPC authorizations. The VPC Administrator, Editor, Operator, and Viewer IAM access roles have been updated so that users with those roles will function as before. However, custom roles that require access to routing tables must be updated. For more information, see [Required permissions](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls).

## 23 May 2023
{: #23-may-2023}

### For all version dates
{: #23-may-2023-all-version-dates}

**Removal of weak VPN for VPC ciphers.** Effective 18 May 2023, the following VPN IKE and IPsec ciphers are removed:

- Authentication algorithms `md5` and `sha1`
- Encryption algorithm `triple_des`
- Diffie–Hellman groups `2` and `5`

As a result, you will no longer be able to create an IKE/IPsec policy or VPN connection that includes a weak cipher.

## 2 May 2023
{: #2-may-2023}

### For all version dates
{: #2-may-2023-all-version-dates}

**Exporting custom images.** You can now [export custom images](/apidocs/vpc/latest#create-image-export-job) to an authorized IBM Cloud Object Storage bucket. Specify the target `storage_bucket` to export the image to. The image will be exported as `qcow2` unless you specify another value using the `format` property. For more information, see [Exporting a custom image to IBM Cloud Object Storage](/docs/vpc?topic=vpc-managing-custom-images&interface=api#custom-image-export-to-cos-api), or start using the new [export jobs](/apidocs/vpc/latest#list-image-export-jobs) methods.

## 18 April 2023
{: #18-april-2023}

### For all version dates
{: #18-april-2023-all-version-dates}

**Resource suspension for bare metal servers.** The [list all bare metal servers](/apidocs/vpc/latest#list-bare-metal-servers) and [retrieve a bare metal server](/apidocs/vpc/latest#get-bare-metal-server) methods now provide `lifecycle_reasons` and `lifecycle_state` properties. A bare metal server that violates IBM Cloud’s [Acceptable Use Policy](https://www.ibm.com/services/us/imc/html/aup1.html){: external} will now have its `lifecycle_state` property set to `suspended`. A suspended bare metal server is automatically powered off and you cannot update, delete, or power it on. For more information, see [Viewing bare metal status and lifecycle_state in the API](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=api) and [Resource suspension](/docs/vpc?topic=vpc-resource-suspension).

## 11 April 2023
{: #11-april-2023}

### For all version dates
{: #11-april-2023-all-version-dates}

**Console type configuration for bare metal server profiles.** When you [retrieve a bare metal server profile](/apidocs/vpc/latest#get-bare-metal-server-profile) or [list all bare metal server profiles](/apidocs/vpc/latest#list-bare-metal-server-profiles), the response now provides a `console_types` property that denotes the console type configuration for a bare metal server with this profile.

**Network interface configuration for bare metal server profiles.** When you [retrieve a bare metal server profile](/apidocs/vpc/latest#get-bare-metal-server-profile) or [list all bare metal server profiles](/apidocs/vpc/latest#list-bare-metal-server-profiles), the response now provides a `network_interface_count` property. When the `type` is `range`, the new property provides `max` and `min` sub-properties that denote the maximum and minimum number of network interfaces that are supported for a bare metal server with the specified profile. The values for `max` and `min` include both the primary network interface and secondary network interfaces. When the `type` is `dependent`, the network interface count depends on another value that is specified when the server is created. For more information about network interfaces, see [Overview of bare metal server network interfaces](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers#overview-bare-metal-network-interfaces), and [Managing network interfaces for bare metal servers on VPC](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers&interface).

## 28 March 2023
{: #28-march-2023}

### For all version dates
{: #28-march-2023-all-version-dates}

**VCPU manufacturer support for instances and dedicated hosts.** When [provisioning](/apidocs/vpc/latest#create-instance) an instance or dedicated host, you can now use the new `vcpu_manufacturer` property in the [instance](/apidocs/vpc/latest#list-instance-profiles) or [dedicated host](/apidocs/vpc/latest#list-dedicated-host-profiles) profile to choose between profiles from different processor manufacturers. You can also view the virtual server instance VCPU configuration through the `vcpu` sub-property `manufacturer`. For more information and limitations, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) and [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui#balanced-dh-pr).

**Network interface configuration for instance profiles.** When you [retrieve an instance profile](/apidocs/vpc/latest#get-instance-profile) or [list all instance profiles](/apidocs/vpc/latest#list-instance-profiles), the response now provides a `network_interface_count` property. When the `type` is `range`, the new property provides  `max` and `min` sub-properties that denote the maximum and minimum number of network interfaces that are supported for a virtual server instance with the specified profile. The values for `max` and `min` include both the primary network interface and secondary network interfaces. When the `type` is `dependent`, the network interface count depends on another value that is specified when the instance is created. For more information about instance profiles and network interface count, see [Bandwidth allocation with multiple network interfaces](/docs/vpc?topic=vpc-profiles&interface=api#bandwidth-multi-vnic).

**Private DNS integration for load balancers.** When you [create](/apidocs/vpc/latest#create-load-balancer) or [update](/apidocs/vpc/latest#update-load-balancer) a load balancer, you can now bind the IP addresses of your VPC load balancers to your private DNS zone by specifying the new `dns.instance` and `dns.zone` properties. When you specify these properties, load balancer IPs will no longer be registered to the publicly resolvable `lb.appdomain.cloud` domain name. For more information, see [IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-nlb-dns&interface=api) and [IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-lb-dns&interface=api).

## 21 March 2023
{: #21-march-2023}

### For all version dates
{: #21-march-2023-all-version-dates}

**Instance provision by volume.**  You can now reuse an existing boot volume to provision a virtual server instance by specifying the existing volume using the `id` or `crn` sub-properties of the `boot_volume_attachment` property. The specified volume must be unattached and must have an operating system with the same architecture as the instance profile. You can use the new volume `attachment_state` property and expanded `operating_system` property to determine its eligibility. You can also use the new [list volumes](/apidocs/vpc/latest#list-volumes) filters to list volumes that have specific `attachment_state`, `operating_system`, and `encryption_type` values.

By default, a boot volume created as part of provisioning a virtual server instance will be deleted when the instance is deleted. You can control this by specifying the `delete_volume_on_instance_delete` property when [creating the instance](/apidocs/vpc/latest#create-instance) or updating the [boot volume attachment](/apidocs/vpc/latest#update-instance-volume-attachment). For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api).

**VPC route priority.** You can now control the priority of [VPC routes](/docs/vpc?topic=vpc-about-custom-routes). When you [create](/apidocs/vpc/latest#create-vpc-routing-table-route) or [update](/apidocs/vpc/latest#update-vpc-routing-table-route) a VPC route, use the new `priority` property to specify a value between `0` and `4` (default: `2`). Smaller values have higher priority. For more information, see [Determining route preference](/docs/vpc?topic=vpc-about-custom-routes#cr-determining-route-pref).

**Modifiable next hop for VPC routes.** You can now [update](/apidocs/vpc/latest#update-vpc-routing-table-route) the `next_hop` property of a VPC route. For more information about next hop, see [Creating a route](/docs/vpc?topic=vpc-create-vpc-route).

## 7 March 2023
{: #7-march-2023}

### For all version dates
{: #7-march-2023-all-version-dates}

**Idle connection timeout control for application load balancers.** You can now control the maximum time a client can be inactive when connected to the server by specifying the `idle_connection_timeout` property when [creating a load balancer](/apidocs/vpc/latest#create-load-balancer), [creating a load balancer listener](/apidocs/vpc/latest#create-load-balancer-listener), and [updating a load balancer listener](/apidocs/vpc/latest#update-load-balancer-listener). The `idle_connection_timeout` value defaults to the minimum of 50 seconds, and has a maximum of 2 hours, specified in seconds. For more information, see [Creating an application load balancer](/docs/vpc?topic=vpc-load-balancers&interface=ui).

## 14 February 2023
{: #14-february-2023}

### For all version dates
{: #14-february-2023-all-version-dates}

**VPC instance metadata new endpoint URL.** You can now use the fully qualified domain name (FQDN) `api.metadata.cloud.ibm.com` for the metadata service endpoint. The FQDN resolves to the link-local IP address `169.254.169.254` without requiring the application of special configurations. For more information, see [Endpoint URLs](/apidocs/vpc-metadata#endpoint-url-metadata) in the VPC Instance Metadata API.

**VPC instance metadata communication protocol and hop limit.** You can now control the communication protocol and hop limit for IP response packets used by the [VPC Instance Metadata service](/docs/vpc?topic=vpc-imd-about). When you [create](/apidocs/vpc/latest#create-instance) or [update](/apidocs/vpc/latest#update-instance) an instance, use the new `metadata_service.protocol` property to specify either `http` (default) or `https` (secure access) communication. In addition, use the new `metadata_service.response_hop_limit` property to specify a value between `1` (default) and `64`. Both of these properties apply only when the metadata service is enabled by setting `metadata_service.enabled` to `true`. The default is `false`. For more information, see [Configure metadata settings on an existing instance with the API](/docs/vpc?topic=vpc-imd-configure-service&interface=api#metadata-config-api).

## 7 February 2023
{: #7-february-2023}

### For all version dates
{: #7-february-2023-all-version-dates}

**Snapshot clones for fast restore.** You can now quickly restore a volume from a snapshot by using a fast restore snapshot clone. You can create a fast restore clone when you [create](/apidocs/vpc/latest#create-snapshot) a new snapshot or [update](/apidocs/vpc/latest#create-snapshot-clone) an existing snapshot by adding one or more zonal clones for a snapshot in the same region as the snapshot. Later, you can restore a volume, and all of its data, from that fast restore snapshot clone. When you no longer need a zonal snapshot clone, you can [delete](/apidocs/vpc/latest#delete-snapshot-clone) it. Although the delete operation cannot be reversed, you can create an new, equivalent zonal clone from the snapshot.

When [creating](/apidocs/vpc/latest#create-backup-policy-plan) or [updating](/apidocs/vpc/latest#update-backup-policy-plan) a backup policy plan, you can now specify the `clone_policy.zones` in which backup service will create snapshot clones.

For more information, see [Restoring a volume using fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-use-fast-restore) or dive into the new [API methods](/apidocs/vpc/latest#list-snapshot-clones).

## 31 January 2023
{: #31-january-2023}

### For all version dates
{: #31-january-2023-all-version-dates}

**Bare metal server secure boot.** When you [create](/apidocs/vpc/latest#create-bare-metal-server) or [update](/apidocs/vpc/latest#update-bare-metal-server) a bare metal server, you can now enable secure boot. The default is `false` (disabled). If enabled, the image must support secure boot or the server will fail to boot. To toggle secure boot, the server must be `stopped`. For more information, see [Bare metal server images](/docs/vpc?topic=vpc-bare-metal-image).

**Bare metal server trusted platform module (TPM) support.** When you [create](/apidocs/vpc/latest#create-bare-metal-server) or [update](/apidocs/vpc/latest#update-bare-metal-server) a bare metal server, you can now set a TPM mode. Specify a `mode` value (`disabled` or `tpm_2`) in the `trusted_platform_module` property. The default is `disabled`. To change the TPM mode, the server must be `stopped`. To determine the supported TPM modes, use the `supported_trusted_platform_modes` property included in the [bare metal server profile](/apidocs/vpc/latest#list-bare-metal-server-profiles). For more information, see [Secure boot with Trusted Platform Module (TPM)](/docs/vpc?topic=vpc-secure-boot-tpm).

## 17 January 2023
{: #17-january-2023}

### For all version dates
{: #17-january-2023-all-version-dates}

**Bare metal server DELETE response code change.** Bare metal `DELETE` methods now return an HTTP response code of `202` upon success:

- [Delete a network interface](/apidocs/vpc/latest#delete-bare-metal-server-network-interface)
- [Disassociate a floating IP from a network interface](/apidocs/vpc/latest#remove-bare-metal-server-network-interface-floatin)
- [Delete a bare metal server](/apidocs/vpc/latest#delete-bare-metal-server)

Unlike previous response code changes, the transition from `204` to `202` applies to all API versions.  Therefore, a response code of `204` will not be returned for any API requests for these methods, regardless of the `version` query parameter value. Future transitions from `204` to `202` will be tied to a dated API version, as described in [Upcoming changes](#upcoming-changes).
{: note}

## 20 December 2022
{: #20-december-2022}

### For all version dates
{: #20-december-2022-all-version-dates}

**Backup policy jobs.** You can now [list all jobs for a backup policy](/apidocs/vpc/latest#list-backup-policy-jobs) or [retrieve a backup policy job](/apidocs/vpc/latest#get-backup-policy-job). A backup policy job is triggered when a scheduled backup snapshot is being created or deleted. If the create or delete action is successful, the job contains information about the backup snapshot that was created or deleted. If the job ran unsuccessfully, the job contains the reason for the failure. For more information, see [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## 13 December 2022
{: #13-december-2022}

### For all version dates
{: #13-december-2022-all-version-dates}

**Health states for block storage volumes.** When you [list all volumes](/apidocs/vpc/latest#list-volumes) or [retrieve volume details](/apidocs/vpc/latest#get-volume), the responses now include `health_state` and `health_reasons` properties. For more information, see [Block storage volume health states](/docs/vpc?topic=vpc-block-storage-vpc-monitoring&interface=api#block-storage-vpc-health-states).

## 15 November 2022
{: #15-november-2022}

### For all version dates
{: #15-november-2022-all-version-dates}

**Access management tag support.** As described in [Authorization](/apidocs/vpc/latest#api-authorization) in the VPC API reference, you can now use [IBM Cloud Identity and Access Management](/docs/account?topic=account-iamoverview) to control access to VPC resources by using access management tags. For details, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started&interface=api#iam-access-management-tags).

Some VPC APIs currently require additional authorizations beyond those defined in the API specification. For more information, see [Known issues](/docs/vpc?topic=vpc-known-issues#api-spec-auth-known-issue).
{: important}

## 25 October 2022
{: #25-october-2022}

### For all version dates
{: #25-october-2022-all-version-dates}

**VPC route naming restriction.** You can no longer create VPC routes that begin with the prefix `ibm-` or rename an existing route to have the prefix `ibm-`. Existing routes that begin with `ibm-` will not be affected. If you have automation that creates routes using the prefix `ibm-`, you will need to remove or change the prefix used by your automation for it to succeed.

## 11 October 2022
{: #11-october-2022}

### For all version dates
{: #11-october-2022-all-version-dates}

**Public internet ingress routing.** You can now route public internet ingress traffic destined to a floating IP to a next-hop IP. When you [create a new VPC routing table](/apidocs/vpc/latest#create-vpc-routing-table) or [update an existing VPC routing table](/apidocs/vpc/latest#update-vpc-routing-table), the new `route_internet_ingress` property lets you route traffic that originates from the public internet. For more information, see [Creating a routing table by using the API](/docs/vpc?topic=vpc-create-vpc-routing-table&interface=api) and limitations and guidelines for [Ingress routes](/docs/vpc?topic=vpc-about-custom-routes&interface=ui#routes-ingress).

## 4 October 2022
{: #4-october-2022}

### For all version dates
{: #4-october-2022-all-version-dates}

**Enhanced network interface support for flow logs.** Flow logs are now collected for all network interfaces attached to a subnet, even if those network interfaces are in another account. As a result, flow logs for network interfaces associated with IBM Cloud Kubernetes Service (IKS)/Red Hat OpenShift Kubernetes Service (ROKS) worker nodes, load balancers, and VPN gateways are now collected. For example, if you have an existing [flow log collector](/apidocs/vpc/latest#get-flow-log-collector) that targets a VPC or subnet that also has attached IKS worker nodes, it will now collect flow logs for traffic flowing through those IKS worker nodes in those VPCs and subnets. For more information, see [Flow log limitations](/docs/vpc?topic=vpc-limitations-flow-logs).

## 27 September 2022
{: #27-september-2022}

### For all version dates
{: #27-september-2022-all-version-dates}

**Sharing images across accounts within an enterprise.** You can now use a [catalog to share custom images](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=api) with users in other accounts within the same enterprise. When you [create an image](/apidocs/vpc/latest#create-image), a new `catalog_offering` property includes a `managed` sub-property that is set to `false` by default.  When the custom image is imported to a catalog the `managed` sub-property is set to `true`, indicating that the image is added to a catalog offering `version` and is managed from a catalog. Any user who has been authorized to the catalog offering version can [provision a virtual server instance](/apidocs/vpc/latest#create-instance) with that image by specifying the offering version's CRN as `catalog_offering.version.crn`. Alternatively, users can specify the offering's CRN as `catalog_offering.offering.crn` to provision a virtual server instance with the latest image associated with the catalog offering. For more information, see [Custom images in a private catalog](/docs/vpc?topic=vpc-private-catalog-image-instance-group&interface=ui), the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui), and the [Import offering](/apidocs/resource-catalog/private-catalog#import-offering){: external} method in the Catalog Management API.

The image may not be deleted or used in a different catalog product offering version while it is managed from a catalog. If the catalog is deleted, a 7 day reclamation period will apply that prevents any images managed by the catalog from being deleted or re-used during the reclamation period. For more information, see [Deleting a custom image in a private catalog](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=ui#deleting-private-catalog-custom-image-vpc) and [Using resource reclamations](/docs/account?topic=account-resource-reclamation&interface=api).

Image references may refer to custom images in other accounts. Before using this feature, verify that your clients handle image reference lookup failures gracefully and do not assume inaccessible images have been deleted, even when running with full access to your images.  To avoid possible retrieval or use of the wrong image by `name`, specify the image `id`, `crn`, or `href` instead. For more information, see [Using cross-account image references in a private catalog in the API](/docs/vpc?topic=vpc-custom-image-cloud-private-catalog&interface=api#private-catalog-image-reference-vpc-api).
{: important}

**Increased network interface limits for virtual server instances.** You can now have up to 14 secondary network interfaces on a virtual server instance. The previous limit for secondary network interfaces was 4. The number of interfaces that a virtual server instance supports is dependent on the VCPU count that is included in the [instance profile](/docs/vpc?topic=vpc-profiles). For more information about the number of interfaces that a virtual server supports, see [Bandwidth allocation with multiple network interfaces](/docs/vpc?topic=vpc-profiles&interface=ui#bandwidth-multi-vnic). To utilize the increased limit for network interfaces, you can create secondary network interfaces by specifying `network_interfaces` when you [create an instance](/apidocs/vpc/latest#create-instance). You also can add secondary network interfaces to an existing instance by [creating a network interface on an instance](/apidocs/vpc/latest#create-instance-network-interface).

For an existing, running instance with 17 or more vCPUs to take advantage of the new network interface limits, it must be stopped and then started again. A reboot action on the running virtual server does not activate the increased network interface limit.
{: note}

**IBM&reg; LinuxONE Bare Metal Servers.** Accounts with access to the profiles for s390x bare metal servers can now [create](/apidocs/vpc/latest#create-bare-metal-server) LinuxONE Bare Metal Servers. These profiles have a `cpu_architecture` of `s390x` and must be used with Red Hat Enterprise Linux for s390x and SUSE Linux Enterprise Server (SLES) for s390x. For more information, see [Creating bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=api).

In support of s390x bare metal servers, the following enumerations have been expanded:

- Because s390x local disks are attached through Fiber Channel protocol, a value of `fcp` has been added to the `interface_type` enumeration that is returned when you [retrieve](/apidocs/vpc/latest#get-bare-metal-server-disk) or [list](/apidocs/vpc/latest#list-bare-metal-server-disks) the disks for a bare metal server.

- Because s390x provides TCP/IP connectivity by using HiperSockets, a value of `hipersocket` has been added to the `interface_type` enumeration returned when you [retrieve](/apidocs/vpc/latest#get-bare-metal-server-network-interface) or [list](/apidocs/vpc/latest#list-bare-metal-server-network-interfaces) the network interfaces on an s390x bare metal server. Similarly, when you [create](/apidocs/vpc/latest#create-bare-metal-server) an s390x bare metal server, or [add](/apidocs/vpc/latest#create-bare-metal-server-network-interface) a network interface to an existing s390x bare metal server, you must specify an `interface_type` of `hipersocket`. For more information, see the [_IBM HiperSockets Implementation Guide_](https://www.redbooks.ibm.com/abstracts/sg246816.html){: external}.

s390x bare metal servers have different network bandwidth and maximum network interface limits from x86 bare metal servers.

## 20 September 2022
{: #20-september-2022}

### For all version dates
{: #20-september-2022-all-version-dates}

**Deprecated VPN for VPC ciphers.** The following VPN IKE and IPsec ciphers are now deprecated:

- Authentication algorithms `md5` and `sha1`
- Encryption algorithm `triple_des`
- Diffie–Hellman groups `2` and `5`

You have until 13 December 2022 to upgrade to more secure ciphers. After this date, VPN connections using deprecated ciphers will have a `status` of `down` (and no longer transfer data) until you upgrade from the weak cipher.

**Additional VPN for VPC ciphers.** VPN gateways now provide new algorithms to help meet your security and compliance requirements.

[IKE policy](/apidocs/vpc/latest#list-ike-policies) methods now support the `sha384` value for the `authentication_algorithm` property, `aes192` value for the `encryption_algorithm` property, and groups 15-18, 20-24, and 31 for the `dh_group` property.

[IPsec policy](/apidocs/vpc/latest#list-ipsec-policies) methods now support `sha384` and `disabled` values for the `authentication_algorithm` property, `aes192`, `aes128gcm16`, `aes192gcm16`, and `aes256gcm16` values for the `encryption_algorithm` property, and groups 15-18, 20-24, and 31 for the `dh_group` property.

Specifying IKE and IPsec policies when configuring a VPN connection is optional. If a policy is not specified, one is chosen through _auto-negotiation_. For more information, see [About policy negotiation](/docs/vpc?topic=vpc-using-vpn#policy-negotiation).
{: note}

## 13 September 2022
{: #13-september-2022}

### For all version dates
{: #13-september-2022-all-version-dates}

**Updating subnets for application load balancers.** You can now update the subnets attached to an application load balancer by specifying `subnets` when [updating a load balancer](/apidocs/vpc/latest#update-load-balancer). The specified subnets must be in the same VPC as the load balancer's current subnets. If the update requires moving your load balancer to a different zone, its `provisioning_status` will change to `migrate_pending` until the move is complete. For more information, see [Updating subnets for Application Load Balancers for VPC](/docs/vpc?topic=vpc-alb-updating-subnets&interface=api).

Verify that any clients retrieving the `provisioning_status` property will gracefully handle unknown values. For example, a client might bypass the load balancer, log a message, or halt processing and surface an error.

Because the `subnets` property is an array, the specified value will replace the load balancer's existing array of subnets. To guard against concurrent updates, you must provide the resource's current ETag using the `If-Match` header. For guidance on the use of ETags, see [Concurrent update protection](/apidocs/vpc/latest#concurrent-update-protection).
{: important}

### For version `2022-09-13` or later
{: #version-2022-09-13}

**Load balancer DELETE response code change.** For requests using a `version` query parameter of `2022-09-13` or later, all load balancer `DELETE` methods will return an HTTP response code of `202` upon success:

- [Delete a load balancer](/apidocs/vpc/latest#delete-load-balancer)
- [Delete a load balancer listener](/apidocs/vpc/latest#delete-load-balancer-listener)
- [Delete a load balancer listener policy](/apidocs/vpc/latest#delete-load-balancer-listener-policy)
- [Delete a load balancer listener policy rule](/apidocs/vpc/latest#delete-load-balancer-listener-policy-rule)
- [Delete a load balancer pool](/apidocs/vpc/latest#delete-load-balancer-pool)
- [Delete a load balancer pool member](/apidocs/vpc/latest#delete-load-balancer-pool-member)

The underlying deletion operations were already asynchronous, and remain unchanged.

To avoid regressions, before issuing requests using a `version` query parameter of `2022-09-13` or later, ensure that any clients deleting load balancer resources will also regard a response code of `202` as success.
{: important}

A response code of `204` will continue to be returned for API requests using a `version` query parameter of `2022-09-12` and earlier.

## 6 September 2022
{: #6-september-2022}

### For all version dates
{: #6-september-2022-all-version-dates}

**Improved reserved IP support for bare metal servers**. The following methods have been added for convenience and parity with the virtual server instance reserved IP methods:

- [List all reserved IPs bound to a network interface](/apidocs/vpc/latest#list-instance-network-interface-ips) for a bare metal server
- [Retrieve bound reserved IP](/apidocs/vpc/latest#get-instance-network-interface-ip) for a bare metal server

## 23 August 2022
{: #23-august-2022}

### For all version dates
{: #23-august-2022-all-version-dates}

**Additional user tag support for boot and data volumes.** You can now add user tags to boot and data volumes when provisioning a virtual server instance or adding a volume attachment. You can specify the `user_tags` property when you [create an instance](/apidocs/vpc/latest#create-instance), [create an instance template](/apidocs/vpc/latest#create-instance-template), and [create a volume attachment](/apidocs/vpc/latest#create-instance-volume-attachment). For more information, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage&interface=api) and [Working with tags](/docs/account?topic=account-tag&interface=api).

## 16 August 2022
{: #16-august-2022}

### For all version dates
{: #16-august-all-version-dates}

**Improved VLAN support for bare metal servers.** The restriction limiting a VLAN ID in the `allowed_vlans` property to a single PCI [network interface on a bare metal server](/apidocs/vpc/latest#get-bare-metal-server-network-interface) has been removed. As a result, you can now move VLAN interfaces between PCI interfaces on the same bare metal server.

## 26 July 2022
{: #26-july-2022}

### For all version dates
{: #26-july-2022-all-version-dates}

**Resource suspension for virtual server instances.** The [list all instances](/apidocs/vpc/latest#list-instances) and [retrieve an instance](/apidocs/vpc/latest#get-instance) methods now provide `lifecycle_reasons` and `lifecycle_state` properties. A virtual server instance that violates IBM Cloud’s [Acceptable Use Policy](https://www.ibm.com/services/us/imc/html/aup1.html){: external} will now have its `lifecycle_state` property set to `suspended`. A suspended instance is automatically powered off and you cannot update, delete, or power it on. For more information, see [Viewing instance status and lifecycle_state in the API](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api#instance-status-api) and [Resource suspension](/docs/vpc?topic=vpc-resource-suspension).

## 5 July 2022
{: #5-july-2022}

### For all version dates
{: #5-july-2022-all-version-dates}

**Client VPN for VPC.** Client-to-site connectivity is now available. This feature allows remote devices to securely connect to a VPC using an OpenVPN (or other compatible) software client. For more information about VPN client-to-site connectivity and how it complements the existing VPN site-to-site connectivity, see [About client-to-site VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-overview).

- A **VPN server** allows VPN clients from the internet to connect to a VPC. When [creating a VPN server](/apidocs/vpc/latest#create-vpn-server), you can specify a security group to protect the VPN server, and subnets in your VPC in which the VPN server will allocate its [reserved IP addresses](/apidocs/vpc/latest#list-subnet-reserved-ips). For more information, see [Creating a VPN server](/docs/vpc?topic=vpc-vpn-create-server) and the new [VPN server methods](/apidocs/vpc/latest#list-vpn-servers).
- A **VPN client** represents a client connecting from the internet. You can [retrieve the OpenVPN client configuration](/apidocs/vpc/latest#get-vpn-server-client-configuration) to use to configure a VPN client for a VPN server. For more information, see [Setting up a client VPN environment and connecting to a VPN server](/docs/vpc?topic=vpc-vpn-client-environment-setup) and the new [VPN client methods](/apidocs/vpc/latest#list-vpn-server-clients).
- A **VPN route** controls which subnets the VPN client can access and how the traffic from the VPN client reaches these subnets. For more information, see [Managing VPN routes](/docs/vpc?topic=vpc-vpn-client-to-site-routes) and the new [VPN server route methods](/apidocs/vpc/latest#list-vpn-server-routes).

**Configuring route propagation for VPN gateways and VPN servers.** When you [create a VPC routing table](/apidocs/vpc/latest#create-vpc-routing-table), you can now control if the routing table accepts routes from a VPN gateway or server by specifying the `accept_routes_from` property. When you [view a route in a VPC routing table](/apidocs/vpc/latest#get-vpc-routing-table-route), the new `origin` property shows who created the route (either `user` or `service`), and the `service` routes include a new `creator` property that references the resource that created the route. Routes with the `creator` property present cannot be deleted directly. For more information, see [Configuring route propagation for VPN gateways](/docs/vpc?topic=vpc-advertise-routes-s2s&interface=ui) and [VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation).

**Concurrent update protection.** ETags returned on `GET`, `POST`, and `PATCH` requests represent the modifiable state of the resource. When updating the `accept_routes_from` property on a [routing table](/apidocs/vpc/latest#update-vpc-routing-table), or updating the `client_authentication`, `client_dns_servers`, or `subnets` properties on a [VPN server](/apidocs/vpc/latest#update-vpn-server), you must provide the resource's ETag using the `If-Match` header. For general guidance on the use of ETags, see [Concurrent update protection](/apidocs/vpc/latest#concurrent-update-protection).

## 28 June 2022
{: #28-june-2022}

### For all version dates
{: #28-june-2022-all-version-dates}

**Block storage.** You can now create a volume from a snapshot without having to also create and attach it to a virtual server instance. When you [create a volume](/apidocs/vpc/latest#create-volume), a new `source_snapshot` property lets you specify a snapshot which will be used as source data for the new volume. The volume's data is fully restored later, when you attach it to an instance. Volume performance is initially degraded until the volume data is fully restored. For more information, see [Restoring an unattached data volume from a snapshot with the API](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-unattached-api).

**Cross-zone member support for network load balancers.** You can now [create a load balancer pool](/apidocs/vpc/latest#create-load-balancer-pool) with members across any zone in the region. You can also use the [create pool member](/apidocs/vpc/latest#create-load-balancer-pool-member) and [replace pool member](/apidocs/vpc/latest#replace-load-balancer-pool-members) methods to update an existing pool with members across any zone in the region. The zone of the network load balancer is still identified by the subnet that you specify when you create a load balancer.

Network load balancers with `route_mode` enabled do not support cross zone members.

## 21 June 2022
{: #21-june-2022}

### For all version dates
{: #21-june-2022-all-version-dates}

**Backup for VPC.** You can now create backup policies to schedule automatic backups of your block storage volumes. Backups are made when a user tag in a block storage volume matches a user tag defined in a backup policy. Backups are created by a schedule defined in a [backup plan](/apidocs/vpc/latest#create-backup-policy-plan). Each plan also has a deletion policy for managing backups created by the plan, which you can customize by specifying the `deletion_trigger` sub-property. At the scheduled interval, a backup snapshot is created of that volume. You can have up to four backup plans per policy. See [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

The backup policy jobs API remains in [beta](/docs/vpc?topic=vpc-api-change-log-beta#24-may-2022-beta).
{: note}

## 29 March 2022
{: #29-march-2022}

The `2022-03-29` release includes incompatible changes. To avoid regressions in client functionality, be sure to read and follow the [`2022-03-29` API migration guide](/docs/vpc?topic=vpc-2022-03-29-migration) before specifying version `2022-03-29` or later in any API requests.
{: important}

### For version `2022-03-29` or later
{: #version-2022-03-29}

**Reserved IPs for compute.** Using a `version` query parameter of `2022-03-29` or later, you can now fully control the IP addresses assigned to your network interfaces by specifying a new or existing reserved IP when you [create an instance](/apidocs/vpc/latest#create-instance) or [create a bare metal server](/apidocs/vpc/latest#create-bare-metal-server).

**Migration of network interface IP addresses.** In support of reserved IPs, for requests using a `version` query parameter of `2022-03-29` or later, the network interface `primary_ipv4_address` string property has been migrated to the `primary_ip` object property. See [Migrating use of IP addresses](/docs/vpc?topic=vpc-2022-03-29-migration#migrate-ip-addresses) for guidance on how to migrate to `primary_ip`.

**Removal of security group network interfaces.** The methods for associating security groups with network interfaces that were deprecated in [API version `2021-02-23`](/docs/vpc?topic=vpc-api-change-log#23-february-2021) have been removed as of version `2022-03-29`. See [Migrating use of security group associations](/docs/vpc?topic=vpc-2022-03-29-migration#migrate-security-group-assoc) for guidance on how to migrate to use security group targets, which allow security groups to be associated with VPC resources beyond network interfaces, such as endpoint gateways and load balancers.

### For all version dates
{: #29-march-2022-all-version-dates}

**Reserved IP management.** You can explicitly [Reserve an IP in a subnet](/apidocs/vpc/latest#create-subnet-reserved-ip) ahead of time, and [List all reserved IPs on a subnet](/apidocs/vpc/latest#list-subnet-reserved-ips) to see all your VPC resources that are using IP addresses on that subnet, including load balancers and VPN gateways (with [limitations](/docs/vpc?topic=vpc-known-issues#ip-known-issues)). For more information, see [Managing IP addresses](/docs/vpc?topic=vpc-managing-ip-addresses).

**UDP support for network load balancers.** When [creating a network load balancer](/apidocs/vpc/latest#create-load-balancer) (NLB), you can now set User Datagram Protocol (UDP) as the communications protocol for NLB listeners and pools by specifying `udp` for the `protocol` sub-property of the [`listener`](/apidocs/vpc/latest#create-load-balancer-listener-request) and [`pool`](/apidocs/vpc/latest#create-load-balancer-pool) properties respectively. (Health checks do not support UDP for monitoring the health of pool members.) For more information, see [Configuring UDP for network load balancers](/docs/vpc?topic=vpc-nlb-udp&interface=api).

You must set the same protocol for the load balancer pool and listeners using that pool.
{: tip}

Not all network load balancer offerings will support UDP. Before creating a UDP network load balancer (or updating an existing NLB listener to use UDP), check that the `udp_supported` property of the [load balancer profile](/apidocs/vpc/latest#list-load-balancer-profiles) is `true`.
{: important}

## 22 March 2022
{: #22-march-2022}

### For all version dates
{: #22-march-2022-all-version-dates}

**Concurrent update protection.** To prevent multiple clients from unknowingly overwriting each other's updates, select API methods support entity-tags and conditional requests. For details, see [Concurrent update protection](/apidocs/vpc/latest#concurrent-update-protection) in the Virtual Private Cloud API.

## 22 February 2022
{: #22-february-2022}

### For all API version dates
{: #22-february-2022-all-version-dates}

**Instance availability policies for compute host failures.** A new `availability_policy` property has been added to the [create](/apidocs/vpc/latest#create-instance) and [update](/apidocs/vpc/latest#update-instance) instance methods to control the behavior when the instance's underlying compute host experiences a failure. The `host_failure` sub-property can be used to set the host failure `availability_policy` of the virtual server instance. The default policy is `restart`, which relocates the instance to a healthy host and restarts the instance. The policy may be set to `stop` to have the instance remain stopped if the compute host experiences a failure.

For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=api).

## 15 February 2022
{: #15-february-2022}

### For all version dates
{: #15-february-2022-all-version-dates}

**Resizable boot volumes.** You can now increase the capacity of a boot volume, up to 250 gigabytes (GB). When [creating an instance](/apidocs/vpc/latest#create-instance) from an image or an [instance template](/apidocs/vpc/latest#create-instance-template), you can specify a larger capacity than the image's `minimum_provisioned_size` default. Specify `capacity` in the `volume` sub-property of the `boot_volume_attachment` property.  You can also increase the size of an existing boot volume by specifying the `capacity` property when [updating the volume](/apidocs/vpc/latest#update-volume).

## 8 February 2022
{: #8-february-2022}

### For all version dates
{: #8-february-2022-all-version-dates}

**Port ranges for public network load balancers.** When [creating a public network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer&interface=api) you can now specify a range of listener ports. When you configure a load balancer with a port range, the `port` property of the load balancer's [pool members](/apidocs/vpc/latest#list-load-balancer-pool-members) will not be used for port translation on incoming traffic. Instead, traffic will arrive at the member on the same port it arrived on at the listener.

Before using this feature on a load balancer, update client applications that integrate with it to check the `port_min` and `port_max` properties on the [load balancer listener](/apidocs/vpc/latest#get-load-balancer-listener). If those properties do not have the same value, the client must consider the inclusive range between `port_min` and `port_max` as the possible ports on which traffic can arrive at the member. However, if `port_min` and `port_max` have the same value, the behavior will be unchanged and traffic will arrive on the port specified by the `port` property of the load balancer pool member.

## 1 February 2022
{: #1-february-2022}

### For all version dates
{: #1-february-2022-all-version-dates}

**Bare metal servers for VPC.** You can now create bare metal servers to host VMware&reg; clusters in {{site.data.keyword.vpc_short}}. You can set up VMware management applications and create VMware virtual machines on the bare metal servers. The new [bare metal server](/apidocs/vpc/latest#list-bare-metal-servers) APIs use a similar structure and employ the same concepts as the existing [instance](/apidocs/vpc/latest#list-instances) APIs. There is also a parallel but separate set of [bare metal server profile](/apidocs/vpc/latest#list-bare-metal-server-profiles) APIs with similar conventions to the existing [instance profile](/apidocs/vpc/latest#list-instance-profiles) APIs. After you've learned one concept, it will apply to the other. However, be aware that there are some [limitations](/docs/vpc?topic=vpc-known-issues#bare-metal-servers-limitations).

For more information, see [About Bare Metal Servers for VPC](/docs/vpc?topic=vpc-about-bare-metal-servers) and [Bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=api), or dive into the new [API methods](/apidocs/vpc/latest#list-bare-metal-server-profiles).

## 25 January 2022
{: #25-january-2022}

### For all version dates
{: #25-january-2022-all-version-dates}

**Security groups for endpoint gateways.** For enhanced security, you can now associate security groups with [endpoint gateways](/docs/vpc?topic=vpc-about-vpe). When you [create an endpoint gateway](/apidocs/vpc/latest#create-endpoint-gateway), you can now specify the `security_groups` property, which associates those security groups with the endpoint gateway. If you do not specify `security_groups`, the endpoint gateway will be associated with the VPC's default security group. Before using the default security group, review your default security group rules and, if necessary, edit the rules to accommodate your endpoint gateway traffic.

Responses that return an endpoint gateway now include the `security_groups` property. On endpoint gateways created before 25 January 2022, the `security_groups` property in the response is an empty array (`[]`), and no security groups are set.

You can update security groups for an endpoint gateway by [adding an endpoint gateway](/apidocs/vpc/latest#create-security-group-target-binding) to or [removing an endpoint gateway](/apidocs/vpc/latest#delete-security-group-target-binding) from a security group's targets.

You will not be able to remove the only remaining security group from an endpoint gateway. As a result, if you add a security group to an endpoint gateway which had no security groups, you will not be able to revert the endpoint gateway to have no security groups.

Finally, the security group `targets` property can now refer to an endpoint gateway, as can the responses for the [get security group target](/apidocs/vpc/latest#get-security-group-target) and [list security group target](/apidocs/vpc/latest#list-security-group-targets) methods.

**Snapshots for VPC.** A `captured_at` property has been added to each [snapshot](/apidocs/vpc/latest#list-snapshots), indicating the date and time when the snapshot was captured from the volume. The `captured_at` timestamp value is a close approximation to the actual snapshot time, typically within a few seconds. The actual snapshot capture is between the `created_at` and `captured_at` timestamps. (The `created_at` property indicates when the [snapshot creation](/apidocs/vpc/latest#create-snapshot) process was initiated.)

If `captured_at` is absent from the response, the snapshot's data has not yet been captured. Additionally, the property may be absent for snapshots created before 1 January 2022.

## 23 November 2021
{: #23-november-2021}

### For all version dates
{: #23-november-2021-all-version-dates}

**Snapshots for VPC.** Restrictions have been removed for deleting snapshots. You can now [delete](/apidocs/vpc/latest#delete-snapshot) any snapshot in the chain of snapshots.  If the snapshot is actively being used to restore a volume, the snapshot will remain in `deleting` until the restore completes. The `deletable` property, which indicated whether a snapshot could be deleted, has been deprecated.

## 19 October 2021
{: #19-october-2021}

### For all version dates
{: #19-october-2021-all-version-dates}

**GPU instances.** Updated instance and instance profile methods now include details about GPUs attached to the instance. New profiles provide support for GPUs. These GPUs provide accelerated computing to help you run workloads with more powerful compute capabilities.

The [list all instances](/apidocs/vpc/latest#list-instances) method returns a new `gpu` property with additional sub-properties: `count`, `manufacturer`, `model`, and `memory`. The [retrieve an instance profile](/apidocs/vpc/latest#get-instance-profile) method returns new properties: `gpu_count`, `gpu_manufacturer`, `gpu_model`, and `gpu_memory`. For more information, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

## 28 September 2021
{: #28-september-2021}

### For all version dates
{: #28-september-2021-all-version-dates}

**Route mode for VNF support for network load balancers.** [Network load balancers](/apidocs/vpc/latest#create-load-balancer) now support a new "route mode" enabling virtual network functions (VNFs) as back-end targets. A `route_mode` property has been added to the load balancer resource to indicate if the load balancer is in route mode. A `route_mode_supported` property has been added to the load balancer profile resource to indicate if the profile supports route mode. Presently, only network load balancer profiles support route mode.

The [Create load balancer](/apidocs/vpc/latest#create-load-balancer) and [Create load balancer listener](/apidocs/vpc/latest#create-load-balancer-listener) methods now accept properties `port_min` and `port_max`. You can request a load balancer listener for a single port by setting either the `port` property, or by setting the `port_min` and `port_max` properties to the same value. All load balancer listener responses now include `port_min` and `port_max` properties, with `port_min` matching the value of the existing `port` property.

When creating load balancers with route mode enabled, you must specify the listener's `port_min` value as `1`,  the `port_max` value as `65535`, and omit the `port` property. Other port range values are not currently supported, as noted in [Known issues](/docs/vpc?topic=vpc-known-issues).
{: note}

For more information, see [Creating a route mode Network Load Balancer for VPC](/docs/vpc?topic=vpc-nlb-vnf&interface=api).

## 7 September 2021
{: #7-september-2021}

### For all version dates
{: #7-september-2021-all-version-dates}

**Instance bandwidth.** New properties have been added to the [create](/apidocs/vpc/latest#create-instance) and [update](/apidocs/vpc/latest#update-instance) instance methods to allow adjustment to the amount of total bandwidth (in megabits per second) allocated exclusively to attached volumes. The range of acceptable volume bandwidth values depends on the selected instance profile. A new `total_volume_bandwidth` property, added to each [instance profile](/apidocs/vpc/latest#list-instance-profiles), provides the range of possible values, and the default value used when creating an instance. An increase in `total_volume_bandwidth` will result in a corresponding decrease to `total_network_bandwidth`.

The volume bandwidth allocated to your existing instances will be unaffected unless:

- The instance's `total_volume_bandwidth` is lowered
- The total bandwidth requested by the instance's attached volumes exceeds the amount already requested by its attached volumes

For more information about this feature, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

## 31 August 2021
{: #31-august-2021}

### For all version dates
{: #31-august-2021-all-version-dates}

**Block storage volumes:**

- **Adjustable IOPS.** To manage the performance the your data volumes attached to running virtual server instances, use the [update volume](/apidocs/vpc/latest#update-volume) method to specify a different tiered `profile` value, or a different `iops` value within the [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#custom) tier. For more information, see [Adjusting IOPS for block storage volumes](/docs/vpc?topic=vpc-adjusting-volume-iops&interface=api).

- **Expandable volumes.** You can now expand a secondary volume attached to a running virtual server instance. Use the `capacity` property in the [update volume](/apidocs/vpc/latest#update-volume) method to request a new volume capacity, up to 16 TB (depending on the volume's profile).  While the volume's capacity is being updated, the volume will remain available for use, but will have a `status` value of `updating`. For more information, see [Expanding block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

   If you expand an existing data volume, be aware that existing applications will be exposed to the new `updating` value. To avoid disruption, first check that your applications are written to gracefully handle unexpected `status` values.
   {: important}

## 24 August 2021
{: #24-august-2021}

### For all version dates
{: #24-august-2021-all-version-dates}

**Application load balancers.** Use the [HTTPS redirect feature](/docs/vpc?topic=vpc-load-balancers-about#https-redirect-listener) to redirect traffic from an HTTP load balancer listener to an HTTPS listener.

An HTTPS redirect can be configured on either [load balancer listeners](/docs/vpc?topic=vpc-load-balancers-about#https-redirect-listener) or [load balancer policies](/docs/vpc?topic=vpc-layer-7-load-balancing#layer-7-policy), or both.  An HTTPS redirect is configured on the listener using the new `https_redirect` property, and will be used if none of the listener policy's rules match (or if it has no rules). An HTTPS redirect is configured on a load balancer policy using the new policy `action` value of `https_redirect`, and will be used only when all of the policy's rules match.

If you configure an HTTPS redirect on a listener policy, be aware that existing applications will be exposed to the new `https_redirect` value. To avoid disruption, check that your applications are written to gracefully handle unexpected `action` values first.
{: important}

Additional API restrictions are enforced after an HTTPS redirect is configured:

- You will not be able to [update the `protocol` and `accept_proxy_protocol` properties](/apidocs/vpc/latest#update-load-balancer-listener) of the HTTP and HTTPS listeners. Instead, delete the listener and create a new listener with the new property values.
- You will not be able to [delete an HTTPS listener](/apidocs/vpc/latest#delete-load-balancer-listener) until the HTTP listener referring to it is deleted.

## 17 August 2021
{: #17-august-2021}

### For all version dates
{: #17-august-2021-all-version-dates}

**Larger size boot volumes for custom images.** You can import custom images with a boot disk size from 10 GB to 250 GB, which will become the image's `minimum_provisioned_size` after import. When you specify the image as part of [creating an instance](/docs/vpc?topic=vpc-creating-virtual-servers), the boot volume `capacity` is set to the image's `minimum_provisioned_size`. For details, see [Planning custom images](/docs/vpc?topic=vpc-planning-custom-images).

**Placement groups.** Placement groups for {{site.data.keyword.vpc_full}} are logical groupings of virtual server instances that can be configured to reduce the risk of correlated failures inherent in your physical environment, such as networking issues, power loss, or hardware failure. Define a placement group strategy for high-availability workloads, such as for host or power spread. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc) or dive into the new [API methods](/apidocs/vpc/latest#list-placement-groups).

## 10 August 2021
{: #10-august-2021}

### For all version dates
{: #10-august-2021-all-version-dates}

**LinuxONE (s390x processor architecture).** You can now [create virtual server instances](/apidocs/vpc/latest#create-instance) on LinuxONE in {{site.data.keyword.Bluemix}} using new virtual server instance profiles. Instances provisioned with these profiles will have a VCPU architecture of s390x and interoperate with other VPC storage and networking features such as block storage volumes, floating IPs, and security groups. For more information, see [x86 instance profiles](/docs/vpc?topic=vpc-profiles&interface=api#balanced), and [Service limitations](/docs/vpc?topic=vpc-limitations).

## 27 July 2021
{: #27-july-2021}

### For version `2021-07-27` or later
{: #version-2021-07-27}

**Instances.** When you [create an instance](/docs/vpc?topic=vpc-creating-virtual-servers) using a `version` query parameter of `2021-07-27` or later, the value for the boot volume's `capacity` is now based on the image's `minimum_provisioned_size`. If you use a `version` query parameter of `2021-07-26` or earlier, the value for the boot volume's `capacity` will continue to be 100 GB.

## 29 June 2021
{: #29-june-2021}

### For all version dates
{: #29-june-2021-all-version-dates}

**Keys.** Pagination has been added to the [List all keys](/apidocs/vpc/latest#list-keys) method. Pagination will not occur until your account includes more than 50 keys in a region, but we recommend that you update your existing client applications in preparation. Contact IBM support if you need assistance.

## 15 June 2021
{: #15-june-2021}

### For all version dates
{: #15-june-2021-all-version-dates}

**Load balancer pools.** New cookie-based values have been added to the `session_persistence` enumeration returned by the load balancers pool methods. If you [create](/apidocs/vpc/latest#create-load-balancer-pool) or [update](/apidocs/vpc/latest#update-load-balancer-pool) pools with these new values to enforce session persistence, client applications will expose cookie values in all requests. For details, see [Cookie-based session persistence](/docs/vpc?topic=vpc-advanced-traffic-management#cookie).

## 8 June 2021
{: #8-june-2021}

### For version `2021-06-08` or later
{: #version-2021-06-08}

**Load balancers.** For requests using a `version` query parameter of `2021-06-08` or later, you can now use pagination when [listing all load balancers](/apidocs/vpc/latest#list-load-balancers) in the region. Requests using a `version` query parameter of `2021-06-07` or earlier remain unpaginated, but may time out if you have many load balancers.

If you expect to use many load balancers at once, migrate your applications to the paginated API to improve responsiveness and reliability. Contact [IBM support](/docs/get-support?topic=get-support-using-avatar) if you need help migrating your existing client applications.

## 25 May 2021
{: #25-may-2021}

### For all version dates
{: #25-may-2021-all-version-dates}

**Image from volume.** On a `POST /images` request, you can now specify `source_volume` with an instance boot volume identity. Specifying the `encryption_key` property in that request encrypts the image with a root key of your choosing. For details, see [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv&interface=api#image-from-volume-vpc-api).

## 18 May 2021
{: #18-may-2021}

### For all version dates
{: #18-may-2021-all-version-dates}

**Snapshots for VPC.** Use the new regional snapshot service to create point-in-time copies of your block storage boot or data volumes. Select a snapshot during instance provisioning and restore a new, fully-provisioned boot volume to start the instance. You can also create and attach a data volume from a snapshot within a running virtual server instance. Learn about [creating and using snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) and explore the new [API methods](/apidocs/vpc/latest#delete-snapshots).

## 6 May 2021
{: #6-may-2021}

### For all version dates
{: #6-may-2021-all-version-dates}

**Scheduled scaling.** Use scheduled scaling for VPC to schedule actions that automatically add or remove instance group capacity, based on daily, intermittent, or seasonal demand. You can create multiple scheduled actions that scale capacity monthly, weekly, daily, hourly, or even every set number of minutes. Explore the instance group [managers methods](/apidocs/vpc/latest#list-instance-group-managers) and the new [manager actions methods](/apidocs/vpc/latest#list-instance-group-manager-actions).

## 30 March 2021
{: #30-march-2021}

### For all version dates
{: #30-march-2021-all-version-dates}

**Instance storage.** New instance profiles can now optionally include a set of solid state disks. These instance storage disks provide temporary storage to improve the performance of cloud-native workloads that need scratch space, large data caches, or data replicated across availability zones.

The following API methods have been added:

- [List all disks on an instance](/apidocs/vpc/latest#list-instance-disks) (`GET /instances/{instance_id}/disks`)
- [Retrieve an instance disk](/apidocs/vpc/latest#get-instance-disk) (`GET /instances/{instance_id}/disks/{id}`)
- [Update an instance disk](/apidocs/vpc/latest#update-instance-disk) (`PATCH /instances/{instance_id}/disks/{id}`)
- [List all disks on a dedicated host](/apidocs/vpc/latest#list-dedicated-host-disks) (`GET /dedicated_hosts/{dedicated_host_id}/disks`)
- [Retrieve a dedicated host disk](/apidocs/vpc/latest#get-dedicated-host-disk) (`GET /dedicated_hosts/{dedicated_host_id}/disks/{id}`)
- [Update a dedicated host disk](/apidocs/vpc/latest#update-dedicated-host-disk) (`PATCH /dedicated_hosts/{dedicated_host_id}/disks/{id}`)

API methods that return instance and dedicated host profiles now include a `disks` property with information about the storage capability (where present) of resources provisioned with those profiles.

API methods that return instances and dedicated hosts now include a `disks` property with information about the disks configured for those resources.

For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

**Virtual server instance console.** You can now access your instances by connecting to a VNC or serial console. Learn about [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console), and explore the new instance console API methods:

- [Create a console access token for an instance](/apidocs/vpc/latest#create-instance-console-access-token) (`POST /instances/{instance_id}/console_access_token`)
- [Retrieve the console WebSocket for an instance](/apidocs/vpc/latest#get-instance-console) (`GET /instances/{instance_id}/console`)

**Instance resize.** You can now resize an instance by providing the `profile` property in the API method `PATCH /instances/{id}` ([Update an instance](/apidocs/vpc/latest#update-instance)). For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance&interface=api).

## 23 March 2021
{: #23-march-2021}

### For all version dates
{: #23-march-2021-all-version-dates}

**New parameter-based rule types for an application load balancer.** When creating a load balancer listener policy rule, the `field` property may now be set to `query` or `body` to perform additional forms of layer 7 load balancing:

- `query` - Write layer 7 rules that use the query string to route traffic to a specific target. For a `query` type rule, `field` and `value` must be percent-encoded, same as the query string in the URL.
- `body` - If the body of the `POST` request uses form encoding (UTF-8), then you can create layer 7 rules to route traffic based on the parameter name and value in the body. The `Content-Type` in the request is ignored.

If you use these new rule types, be aware that existing client applications will be exposed to those new values in the existing  properties. To avoid disruption, check that client applications are written to gracefully handle unexpected values for these properties before using these new rule types for an application load balancer.
{: important}

## 19 March 2021
{: #19-march-2021}

### For all version dates
{: #19-march-2021-all-version-dates}

**Bring your own license.** You can now [bring your own license](/docs/vpc?topic=vpc-byol-vpc-about) (BYOL) for custom images that you create and import to IBM Cloud VPC. When you import a custom image, you can choose from new `byol` Red Hat Enterprise Linux (RHEL) and Windows operating systems.

A new `dedicated_host_only` property has been added to operating system resources. Any instance with a boot volume created from an image with `operating_system.dedicated_host_only` set to `true` must be placed on a dedicated host (or into a dedicated host group). Because Windows BYOL images have `dedicated_host_only` set to `true`, they must be placed on a dedicated host (or into a dedicated host group). There are no restrictions on placing instances using RHEL BYOL images.

Every operation that returns an `OperatingSystem` resource now includes a `dedicated_host_only` property.

## 9 March 2021
{: #9-march-2021}

### For all version dates
{: #9-march-2021-all-version-dates}

**Additional VPN for VPC IKEv2 encryption/hash/Diffie Hellman (DH) group support.** For enhanced security, VPN for VPC now supports SHA2-512 (a Secure Hash Algorithm) and DH group 19 (a 256-bit elliptic curve algorithm) to generate a symmetric key.

If you use these new algorithms, be aware that existing client applications will be exposed to those new values in the existing `authentication_algorithm` and `dh_group` properties. To avoid disruption, check that client applications are written to gracefully handle unexpected values for these properties before using these new algorithms.
{: important}

The following VPN for VPC methods have been updated:

- In IKE policies, the `authentication_algorithm` property now includes a `sha512` value, and the `dh_group` property includes a `19` value.
- In IPsec policies, the `authentication_algorithm` property now includes a `sha512` value, and the `pfs` property includes a `group_19` value.

**New VPN gateway property.** Each element of the existing VPN gateway `members` array now includes a `private_ip` property, which provides the IP address assigned to that VPN gateway member.

## 23 February 2021
{: #23-february-2021}

### For all version dates
{: #23-february-2021-all-version-dates}

**Application load balancer security group integration.** For enhanced security, application load balancers can now be associated with security groups. You can specify one or more security groups when you create the application load balancer, and associate security groups with your existing application load balancers. If you omit security groups during load balancer creation, the default security group for the VPC is used.

If you plan to use default security groups for new application load balancers, review your default security group rules. If necessary, edit the rules to accommodate your expected application load balancer traffic.
{: tip}

The following load balancer methods have been updated:

- [Create a load balancer](/apidocs/vpc/latest#create-load-balancer) (`POST /load_balancers`) can now accept a list of security groups
- [Get load balancer details](/apidocs/vpc/latest#get-load-balancer) (`GET /load_balancers/{id}`) now returns references to the security groups to which a load balancer is attached

New security group methods have been added for managing security group targets:

- [Attach a security group to a target network interface or load balancer](/apidocs/vpc/latest#create-security-group-target-binding) (`PUT /security_groups/{security_group_id}/targets/{id}`)
- [List targets attached to a security group](/apidocs/vpc/latest#list-security-group-targets) (`GET /security_groups/{security_group_id}/targets`)
- [Retrieve a target in a security group](/apidocs/vpc/latest#get-security-group-target) (`GET /security_groups/{security_group_id}/targets/{id}`)
- [Delete targets from a security group](/apidocs/vpc/latest#delete-security-group-target-binding) (`DELETE /security_groups/{security_group_id}/targets/{id}`)

Use the security group target methods to manage security group attachments to both load balancers and network interfaces. The original methods specific to network interfaces are now deprecated:

- `GET /security_groups/{security_group_id}/network_interfaces`
- `DELETE /security_groups/{security_group_id}/network_interfaces/{id}`
- `GET /security_groups/{security_group_id}/network_interfaces/{id}`
- `PUT /security_groups/{security_group_id}/network_interfaces/{id}`

For more information, see [Integrating an IBM Cloud Application Load Balancer for VPC with security groups](/docs/vpc?topic=vpc-alb-integration-with-security-groups).

**Bring Your Own IP (BYOIP) support for VPC.** VPC address prefixes are no longer restricted to [RFC-1918](https://datatracker.ietf.org/doc/html/rfc1918){: external} addresses. You must now configure VPCs that use both non-RFC-1918 addresses and have public connectivity (floating IPs or public gateways) using a custom route that contains the new `delegate_vpc` property. You must specify this property for destination CIDRs that are non-RFC-1918 compliant and outside of the VPC, such as for destinations that are reachable through {{site.data.keyword.dl_full}}, {{site.data.keyword.cloud_notm}} Transit Gateway, or VPC classic access.

The `delegate_vpc` property is not required if a VPC uses only RFC-1918 addresses or has no public connectivity.
{: note}

The following API methods have been updated:

- View the `delegate_vpc` property in the requests and responses for `/vpcs/{vpc_id}/routing_tables/{routing_table_id}/routes`.
- View reserved IP ranges in `POST /vpcs/{vpc_id}/address_prefixes`, which creates an address pool prefix.

## 27 January 2021
{: #27-january-2021}

### For all version dates
{: #27-january-2021-all-version-dates}

**Checksum (SHA256) for imported images.** When you import a custom image to {{site.data.keyword.vpc_short}}, you can now view the checksum that was generated for the image during the import operation. By generating a checksum for the image locally, and checking that the checksums match, you can verify the integrity of the imported image. For more information, see [Importing and validating custom images into VPC](/docs/vpc?topic=vpc-importing-custom-images-vpc&interface=api).

The `sha256` checksum is available in the `file` details in API method `GET /images/{id}`. See [Retrieve an image](/apidocs/vpc/latest#get-image).

## 19 January 2021
{: #19-january-2021}

### For all version dates
{: #19-january-2021-all-version-dates}

The quantity of memory for virtual server instance profiles is now provisioned in gibibytes (GiB), instead of gigabytes (GB). For example, creating a new `bx2-4x16` virtual server instance provisions the instance with 16 GiB (17,179,869,184 bytes), instead of 16 GB (16,000,000,000 bytes). Virtual server instances that have already been provisioned are not affected.

### For version `2021-01-19` or later
{: #version-2021-01-19}

For requests using a `version` query parameter of `2021-01-19` or later, memory for virtual server instances is now expressed in gibibytes (GiB), instead of gigabytes (GB). For example, the `memory` property returned from `GET /instances/{id}` now reports in GiB (truncated to a whole number).

## 16 December 2020
{: #16-december-2020}

### For all version dates
{: #16-december-2020-all-version-dates}

**Customer-managed encryption for block storage volumes and encrypted custom images.** When you disable or delete a customer root key (CRK) that is encrypting your block storage or custom image resources, the API displays a status of `unusable` for these resources, along with the reason codes `encryption_key_deleted` or `encryption_key_disabled`.

The `unusable` status appears in the following API methods:

- [List all volumes](/apidocs/vpc/latest#list-volumes) (`GET /volumes`)
- [Retrieve a specific volume](/apidocs/vpc/latest#get-volume) (`GET /volumes/{id}`)
- [List all images](/apidocs/vpc/latest#list-volumes) (`GET /images`)
- [Retrieve the specified image](/apidocs/vpc/latest#get-image) (`GET /images/{id}`)

For more information on key states and resource statuses, see [User actions that impact root key states and resource status](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-root-key-states).

**Dedicated hosts** are now supported in the VPC API. Learn more about using [dedicated hosts](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances&interface=api) and explore the new [API methods](/apidocs/vpc/latest#list-dedicated-host-groups).

## 20 November 2020
{: #20-november-2020}

### For all version dates
{: #20-november-2020-all-version-dates}

**Datapath log forwarding with {{site.data.keyword.la_full_notm}}** is now available for [IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancers#load-balancers). Data and health check logs are valuable for debugging, analysis, and maintenance purposes. With the datapath logging feature enabled, your load balancer forwards these logs to your account's [IBM Log Analysis](https://cloud.ibm.com/observe/logging){: external} dashboard.

View the `logging` property in the following API methods:

- [List all load balancers](/apidocs/vpc/latest#list-load-balancers) (`GET /load_balancers`)
- [Retrieve a load balancer](/apidocs/vpc/latest#get-load-balancer) (`GET /load_balancers/{id}`)

For more information, see [Datapath log forwarding with IBM Log Analysis](/docs/vpc?topic=vpc-datapath-logging#datapath-logging).

## 19 November 2020
{: #19-november-2020}

### For all version dates
{: #19-november-2020-all-version-dates}

**Support for ingress routing** is included as part of [routing tables](/apidocs/vpc/latest#list-vpc-routing-tables), which were released on 30 October 2020. Use [ingress routing](/apidocs/vpc/latest#create-vpc-routing-table) to control the policy for packets that are coming in to your VPC or one of its zones. The policy can vary, depending on the type of source and the destination IP address range.

Routing tables for the VPC API are the same for both egress and ingress routing, with the following additional properties that you can specify for ingress routing:

- `route_direct_link_ingress`
- `route_transit_gateway_ingress`
- `route_transit_gateway_ingress`

For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

## 13 November 2020
{: #13-november-2020}

### For version `2020-11-13` or later
{: #version-2020-11-13}

**Static-route-based VPN gateways** are now available for requests using a `version` query parameter of `2020-11-13` or later. For a [static-route-based VPN gateway](/apidocs/vpc/latest#create-vpn-gateway), virtual tunnel interfaces are created. Any traffic that is routed to these interfaces with [user-defined routes](/docs/vpc?topic=vpc-create-vpc-route) is encrypted. For more information, see [About VPN gateways](/docs/vpc?topic=vpc-using-vpn).

## 30 October 2020
{: #30-october-2020}

### For all version dates
{: #30-october-2020-all-version-dates}

- **Custom routing tables** are now supported in the VPC API. This feature controls where network traffic is directed on a per-subnet basis. Explore new API methods for [routing tables](/apidocs/vpc/latest#list-vpc-routing-tables) and [routes](/apidocs/vpc/latest#create-vpc-routing-table-route). This feature subsumes the [VPC routing API](/apidocs/vpc/latest#list-vpc-routes), which remains supported but is deprecated and might be removed in a future API release.

- **Virtual private endpoint gateways.** Use [virtual private endpoint gateways](/apidocs/vpc/latest#list-endpoint-gateways) to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choice, which is allocated from a subnet within your VPC. For more information, see [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

- **VPC network interfaces.** IP anti-spoofing checks had already been provided for enhanced security. However, certain use cases, such as having a virtual server instance act as a network gateway, require selective disabling of these checks. To accommodate these use cases, if you have the `is.instance.instance.ip-spoofing` IAM action, you can now enable the `allow_ip_spoofing` property when you [create a network interface](/apidocs/vpc/latest#create-instance-network-interface). Alternatively, toggle the property when you [update an existing network interface](/apidocs/vpc/latest#update-instance-network-interface). See also [About IP spoofing checks](/docs/vpc?topic=vpc-ip-spoofing-about).

- **Proxy protocol for application load balancers for VPC.** When you configure a load balancer [pool](/apidocs/vpc/latest#create-load-balancer-pool) to use proxy protocol, the pool will pass information about the client to a back-end pool member when a connection is opened.

    You can also configure a load balancer [listener](/apidocs/vpc/latest#create-load-balancer-listener) to accept proxy protocol information. This feature is useful when the client is, itself, a proxy (which, in turn, was connected to by the actual client) that supports the proxy protocol. This allows client information to be obtained and passed on to any pools that, themselves, have the proxy protocol enabled.

    For more information, see [Enabling proxy protocol](/docs/vpc?topic=vpc-advanced-traffic-management#proxy-protocol-enablement).

## 5 October 2020
{: #2020-10-05}

### For all version dates
{: #2020-10-05-all-version-dates}

**Encrypted images.** Use the VPC API to create your own image, encrypt it with your own key, and import it, encrypted, into {{site.data.keyword.cloud_notm}}. After you import the image, use it like any other image. If you use the image to provision an instance, its boot volume is encrypted using the image's root encryption key or another root encryption key of your choosing.

Dive into the APIs to [import an encrypted image](/apidocs/vpc/latest#create-image) and [provision an instance](/apidocs/vpc/latest#create-instance) from that encrypted image. See also [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image).

## 31 August 2020
{: #2020-08-31}

### For all version dates
{: #2020-08-31-all-version-dates}

**Network load balancers.** You can now use the [load balancers API](/apidocs/vpc/latest#list-load-balancer-profiles) to distribute traffic among multiple server instances within the same region of your VPC. To learn how to create and manage a network load balancer, see [About IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-network-load-balancers).

The network load balancers API is shared between {{site.data.keyword.cloud_notm}} application load balancers and network load balancers.
{: note}

## 25 August 2020
{: #2020-08-25}

### For all version dates
{: #2020-08-25-all-version-dates}

This API release supports the following changes:

- Create an instance group of a fixed size and use it as a stand-alone feature
- Use the instance group manager to manage the instance group, which can associate an auto scale policy with that group

For more information, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

The following new methods are available for [instance groups](/apidocs/vpc/latest#list-instance-groups):

- `GET` and `POST` for `/instance_groups`
- `DELETE`, `GET`, and `PATCH` for `/instance_groups/{id}`
- `DELETE` for `/instance_groups/{instance_group_id}/load_balancer`
- `GET` and `POST` for `/instance_groups/{instance_group_id}/managers`
- `DELETE`, `GET`, and `PATCH` for `/instance_groups/{instance_group_id}/managers/{id}`
- `GET` and `POST` for `/instance_groups/{instance_group_id}/managers/{instance_group_manager_id}/policies`
- `DELETE`, `GET`, and `PATCH` for `/instance_groups/{instance_group_id}/managers/{instance_group_manager_id}/policies/{id}`
- `GET` for `/instance_groups/{instance_group_id}/memberships`
- `DELETE`, `GET`, and `PATCH` for `instance_groups/{instance_group_id}/memberships/{id}`

You can also use the new [instance template](/apidocs/vpc/latest#list-instance-templates) feature independently of auto scale. For example, create a template, and then create instances from that template, without creating an instance group.

The following new endpoints are now available for instances:

- `GET` and `POST` for `/instance/templates`
- `GET`, `PATCH`, and `DELETE` for `/instance/templates/{id}`

`POST /instances` now supports a `source_template`.

## 23 July 2020
{: #2020-07-23}

### For all version dates
{: #2020-07-23-all-version-dates}

The [flow log collectors API](/apidocs/vpc/latest#list-flow-log-collectors) is now generally available.

## 22 July 2020
{: #2020-07-22}

### For all version dates
{: #2020-07-22-all-version-dates}

This API release supports the following enhancements for customer-managed encryption for block storage boot and data volumes:

- `POST /instances` -- [Create an instance](/apidocs/vpc/latest#create-instance) and new volume, encrypted, using your customer root key (CRK). CRKs are imported to {{site.data.keyword.cloud_notm}} or created in a key management service.
- `POST /volumes` -- Create an unattached data [volume](/apidocs/vpc/latest#create-volume), encrypted, using your CRK.

## 12 May 2020
{: #2020-05-12}

### For all version dates
{: #2020-05-12-all-version-dates}

Configure load balancer pool resources and their health monitors to use the HTTPS protocol. This enhancement enables end-to-end SSL encryption with HTTPS listeners, along with HTTPS health checks for increased availability. See the [load balancers API](/apidocs/vpc/latest#list-load-balancers).

## 1 May 2020
{: #2020-05-02}

### For all version dates
{: #2020-05-02-all-version-dates}

This API release supports the following changes:

- [Flow log collectors](/apidocs/vpc/latest#list-flow-log-collectors) API is available as beta
- `GET` /security_groups now supports `vpc.crn` and `vpc.name` filters

## 17 April 2020
{: #2020-04-17}

### For all version dates
{: #2020-04-17-all-version-dates}

{{site.data.keyword.cloud_notm}} interprets volume capacity units in gibibytes, but the API documentation used gigabytes. This issue is now resolved in the documentation.

## 10 April 2020
{: #2020-04-10}

### For all version dates
{: #2020-04-10-all-version-dates}

Usage recommendations are provided for the following load balancer properties:

- `protocol` property for load balancer pools
- `type` property for load balancer pool health monitors

The guidance notes that new values for these properties might be added in the future, and unexpected values are handled gracefully. See the [load balancers API](/apidocs/vpc/latest#list-load-balancers).

## 6 February 2020
{: #2020-02-06}

### For all version dates
{: #2020-02-06-all-version-dates}

Support is temporarily suspended for creating instances from an existing boot volume. This feature was available through the API only, with no CLI or UI support. In the interim, you must specify the `image` property when you call `POST /instances`.

You can still create instances that reference existing data volumes.
{: note}

## 3 December 2019
{: #2019-12-03}

### For all version dates
{: #2019-12-03-all-version-dates}

Device IDs are now shown when you retrieve an instance's volume attachments.

## 26 November 2019
{: #2019-11-26}

### For all version dates
{: #2019-11-26-all-version-dates}

This API release supports the following changes:

- [Network access control list (ACL)](/apidocs/vpc/latest#list-network-acls) methods
- Instance filtering by VPC

## 21 November 2019
{: #2019-11-21}

### For all version dates
{: #2019-11-21-all-version-dates}

A VPC’s cloud service endpoint source IPs now appear in output. Learn about [cloud service endpoint source addresses](/docs/vpc?topic=vpc-vpc-behind-the-curtain#cse-source-addresses) and how [DNS resolves shared cloud service endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc#dns-domain-name-system-resolver-endpoints).

## 5 November 2019
{: #2019-11-05}

### For all version dates
{: #2019-11-05-all-version-dates}

This API release supports the following changes:

- [Load balancers](/apidocs/vpc/latest#list-load-balancers) API is available as beta
- [VPN gateways](/apidocs/vpc/latest#list-vpn-gateways) are available as beta
- Pagination is now supported for [instances](/apidocs/vpc/latest#list-instances)
- Classic access to VPCs (also known as classic peering) is supported
