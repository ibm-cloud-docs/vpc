---

copyright:
  years: 2018, 2022
lastupdated: "2022-03-31"

keywords: known issues, bugs, defects

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues
{: #known-issues}

Known issues might change over time, so check back occasionally.
{: shortdesc}

## Reserved IP known issues
{: #ip-known-issues}

The following issues apply to reserved IPs. These issues will be resolved in a future release.

**Issue:** The reserved IP functionality is not yet released for CLI. The following reserved IP functionality will be added to the CLI at a later date:

- Create instance with reserved IP
- Create bare-metal server with reserved IP

**Workaround** Until the GA version of the CLI is released, set the following environment variables.

```sh
export IBMCLOUD_IS_FEATURE_RESERVED_IP_PII=true
export IBMCLOUD_IS_FEATURE_BARE_METAL_SERVER=true
```
{: codeblock}

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

## Instance metadata service known issues
{: #instance-metadata-known-issues}

The following issues apply to the VPC API or Instance Metadata API. These issues will be resolved in a future release.

**Issue:** When you use the [VPC API](/apidocs/vpc) to manage instances, if the metadata service is disabled when an instance is created, and is subsequently enabled while the instance is running, the metadata service appear enabled but isn't fully functional for the running instance.

**Workaround:** After metadata service is enabled for the first time, use the VPC API to stop the instance. After the instance is stopped, start the instance. This workaround is necessary once only. The metadata service is enabled and disabled, as expected, after you take this action.

**Issue:** When you use instance templates in the VPC API, default trusted profile information is not included in the response. As a result, default trusted profile information does not appear in instance template information as expected. This issue occurs when you use the following methods:

- [Create an instance template](/apidocs/vpc#create-instance-template) and specify a default trusted profile
- [Retrieve an instance template](/apidocs/vpc#get-instance-template) to view its contents
- [Update an instance template](/apidocs/vpc#update-instance-template) to change the name of a template that has 'default_trusted_profile' information stored in its prototype.

**Workaround:** Do not use instance templates to create instances with a default trusted profile.

## Instance metadata service Activity Tracker events issues
{: #instance-metadata-activity-tracker-event-known-issues}

**Issue:** Activity Tracker events for the metadata service are undergoing changes and might not match the [events as documented](/docs/vpc?topic=vpc-at-events#events-metadata). The documented events are still useful for audit purposes but must not be used for automation.

## Network load balancers fail if port settings fall outside the supported range
{: #nlb-port-range}

**Issue**: Cannot create network load balancers with specific port ranges

Currently, the `port_min` and `port_max` properties are supported only when routing mode is enabled, and only when the entire port range is specified (`port_min` of `1` and `port_max` of `65535`). Support for allowing an arbitrary port range to be specified is planned for a future release.

## Virtual server instances must be stopped before they can be deleted
{: #API-1144}

**Issue:** The virtual server instances cannot be deleted.

**Workaround:** Stop the instance before you attempt to delete it.

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
