---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-17"

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}

# FAQs for auto scale
{: #faqs-auto-scale}

## What elements do I need to create to set up auto scaling?
{: #faq-auto-scale-0}
{: faq}

If you are using {{site.data.keyword.cloud_notm}} console, you need to create an instance template, an instance group, and if 
you choose the dynamic scaling method, you must create scaling policies. For more information, see [Setting up auto scale with 
the UI](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#setting-up-autoscale-overview). If you are using the {{site.data.keyword.cloud_notm}} CLI
or API you must also create an instance group manager. For more information, see 
[Setting up auto scale with the CLI](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#setting-up-auto-scale-with-the-cli). 

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

```
Σ(Current average utilization of each instance)/target utilization = membership count
```

For more information about how it works, see [Auto Scale for VPC](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#auto-scale-vpc).

## What permissions do I need for using auto scale?
{: #faq-auto-scale-3}
{: faq}

You can check the required permissions for actions on instance templates, instance groups, instance group managers, 
memberships, and policies in the [Required permissions for VPC resources](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls). 
For more information about using {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) to assign users access, 
see [Granting user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).

## Can I set multiple scaling policies?
{: #faq-auto-scale-4}
{: faq}

You can set scaling policies for these metrics: CPU utilization (%), RAM utilization (%), Network in (Mbps), Network out (Mbps). 
You can define more than one target metric policy, but only one policy for each type of metric.

## Are there any limitations on network interfaces, floating IP addresses, or volumes when I use auto scale?
{: #faq-auto-scale-5}
{: faq}

Instance groups do not support instance templates that have the following configurations:
- Secondary network interfaces are not supported. Only one, primary network interface for an instance template is supported         in an instance group.
- A primary IP address or floating IP addresses assigned to the primary interface is not supported.
- Attached data volumes are not supported. 

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

You can add health checking by associating a load balancer when you create your instance group. For more information about load balancers, see [About IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancers). For more information about creating a load balancer, a load balancer pool, and configuring health checks, see [Creating an IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancer). For additional information, see [Integrating a load balancer with instance groups](/docs/vpc?topic=vpc-lbaas-integration-with-instance-groups) and [Working with health checks](/docs/vpc?topic=vpc-alb-health-checks).
