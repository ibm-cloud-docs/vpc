---

copyright:
  years: 2021
lastupdated: "2021-11-05"

keywords: bare metal servers, managing network interface, bare metal network interface, configure network interface, create network interface, delete network interface, associate network interface, update network interface

subcollection: vpc

---

{:beta: .beta}
{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing network interfaces for Bare Metal Servers on VPC (Beta)
{: #managing-nic-for-bare-metal-servers}

After you create a bare metal server, you can add new network interfaces or edit existing network interfaces. When you edit a network interface, you can change its name, associate or unassociate a floating IP address, or access the security group that is associated with an interface. For more information about the networking features of Bare Metal Server for VPC, see [Network of Bare Metal Server for VPC](/docs/vpc?topic=vpc-bare-metal-servers-network).
{: shortdesc}

Bare metal servers on VPC is a Beta feature that requires special approval. Contact your IBM Sales representative if you're interested in getting access. 
{: beta}

## Overview of bare metal server network interfaces
{: #overview-bare-metal-network-interfaces}

You can create two types of network interface on the bare metal servers - PCI interface and VLAN interface.

* A PCI interface is a physical network interface. The VLAN interface is a virtual interface that is associated with a PCI interface. The maximum number of PCI interfaces per bare metal server is eight.

*  The VLAN interface automatically tags traffic that is routed through it with the VLAN ID. Inbound traffic that is tagged with a VLAN ID is directed to the appropriate VLAN interface. The VLAN interface has its own security groups and doesn't inherit security groups from the PCI interface. You aren't limited on the maximum number of VLAN interfaces per bare metal server.

When you create a bare metal server, a primary PCI interface is created for you. Optionally, you can add one or more secondary PCI or VLAN interfaces. You can also add, update, or delete the network interfaces.

You can associate one or more floating IPs with a network interface. The multiple floating IPs feature enables the VMware&reg; NSX-T Data Center to assign floating IPs. For more information about associating floating IP, see [Associate floating IPs with a network interface](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers#add-fips-to-nic).

## Network interface configurations
{: #nic-configs}

You can specify the following configurations for both PCI and VLAN interfaces:

| Field | Value |
|-----|-----|
| Name | Name of the interface. |
| Subnet | Specify the subnet that the network interface is associated with. |
| Floating IP | After the network interface is created, you can associate one floating IP for external connectivity. |
| Primary IPv4 address | The primary IPv4 address of the network interface. If specified, it must be an available address on the network interface's subnet. If unspecified, an available address on the subnet is automatically selected. |
| Allow IP spoofing | Turning IP spoofing _off_ prevents source IP spoofing on an interface. Turning IP spoofing _on_ allows source IP spoofing. The default option is _off_. You must have the **Advanced Network Operator** IAM role to modify this configuration. |
| Enable infrastructure NAT | Turning on infrastructure NAT allows the VPC infrastructure to perform any needed NAT operations. If infrastructure NAT is off, the packet passes unmodified to and from the network interface, allowing the workload to perform NAT operations. The default option is _on_. You must have the **Advanced Network Operator** IAM role to modify this configuration. **Allow IP spoofing** must be turned off if **Enable infrastructure NAT** is turned _off_. |
| Security groups | You can select the security groups that are used to control the traffic for the network interface. For VLAN interfaces, you need to specify the following two configurations: 1. **Allow interface to float**: Decide whether the interface needs to float to any other server within the same resource group. If enabled, the interface automatically floats if the network detects a GARP or RARP on another bare metal server within the resource group. The default option is _off_. You can't change this configuration after the VLAN interface is created. 2. **VLAN ID**: You must specify the VLAN ID tag to use for all traffic on this VLAN interface. |
| Associated PCI interface | If more than one PCI interfaces are created on the bare metal server, you must select a PCI interface to associate to this VLAN interface. Make sure that you associate the VLAN interfaces with the same VLAN ID that is on a bare metal server with one subnet. You can't create two VLAN interfaces with the same ID in two subnets. However, you can associate VLAN interfaces with different VLAN ID with one subnet. |
| Allowed VLANs (PCI interface only) | Specify the VLAN IDs of the VLAN interfaces that can use the PCI interface. |
{: caption="Table 1. Bare metal server network interface configurations" caption-side="top"}

You can't add the same VLAN ID to the Allowed VLANs lists of two PCI interfaces on a single bare metal server. You can't create a new PCI interface if it contains VLAN IDs that are specified in the Allowed VLANs list of any existing PCI interface.
{: note}

## Creating a network interface
{: #create-nic}

You can create one or more network interfaces on a bare metal server when you create the server. You can also add new interfaces to an existing bare metal server.

A VLAN interface must be associated with a PCI interface. When you use {{site.data.keyword.cloud}} UI to create VLAN interfaces, you must specify the **Associated PCI interface** field. If you use the CLI or API, you need to use the `allowed_vlans` property of the PCI interface to specify the VLAN IDs that can be associated with it. Then, when you create VLAN interfaces, the ID of the VLAN interface must be added to the `allowed_vlans` field of a PCI interface. Otherwise, the VLAN interface can't be created.
{: important}

### Creating a network interface during provisioning
{: #create-network-interface-bare-metal-provisioning}

To create a new interface or update the primary PCI network interface while you create a bare metal server, use the following steps. 

1. On the **New bare metal server for VPC** page, go to the **Network interfaces** section.

2. Click **New interface** to create a secondary interface.

3. Click the edit icon of the primary PCI network interface to edit the primary interface.

### Creating a network interface for an existing bare metal server
{: #create-network-interface-existing-bare-metal}

To add a network interface to an existing bare metal server, do the following step.

1. In the **Network interfaces** section of the **Bare metal server details** page, click **New interface** to create a new interface.

PCI interfaces can be only created or deleted when the bare metal server is in **Stopped** state.
{: important}

## Associating floating IPs with a network interface
{: #add-fips-to-nic}

You can associate one or more floating IPs with one network interface. 

To associate multiple floating IPs to a network interface, make sure that both **Allow IP spoofing** and **Enable infrastructure NAT** are off on the network interface.
{: important}

Follow these steps to associate a floating IP with the network interface:

1. Under the **Network interfaces** section of the **Bare metal server details** page, identify the interface that you want to associate the floating IP with.

2. Click the edit icon of the target interface.

3. On the **Edit network interface** page, locate the **Floating IP address** field, check the floating IPs that you want to associate with the network interface.

4. Click **Save** when you finish adding floating IPs.

You can disassociate the associated floating IP from the network interface.
{: tip}

## Updating a network interface
{: #update-nic}

After you created a network interface, you can change its **Enable infrastructure NAT** and **Allow IP spoofing** setting. You can also change the floating IPs that are attached to it. For PCI interface, updating its associated VLAN IDs is available.

Follow these steps to update the network interface:

1. Under the **Network interfaces** section of the ***Bare metal server details** page, identify the interface that you want to update.

2. Click the edit icon of the target interface.

3. Update the interface.

4. Click **Save**. 

## Deleting a network interface
{: #delete-nic}

You can delete the network interfaces that you created by clicking the delete icon of the network interface on the **Bare metal server details** page.

Keep the following information in mind when you delete network interfaces:

* Network interface deletion can't be reversed.
* The floating IPs that are associated with the network interface are implicitly disassociated.
* You can delete PCI interfaces only when the bare metal server is **Stopped**.
* You can't delete the primary network interface.
