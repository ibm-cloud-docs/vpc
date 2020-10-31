---

copyright:
  years: 2019. 2020
lastupdated: "2020-10-30"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Setting up advanced routing
{: #advanced-routing}

You can control the flow of network traffic in your VPC by configuring routes. Use VPC routes to specify the next hop for packets, based on their destination addresses.
{:shortdesc}

Multiple route tables can exist for each zone in your VPC. When a packet leaves a subnet, it is evaluated against the routing table in the subnet's zone to determine where to send the packet next. Each routing table has a default route, but you can add custom static routes to your routing tables.

A VPC route has three main components: the destination CIDR, the next hop to which to route packets, and the zone. Any traffic that originates in the specified zone of the VPC and has a destination address within the specified destination CIDR is routed to the next hop. If the destination address is within the destination CIDR for multiple VPC routes, the most specific route is used. If two or more equally specific routes exist, the traffic is round-robin distributed between each route.

## Creating routes
{: #create-route}

### Using the {{site.data.keyword.cloud_notm}} console to create routes
{: #create-a-route-from-the-user-interface}

Go to the details page for a VPC and click **Routes** in the navigation.

### Using the CLI to create routes
{: #create-a-route-using-the-cli}

Use the [ibmcloud is vpc-route-create](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#vpc-route-create) command.

### Using the API to create routes
{: #create-a-route-using-the-api}

Use the [routes](https://{DomainName}/apidocs/vpc#create-a-route-on-your-vpc) API.

## Limitations
{: #routing-limitations}

- Custom-default static routes are not available at this time. Only non-default static routes are supported.
- You can set the next hop to be an IP address only.

## Configuring a virtual server instance with multiple interfaces
{: #configure-multiple-interfaces}

There are cases when virtual servers communicate with each other through multiple interfaces, for example, to use different subnets for different communication purposes: one subnet for data communication, and another subnet for control communication. Due to a single default route in routing and IP spoofing for security, the communication on interfaces other than the primary interface might not work.

You can solve this problem, on a Linux platform such as `Ubuntu`, in two ways:

* Add a static route for the second subnet.
* Add a separate routing table for the second subnet.

The following sections describe these solutions in more detail.

Here is the virtual server setup:

* There are two virtual servers (`VSI-1` and `VSI-2`) where both virtual servers belong to the same VPC.
* Each virtual server has two interfaces from two subnets:
  * VSI-1 has interface `eth0` from `net_1_0`, and interface `eth1` from `net_1_1`.
  * VSI-2 has interface `eth0` from `net_2_0`, and interface `eth1` from `net_2_1`.
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

### Adding a static route for the second interface
{: #adding-static-route-second-interface}

This solution makes one subnet the default gateway (automatically by virtual server instance creation), and adds a static route for the second subnet:

* On `VSI-1`:

   ```
   ip route add cidr_2_1 via gw_ip_1_1 dev eth1
   ```
   {: pre}

* On `VSI-2`:

   ```
   ip route add cidr_1_1 via gw_ip_2_1 dev eth1
   ```
   {: pre}

### Adding a separate routing table for the second interface
{: #adding-route-table-second-interface}

* On `VSI-1`:

```
   echo 201 eth1tab >> /etc/iproute2/rt_tables
   ip route add cidr_1_1 dev eth1 proto kernel scope link src ip_1_1 table eth1tab
   ip route add default via gw_1_1 dev eth1 table eth1tab
```
{: codeblock}

* On `VSI-2`:

```
echo 201 eth1tab >> /etc/iproute2/rt_tables
ip route add cidr_2_1 dev eth1 proto kernel scope link src ip_2_1 table eth1tab
ip route add default via gw_2_1 dev eth1 table eth1tab
ip rule add from ip_2_1 table eth1tab
```
{: codeblock}

Though this approach is a bit more complicated than adding a static route, it provides a way to customize routing policies for different interfaces.
