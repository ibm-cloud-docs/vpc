---

copyright:
  years: 2020
lastupdated: "2020-07-30"

keywords: load balancer, autoscaling, instance groups, managed pool, load balancer for vpc, pool

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Integrating a load balancer with instance groups
{: #lbaas-integration-with-instance-groups}

IBM Cloud Application Load Balancer allows you to attach [instance groups](/docs/vpc?topic=vpc-creating-auto-scale-instance-group) to a load balancer pool.
{: shortdesc}

An instance group is a collection of virtual server instances. With them, you no longer need to manually manage your pool members. Attaching an instance group to a load balancer allows your pool backend members to automatically scale up/down depending upon your usage and requirements.

You can choose to have a static number of instances or choose to dynamically scale instances according to your requirements.
The pool members `add`/`delete` based on the `application health checks` and the `instance group policies` you configure.

For example, when application health checks fail, the system automatically adds a new membership to the instance group to replace the bad member.

In addition, members can be `added`/`deleted` to the pool based on your CPU, network, and memory utilization scaling policies.

When you attach an instance group with a pool, it becomes a managed pool, and you can no longer `add`/`delete`/`update` members to it. However, updates to the pool are still allowed.

The maximum number of backend members allowed in a load balancer pool is 50, and you cannot configure an instance group membership count beyond that limit.
{: important}

## Creating managed pools and instance groups
{: #creating-pools-groups}

To create a managed pool, first create a regular pool and ensure there are no members attached to it. You can then attach an instance group.

To create an instance group:

1. [Create an instance group template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-instance-template).

  An instance group template defines your backend member instance configurations.

2. Next, [create an instance group](/docs/vpc?topic=vpc-creating-auto-scale-instance-group). During the instance group creation, you specify the application port, load balancer and pool identities.

  For an existing instance group you can update it with the load balancer pool identity. You should also choose a scaling method. Options include:

   * A static method that allows a fixed number of backend members
   * A dynamic method for utilization based scaling

3. Finally, [create your instance group scaling policies](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-scaling-policies).

  Configure scaling policies only if you are using a dynaminc scaling method.

  When configuring these policies, you define certain metrics (like the CPU utilization percentage) and the desired target utilization for that metric. Together, the metric and the average target utilization determine when your instance group dynamically adds or removes virtual server instances from the group.
  {: tip}

## Related links
{: #lbaas-autoscaling-related-links}

* [Creating an instance group template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-instance-template)
* [Creating an instance group](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-instance-group)
* [Creating scaling policies](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#creating-scaling-policies)
* [FAQs for application load balancer](/docs/vpc?topic=vpc-load-balancer-faqs)
* [FAQs for auto scale](/docs/vpc?topic=vpc-faqs-auto-scale)
