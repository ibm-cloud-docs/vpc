---

copyright:
  years:  2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Adding a network attachment (and VNI) to an existing target resource
{: #vni-attach-vni}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

You can attach a virtual network interface (VNI) to a file share mount, virtual server instance, or bare metal server.
{: shortdesc}

To attach an existing VNI to an existing instance, create a new network attachment in the instance. It will create a new interface in the guest OS of the instance.

## Before you begin
{: #vni-prerequisites-fs-target}

* You must have an Administrator IAM role permissions to configure IP spoofing and infrastructure NAT.
* Infrastructure NAT must be enabled (`true`).
* The target that you plan to attach the VNI to must have been created using VNIs, not child network interfaces.

## Attaching a virtual network interface to a file share mount
{: #add-network-fs}

To attach a VNI to file share, see [Creating file shares and mount targets](/docs/vpc?topic=vpc-file-storage-create&interface=ui) and [Mount targets for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-share-mount-targets).

## Attaching a virtual network interface to a virtual server instance
{: #add-network-vsi}

To attach a virtual network interface to a virtual server instance, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui) and [Adding a virtual network interface](/docs/vpc?topic=vpc-using-instance-vnics&interface=ui#add-virtual-network-interface).

## Attaching a virtual network interface to a bare metal server
{: #add-network-bm}

To attach a virtual network interface to a bare metal server, see [Creating bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=ui) and [Adding a virtual network interface](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers&interface=ui#bare-metal-vni).
