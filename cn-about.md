---

copyright:
  years: 2024
lastupdated: "2024-10-15"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About cluster networks
{: #about-cluster-network}

Contact your IBM Support representative if you are interested in getting early access to this offering. It is currently available for early access users of the `gx3d-160x1792x8h100` [virtual server instance profiles](/docs/vpc?topic=vpc-profiles#gpu) in the `us-east` region.
{: beta}

A cluster network is a software-defined network within a Virtual Private Cloud (VPC) used to connect multiple computing systems or nodes in a way that optimizes performance and communication between them. These networks are designed to support tasks that require high-speed data transfer and low latency, such as high-performance computing (HPC) and large-scale data processing. Ideal for large-scale AI training use cases, cluster networks also allow you to define sets of performance criteria for a given group of interconnected systems.
{: shortdesc}

Each cluster network has an associated cluster network profile that describes the type of components that it can connect with. Within the cluster network are a set of basic networking abstractions to provide you with the flexibility and control you need to configure high performance workloads.
{: note}

Key features include:

Isolation and optimization
:   Provides high bandwidth and low latency networking for groups of compute resources. Cluster networks are isolated in a separate IPv4 address space, which are not routed externally.

Specialized technologies
:   Supports advanced networking technologies like Remote Direct Memory Access (RDMA), which allows direct data transfer between the memory of different nodes without involving the CPU, further enhancing performance.

High-performance computing
:   Suited for demanding applications, such as artificial intelligence (AI) training or complex simulations, where high bandwidth and low latency is critical.

Use cases
:   Used in scenarios, such as scientific simulations, large-scale machine learning training, and other data-intensive applications where efficient and rapid data processing is critical.

Flexibility and control
: Is a supplemental network to the existing VPC network. The cluster network attachments are separate from the VPC networks, allowing for users to mix their high speed RDMA networks along side their VPC networks.

## Getting started with cluster networks
{: #cluster-network-getting-started}

A cluster network enhances the efficiency and speed of data transfer within a networked group of systems, making it an essential component for high-performance computing tasks. Follow these general steps to create a simple cluster network for AI training:

1. Review [planning considerations for cluster networks](/docs/vpc?topic=vpc-planning-cluster-network) and be aware of any [known issues and limitations](/docs/vpc?topic=vpc-limitations-cluster-network).
1. Ensure that you have an existing VPC in a region that has capacity for NVIDIA H100 profiles with clustering support.

   Currently, the only supported zone is `us-east-wdc07-a`. For more information about zones, see [zone mapping per account](/docs/overview?topic=overview-locations#zone-mapping).
   {: note}

1. [Create a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=ui). Currently, only the [NVIDIA H100 cluster profile](/docs/vpc?topic=vpc-profiles&interface=ui#gpu) is supported.
1. [Create cluster network subnets](/docs/vpc?topic=vpc-create-cluster-network-subnet&interface=ui) (8, 16, or 32) as child objects on the cluster network.
   If creating a cluster network in the UI, you can create cluster network subnets at the same time.  It is recommended to select 8 subnets, but select use cases utilize a larger number of subnets.
   {: tip}

   Subnets within the H100 cluster network type are routable to each other. However, the cluster network is not routable externally.
   {: note}

1. Do one of the following:

   * Provision a [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui).
   * [Create an instance template](/docs/vpc?topic=vpc-create-instance-template&interface=ui), then create an instance. Make sure to:

      - Select the NVIDIA H100 instance profile.
      - Enable clustering.
      - Add attachments (8, 16, or 32) that correspond with your cluster subnets.

   Advanced users might want to preallocate IP addresses or interfaces, however, it is recommended that you create IPs or interfaces when creating an instance.
   {: note}

### Next step
{: #next-step-cluster-networks}

After you set up your cluster network, [create a virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui) and attach it to the cluster network. You can also pre-create interface objects on the cluster network, or do this when you provision your instance.

## Cluster network use cases
{: #use-cases-cluster-network}

Cluster Networks for VPC supports the following use cases.

### Use case 1: Networking H100-enabled instances to use RDMA on IBM Cloud
{: #cn-use-case-a}

The following diagram demonstrates how you can connect your networking H100-enabled instances through cluster networks to use RDMA on IBM Cloud:

First, make sure that you have a cluster network set up with cluster network subnets. Then, create H100 instances that will connect to the cluster network. When the instance is created the user specify VPC networks and the cluster networks. The VPC networks provide connectivity to your cloud resources and can provide external routing. The cluster networks are additional interfaces within your instance which provide connectivity between the nodes.

![Networking H100-enabled instances to use RDMA on IBM Cloud](images/clusternetwork_usecase1.svg "Networking H100-enabled instances to use RDMA on IBM Cloud"){: caption="Networking H100-enabled instances to use RDMA on IBM Cloud" caption-side="bottom"}

### Use case 2: Securing your cluster network from the rest of the IBM Cloud ecosystem
{: #cn-secure-cluster-network}
{: #cn-use-case-b}

The cluster network is isolated to a separate network domain than the VPC cloud network. The cluster network isolation domain allows the user to be secure without needing to utilize security groups, Network ACLs, or routing tables.Communication within the cluster network only occurs between devices directly connected to the cluster network.

Resources that are connected to the cluster network also must connect at least one VPC network. The VPC network supports all of the IBM Cloud network use cases of a standard VPC resources - Floating IPs, Public Gateways, Transit Gateway, and more. As such, it's recommended that the user review their security policies of the Virtual Network Interfaces attached to resources on the cluster network.

![Securing your cluster network from the rest of the IBM Cloud ecosystem](images/clusternetwork_usecase2.svg "Securing your cluster network from the rest of the IBM Cloud ecosystem"){: caption="Securing your cluster network from the rest of the IBM Cloud ecosystem" caption-side="bottom"}

To maintain minimal access to the cluster network, you must:

* Limit the access to the instances that are connected to the cluster network.
* Ensure that there is a tight security group for the VPC Virtual Network Interfaces (VNIs) on each instance that has access to the cluster network.
   * Make sure to carefully guard inbound TCP requests and consider guarding outbound TCP requests. For more information, see [Setting up a security group for your resource](/docs/vpc?topic=vpc-configuring-the-security-group&interface=cli).
   * Ensure that the subnets attached to your cluster network enabled instances have appropriate network ACLs.
   * Configure your IAM policies for cluster network permissions. For more information, see [Cluster network IAM permissions](/docs/account?topic=account-iam-service-roles-actions#is.cluster-network-roles).

## Related links
{: #related-links-cluster-network}

* [IAM permissions](/docs/account?topic=account-iam-service-roles-actions#is.cluster-network-roles)
* [Quotas and service limits](/docs/vpc?topic=vpc-quotas&q=service+limits&tags=vpc#cluster-networks-quotas)
* [AT events](/docs/vpc?topic=vpc-at_events&q=tracker&tags=vpc#events-cluster-network)
* [FAQs](/docs/vpc?topic=vpc-faqs-cluster-network)
* [Troubleshooting](/docs/vpc?topic=vpc-troubleshoot-cn-1)
