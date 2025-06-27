---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-27"

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for auto scale
{: #faqs-auto-scale}

## What elements do I need to create to set up auto scaling?
{: #faq-auto-scale-0}
{: faq}

If you are using {{site.data.keyword.cloud_notm}} console, you need to create an instance template, an instance group, and if
you choose the dynamic scaling method, you must create scaling policies. For more information, see [Setting up auto scale with
the UI](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#setting-up-autoscale-overview). If you are using the {{site.data.keyword.cloud_notm}} CLI
or API you must also create an instance group manager. For more information, see
[Setting up auto scale with the CLI](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=cli#setting-up-autoscale-cli).

## How much am I charged for using auto scale?
{: #faq-auto-scale-1}
{: faq}
 
Auto scale for VPC is free, but you are charged for the resources that you consume. For example, you are charged for virtual server
instances that are created in the instance group.

## How does auto scaling work?
{: #faq-auto-scale-2}
{: faq}

You set scaling policies that define your desired average utilization for metrics like CPU, memory, and network usage. The
policies that you define determine when virtual server instances are added or removed from your instance group.

Auto scale uses the following computation to determine how many instances are running at any given time:

```sh
Σ(Current average utilization of each instance)/target utilization = membership count
```

For more information about how it works, see [Auto Scale for VPC](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#auto-scale-vpc).

## What permissions do I need for using auto scale?
{: #faq-auto-scale-3}
{: faq}

You can check the required permissions for actions on instance templates, instance groups, instance group managers,
memberships, and policies in the [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started&interface=ui).
For more information about using {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) to assign users access,
see [Granting user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).

## Can I set multiple scaling policies?
{: #faq-auto-scale-4}
{: faq}

You can set scaling policies for these metrics: CPU utilization (%), RAM utilization (%), Network in (Mbps), Network out (Mbps).
You can define more than one target metric policy, but only one policy for each type of metric.

## Are there any limitations on network interfaces or floating IP addresses when I use auto scale?
{: #faq-auto-scale-5}
{: faq}

Instance groups do not support instance templates that have the following configurations:
- Secondary network interfaces are not supported. Only one, primary network interface for an instance template is supported         in an instance group.
- A primary IP address or floating IP addresses assigned to the primary interface is not supported.

## Can I use custom metrics for auto scale?
{: #faq-auto-scale-6}
{: faq}

No. Currently custom metrics are not supported.

## How many IP addresses are available for instances?
{: #faq-auto-scale-7}
{: faq}

Currently 6 IP addresses in each subnet are allotted as overhead. The remaining IP addresses in the subnet are available to assign to instances that are provisioned in the instance group.

Ensure that you use a subnet size of 32 or greater. Using the same subnet for multiple instance groups can create capacity issues.

## How are instances balanced across subnets?
{: #faq-auto-scale-8}
{: faq}

Currently instances are provisioned at random to one of the instance group's subnets.

## What happens when I update an instance template?
{: #faq-auto-scale-9}
{: faq}

When the instance template that is used by an instance group is updated, all future instances that are created for the instance group use the new instance template. No changes are made to existing instances in the instance group.

## Can I update all instances in an instance group with a new instance template?
{: #faq-auto-scale-10}
{: faq}

You can update all of the instances in an instance group by deleting the existing memberships and applying a new instance template. For more information, see [Pausing auto scale to apply a new instance template](/docs/vpc?topic=vpc-managing-instance-group#pausing-for-maint).

## How do I add health checks?
{: #faq-auto-scale-11}
{: faq}

You can add health checking by associating a load balancer when you create your instance group. For more information about load balancers, see the following topics:

- [About IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancers-about).
- [About IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-network-load-balancers).

For more information about creating a load balancer, a load balancer pool, and configuring health checks, see the following topics:

- [Creating an IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancers)
- [Creating an IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer)
- [Creating an IBM Cloud Private Network Load Balancer with routing mode for VPC](/docs/vpc?topic=vpc-nlb-vnf)
- [Integrating a load balancer with instance groups](/docs/vpc?topic=vpc-lbaas-integration-with-instance-groups)
- [Working with health checks](/docs/vpc?topic=vpc-alb-health-checks)

## Do I need to pre-provision resources for auto scaling?
{: #faq-auto-scale-12}
{: faq}

During an auto-scaling event, auto scale dynamically allocates instances according to the instance template defined in the instance group. Instance templates do not support a secondary network interface. If you want to include a secondary network interface as part of an instance provisioned by auto scale, you must create that resource separately and attach it to the instances after they are provisioned.

## Why isn't my instance group scaling?
{: #faq-auto-scale-13}
{: faq}

Instance groups can fail to create instances for various reasons. You can use {{site.data.keyword.atracker_full_notm}} to find specific details related to instance group events. For more information, see [Instance group events](/docs/vpc?topic=vpc-at_events#events-compute-instance-group).

## How does the application port function if I set a port range for the network load balancer listener instead of a single port?
{: #faq-auto-scale-14}
{: faq}

If you set a port range for the network load balancer listener, then the instance group's application port will only be used for checking the health status of back-end members if you did not set a health check port for the pool.

## How do I know which load balancer offerings support auto scaling?
{: #faq-auto-scale-15}
{: faq}

Not all network load balancer offerings support integration with instance groups. Load balancers support auto scaling if the `instance_groups_supported` property of the [load balancer detail](/apidocs/vpc/latest#get-load-balancer) is `true`.
