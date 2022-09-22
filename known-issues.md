---

copyright:
  years: 2018, 2022

lastupdated: "2022-09-22"


keywords: known issues, bugs, defects

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{: shortdesc}

## `resource_type` known issues
{: resource-type-known-issues}

**Issue**: Currently, not all operations that return responses with embedded `VPCReference` and `SubnetReference` schemas include the documented `resource_type` sub-property.

**Workaround**: Before developing a client that makes use of the `resource_type` property of the `VPCReference` or `SubnetReference` schemas, check that the property is included in the responses returned for the operations used by your client.

## Reserved IP known issues
{: #ip-known-issues}

The following issues apply to reserved IPs. These issues will be resolved in a future release.

**Issue:** Reserved IPs that are bound to VPN gateways, IKS worker nodes, or DNS service instances will appear as having no target (the `target` property is not included when retrieving the reserved IP resource).

As a result, such reserved IPs may appear to be unbound. Despite appearing to be unbound, these reserved IPs cannot be deleted until their target resource is deleted.

In the console, reserved IPs that are labeled "unbound" might be bound to a resource that can't be displayed.

**Issue:** The instance metadata API does not currently support reserved IPs.

**Workaround:** Continue to use the `primary_ipv4_address` property to retrieve the IP address for each network interface on an instance. See the [VPC Metadata API](/apidocs/vpc-metadata).

**Issue:** When you use the VPC API to [list floating IPs on a bare metal server network interface](/apidocs/vpc#list-bare-metal-server-network-interface-floating-), you might get an incomplete list of the floating IPs associated with the bare metal server network interface.

The floating IP associated with a bare metal network interface is not available before the network interface `status` is `available`.

**Workarounds:**
- Wait for the bare metal server network interfaces to be `available` before listing the floating IPs on the interfaces.
- [List all floating IPs](/apidocs/vpc#list-floating-ips) to view those associated with bare metal server interfaces that are not yet `available`.

## Instance metadata service Activity Tracker events issues
{: #instance-metadata-activity-tracker-event-known-issues}

**Issue:** Activity Tracker events for the metadata service are undergoing changes and might not match the [events as documented](/docs/vpc?topic=vpc-at-events#events-metadata). The documented events are still useful for audit purposes but must not be used for automation.

## Network load balancers fail if port settings fall outside the supported range
{: #nlb-port-range}

**Issue**: Cannot create network load balancers with specific port ranges

Currently, the `port_min` and `port_max` properties are supported only when routing mode is enabled, and only when the entire port range is specified (`port_min` of `1` and `port_max` of `65535`). Support for allowing an arbitrary port range to be specified is planned for a future release.

## Checksum not available for some public images
{: #RIOS-1410}

**Issue:** When you use the API or CLI to list images, some public stock images might not include a checksum. The checksum is for informational purposes only for stock images. No fix is available.

## Boot volume has larger minimum provisioned size when you create a custom image by using IFV
{: #boot-volume-larger-minimum-provisioned-size}

**Issue**: If your custom image is not encrypted and the image is under 100 GB virtual disk size, deploying that image to an instance and creating a custom image from that instance's boot volume (Image from a volume feature) results in a `minimum_provisioned_size` of 100 GB. No fix is available.

## Bare metal servers limitations
{: #bare-metal-servers-limitations}

**Issue:** Flow log collectors are not integrated with bare metal servers. As a result, if you create a flow log collector for a VPC, traffic that flows to and from bare metal servers in that VPC aren't logged.

**Issue:** Network load balancers are not integrated with bare metal servers. As a result, if you create a network load balancer, you can't target a bare metal server as a load balancer pool member target.

**Issue:** You can't delete a subnet when you delete a bare metal server. Wait ~2 minutes after bare metal deletion before you delete the subnet.

Because all bare metal profiles are VMware&reg; certified, the `supported_image_flags` image property and `required_image_flags` profile property that expressed this ability during the beta period are discontinued. These properties might still be visible to API and CLI consumers, but they aren't supported and must not be used. These properties will be removed entirely in a future release.
{: note}

## Backup for VPC (beta) known issues
{: #backup-as-a-service-known-issues}

The following issues are currently present in the Cloud Console with no known workaround.

**Issue:** Backup policy details show zero (0) as the number of applied resources, even though the backup is still applied to the list of volumes.

**Issue:** The block storage details page might not show all the matched backup policies if the volume has more than one user tag.

## VSI monitoring known issues
{: #vsi-monitoring-known-issues}

**Issue:** Volumes that are created from snapshots and volumes that are resized do not display metrics on the VSI monitoring console page or in the IBM Cloud Monitoring dashboard for "VPC VSI Gen 2 Overview". No known workaround.

## Virtual server instance Activity Tracker events known issues
{: #at-virtual-server-instances-known-issues}

**Issue:** AT event log entries are missing `target.resourceGroupId` for some actions related to virtual server instances, such as updating or creating a virtual server instance. Instead, the resource group ID might appear in either the `requestData` or `responseData` sections of the event.

## Custom images in a private catalog known issue
{: #custom-images-private-catalog-known-issues}

**Issue:** If you have imported one or more images into a virtual server image for VPC catalog product offering version and you edit that version, an additional version ending in "draft" is created. You can't provision an instance from this draft version. Draft versions might appear on the Virtual server instance creation page in the UI or in the output of the CLI command `ibmcloud is catalog-image-offering`.

## VPC property issue for Security and Compliance Center
{: #RCS-4957}

**Issue:** If you set a [config rule](/docs/vpc?topic=vpc-manage-security-compliance&interface=ui#govern-vpc) for a virtual server instance with the `metadata_service_enabled` property set to *is_false*, the compliance policy currently has no effect.
