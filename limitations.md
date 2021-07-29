---

copyright:
  years: 2018, 2021
lastupdated: "2021-07-29"

keywords: vpc, limitations, restrictions

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Service limitations
{: #limitations}

Limitations might change as capabilities are added, so feel free to check back from time to time.
{:shortdesc}

## General restrictions
{: #general-restrictions}

The following features are not supported, including all properties associated with these features:

  * The following concepts are not supported:
    * IPV6
    * Multiple IP addresses on the same network interface

  * Virtual server instance name change: If you update the name of a virtual server, the name change might not appear consistently in different areas of the {{site.data.keyword.cloud}} console. For example, the virtual server name change might not be reflected in the {{site.data.keyword.cloud_notm}} console, or on the billing invoice, yet it appears correctly in the user's list of running instances.

  * Direct Link on Classic access to VPC is supported through [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure) only. Direct Link (release 2.0) does not have this limitation.

  * Nested virtualization on virtual server instances is disabled for now. But you can try [enabling it manually](/docs/vpc?topic=vpc-troubleshooting-your-virtual-servers-for-vpc#troubleshoot-enable-nested-virtualization). However, this feature may not work properly in scenarios such as live migration of virtual server instances.

### Billing note
When the VPC billing system reports network traffic from load balancers and VPNs, the resources are identified with a nonstandard name. The resource is identified using the prefix `instance-`, followed by the last 16 digits of the back-end virtual server instance.

For example, if the load balancer UUID is `10000`, but it is running in a virtual server instance that has an internal UUID of `75007600770078007900`. The last 16 digits of the instance UUID are: `7600770078007900`.

The CRN will identify the load balancer as follows:`crn:v1:bluemix:public:is:eu-gb:a/0000000001::load-balancer:instance-7600770078007900`.

## Virtual private cloud restrictions
{: #virtual-private-cloud-restrictions}

An {{site.data.keyword.vpc_short}} cannot be peered with other VPCs natively. It is possible to connect VPCs using either Transit Gateway, VPN Gateways, or Floating IPs.

* With both VPN Gateways and Floating IPs, there is no automatic route advertisement between the two VPCs. Static routes must be used in each VPC to enable layer 3 connectivity between the two VPCs. See [Example: Connecting two VPCs using VPN](/docs/vpc?topic=vpc-vpn-example) for how you can achieve VPC-to-VPC connectivity by using this method.
* With Transit Gateway, it advertises the root subnets of each VPC allowing traffic to be routed without the use of static routes. For more information, see [Getting started with IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-getting-started).

## Network restrictions
{: #network-restrictions}

* Multi-zone regions:
  * A security group can be configured in a single zone only.
  * A security group can’t reference another security group in a different zone in the same region.

* VPN: A VPN gateway serves only subnets that are in the zone in which the VPN is created. For more information, see [About VPN](/docs/vpc?topic=vpc-using-vpn).

## Compute restrictions
{: #compute-restrictions}

* Every x86-based profile has a network performance value of 2 Gbps per vCPU, with a cap of 80 Gbps.
* Each x86-based network interface has a network performance cap of 16 Gbps. <!-- You might need to attach multiple network interfaces to your virtual server instance to optimize network performance. -->
* Start/Stop actions are not registered under virtual server instance activity in the UI.
* Activity Tracker logs (request logs and resource lifecycle event logs) are not available.
* Updating the profile of a created instance is not supported.
* The placement group of the instance can't be changed after an instance is provisioned with a placement group. You must delete the instance to remove it from the placement group.

* We have temporarily suspended API support for creating new instances from an existing boot volume. For more information, see the [API change log](/docs/vpc?topic=vpc-api-change-log).

## Storage restrictions
{: #storage-restrictions}

Block storage volume names must be unique across the entire VPC infrastructure. A volume created on Generation 2 compute resources can't have the same name as a volume created on Generation 1. For more information on volume naming, see [Guidelines for naming volumes](/docs/vpc?topic=vpc-managing-block-storage#volume-name-conventions).

## LinuxONE (s390x processor architecture) virtual server instance restrictions  
{: #LinuxONE-vsi-restrictions}

The following features are currently not supported:

* Dedicated hosts
* Snapshots
* Flow logs

## API considerations
{: #api-considerations}

* For API limitations and considerations, see [API application migration considerations](/docs/vpc?topic=vpc-api-integration-migration).
