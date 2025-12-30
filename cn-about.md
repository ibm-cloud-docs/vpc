---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About cluster networks
{: #about-cluster-network}

A cluster network is a software-defined network within a Virtual Private Cloud (VPC) that connects multiple computing systems, or nodes. It improves communication between these systems, providing fast data transfer and low latency. This service makes it ideal for demanding tasks, such as high-performance computing (HPC), large-scale data processing, and artificial intelligence (AI) model training. Cluster networks are built to meet high-performance standards, making them a strong fit for environments that rely on efficient, high-speed communication between systems.
{: shortdesc}

Each cluster network has an associated cluster network profile that defines the type of components that it can connect with. Within the cluster network is a set of basic networking abstractions, such as instances, interfaces, and subnets, giving you the flexibility and control that you need to configure high-performance workloads.
{: note}

Key features include:

High-performance computing
:   Suited for demanding applications, such as AI training or complex simulations, where high bandwidth and low latency are critical.

Direct data transfer
:   Supports advanced networking technologies, such as Remote Direct Memory Access (RDMA), which allows direct data transfer between the memory of different nodes without involving the CPU, further enhancing performance.

Isolation and optimization
:   Provides high bandwidth and low latency networking for groups of compute resources. Cluster networks are isolated in a separate IPv4 address space that isn't routed externally.

Flexibility and control
:   Acts as a supplemental network to the existing VPC network. Cluster network attachments remain separate from VPC networks, supporting integration of high-speed RDMA networks alongside VPC networks.

{{site.data.keyword.cloud}} supports the following cluster network profiles

## Cluster network offerings in IBM Cloud
{: #cluster-network-offerings}

### NVIDIA Hopper 1
{: #nvidia-hopper-1}

The Hopper 1 cluster network profile is a high-performance {{site.data.keyword.cloud_notm}} cluster that is powered by NVIDIA H100 and H200 GPUs, connected with NVLink and 400 Gbps InfiniBand networking. Hopper 1 is optimized for cloud deployment and designed for large-scale AI training and inference with high memory bandwidth and low-latency GPU communication.

## Getting started with cluster networks
{: #cluster-network-getting-started}

A cluster network enhances the efficiency and speed of data transfer within a networked group of systems, making it an essential component for high-performance computing tasks.

Follow these general steps to create a simple cluster network for AI training:

1. Review [planning considerations](/docs/vpc?topic=vpc-planning-cluster-network) for cluster networks and be aware of any [known issues](/docs/vpc?topic=vpc-known-issues-cluster-networks).
1. Determine the total resources that are required for your cluster by multiplying the number of instances you intend to create by the resources defined in the corresponding [instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#gpu).
1. Check the calculated total resources required for your cluster against the [default quotas](/docs/vpc?topic=vpc-quotas&q=service+limits&tags=vpc#cluster-networks-quotas) to determine whether a quota increase is necessary.
1. [Create a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=ui).
1. [Create a cluster network subnet](/docs/vpc?topic=vpc-create-cluster-network-subnet&interface=ui) if you didn't already during cluster network creation.
1. Create virtual server instances by using one of the following methods:

   * [Create a virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui).
   * [Create an instance template](/docs/vpc?topic=vpc-create-instance-template&interface=ui), then create an instance. Make sure to:

      - Select the required instance profile. For Hopper 1, the available instance profiles are `gx3d-160x1792x8h100` and `gx3d-160x1792x8h200`;
      - Enable clustering.
      - Add attachments corresponding to your cluster subnets.

    When you create your instance template, you can use it to provision single virtual server instances or multiple instances at the same time as part of an instance group.

## Cluster network use cases
{: #use-cases-cluster-network}

Review the use case that aligns with your selected instance profile to guide you through the appropriate configuration and security measures for your cluster network deployment.

### Use case 1: Networking H100/H200 instances to use RDMA with the Hopper 1 cluster network
{: #cn-use-case-networking}
{: #cn-use-case-a}

The following diagram illustrates how to connect H100/H200 instances to a Hopper 1 cluster network to enable RDMA on {{site.data.keyword.cloud_notm}}.

First, make sure that you have a [cluster network]((/docs/vpc?topic=vpc-create-cluster-network&interface=ui)) set up with [cluster network subnets]((/docs/vpc?topic=vpc-create-cluster-network-subnet&interface=ui)) as shown. Then, connect H100 or H200 instances to both the cluster network and the VPC networks. VPC networks connect your cloud resources and can provide external routing. Cluster networks add interfaces within your instance to enable direct connectivity between nodes.

![Networking H100 and H200 instances to use RDMA on IBM Cloud](images/cn-config-hopper-1.svg "Networking H100-enabled instances to use RDMA on IBM Cloud"){: caption="Networking Hopper 1 enabled instances to use RDMA on IBM Cloud" caption-side="bottom"}

### Use case 2: Securing your cluster network from the rest of the IBM Cloud ecosystem
{: #cn-secure-cluster-network}
{: #cn-use-case-b}

The cluster network operates within a separate network domain from the VPC network. This isolation provides security without requiring access controls, such as security groups, network ACLs, or routing tables. Communication within the cluster network occurs only between devices that are directly connected to the cluster network.

Resources that are connected to the cluster network must also connect at least one VPC network. The VPC network supports all standard VPC resources, such as floating IP addresses, public gateways, transit gateways, and more. It is recommended that you review your security policies for the Virtual Network Interfaces (VNIs) attached to resources on the cluster network.

![Securing your cluster network from the rest of the IBM Cloud ecosystem](images/clusternetwork_usecase2.svg "Securing your cluster network from the rest of the IBM Cloud ecosystem"){: caption="Securing your cluster network from the rest of the IBM Cloud ecosystem" caption-side="bottom"}

To maintain minimal access to the cluster network:

* Limit access to only the instances that are connected to the cluster network.
* Configure a strict security group for the VNIs on each instance with access to the cluster network.

   Carefully guard inbound TCP requests and consider guarding outbound TCP requests. For more information, see [Setting up a security group for your resource](/docs/vpc?topic=vpc-configuring-the-security-group&interface=cli).
   {: note}

* Make sure that the subnets attached to cluster network-enabled instances have appropriate network ACLs.
* Configure IAM policies for cluster network permissions.

## Related links
{: #related-links-cluster-network}

* [Quotas and service limits](/docs/vpc?topic=vpc-quotas&q=service+limits&tags=vpc#cluster-networks-quotas)
* [IAM permissions](/docs/account?topic=account-iam-service-roles-actions#is.cluster-network-roles)
* [AT events](/docs/vpc?topic=vpc-at_events&q=tracker&tags=vpc#events-cluster-network)
* [FAQs](/docs/vpc?topic=vpc-faqs-cluster-network)
* [GPU profile information](/docs/vpc?topic=vpc-profiles#gpu)
