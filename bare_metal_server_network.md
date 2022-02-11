---

copyright:
  years: 2021, 2022
lastupdated: "2021-02-09"

keywords: bare metal server network, bare metal network, nics, pci, vlan, network overview

subcollection: vpc

---

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

# Networking overview for Bare Metal Servers on VPC
{: #bare-metal-servers-network}

The following information an overview of the networking features of Bare Metal Servers for VPC and how it supports VMware vSphere&reg; networking. It is recommended that you go through this information before you build a VMware network on bare metal servers.
{: shortdesc}

The following information is for customers with basic network knowledge of {{site.data.keyword.cloud}} VPC and VMware vSphere. If you aren't familiar with VPC networking, see [About networking](/docs/vpc?topic=vpc-about-networking-for-vpc). If you are not familiar with VMWare vSphere networking, see [Introduction to vSphere Networking](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.networking.doc/GUID-8CDF29B2-ABA8-4F34-9FEF-14987BC13265.html){: external}.

Bare Metal Server for VPC provides full support for VPC networking features. The network is fully software-defined, so you can configure it through the API.

Each bare metal server supports 100 Gbps bandwidth. The bandwidth is shared by the network interfaces that are on the bare metal server.

For more information about managing network interfaces, see [Managing network interfaces for a bare metal server](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers).

## Bare metal server network interfaces 
{: #bare-metal-servers-nics-intro}

You can create two types of network interfaces on a bare metal server: 
| Network interface | Description |
|-----|-----|
| PCI (peripheral component interconnect) | A physical network interface. |
| VLAN (virtual LAN) interface | A virtual network interface that is associated with a PCI interface through the VLAN ID. The VLAN interface automatically tags traffic that is routed through it with the VLAN ID. Inbound traffic that is tagged with a VLAN ID is directed to the appropriate VLAN interface. |
{: caption="Table 1. Types of network interfaces for bare metal servers" caption-side="bottom"}

The following diagram illustrates the relationship between PCI interfaces and VLAN interfaces.

![Figure showing bare metal server network interfaces](images/bare_metal_server_network_interface.png "Figure showing bare metal server network interfaces"){: caption="Figure 1. Network interfaces on a bare metal server" caption-side="bottom"}

### Characteristics of the PCI and VLAN interface
{: #bare-metal-servers-interface-characteristics}

The following list highlights characteristics of the PCI and VLAN interface.

* You can create up to 8 PCI interfaces on a bare metal server. To add or remove PCI interfaces, the bare metal server must be **STOPPED**.

* You don't have limitations on the maximum number of VLAN interfaces that can you can create.

* You can attach security groups to both PCI and VLAN interfaces to handle incoming and outgoing traffic to the network interface. The VLAN interface maintains its own security group rules that might be different from its associated PCI interface rules.

* All network interfaces are backed by two redundant physical ports that are on the TORs (top-of-rack) switch. {{site.data.keyword.cloud}} manages the aggregation, so you don't need to create multiple PCI interfaces for redundancy.

* Optionally, you can set VLAN interfaces to "floatable" to support vMotion between the bare metal servers on a VMware compatible network.

* You can associate more than one floating IP with a network interface. Having multiple floating IPs enables the VMware NSX-T Data Center to assign floating IPs to the VMware VMs.

## Mapping network concepts between Bare Metal Servers for VPC and VMware vSphere
{: #mapping-network-concepts}

Bare Metal Servers for VPC fully supports VMware vSphere networking functions. To set up networks in the vSphere environment, you need to first understand the mapping of the networking concepts between your bare metal server and vSphere. 

The following table describes the mapping of network concepts between Bare Metal Servers for VPC and VMware vSphere.

| Bare Metal Servers for VPC | VMware vSphere |
|---------|---------|
| PCI interface | Uplink of the bare metal server on a standard vSwitch or distributed vSwitch. |
| VLAN interface | Virtual network adapter of the VMKernel or VM |
| VLAN ID | VLAN ID for port group |
{: caption="Table 2. Mapping of network concepts between Bare Metal Servers for VPC and VMware vShpere" caption-side="bottom"}

The PCI interface of the bare metal server maps to the Uplink in vSphere. When you provision a bare metal server, a primary PCI interface is created by default. This primary PCI interface automatically becomes the bare metal server’s uplink on `vSwitch0`. Its IP address is also used by the `vmk0` adapter in the **Management network** port group on `vSwitch0`. The VLAN ID of the **Management network** port group is automatically set to '0'.

After you add a standard vSwitch or distributed vSwitch, you must select one of the available PCI interfaces as the Uplink on the new vSwitch. Therefore, before you add a vSwitch, you need to make sure that at least one PCI interface is used as the Uplink.

The IP of the PCI interface can be used by the VMkernel adapter.
{: note}

The PCI interfaces that are created on a bare metal server are displayed in VMware vSphere as `vmnic0`, `vmnic1`, `vmnic2`, and so on. You can identify the target PCI interface in vSphere through its MAC address.

The VLAN interface maps to the VMkernel network adapter or a VMware virtual machine network adapter in vSphere. When you create a VLAN interface, you must specify a VLAN ID for the VLAN interface. This VLAN ID maps to the VLAN ID that is used to label port groups in vSphere.

Before you create a server and vmKernel in vSphere, you must create the VLAN interfaces with appropriate VLAN ID. When you create the VM or VMKernal NIC, you need to specify its IP address to that of the corresponding VLAN interface that was created on server. 

### Configuration tips for VMware vSphere network interfaces 
{: #nic-config-tips}

1. If you plan to enable vMotion on a port group in vSphere, you must set the VLAN interfaces with the target VLAN ID to floatable on the bare metal servers.

   This configuration cannot be changed after the VLAN interface is created.
   {: note}
  
2. A bare metal server can have multiple standard vSwitches. The bare metal server can also be added to different distributed vSwitches. Before you create a new standard vSwitch or add a bare metal server to a distributed vSwitch, make sure that the bare metal server has at least one available PCI interface.

### Limitations of network interfaces
{: #nic-limits}

1. You need to associate the VLAN interfaces with the same VLAN ID with one subnet. But, you can create VLAN interfaces with different VLAN IDs in one subnet.

   For example, you can create multiple VLAN interfaces with VLAN ID `111` and `222` within `subnet A`. However, if you create one VLAN interface with VLAN ID `111` within `subnet A` and `subnet B` separately, the two network interfaces can't work as expected.

   You need to design your network properly before you create the network interfaces. A good practice is to set up a VLAN ID - subnet one-to-one mapping network topology.
   {: note}

2. In a VMware environment, traffic between VLAN network interfaces that have the same VLAN ID that are on the same bare metal server are typically switched by the standard vSwitch within the server and never reach the VPC network.

   For example, on a bare metal server host, the default Standard vSwitch is `vSwitch0`. You can create a Port Group with VLAN ID `111` and add it to `vSwitch0`. Traffic between network interfaces that is attached to Port Group `111` is controlled by `vSwitch0`.

   This setting has the following consequence:
   
   - Security Group rules that control traffic between the network interfaces in Port Group `111` aren't applied. If you need Security Group rules, you need to use separate VLAN IDs for the VLAN interfaces.

3. In a distributed vSwitch topology, to enable vMotion in a specific port group, you need to make sure that the VLAN ID of this port group is included in the VLAN allowlist of all the bare metal servers.

   For example, in a distributed vSwitch topology that has two bare metal servers, the PCI interface of "bare-metal-server-1" has a VLAN allowlist of `[111, 222, 333]`, the PCI interface of "bare-metal-server-2" has a VLAN allowlist of `[333, 444, 555]`, only VMs with the VLAN ID `333` can migrate between the two servers through vMotion.

   The PCI interfaces don’t need to be in the same subnet.
   {: tip}

4. A bare metal server can have only one Uplink that is set on a standard vSwitch or distributed vSwitch. Otherwise, the network might not work properly.

## Example vSphere network topology
{: #bare-metal-servers-example-vmotion-topology}

In the following Distributed vSwitch topology, the VMs can migrate between host "10.240.128.42" and "10.240.128.43" by using vMotion.

![Figure showing an example Distributed vSwitch topology that enables vMotion](images/bare_metal_server_example_topology.png "Figure showing an example Distributed vSwitch topology that enables vMotion"){: caption="Figure 2. Example Distributed vSwitch topology that enables vMotion" caption-side="bottom"}

To create a simple topology like Figure 2, use the following steps:

1. On each bare metal server, check that you did following actions. 

   - Create a PCI interface and make sure that VLAN ID `777` is included in the PCI interface VLAN allowlist.
   
   - Create at least one VLAN interface with VLAN ID that is specified to `777`.
   
   - Record the MAC address and IP address of the new network interfaces.

2. In vSphere Client, check that you did the following actions.

   - Enable vMotion on the “vmk0” VMkernel adapter of the 2 ESXi hosts.
   
   ![Figure that shows how to enable vMotion on vmk0](images/bare_metal_server_vmk0_enable.png "Figure that shows how to enable vMotion on vmk0"){: caption="Figure 3. Enable vMotion on vmk0" caption-side="bottom"}
   
   - Create a distributed vSwitch and add the two ESXi hosts to the distributed vSwitch.
   
   - Identify the PCI interfaces that you created in the vmnic list (by MAC address).
   
   - Set the identified vmnics as the uplinks of the hosts. In this example, the identified vmnics are named "vmnic3".
   
   - Create a Distributed Port Group ("DPortGroup") and set the VLAN ID to `777`.
   
   - Create a server and set its IP address to the IP of the VLAN interface (VLAN ID `777`) that you created previously.
  
You can now migrate the VM by using vMotion.  

The VLAN interface that is used by the VM is migrated to the destination host.
{: note}

You can set up network topologies that are more complicated than the example, which is beyond the scope of this information. For more information, see the [VMware vSphere documentation](https://docs.vmware.com/en/VMware-vSphere/index.html).
