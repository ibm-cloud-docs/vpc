---

copyright:
  years: 2023
lastupdated: "2023-12-11"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# About virtual network interfaces
{: #vni-about}

This VPC feature is available only to accounts with special approval to preview it.
{: preview}

A virtual network interface (VNI) is a logical abstraction of a network interface in a subnet. It can be attached to a target resource, providing that resource with network connectivity. As a top-level resource with a CRN, a VNI's lifecycle is independent of the target resource it is attached to (unless `auto_delete` is set to `true`). In addition, it has its own set of IAM permissions.
{: shortdesc}

A VNI has the following properties that define networking policies:

* Primary IP and secondary IPs
* Security groups
* IP spoofing
* Infrastructure NAT

These policies are preserved when the VNI is detached from a target and attached to a different target. Permissions to change the properties are set on the VNI, and not on the VNI's target.

Floating IPs and flow log collectors are resources you can attach to a VNI. These attached resources remain attached to the VNI when the VNI switches attachment from one target to another. In addition:

* References to the floating IPs attached to a VNI are retrievable as a child collection of the VNI.
* A flow log collector attached to a VNI can be retrieved by listing your flow log collectors, filtering by `target.id`, then specifying the `id` of the VNI.

Not all supported target resources support all VNI policies. For more information, see [known limitations](/docs/vpc?topic=vpc-vni-known-issues).
{: note}

## Key benefits and capabilities
{: #key-benefits-capabilities}

Virtual network interface highlights include:

* The ability to assign multiple, secondary private IPv4 IP addresses from a VPC subnet.
* The option to define and enforce security groups and unique IAM policies to give you more control over network security.
* Scalability and flexibility for high-availability solutions.
* Existence without being attached to a server, and the option to move from one instance or resource to another, reducing failover time.
* The ability to attach floating IPs, primary/secondary reserved IPs, and security groups.

## Planning considerations
{: #vni-planning}

Review the following considerations before creating a VNI:

* Creating resources with child network interfaces will continue to be supported, but new networking features, like secondary IP addresses, are supported only on virtual network interfaces.
* Although an instance or bare metal server does not allow a mix of interface types, a subnet allows a mix of instances and bare metal servers -- some with child network interfaces and some with virtual network interfaces.
* You must configure the primary IP address when creating a virtual network interface. You can configure additional (secondary) reserved IP addresses when creating the virtual network interface, or you can later add secondary IP addresses individually. All secondary IP addresses must be in the same subnet as the primary IP address.
* You can configure one floating IP address on a virtual network interface that has infrastructure NAT enabled. A virtual network interface configured this way can be used by an instance or a bare metal server.
* For a virtual network interface that allows IP spoofing and has infrastructure NAT disabled, you can configure multiple floating IP addresses. A virtual network interface configured this way can only be used by a bare metal server.
* All IP addresses on a virtual network interface must be in the same subnet.
* If a virtual network interface's `auto-delete` property is set to `false`, if you then delete the virtual server instance, the virtual network interface will remain. The virtual network interface can then be attached to a different target resource.
* After a VNI detaches from a target, you can reattach it to one of the following:

   * A new share mount target (if ip-spoofing is disabled, there no secondary IPs, and infrastructure NAT is set to `true`)
   * A new network attachment on an instance (if infrastructure NAT is set to `true`)
   * A new network attachment on a bare metal server (when bare metal is supported)

## Support for old API clients
{: #vni-old-api-clients}

For compatibility with old clients, the API response provides a read-only representation of each network attachment and its associated virtual network interface as a child network interface:

* When retrieving an [instance network interface](/apidocs/vpc#get-instance-network-interface), the properties are derived as follows:

| Network interface property | Mapped from                                                                               |
|:---------------------------|:------------------------------------------------------------------------------------------|
| `allow_ip_spoofing`        | `virtual_network_interface.allow_ip_spoofing`                                             |
| `created_at`               | `network_attachment.created_at`                                                           |
| `floating_ips`             | `virtual_network_interface.floating_ips`                                                  |
| `href`                     | `network_attachment.href` (`/network_attachments` is replaced with `/network_interfaces`) |
| `id`                       | `network_attachment.id`                                                                   |
| `name`                     | `network_attachment.name`                                                                 |
| `port_speed`               | `network_attachment.port_speed`                                                           |
| `primary_ip`               | `virtual_network_interface.primary_ip`                                                    |
| `resource_type`            | "network_interface"                                                                       |
| `security_groups`          | `virtual_network_interface.security_groups`                                               |
| `status`                   | See the "Instance network interface status values" table                                  |
| `subnet`                   | `virtual_network_interface.subnet`                                                        |
| `type`                     | `network_attachment.type`                                                                 |
{: caption="Table 1: Instance network interface properties mapping" caption-side="bottom"}

The `status` value is determined according to the following table:

|     Network Attachment `lifecycle_state`      |             VNI `lifecycle_state`             | `status`    |
|:---------------------------------------------:|:---------------------------------------------:|:------------|
|                  `deleting`                   |                     _any_                     | `deleting`  |
|                     _any_                     |                  `deleting`                   | `deleting`  |
|                   `failed`                    |              _any except_ `deleting`          | `failed`    |
|             _any except_ `deleting`           |                   `failed`                    | `failed`    |
| `pending`, `suspended`, `updating`, `waiting` |     _any except_ `deleting` _or_ `failed`     | `pending`   |
|       _any except_ `deleting` _or_ `failed`   | `pending`, `suspended`, `updating`, `waiting` | `pending`   |
|                   `stable`                    |                   `stable`                    | `available` |
{: caption="Table 2: Instance network interface status values" caption-side="bottom"}

* The same properties are derived when retrieving a [bare metal server network interface](/apidocs/vpc#get-bare-metal-server-network-interface), in addition to the following properties that are specific to bare metal server network interfaces:

| Network interface property  | Mapped from                                           |
|:----------------------------|:------------------------------------------------------|
| `enable_infrastructure_nat` | `virtual_network_interface.enable_infrastructure_nat` |
| `mac_address`               | `virtual_network_interface.mac_address`               |
| `interface_type`            | `network_attachment.interface_type`                   |
| `allowed_vlans`             | `network_attachment.allowed_vlans`                    |
| `vlan`                      | `network_attachment.vlan`                             |
| `allow_interface_to_float`  | `network_attachment.allow_interface_to_float`         |
{: caption="Table 3: Bare metal server network interface properties mapping" caption-side="bottom"}

## Getting started with virtual network interfaces
{: #vni-getting-started}

You can use a VNI to manage the IP addresses and security groups in a separate resource with a lifecycle that is independent of your target resource.

1. Ensure that you have a VPC and subnet attached. For more information, see creating VPC resources [using the IBM Cloud console](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console) or [creating with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api).
1. Review [planning considerations](/docs/vpc?topic=vpc-vni-planning) and any [known issues and limitations](/docs/vpc?topic=vpc-vni-known-issues).
1. Ensure that you have the correct [IAM permissions](/docs/vpc?topic=vpc-vni-iam) to create a VNI.
1. [Create a virtual network interface](/docs/vpc?topic=vpc-vni-create&interface=ui) with a private IP address, a public IP address, and security groups.
1. Attach your VNI to a supported target resource when provisioning the target. Currently, there are three supported target types:

   * Virtual server instance
      * When provisioning an instance, attach the VNI to the primary network attachment child resource. See [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).
      * If you have an existing virtual server instance with a primary network attachment, you can provision an additional network attachment on the virtual server instance, attaching the VNI to the new network attachment. See [Creating network attachments](/docs/vpc?topic=vpc-using-instance-vnics&interface=ui#add-virtual-network-interface).
   * Bare metal server
      * When provisioning a bare metal server, attach the VNI to the primary network attachment child resource. See [Creating Bare Metal Servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).
      * If you have an existing bare metal server with a primary network attachment, you can provision an additional network attachment on the bare metal server, attaching the VNI to the new network attachment. See [Creating network attachments](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers&interface=ui#bare-metal-vni).
   * File share mount
      * When provisioning a new file share mount, attach the VNI to the file share mount target. See [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create&interface=ui) and [Mount targets for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets).

It's also possible to create a VNI in the context of provisioning each of these targets. In other words, you don't need to create a VNI ahead of time. Later, you can delete the instance and preserve the VNI by ensuring that `--auto-delete` is `false` (**Auto release** switch). Then, you can create a new instance using the same VNI.

## Use case 1: High availability with virtual network interfaces
{: #vni-ha-use-case}

Virtual network interfaces reduce the time required to switch to a failover instance. In a virtual server instance that doesn't use virtual network interfaces, a failover requires routing changes that can take several minutes to propagate. In the following example, you have a primary virtual server instance configured with a network interface, and a backup virtual server instance that has its own network interface. If the primary virtual server instance fails, you must bring the backup virtual server instance online, and reconfigure your routes to use the backup instance's network interface.

![Traditional method of using network interfaces](/images/vni-use-case2.svg "Traditional network interface workflow"){: caption="Figure 1. Traditional network interface workflow" caption-side="bottom"}

By using a virtual network interface, you can move it from one instance to another. The independent lifecycle of the virtual network interface means that it keeps its IP address and you don't have to reconfigure your routes, reducing failover time to seconds.

![Virtual network interfaces method](/images/vni-use-case1.svg "Virtual network interface workflow"){: caption="Figure 2. Virtual network interface workflow" caption-side="bottom"}

## Use case 2: Secondary IP addresses with virtual network interfaces
{: #vni-secip-use-case}

A virtual server instance that runs several instances of an application can be segmented such that each instance of the application has its own IP address.

For this example, a single virtual server instance is running three instances of a SQL database application. Each SQL instance needs to have its own IP address. By using secondary IP addresses on a virtual network interface, you can assign different IP addresses to each application instance.

![Secondary IP addresses in a virtual network interface](/images/vni-use-case3.svg "Virtual network interface using secondary IP addresses"){: caption="Figure 2. Secondary IP addresses in a virtual network interface" caption-side="bottom"}

## Related links
{: #vni-related-links}

These links provide additional information about virtual network interfaces for VPC:

* [IAM permissions](/docs/vpc?topic=vpc-vni-iam)
* [Activity tracker events](/docs/vpc?topic=vpc-at-events&interface=ui#events-vni)
* [Quotas](/docs/vpc?topic=vpc-quotas&interface=ui#virtual-network-interfaces-quotas)
* [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_virtual_network_interface){: external}
* [Troubleshooting](/docs/vpc?topic=vpc-troubleshoot-vni-1)
