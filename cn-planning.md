---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations for cluster networks
{: #planning-cluster-network}

Review the following planning considerations before you start creating a cluster network.
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

## Considerations for cluster subnets
{: #cn-considerations-cluster-subnets}

* Instance profiles have a recommended number of cluster subnets. The {{site.data.keyword.cloud}} recommendation for cluster subnets for a specific instance type is:

   | Instance Profile Type | Recommended Cluster Subnets to Cluster vNICs |
   | --------------------- | -------------------------------------------- |
   | gx3d-160x1792x8h100   | 1:1                                          |
   {: caption="Recommended number of cluster subnets for instance profiles." caption-side="bottom"}

   * `gx3d-160x1792x8h100` supports either 8, 16 or 32 cluster vNICs. As a result, you should have a corresponding number of cluster subnets.
   * If you are unsure about the correct number of cluster vNICs for your workload, IBM Cloud recommends the use of 8 for the `gx3d-160x1792x8h100` profile.

   Make sure that the IP address ranges used for your cluster network subnets do not overlap with the IP addresses of your cloud subnets. Overlapping ranges can lead to networking issues.
   {: note}

* Start by setting up your cluster network and subnets, then define instance templates to scale your training cluster with the number of nodes that you want. For more information, see [Getting started with cluster networks](/docs/vpc?topic=vpc-about-cluster-network#cluster-network-getting-started).
   * You can [create an instance template](/docs/vpc?topic=vpc-create-instance-template&interface=ui) to define instance details for provisioning one or more virtual servers. When you create your instance template, you can use it to provision single virtual server instances, or to provision multiple instances at the same time as part of an instance group.
   * Optionally, when you provision an instance, you can include the cluster network configuration within the instance provision request.

## Considerations for IP address ranges (prefixes)
{: #cn-considerations-static-IP-address}

You can apply the following considerations only through the API and CLI.
{: note}

   * Cluster networks are isolated from the VPC network. You are allowed to specify cluster network `subnet_prefixes` that overlap with the VPC's [default](/docs/vpc?topic=vpc-configuring-address-prefixes) or [custom](/docs/vpc?topic=vpc-vpc-addressing-plan-design) address prefixes. However, this configuration can result in instances that have two network interfaces (one on the VPC network, and one on the cluster network) with the same IP address. In such a scenario, you must be careful to isolate the network interfaces in the guest operating system. For example, Linux guests can use [network namespaces](https://www.man7.org/linux/man-pages/man8/ip-netns.8.html).

   * The default `subnet_prefixes` for cluster networks (`["cidr": "10.0.0.0/9"]`) do not overlap with the [default address prefixes for VPCs](/docs/vpc?topic=vpc-configuring-address-prefixes). To simplify the guest operating system configurations, either use the defaults, or plan your custom address ranges to avoid overlap.
   * If you need to plan for statically-configured IP addresses, especially for a large number (such as 1000), it’s important to approach the task methodically. First, determine the total number of required IP addresses. Then, consider how these addresses are to be assigned to various instances. To manage IP address assignments efficiently, implement a clear naming scheme that helps you organize and connect the IP addresses to their respective instances. For example, you might use a naming convention that reflects the function or location of each instance, making it simpler to identify and manage them.
   * When statically configuring IP addresses for network interfaces in a guest OS:
      * Make sure that the IP addresses for cluster network interfaces are in the address range of one of the `subnet_prefixes` in the cluster network. Cluster network interfaces with an IP address outside the address range of one of the `subnet_prefixes` do not receive packets.
      * If a statically configured IP address does not match the cluster network interface's primary reserved IP or one of its secondary reserved IP addresses, you must create a VPC routing table route to redirect traffic for the statically configured IP to one of the reserved IP addresses. Additionally, you must set `allow_ip_spoofing` to `true` on the associated cluster network interface to allow outbound packets with the statically configured IP as the source address.

## Related link
{: #related-link-cluster-networks}

[Known issues for cluster networks](/docs/vpc?topic=vpc-known-issues-cluster-networks)
