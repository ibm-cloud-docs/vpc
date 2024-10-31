---

copyright:
  years: 2024
lastupdated: "2024-10-31"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations for cluster networks
{: #planning-cluster-network}

Contact your IBM Support representative if you are interested in getting early access to this offering. It is currently available for early access users of the `gx3d-160x1792x8h100` [virtual server instance profiles](/docs/vpc?topic=vpc-profiles#gpu) in the `us-east` region.
{: beta}

Review the following planning considerations before creating a cluster network.
{: shortdesc}

## Compute quota considerations
{: #cn-considerations-quotas}

* Ensure that you have a sufficient [compute quota](/docs/vpc?topic=vpc-quotas&interface=ui) to support the resources you will deploy in to your cluster network. Typically, these instances require a quota increase prior to provisioning.

   Currently, NVIDIA H100 is the only instance profile that you can select. For more information about this profile, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui).
   {: note}

## Considerations for cluster subnets
{: #cn-considerations-cluster-subnets}

* Instance profiles have a recommended number of cluster subnets. To identify {{site.data.keyword.cloud}}'s recommendation for cluster subnets for a given instance type:

   | Instance Profile Type | Recommended Cluster Subnets to Cluster vNICs |
   | --------------------- | -------------------------------------------- |
   | gx3d-160x1792x8h100   | 1:1                                          |
   {: caption="Recommended number of cluster subnets for instance profiles." caption-side="bottom"}

   * `gx3d-160x1792x8h100` supports either 8, 16 or 32 cluster vNICs. As a result, you should have a corresponding number of cluster subnets.
   * If you are unsure about the correct number of cluster vNICs for your workload, IBM Cloud recommends using 8 for the `gx3d-160x1792x8h100` profile.

   Ensure that the IP address ranges used for your cluster network subnets do not overlap with those of your cloud subnets. Overlapping ranges can lead to networking issues.
   {: note}

 * Start by setting up your cluster network and subnets, then define instance templates to scale your training cluster with the desired number of nodes. For more information, see [Getting started with cluster networks](/docs/vpc?topic=vpc-about-cluster-network#cluster-network-getting-started).
   * You can [create an instance template](/docs/vpc?topic=vpc-create-instance-template&interface=ui) to define instance details for provisioning one or more virtual servers. When you create your instance template, you can use it to provision single virtual server instances, or to provision multiple instances at the same time as part of an instance group.
   * Optionally, when you provision an instance, you can include the cluster network configuration within the instance provision request.


## Considerations for statically-configured IP addressses
{: #cn-considerations-static-IP-address}

   * If you need to plan for statically-configured IP addresses, especially for a large number (such as 1000), it’s important to approach the task methodically. First, you should determine the total number of required IP addresses. Then consider how these addresses will be assigned to various instances. To manage this efficiently, implement a clear naming scheme that helps you organize and connect the IP addresses to their respective instances. For example, you might use a naming convention that reflects the function or location of each instance, making it easier to identify and manage them.

## Advanced considerations for large-scale enterprises
{: #cn-advanced-considerations}

   * If you are working in a large-scale IP range, ensure that the statically-configured IPs do not conflict. It is recommended that you let the VPC dynamically set the IP addresses on your cluster vNICs. This way, you can be assured the IPs will not conflict with any of the VPC default address prefixes. If you are connecting formerly isolated networks and the networks have VPC prefixes and classic infrastructure subnets that overlap, this can cause networking issues. If traffic does not appear to be routing to the correct network, this could be the issue.
   * If your cluster attached instances need to access services on IP ranges that may overlap with your cluster network's IP ranges, special routing rules must be taken into consideration. Ensure that within your cluster network, you choose an IP range that will be unique from any other services that your cluster-attached instances may want to access on the IBM Cloud network. 

You can only apply these advanced considerations through the API and CLI. 
{: note}
