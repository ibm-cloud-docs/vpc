---

copyright:
  years: 2022
lastupdated: "2023-04-24"

keywords: network load balancer, autoscaling, instance groups, managed pool, network load balancer for vpc, pool

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Integrating an {{site.data.keyword.cloud_notm}} {{site.data.keyword.nlb_full}} with instance groups
{: #nlb-integration-with-instance-groups}

{{site.data.keyword.cloud}} {{site.data.keyword.nlb_full}} (NLB) can attach [instance groups](/docs/vpc?topic=vpc-creating-auto-scale-instance-group) to a load balancer pool.
{: shortdesc}

An instance group is a collection of virtual server instances. With them, there is no need to manually manage your pool members. Attaching an instance group to a load balancer allows your pool back-end members to automatically scale up or down depending upon your usage and requirements.

You can choose to have either a static number of instances or to dynamically scale instances according to your requirements. When dynamically scaling, the number of pool members increases or decreases based on the application health checks and instance group policies you configure.

For example, when application health checks fail, the system automatically adds a membership to the instance group to replace the bad member.

In addition, members can be added or deleted to the pool based on your CPU, network, and memory utilization scaling policies.

When you attach an instance group with a pool, it becomes a managed pool, and you no longer can add or delete members to it or update them. However, you can still make updates to the pool itself.

The maximum number of back-end members that are allowed in a load balancer pool is 50, you cannot configure an instance group membership count beyond that limit.
{: important}

When you configure a listener with a [range of ports](/docs/vpc?topic=vpc-nlb-port-ranges), the instance group's application port is only used for checking the health status of back-end members.
{: note}

## Creating managed pools and instance groups
{: #nlb-creating-pools-groups}

To create a managed pool and attach an instance group, perform the following procedure:

1. [Create an instance group template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-instance-template).

   This template defines your back-end member instance configurations.

1. [Create an instance group](/docs/vpc?topic=vpc-creating-auto-scale-instance-group). When creating your instance group, specify the application port, load balancer, and pool identities.

    For an existing instance group, you can update it with the load balancer pool ID. You must also choose a scaling method from the following options:

    * A static method that allows a fixed number of back-end members
    * A dynamic method for utilization based scaling

1. [Create your instance group scaling policies](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-scaling-policies).

    Configure scaling policies only if you are using a dynamic scaling method.

    When you configure these policies, you define certain metrics (such as the CPU utilization percentage) and the target utilization you want for that metric. Together, the metric and the average target utilization determine when your instance group dynamically adds or removes virtual server instances to or from the group. 
    {: tip}

## Related links
{: #lbaas-autoscaling-related-links}

* [Creating an instance group template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-instance-template)
* [Creating an instance group](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-instance-group)
* [Creating scaling policies](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-scaling-policies)
* [Setting public network load balancer port ranges](/docs/vpc?topic=vpc-nlb-port-ranges)
* [FAQs for network load balancers](/docs/vpc?topic=vpc-nlb-faqs)
* [FAQs for auto scale](/docs/vpc?topic=vpc-faqs-auto-scale)
