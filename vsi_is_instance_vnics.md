---

copyright:
  years: 2018, 2022
lastupdated: "2022-11-08"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing network interfaces
{: #using-instance-vnics}

After you create a virtual server instance, you can add new network interfaces or edit the interfaces that are already associated with the instance. When you edit a network interface, you can change its name, associate or unassociate a floating IP address, or access the security group that is associated with an interface.
{: shortdesc}

## About network interfaces
{: #about-network-interfaces}

A network interface connects a virtual server instance to a network. When you create a virtual server instance, you can use network interfaces to assign multiple IP addresses.

The following list highlights how network interfaces work with your instance.

* You can create and assign multiple network interfaces for each virtual server instance. The number of network interfaces that you can assign to a virtual server instance depends on the vCPU count that is included in the [profile](/docs/vpc?topic=vpc-profiles) that is used to provision an instance.
   * 2 - 16 vCPUs: Up to five network interfaces
   * 17 - 48 vCPUs: Up to 10 network interfaces
   * 49 or more vCPUs: Up to 15 network interfaces
* You can attach each network interface to a different subnet in the same zone.
* Each network is assigned a unique and immutable Media Access Control (MAC) address and receives a private IP address from the subnet range.
* You can attach one floating IP address to the virtual server instance. The floating IP address must be attached to the primary network interface to establish the data path. By default the primary interface is `eth0` in {{site.data.keyword.cloud_notm}} console.
* If you want to assign a floating IP address to a secondary NIC (`eth1`), you can use the [CLI](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#floating-ips-cli-ref).
* You can associate and unassociate a single floating IP address for the instance.
* You can assign security groups to each network interface.
* You can change the name of any existing network interface.

Bandwidth is distributed across the network interfaces that are attached to the virtual server instance. For more information, see [Bandwidth allocation](/docs/vpc?topic=vpc-profiles&interface=ui#bandwidth-allocation).

If you assign a new network interface to a virtual server instance while it is running, you must configure the network interface for the instance to use. You can either stop and then restart the instance, or you can manually configure the interface in the guest operating system. For example, on a Linux-based operating system, you can use the `ip link set dev <interface> up` to retrieve the IP address configuration for the interface.
{: note}

## Adding or editing network interfaces
{: #editing-network-interfaces}

To add or edit the network interfaces associated with your virtual server instances, complete the following steps.
1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. Click the name of a virtual server instance that includes the network interface that you want to edit. Or you can add a network interface to the virtual server instance.
3. On the Instance details page, find the **Network interfaces** section.
4. For specific steps for adding a floating IP address or adding a network interface, see the following sections.

### Adding a floating IP address
{: #adding-floating-ip}

If you want to add a floating IP address to a primary network interface to allow traffic from the internet to access your virtual server instance, complete the following steps.

1. If you are adding a floating IP address to the virtual server instance for the first time, identify the primary network interface in the **Network interfaces** section of the **Instance details** page. By default, the first interface is named `eth0`. Initially associating the floating IP address with the primary network interface helps establish the data path. Later, you can associate the floating IP to a different primary network interface.
2. Click the pencil icon to edit the primary network interface.
3. On the **Edit network interface** page, locate the **Floating IP address** field. You can select **Reserve a new floating IP** or you can select an existing floating IP address.
4. After you make your selection, click **Save**.

### Adding a network interface
{: #add-network-interface}

You can add up to 15 network interfaces to your virtual server instance, depending on the vCPU count that is included in the instance profile. A network interface establishes a unique IP address,
giving the option of multiple IP address for your virtual server instance. Before you begin adding an interface, make sure that
you created a unique subnet to associate with the new network interface. Each network interface must be on a different subnet within
the same zone.

To add a network interface to your virtual server instance, complete the following steps.

1. In the **Network interfaces** section of the **Instance details** page, click **New interface**.
2. On the **New network interface** page, the interface name defaults to an incremented number. If this instance is the first new interface after your primary interface, the default name is `eth1`. You can change the name if you want.
3. Select a unique subnet from the subnets that are assigned to your existing network interfaces.
4. Select the security group that you want to associate with the network interface.
5. Click **Create**.
6. If your virtual server instance was running when you added the network interface, you must configure the network interface for the instance. You can either stop and then restart the instance, or you can manually configure the interface in the guest operating system. For example, on a Linux-based operating system, you can use the `ip link set dev <interface> up` to retrieve the IP address configuration for the interface.

### Adding a virtual network interface
{: #add-virtual-network-interface}

This VPC feature is a preview and available only to accounts with special approval.
{: preview}

You can create a virtual network interface without attaching it to a target. The virtual network interface can exist even when its target is removed. For more information, see [Working with virtual network interfaces](/docs/vpc?topic=vpc-vni-create&interface=ui).

Virtual network interfaces can be attached to new virtual server instances, and cannot be added to existing virtual server instances with child network interfaces.
{: note}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
1. Click **Create** to begin creating a new virtual server instance capable of using a virtual network interface.
1. In the **Networking** section, select whether to create one of the following:
   * **Network attachment with a virtual network interface**: a network interface that has additional features, such as secondary IP addresses and a lifecycle separate from the virtual server instance you are creating.
   * **Instance network interface**: a child network interface.

## Configuring a virtual server instance with multiple interfaces
{: #configure-multiple-interfaces}

Sometimes virtual servers instances communicate with other instances through multiple network interfaces, for example, to use different subnets for different communication purposes. You might want one subnet for data communication and another subnet for control communication. Because a virtual server instance is configured with a single default route that is associated with the primary network interface, communication on other interfaces might not work initially. (IP spoofing checks for security purposes can also prevent the communication flow on nonprimary network interfaces.)

You can establish communication for a secondary network interface by using one of the following options. These examples are for a Linux platform such as `Ubuntu`.

* Add a static route for the second subnet.
* Add a separate routing table for the second subnet.

The routing tables for virtual server instances are generated by default when an instance is provisioned. Modifying the generated routing table is a nontrivial task. If you need to modify the routing table of a virtual server instance, when you provision the instance you can include user-data to describe and reconfigure the instance's routing table. Another option is to use a custom image to provision an instance and control what is created for multiple network interfaces. When you use the default images to provision instances, the network configuration is created for you. You have limited control of what is generated.
{: important}

The following sections describe these solutions in more detail.

Example virtual server setup:

* Two virtual servers (`Virtual-server-1` and `Virtual-server-2`) where both virtual servers belong to the same VPC.
* Each virtual server has two interfaces from two subnets:
   * _Virtual-server-1_ has interface `eth0` from `net_1_0`, and interface `eth1` from `net_1_1`.
   * _Virtual-server-2_ has interface `eth0` from `net_2_0`, and interface `eth1` from `net_2_1`.
* Each virtual server's `net_*_0` is set up with a default route.
* Each subnet's gateway IP and CIDR are known.

   Alphabetic names are used here. The real gateway ID would be similar to `192.168.100.1`, the CIDR `192.168.100.0/24`, and the IP address `192.168.100.6`.
   {: note}

| Subnet | Gateway IP | Subnet CIDR | Interface IP |
| --- | --- | --- | --- |
| `net_1_0` | `gw_ip_1_0` | `cidr_1_0` | `ip_1_0` |
| `net_1_1` | `gw_ip_1_1` | `cidr_1_1` | `ip_1_1` |
| `net_2_0` | `gw_ip_2_0` | `cidr_2_0` | `ip_2_0` |
| `net_2_1` | `gw_ip_2_1` | `cidr_2_1` | `ip_2_1` |
{: caption="Table 1. Example of a virtual server instance with multiple interfaces" caption-side="bottom"}

### Adding a static route for the second interface
{: #adding-static-route-second-interface}

This solution makes one subnet the default gateway (automatically by virtual server instance creation), and adds a static route for the second subnet:

* On `Virtual-server-1`:

   ```sh
   ip route add cidr_2_1 via gw_ip_1_1 dev eth1
   ```
   {: pre}

* On `Virtual-server-2`:

   ```sh
   ip route add cidr_1_1 via gw_ip_2_1 dev eth1
   ```
   {: pre}

### Adding a separate routing table for the second interface
{: #adding-route-table-second-interface}

* On `Virtual-server-1`:

```sh
   echo 201 eth1tab >> /etc/iproute2/rt_tables
   ip route add cidr_1_1 dev eth1 proto kernel scope link src ip_1_1 table eth1tab
   ip route add default via gw_1_1 dev eth1 table eth1tab
```
{: codeblock}

* On `Virtual-server-2`:

```sh
echo 201 eth1tab >> /etc/iproute2/rt_tables
ip route add cidr_2_1 dev eth1 proto kernel scope link src ip_2_1 table eth1tab
ip route add default via gw_2_1 dev eth1 table eth1tab
ip rule add from ip_2_1 table eth1tab
```
{: codeblock}

Though this approach is a bit more complicated than adding a static route, it provides a way to customize routing policies for different interfaces.
