---

copyright:
  years: 2024
lastupdated: "2024-11-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning considerations for cluster networks
{: #planning-cluster-network}

Cluster Networks for VPC is available for select customers only. Contact IBM Support if you are interested in using this functionality.
{: preview}

Review the following planning considerations before creating a cluster network.
{: shortdesc}

## Considerations for cluster subnets
{: #cn-considerations-cluster-subnets}

* Instance profiles have a recommended number of cluster subnets. The {{site.data.keyword.cloud}} recommendation for cluster subnets for a given instance type is:

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
