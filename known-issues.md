---

copyright:
  years: 2018, 2024
lastupdated: "2024-05-09"


keywords: known issues, bugs, defects

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{: shortdesc}

## Network load balancer known issues
{: #network-load-balancer-known-issues}

**Issue:** When you create a listener for a network load balancer, you can specify a `protocol` of `tcp` or `udp`. However, each listener in the network load balancer must have a unique `port`. For network load balancer limitations, see [IBM Cloud Network Load Balancer for VPC limitations](/docs/vpc?topic=vpc-nlb-limitations).

## Site-to-site VPN gateway known issues
{: #vpc-vpn-gateway-known-issues}

### Updating the `peer.address` or `peer.fqdn` of a VPN gateway connection issue
{: #vpc-vpn-gateway-connection-update-peer-known-issue}

**Issue:** If the `local.ike_identities` and `peer.ike_identity` properties are not set explicitly when you created the VPN gateway connection, when you [update a VPN gateway connection](/apidocs/vpc/latest#update-vpn-gateway-connection) and specify either `peer.address` or `peer.fqdn` the property value will be changed to match the updated value, instead of being left unchanged. Conversely, if the `local.ike_identities` and `peer.ike_identity` properties are set explicitly when you created the VPN gateway connection, the values cannot be changed without deleting the VPN gateway connection.

## `resource_type` known issues
{: #resource-type-known-issues}

**Issue**: Currently, not all operations that return responses with embedded `VPCReference` and `SubnetReference` schemas include the documented `resource_type` sub-property.

**Workaround**: Before developing a client that makes use of the `resource_type` property of the `VPCReference` or `SubnetReference` schemas, check that the property is included in the responses returned for the operations used by your client.

## Reserved IP known issues
{: #ip-known-issues}

The following issues apply to reserved IPs. These issues will be resolved in a future release.

**Issue:** Reserved IP addresses that are bound to VPN gateways, IKS worker nodes, or DNS service instances will appear as having no target (the `target` property is not included when retrieving the reserved IP resource).

As a result, such reserved IP addresses may appear to be unbound. Despite appearing to be unbound, these reserved IP addresses cannot be deleted until their target resource is deleted.

In the console, reserved IP addresses that are labeled "unbound" might be bound to a resource that can't be displayed.

**Issue:** The instance metadata API does not currently support reserved IPs.

**Workaround:** Continue to use the `primary_ipv4_address` property to retrieve the IP address for each network interface on an instance. See the [VPC Metadata API](/apidocs/vpc-metadata).

**Issue:** When you use the VPC API to [list floating IP addresses on a bare metal server network interface](/apidocs/vpc/latest#list-bare-metal-server-network-interface-floating-), you might get an incomplete list of the floating IP addresses associated with the bare metal server network interface.

The floating IP associated with a bare metal network interface is not available before the network interface `status` is `available`.

**Workarounds:**
- Wait for the bare metal server network interfaces to be `available` before listing the floating IP addresses on the interfaces.
- [List all floating IPs](/apidocs/vpc/latest#list-floating-ips) to view those associated with bare metal server interfaces that are not yet `available`.

## Network load balancers fail if port settings fall outside the supported range
{: #nlb-port-range}

**Issue**: Cannot create network load balancers with specific port ranges

Currently, the `port_min` and `port_max` properties are supported only when routing mode is enabled, and only when the entire port range is specified (`port_min` of `1` and `port_max` of `65535`). Support for allowing an arbitrary port range to be specified is planned for a future release.

## Image known issues
{: #image-vpc-known-issues}

### Checksum not available for some public images
{: #RIOS-1410}

**Issue:** When you use the API or CLI to list images, some public stock images might not include a checksum. The checksum is for informational purposes only for stock images. No fix is available.

### Boot volume has larger minimum provisioned size when you create a custom image by using IFV
{: #boot-volume-larger-minimum-provisioned-size}

**Issue**: If your custom image is not encrypted and the image is under 100 GB virtual disk size, deploying that image to an instance and creating a custom image from that instance's boot volume (Image from a volume feature) results in a `minimum_provisioned_size` of 100 GB. No fix is available.

### Custom images in a private catalog known issue
{: #custom-images-private-catalog-known-issues}

**Issue:** If you have imported one or more images into a virtual server image for VPC catalog product offering version and you edit that version, an additional version ending in "draft" is created. You can't provision an instance from this draft version. Draft versions might appear on the Virtual server instance creation page in the UI or in the output of the CLI command `ibmcloud is catalog-image-offering`.

## Bare metal servers limitations
{: #bare-metal-servers-limitations}

**Issue:** Flow log collectors are not integrated with bare metal servers. As a result, if you create a flow log collector for a VPC, traffic that flows to and from bare metal servers in that VPC aren't logged.

**Issue:** Network load balancers are not integrated with bare metal servers. As a result, if you create a network load balancer, you can't target a bare metal server as a load balancer pool member target.

**Issue:** You can't delete a subnet when you delete a bare metal server. Wait ~2 minutes after bare metal deletion before you delete the subnet.

Because all bare metal profiles are VMware&reg; certified, the `supported_image_flags` image property and `required_image_flags` profile property that expressed this ability during the beta period are discontinued. These properties might still be visible to API and CLI consumers, but they aren't supported and must not be used. These properties will be removed entirely in a future release.
{: note}

## VSI monitoring known issues
{: #vsi-monitoring-known-issues}

**Issue:** Volumes that are created from snapshots and volumes that are resized do not display metrics on the VSI monitoring console page or in the IBM Cloud Monitoring dashboard for "VPC VSI Gen 2 Overview". No known workaround.

## Virtual server instance Activity Tracker events known issues
{: #at-virtual-server-instances-known-issues}

**Issue:** AT event log entries are missing `target.resourceGroupId` for some actions related to virtual server instances, such as updating or creating a virtual server instance. Instead, the resource group ID might appear in either the `requestData` or `responseData` sections of the event.

## Additional authorizations beyond those defined in the API specification
{: #api-spec-auth-known-issue}

**Issue:** Some API implementations have required authorizations that are different from the authorizations requirements that are defined in the [API specification](/apidocs/vpc/latest). The following table lists such APIs and the extra permissions that are required in addition to what is already defined in the specification. This table will be continually updated as these issues are resolved.

| API | Additional access requirements | Action name|
| ----------- | ---------------------------------- | -----------------------------------------|
| PATCH /instances/{instance-id}  | Dedicated Host Operator, Dedicated Host Group Operator | is.dedicated-host.dedicated-host-group.operate (conditional)  \n is.dedicated-host.dedicated-host.operate (conditional) |
| POST /instances | Subnet Editor| is.subnet.subnet.update (conditional) |
| POST /instances/{instance-id}/actions | Instance Editor | is.instance.instance.update |
| POST /instances/{instance-id}/volume\_attachments | Instance Editor | is.instance.instance.update |
| DELETE /instances/{instance-id}/volume\_attachments/{vol-attach-id} | Instance Editor | is.instance.instance.update |
| GET /network\_acls/{nacl-id} | VPC Viewer | is.vpc.vpc.read |
| POST /network\_acls/{nacl-id}/rules | VPC Viewer | is.vpc.vpc.read |
| GET /subnets/{subnet-id}/network\_acl | VPC Viewer | is.vpc.vpc.read |
| PUT /subnets/{subnet-id}/network\_acl | VPC Viewer | is.vpc.vpc.read |
| PATCH /floating\_ips/{fip-id} | Subnet Operator | is.subnet.subnet.operate |
{: caption="Table 1. API additional authorization requirements" caption-side="bottom"}

## Storage known issues
{: #storage-vpc-known-issues}

### Cross-regional copy array issue
{: #snapshots-CRC-known-issue}

**Issue:** When you create a snapshot or list details of a snapshot with the API, the copies array in the API response lists only the direct copies of the snapshot that you specified. If you create a copy of a copy, the second copy is not returned when you query the original snapshot.

### Fast restore snapshots with customer-managed encryption issue
{: #snapshots-fast-restore-known-issue}

**Issue:** When you restore a volume from a snapshot by using the fast restore feature and the encryption key of the snapshot and volume are different, and then you delete the snapshot encryption key from the key management service, the volume might become inaccessible when it's attached or reattached to the virtual server instance.

**Workaround:** To recover the snapshot encryption key, use [the key recovery procedure](/docs/key-protect?topic=key-protect-restore-keys). When the key is recovered, the volume becomes accessible.
