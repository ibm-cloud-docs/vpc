---

copyright:
  years: 2021, 2026
lastupdated: "2026-01-23"

keywords: bare metal server network, bare metal network, nics, pci, vlan, network overview

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Networking overview for Bare Metal Servers on VPC
{: #bare-metal-servers-network}

The following information is an overview of the networking features of {{site.data.keyword.bm_is_full}}. Make sure that you go through this information before you build a network on bare metal servers. Keep in mind that this information is for users with basic network knowledge of {{site.data.keyword.cloud}} VPC. If you aren't familiar with VPC networking, see [About networking](/docs/vpc?topic=vpc-about-networking-for-vpc).
{: shortdesc}

* Bare Metal Server for VPC provides full support for VPC networking features. The network is fully software-defined, so you can configure it through the UI and the API.

* Dynamic Network Bandwidth is only available on {{site.data.keyword.bm_is_short}}. Each bare metal server has a maximum allotted bandwidth. The bandwidth is shared by all the network interfaces on the server. The network bandwidth speed can be selected at provisioning or changed post-provision. For more information on profile generations, see [x86-64 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui).

* For more information about managing network interfaces, see [Managing network interfaces for a bare metal server](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers).

* If you're using VMWare vSphere networking, see [Introduction to vSphere Networking](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/7-0/vsphere-networking.html){: external}.

## Bare metal server network interfaces
{: #bare-metal-servers-nics-intro}

You can create two types of network interfaces on a bare metal server:
| Network interface | Description |
|-----|-----|
| PCI (peripheral component interconnect) | A physical network interface. |
| VLAN (virtual LAN) interface | A virtual network interface that is associated with a PCI interface through the VLAN ID. The VLAN interface automatically tags traffic that is routed through it with the VLAN ID. Inbound traffic that is tagged with a VLAN ID is directed to the appropriate VLAN interface. |
{: caption="Types of network interfaces for bare metal servers" caption-side="bottom"}

### PCI and VLAN interface characteristics
{: #bare-metal-servers-interface-characteristics}

The following list highlights the characteristics of the PCI and VLAN interfaces.

* You can create up to 8 PCI interfaces on a bare metal server. To add or remove PCI interfaces, the bare metal server must be **STOPPED**.

* The maximum number of network interfaces per bare metal server (both PCI and VLAN) is 25. However, you can create up to 128 by using the CLI or API, but doing so can affect the performance of vMotion. It is recommended that you use NSX-T for environments that require large numbers of network interfaces.

* You can attach security groups to both PCI and VLAN interfaces to handle incoming and outgoing traffic to the network interface. The VLAN interface maintains its own security group rules that might be different from its associated PCI interface rules.

* All network interfaces are backed by two redundant physical ports that are on the TORs (top-of-rack) switch. {{site.data.keyword.cloud}} manages the aggregation, so you don't need to create multiple PCI interfaces for redundancy.

* Optionally, you can set VLAN interfaces to "floatable" to support vMotion between the bare metal servers on a VMware compatible network.

* You can associate more than one floating IP with a network interface. Having multiple floating IPs enables the VMware NSX-T Data Center to assign floating IPs to the VMware VMs.

* You can identify the target PCI interface in vSphere through its MAC address. You can retrieve the MAC address for the PCI interface through the UI, [CLI](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-network-interface-view), or [API](/apidocs/vpc/latest#list-bare-metal-server-network-interfaces).

### Limitations of network interfaces
{: #nic-limits}

1. VLAN interfaces with different VLAND IDs can be placed within different subnets. However, you need to associate VLAN interfaces with the same VLAN ID within the same subnet.

   For example, you can create multiple VLAN interfaces with VLAN ID `111` within `subnet A`. However, if you create VLAN interfaces with VLAN ID `111` within `subnet A` and within `subnet B`, the two network interfaces can't work as expected.

   You need to design your network properly before you create the network interfaces. A good practice is to set up a VLAN ID - subnet one-to-one mapping network topology.
   {: note}

2. In a VMware environment, traffic between VLAN network interfaces that have the same VLAN ID and are on the same bare metal server are typically switched by the standard vSwitch within the server and never reach the VPC network.

   For example, on a bare metal server host, the default standard vSwitch is `vSwitch0`. You can create a port group with VLAN ID `111` and add it to `vSwitch0`. Traffic between network interfaces that is attached to port group `111` is controlled by `vSwitch0`.

   This setting has the following consequence:

   - Security group rules that control traffic between the network interfaces in port group `111` aren't applied. If you need security group rules, you need to use separate VLAN IDs for the VLAN interfaces.

3. In a distributed vSwitch topology, to enable vMotion in a specific port group, you need to make sure that the VLAN ID of this port group is included in the VLAN allowlist of all the bare metal servers.

   For example, in a distributed vSwitch topology with two bare metal servers, the PCI interface of "bare-metal-server-1" has a VLAN allowlist of `[111, 222, 333]` and the PCI interface of "bare-metal-server-2" has a VLAN allowlist of `[333, 444, 555]`. Only VMs with the VLAN ID `333` can migrate between the two servers through vMotion.

   The PCI interfaces don’t need to be in the same subnet.
   {: tip}

4. A bare metal server can have only one Uplink that is set on a standard vSwitch or distributed vSwitch. Otherwise, the network might not work properly.

## Mapping network concepts between bare metal servers and VMware vSphere
{: #mapping-network-concepts}

{{site.data.keyword.bm_is_short}} fully supports VMware vSphere networking functions. To set up networks in the vSphere environment, you need to first understand the mapping of the networking concepts between your bare metal server and vSphere.

The following table describes the mapping of network concepts between {{site.data.keyword.bm_is_short}} and VMware vSphere.

| Bare metal server | VMware vSphere |
|---------|---------|
| PCI interface | Uplink of the bare metal server on a standard vSwitch or distributed vSwitch. |
| VLAN interface | Virtual network adapter of the VMKernel or virtual machine |
| VLAN ID | VLAN ID for port group |
{: caption="Mapping of network concepts between Bare Metal Servers for VPC and VMware vShpere" caption-side="bottom"}

The PCI interface of the bare metal server maps to the Uplink in vSphere. When you provision a bare metal server, a primary PCI interface is created by default. This primary PCI interface automatically becomes the bare metal server’s uplink on `vSwitch0`. Its IP address is also used by the `vmk0` adapter in the **Management network** port group on `vSwitch0`. The VLAN ID of the **Management network** port group is automatically set to '0'.

After you add a standard vSwitch or distributed vSwitch, you must select one of the available PCI interfaces as the Uplink on the new vSwitch. Therefore, before you add a vSwitch, you need to make sure that at least one PCI interface is used as the Uplink.

If the VMkernel adapter uses the same IP address as the PCI interface, the Mac address of the VMkernel adapter must use the same Mac address as the PCI interface.
{: note}

The PCI interfaces that are created on a bare metal server are displayed in VMware vSphere as `vmnic0`, `vmnic1`, `vmnic2`, and so on. You can identify the target PCI interface in vSphere through its MAC address.

The VLAN interface maps to the VMkernel network adapter or a VMware virtual machine network adapter in vSphere. When you create a VLAN interface, you must specify a VLAN ID for the VLAN interface. This VLAN ID maps to the VLAN ID that is used to label port groups in vSphere.

Before you create a server and vmKernel in vSphere, you must create the VLAN interfaces with appropriate VLAN ID. When you create the VM or VMKernal NIC, you need to specify its IP address to that of the corresponding VLAN interface that was created on the server.

### Example vSphere network topology
{: #bare-metal-servers-example-vmotion-topology}

To create a simple topology, use the following steps:

1. On each bare metal server, check that you did the following actions.

   - Create a PCI interface and make sure that VLAN ID `777` is included in the PCI interface VLAN allowlist.

   - Create at least one VLAN interface with VLAN ID that is specified to `777`.

   - Record the MAC address and IP address of the new network interfaces.

2. In vSphere Client, check that you did the following actions.

   - Enable vMotion on the “vmk0” VMkernel adapter of the 2 ESXi hosts.

   - Create a distributed vSwitch and add the two ESXi hosts to the distributed vSwitch.

   - Identify the PCI interfaces that you created in the vmnic list (by MAC address).

   - Set the identified vmnics as the uplinks of the hosts. In this example, the identified vmnics are named "vmnic3".

   - Create a Distributed Port Group ("DPortGroup") and set the VLAN ID to `777`.

   - Create a server and set its IP address to the IP of the VLAN interface (VLAN ID `777`) that you created previously.

You can now migrate the VM by using vMotion.

The VLAN interface that is used by the VM is migrated to the destination host.
{: note}

You can set up network topologies that are more complicated than the example, which is beyond the scope of this information. For more information, see the [VMware vSphere documentation](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere.html).

### Configuration tips for VMware vSphere network interfaces
{: #nic-config-tips}

1. If you plan to use vMotion to move a virtual machine between bare metal servers, you must set the VLAN interfaces with the target VLAN ID to `floatable` on the bare metal servers.

   You can't change this configuration after the VLAN interface is created.
   {: note}

2. A bare metal server can have multiple standard vSwitches. The bare metal server can also be added to different distributed vSwitches. Before you create a new standard vSwitch or add a bare metal server to a distributed vSwitch, make sure that the bare metal server has at least one available PCI interface.

## Special considerations for Linux
{: #mapping-network-concepts-special-considerations-linux}

* PCI interfaces show up in the PCI device tree and you can configure them by using standard Linux networking practices.

* You need to create VLAN interfaces within the operating system by using a `macvlan` or equivalent interface.

## MAC address learning
{: #bare-metal-servers-mac-learning}

In the SDN, a network interface has two properties - an IP address and a MAC address. Normally, you need to use that MAC address, or the packet drops. But with bare metal servers, you can use a different MAC address, and {{site.data.keyword.cloud}} 'learns' the MAC address that you're using and allows traffic to flow.

DHCP doesn't work with a custom MAC address because an {{site.data.keyword.cloud}}-provided MAC address is needed to respond to a DHCP request. You need to use a static IP configuration to use custom MAC addresses.
{: important}

## Advanced networking
{: #bare-metal-servers-advanced-networking}

### Infrastructure NAT and IP spoofing
{: #bare-metal-servers-infrastructure-nat-ip-spoofing}

Turning on infrastructure NAT allows the VPC infrastructure to perform any needed NAT operations. If infrastructure NAT is off, the packet passes unmodified to and from the network interface, allowing the workload to perform NAT operations. The default is on. You must have the Advanced Network Operator IAM role to modify this configuration. Allow IP spoofing must be turned off if `Enable infrastructure NAT` is turned off.

   

### Multiple floating IPs
{: #bare-metal-servers-multiple-floating-ips}

To associate multiple floating IPs to a network interface, make sure that both Allow IP spoofing and Enable infrastructure NAT are disabled on the network interface. For more information, see [Associating floating IPs with a network interface](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers#bare-metal-add-fips-to-nic).

### Virtual IP (VIP)
{: #bare-metal-servers-vip}

A VIP is used for moving between interfaces to achieve high availability. Typically, two interfaces belong to two servers. Each interface has a primary IP and the VIP is configured as a secondary IP that can float between the servers for active-passive high availability. For more information about creating a VIP, see [Creating a virtual IP (VIP)](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers#bare-metal-virtual-ips).
