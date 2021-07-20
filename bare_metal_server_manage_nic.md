---

copyright:
  years: 2021
lastupdated: "2021-07-20"

keywords: bare metal servers, managing network interface, tutorial

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

# Managing network interfaces for a bare metal server
{: #managing-nic-for-bare-metal-servers}

This document walks through how to manage network interfaces of the bare metal servers. For information about the networking features of Bare Metal Server for VPC and how it supports VMware networking scenarios, see [Network of Bare Metal Server for VPC](/docs/vpc?topic=vpc-bare-metal-servers-network).

You can create 2 types of network interface on the bare metal servers: PCI interface and VLAN interface.

The PCI interface is a physical network interface. The VLAN interface is a virtual interface that is associated with a PCI interface.

The VLAN interface automatically tags traffic that is routed through it with the VLAN ID. Inbound traffic tagged with a VLAN ID is directed to the appropriate VLAN interface. 

<!--
The VLAN interface has its own security groups and does not inherit those of the PCI interface through which traffic flows.
{: note}-->

When creating a bare metal server, a primary PCI interface will be created by default. Optionally, you can add one or more secondary PCI or VLAN interfaces. You can also add, update, or delete the network interfaces on the bare metal server after the server was created.

The maximum number of PCI interfaces per bare metal server is 8. There is no limitation on the maximum number of VLAN interfaces per bare metal server.
{:tip}

You can associate one floating IP with a network interface. For more information, see [Associate floating IP with a network interface](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers#add-fips-to-nic).
{:tip}

## Network interface configurations
{: #nic-configs}

You can specify the following configurations for both PCI and VLAN interfaces:

* **Name**: Name of the interface.

* **Subnet**: You must specify the subnet that the network interface is associated with.

* **Floating IP**: After the network interface is created, you can associate one floating IP with a it for external connectivity.

* **Primary IPv4 address**: The primary IPv4 address of the network interface. If specified, it must be an available address on the network interface's subnet. If unspecified, an available address on the subnet will be automatically selected.

<!--
* **Allow IP spoofing**: Turning this off will prevent source IP spoofing on an interface. Turning it on will allow source IP spoofing. The default option is off. **Note**: You must be assigned the **Advanced Network Operator** IAM role to specify or modify this configuration.
* **Enable infrastructure NAT**: Turning this on allows the VPC infrastructure to perform any needed NAT operations. If off, the packet would be passed unmodified to/from the network interface, allowing the workload to perform any needed NAT operations. The default option is on. **Note**: You must be assigned the **Advanced Network Operator** IAM role to specify or modify this configuration.
* **Security groups**: You can select the security groups that are used to control the traffic for the bare metal server.
-->

<!--Multiple FIP support: nic needs to turn on **Allow IP spoofing** and turned off **Enable infrastructure NAT**.-->

In addition, for VLAN interfaces, you need to specify the following 2 configurations: 

* **Allow interface to float**: Decide if the interface needs to be able to float to any other server within the same resource group. If enabled, the interface will float automatically if the network detects a GARP or RARP on another bare metal server within the resource group. The default option is off. **Note:** This configuration cannot be changed after the VLAN interface was created.

* **VLAN ID**: You must specify the VLAN ID tag that will be used for all traffic on this VLAN interface.

* **Associated PCI interface**: If more than one PCI interfaces are created on the bare metal server, you must select a PCI interface this VLAN interface is associated with.

You should always associate the VLAN interfaces with the same VLAN ID on a bare metal server with one subnet. If you create two VLAN interfaces with the same ID in two different subnets, the interfaces may not be working properly. However, you can associate VLAN interfaces with different VLAN ID with one subnet.
{:important}

For PCI interface, you need to specify the following:

* **Allowed VLANs**: Specify the VLAN IDs of the VLAN interfaces that can use this PCI interface. 

You cannot add the same VLAN ID to the Allowed VLANs lists of two PCI interfaces on a single bare metal server. A new PCI interface cannot be created if it contains VLAN ID(s) that have already been specified in the Allowed VLANs list of any existing PCI interface.
{: note}

A VLAN interface must be associated with a PCI interface. When using IBM Cloud UI to create VLAN interfaces, you must specify the **Associated PCI interface** field. If you use IBM Cloud CLI or REST API, you need to use the `allowed_vlans` property of the PCI interface to specify the VLAN IDs that can be associated with it. Then, when creating VLAN interfaces, the ID of the VLAN interface must have been added to the `allowed_vlans` field of a PCI interface. Otherwise, the VLAN interface couldn't be created.
{:important}

## Creating a network interface
{: #create-nic}

You can create one or more network interfaces on a bare metal server while creating the server. You can also add new interfaces to an existing bare metal server.

To create new interfaces or update the primary PCI network interface while creating the bare metal server: 

1. On the **New bare metal server for VPC** page, locate the **Network interfaces** section.

2. Click **New interface** to create a secondary interface.

3. Click the edit icon of the primary PCI network interface to edit the primary interface.

To add a new interface to an existing bare metal server:

In the **Network interfaces** section of the **Bare metal server details** page, click **New interface** to create a new interface.

PCI interfaces can only be created or deleted when the bare metal server is in **Stopped** state.
{:important}

## Associating floating IP with a network interface
{: #add-fips-to-nic}

<!--Multiple FIP support: You can associate one or more floating IPs with one network interface. The multiple floating IP function enables VMware’s NSXT-0 to perform the NAT and handoff public IPs to NSX-T enabled VMs. To associate multiple floating IPs to a network interface, ensure that **Allow IP spoofing** has been turned on and **Enable infrastructure NAT** has been turned off on the network interface.{:important}-->

Follow the steps below to associate a floating IP with the network interface:

1. Under the **Network interfaces** section of the **Bare metal server details** page, identify the interface that you want to associate the floating IP with.

2. Click the edit icon of the target interface.

3. On the **Edit network interface** page, locate the **Floating IP address** field, select **Reserve a new floating IP** or select an existing floating IP address.

4. Click **Save**

<!-- Multiple FIP support:
4. Repeat the Step 3 to add more floating IPs.
5. Click **Save** when you have finished adding floating IPs.-->

The associated floating IP can be disassociated from the network interface.
{:tip}

## Updating an existing interface
{: #update-nic}

After you have created a network interface, you can change the floating IP attached to it. For the PCI interface, you can also update the VLAN IDs associated with it.

Follow the steps below to update the network interface:

1. Under the **Network interfaces** section of the ***Bare metal server details** page, identify the interface that you want to update.

2. Click the edit icon of the target interface.

3. Update the interface.

4. Click **Save**. 

## Deleting a network interface
{: #delete-nic}

You can delete the network interfaces that have been created by clicking the delete icon of the network interface on the **Bare metal server details** page.

Notes on deleting network interfaces:

* This operation cannot be reversed.
* The floating IP associated with the network interface are implicitly disassociated.
* PCI interfaces can only be deleted when the bare metal server is in **Stopped** state.
* The primary network interface is not allowed to be deleted.
