---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations for cluster networks
{: #planning-cluster-network}

Review the following planning considerations before you create a cluster network.
{: shortdesc}

## General considerations
{: #cn-general-considerations}

- Cluster network attachments can be set only when the instance is stopped or created.
- Cluster network traffic is isolated and can't be routed outside. Any access to a cluster network must be through an attached instance. As a result, without connectivity between the cluster network and the VPC network, the following resources cannot connect with cluster networks:

   - [Floating IPs](/docs/vpc?topic=vpc-fip-about&interface=ui)
   - [Load balancers](/docs/vpc?topic=vpc-nlb-vs-elb&interface=ui)
   - [Private Path services](/docs/vpc?topic=vpc-private-path-service-intro&interface=ui)
   - [Public gateways](/docs/vpc?topic=vpc-about-public-gateways&interface=ui)
   - [Virtual Private Endpoint gateways](/docs/vpc?topic=vpc-about-vpe&interface=ui)
   - [VPNs](/docs/vpc?topic=vpc-vpn-overview&interface=ui)

- Services that are not supported:
   * [Flow logs](/docs/vpc?topic=vpc-flow-logs&interface=ui)
   * [Network ACLs](/docs/vpc?topic=vpc-using-acls)
   * [Routing tables](/docs/vpc?topic=vpc-about-custom-routes)
   * [Secondary IPs](/docs/vpc?topic=vpc-vni-about-secondary-ip)
   * [Security groups](/docs/vpc?topic=vpc-using-security-groups)

## Cluster network considerations
{: #cn-considerations-cluster-network}

* You must create the cluster network and the virtual server instances in the same region as the VPC.

* Currently, the only supported VPC regions for Hopper 1 cluster networks are Washington DC (`us-east`) and Frankfurt (`eu-de`). For more information, see [cluster network supported regions and zones](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui#cn-supported-regions).

   The H100 cluster network profile is deprecated. To create a cluster network with NVIDIA Hopper HGX instances, use the Hopper 1 cluster network profile, which supports both NVIDIA H100 and H200 instance types.
   {: note}

## Cluster network subnet considerations
{: #cn-considerations-cluster-network-subnets}

* Plan your cluster network subnets based on the recommended number for the instance profile.

   | Cluster network profile | Instance profile       | Subnet-to-vNIC ratio | Supported cluster vNICs | Recommended subnets |
   |-------------------------|------------------------|----------------------|-------------------------|---------------------|
   | `hopper-1`              | `gx3d-160x1792x8h100`  | 1:1                  | 8, 16, or 32            | Match vNIC count    |
   | `hopper-1`              | `gx3d-160x1792x8h200`  | 1:1                  | 8, 16, or 32            | Match vNIC count    |
   {: caption="Recommended cluster subnet configuration per instance profile." caption-side="bottom"}

   If you're unsure how many vNICs to use with Hopper 1 instance profiles, start with 8.
   {: tip}

* Make sure that the IP address ranges used for your cluster network subnets don't overlap with the address ranges of your cloud subnets. Overlapping ranges can lead to networking issues. For more information, see [IP address range and prefix considerations](/docs/vpc?topic=vpc-planning-cluster-network#cn-considerations-static-IP-address).

In the Hopper 1 cluster network, subnets are routable to each other, but not externally.
{: note}

## Virtual server instance considerations
{: #cn-considerations-vsi}

* Determine the total resources that are required for your cluster by multiplying the number of instances you intend to create by the resources defined in the corresponding [instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#gpu).
* Check the calculated total resources required for your cluster against the [default quotas](/docs/vpc?topic=vpc-quotas&q=service+limits&tags=vpc#cluster-networks-quotas) to determine whether a quota increase is necessary.

## IP address range and prefix considerations
{: #cn-considerations-static-IP-address}

You can apply the following considerations only through the API and CLI.
{: note}

   * Cluster networks are isolated from the VPC network, with their own IP address ranges. You can define cluster network `subnet_prefixes` that overlap with the VPC's [default](/docs/vpc?topic=vpc-configuring-address-prefixes) or [custom](/docs/vpc?topic=vpc-vpc-addressing-plan-design) address prefixes. However, this configuration might result in IP address conflicts, wherein the instances have two different interfaces with the same IP address.

   * To prevent such conflicts, you must isolate the network interfaces in the guest operating system. For example, Linux systems can use [network namespaces](https://www.man7.org/linux/man-pages/man8/ip-netns.8.html){: external}.

   * The default `subnet_prefixes` for cluster networks is `["cidr": "10.0.0.0/9"]`, which doesn't overlap with the [default address prefixes for VPCs](/docs/vpc?topic=vpc-configuring-address-prefixes). To simplify the guest operating system configurations, either use the defaults or choose custom ranges that don’t overlap with your VPC.

   * If you are planning to assign static IP addresses, especially at a large scale (such as 1,000), you must plan the task carefully.
      * First, determine the total number of required IP addresses.
      * Then, consider how these addresses are assigned to various instances.
      * Use a clear naming convention to organize and connect the IP addresses to their respective instances. For example, you might use a naming convention that includes the role or location of each instance, helping you easily identify and manage them.
   * When you configure static IP addresses for network interfaces in a guest OS:
      * Make sure that the IP address of each interface is in the address range of one of the cluster network `subnet_prefixes`. Addresses outside these ranges can't receive packets.
      * If a static IP address does not match the interface's primary or secondary reserved IP addresses, you must create a VPC routing table to redirect traffic to one of the reserved IP addresses.
      * Additionally, you must set `allow_ip_spoofing` to `true` on the associated cluster network interface. This action uses the static IP as the source address for the outbound traffic flow.

## Cluster network supported regions and zones
{: #cn-supported-regions}

The following table provides an overview of the supported regions and zones for cluster networks.

| Cluster network profile | Instance profile                                          | Region                       |Zone                 | Universal zone name   |
|------------------------ |-----------------------------------------------------------|------------------------------|---------------------|-----------------------|
| `hopper-1`              | `gx3d-160x1792x8h100` \n `gx3d-160x1792x8h200`            | Frankfurt (`eu-de`)          |`eu-de-2`            | `eu-de-fra02-a`       |
| `hopper-1`              | `gx3d-160x1792x8h100` \n `gx3d-160x1792x8h200`            | Washington DC (`us-east`)    |`us-east-3`          | `us-east-wdc07-a`     |
{: caption="Zone availability for cluster networks and instances." caption-side="bottom"}

To understand how various regions correspond to zones, see [zone mapping per account](/docs/overview?topic=overview-locations#zone-mapping).
{: note}

## Related link
{: #related-link-cluster-networks}

[Known issues for cluster networks](/docs/vpc?topic=vpc-known-issues-cluster-networks)
