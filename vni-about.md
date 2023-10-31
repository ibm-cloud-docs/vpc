---

copyright:
  years: 2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}


# About virtual network interfaces
{: #vni-about}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}
  
A virtual network interface (VNI) is a logical abstraction of a network interface in a subnet. It can be attached to a target resource, providing that resource with network connectivity. A top-level resource with a CRN, a VNI's lifecycle is independent of the target resource it is attached to (unless `auto_delete` is set to `true`), and it has its own set of IAM permissions. 

A VNI has properties that define networking policies:

* Primary IP and secondary IPs
* Security groups
* IP spoofing
* Infrastructure NAT

These policies are preserved when the VNI is detached from a target and attached to a different target. Permissions to change the properties are set on the VNI, and not on the VNI's target.

Floating IPs and flow log collectors are resources that can be attached to a VNI. These attached resources remain attached to the VNI when the VNI is detached from or attached to a target.

* References to the floating IPs attached to a VNI are retrievable as a child collection of the VNI.
* The flow log collector attached to a VNI can be retrieved by listing your flow log collectors and filtering by `target.id`, specifying the `id` of the VNI.

Not all supported target resources support all VNI policies. For more information, see [known limitations](/docs/vpc?topic=vpc-vni-known-issues).
{: note}

## Key benefits and capabilities
{: #key-benefits-capabilities}

Virtual network interface highlights include:

* Assign multiple, secondary private IPv4 IP addresses from a VPC subnet.
* Define and enforce security groups and unique IAM policies to have more control over network security.
* Offers scalability and flexibility for high-availability solutions.
* Exist without being attached to a server, and can move from one instance or resource to another, reducing failover time.
* Ability to attach floating IPs, primary/secondary reserved IPs, and security groups.

## Planning considerations
{: #vni-planning}

Review the following considerations before creating a VNI:

* Creating resources with child network interfaces will continue to be supported, but new networking features, like secondary IP addresses, are supported only on virtual network interfaces.
* Although an instance or bare metal server does not allow a mix of interface types, a subnet allows a mix of instances and bare metal servers -- some with legacy interfaces and some with virtual network interfaces.
* You must configure the primary IP address when creating a virtual network interface. You can configure additional (secondary) reserved IP addresses when creating the virtual network interface, or you can later add secondary IP addresses individually. All secondary IP addresses must be in the same subnet as the primary IP address.
* You can configure one floating IP address on a virtual network interface that has infrastructure NAT enabled. A virtual network interface configured this way can be used by an instance or a bare metal server.
* For a virtual network interface that allows IP spoofing and has infrastructure NAT disabled, you can configure multiple floating IP addresses. A virtual network interface configured this way can be used only by a bare metal server.
* All IP addresses on a virtual network interface must be in the same subnet.
* If a virtual network interface's `auto-delete` property is set to `false`, when the virtual server instance is deleted, the virtual network interface remains. The virtual network interface can then be attached to a different target resource.
* After a VNI is detached from a target, you can attach it to one of the following:

   * A new share mount target (if ip-spoofing is disabled, no secondary IPs and infrastructure NAT is true)
   * A new network attachment on an instance (if infrastructure NAT is true)
   * A new network attachment on a bare metal server (when bare metal is supported)

## Getting started with virtual network interfaces
{: #vni-getting-started}

You can use a VNI to manage the IP addresses and security groups in a separate resource with a lifecycle that is independent of your target resource.

1. Ensure that you have a VPC and subnet attached. For more information, see creating VPC resources [using the IBM Cloud console](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console) or [creating with CLI and API](/docs/vpc?topic=vpc-creating-vpc-resources-with-cli-and-api).
1. Review [planning considerations](/docs/vpc?topic=vpc-vni-planning) and any [known issues and limitations](/docs/vpc?topic=vpc-vni-known-issues).
1. Ensure that you have the [correct IAM permissions](/docs/vpc?topic=vpc-vni-iam) to create a VNI.
1. [Create a virtual network interface](/docs/vpc?topic=vpc-vni-create&interface=ui) with a private IP address, a public IP address, and security groups.
1. Attach your VNI to a supported target resource when provisioning the target. Currently, there are three supported target types:

   * Virtual server instance
      * When provisioning an instance, attach the VNI to the primary network attachment child resource. See [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers).
      * If you have an existing virtual server instance with a primary network attachment, you can provision an additional network attachment on the virtual server instance, attaching the VNI to the new network attachment. See [Creating network attachments](/docs/vpc?topic=vpc-using-instance-vnics&interface=ui#add-virtual-network-interface).
   * Bare metal server
      * When provisioning a bare metal server, attach the VNI to the primary network attachment child resource. See [Creating Bare Metal Servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).
      * If you have an existing bare metal server with a primary network attachment, you can provision an additional network attachment on the bare metal server, attaching the VNI to the new network attachment. See [Creating network attachments](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers&interface=ui#bare-metal-vni).
   * File share mount

      When provisioning a new file share mount, attach the VNI to the file share mount target. See [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create&interface=ui) and [Mount targets for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets).

It's also possible to create a VNI in the context of provisioning each of these targets. In other words, you don't need to create a VNI ahead of time. Later, you can delete the instance and preserve the VNI by ensuring that `--auto-delete` is `false` (**Auto release** switch). Then, you can create a new instance using the same VNI.

## Related links
{: #vni-related-links}

These links provide additional information about virtual network interfaces for VPC:

* [IAM permissions](/docs/vpc?topic=vpc-vni-iam)
* [Activity tracker events](/docs/vpc?topic=vpc-at-events&interface=ui#events-vni)
* [Quotas](/docs/vpc?topic=vpc-quotas&interface=ui#virtual-network-interfaces-quotas)
* [Terraform registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_virtual_network_interface){: external}
* [Troubleshooting](/docs/vpc?topic=vpc-troubleshoot-vni-1)
