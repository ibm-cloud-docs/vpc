---

copyright:
  years: 2020 
lastupdated: "2020-05-20"

keywords: auto scale, autoscale, virtual server instance, creating, UI, console

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:preview: .preview}
{:table: .aria-labeledby="caption"}

# Creating an instance group for auto scaling (Beta)
{: #creating-auto-scale-instance-group}

You can create an instance group in your {{site.data.keyword.vpc_short}} to auto scale according to your requirements by using the {{site.data.keyword.cloud_notm}} console. Based on the target utilization metrics that you define, the instance group can dynamically add or remove instances to achieve your specified instance availability. 
{:shortdesc}

Auto Scale for VPC is only available to accounts with special approval to preview this beta feature. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Auto scale for VPC
{: #auto-scale-vpc}

With Auto Scale for VPC, you can improve performance and costs by dynamically creating virtual server instances to meet the demands of your environment. You set scaling policies that define your desired average utilization for metrics like CPU, memory, and network usage. The policies that you define determine when virtual server instances are added or removed from your instance group. 

As an example, imagine that the fictitious company, Acme Web Retailer, has set up an instance group for auto scaling. They  define that they always want to maintain a minimum of 3 instances and a maximum of 7 instances. They create a dynamic scaling policy for CPU usage with their desired average utilization for instances at 70%. They set an aggregation window of 10 minutes, so the auto scale manager monitors each instance for 10 minutes before calculating the average utilization. If adjustments are needed to meet the target utilization across instances, the auto scale manager provisions or reclaims more instances as needed.   

Auto scale uses the following computation to determine how many instances are running at any given time:

```
Σ(Current average utilization of each instance)/target utilization = membership count
```

If Acme Web Retailer has four virtual server instances running when the aggregation window elapses, the formula looks like this: *VSI1 + VSI2 + VSI3 + VSI4 / 70% = membership count*. CPU utilization for the four running instances is 80%, 70%, 65%, and 85%, so the following computation takes place:

```
80% + 70% + 65% + 85% / 70% = 4.29 
```

Based on this calculation, the auto scale manager rounds **4.29** up to **5** and provisions another instance. Now Acme Web Retailer has a total of five instances and maintains their desired average CPU utilization across the instances in the group. 


## Setting up auto scale 
{: #setting-up-autoscale-overview}

Before you can create an instance group, you need to [create an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{:important}

To create an instance group for auto scale, you must complete the following tasks:
1. Create an instance template that is used to provision instances in your group.
2. Create an instance group in a single region that is made up of like virtual server instances. 
3. Choose a scaling method (static or dynamic) and create scaling policies. 

### Creating an instance template
{: #creating-instance-template}

An instance template is required before you can create an instance group for auto scaling. The instance template defines the 
details of the virtual server instances that are created for your instance group. For example, specify the profile (vCPU and 
memory), image, attached volumes, and network interfaces for the image template. All of the virtual server instances that are 
created for an instance group use the instance template that is defined in the instance group.

To create an instance template, complete the following steps.
1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Auto scale > Instance templates**. 
2. Click **New instance template** and enter the information in Table 1.
3. Click **Create instance template** when the information is complete.

| Field | Value |
|-------|-------|
| Name  | A name is required for your virtual server instance. |
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. |
| Resource group | Select a resource group for the instance. | 
| Tags |  You can assign a label to this resource so that you can easily filter resources in your resource list. | 
| Location | Locations are composed of regions (specific geographic areas) and zones (fault tolerant data centers within a region). Select the location where you want your virtual server instance to be created. |
| Image | All images use cloud-init, which allows you to enter user metadata associated with the instance for post provisioning scripts. |
| Profile |  Select from popular profiles or all available vCPU and RAM combinations. The profile families are Balanced, Compute, and Memory. For more information, see [Profiles](/docs/vpc?topic=vpc-profiles). |
| SSH Key | You must select an existing SSH key or upload a new SSH key to use before you can create the instance. SSH keys are used to securely connect to the instance after it's running. |
| | **Note:** Alpha-numeric combinations are limited to 100 characters. |
| | For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| User data | You can add user data that automatically performs common configuration tasks or runs scripts. For more information, see [User data](/docs/vpc?topic=vpc-user-data). |
| Boot volume | The default boot volume size for all profiles is 100 GB. |
| Data volumes | Data volumes are not supported for instance groups, so if you plan to use this instance template with an instance group do not include data volumes. Otherwise, you can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **New volume**. |
| Network interfaces | Defines the networking connection into the IBM Cloud VPC.  |
{: caption="Table 1. Instance template selections" caption-side="top"}

When you create an instance template, validation steps are performed to ensure that you can use this template to provision a virtual server instance. 
{: tip}

### Creating an instance group
{: #creating-instance-group}

An instance group is a collection of like virtual server instances. You define how many instances to maintain in the group. 
You can set a static number of instances or choose to dynamically scale instances according to your requirements.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Auto scale > Instance groups**.
2. Click **New instance group** and enter the information in Table 2. 
3. If you want to create dynamic scaling policies as part of instance group creation, see [Creating scaling policies](#creating-scaling-policies) for more information. You can also [add policies later](/docs/vpc?topic=vpc-managing-instance-group#creating-target-policies), after you create your instance group. 
3. Click **Create instance group** when the information is complete.

Instance groups do not support instance templates that have the following configurations:
  - Secondary network interfaces are not supported. Only one, primary network interface for an instance template is supported         in an instance group.
  - A primary IP address or floating IP addresses assigned to the primary interface is not supported.
  - Attached data volumes are not supported. 

| Field | Value |
|-------|-------|
| Name  | A name is required for your virtual server instance. |
| Virtual private cloud | Specify the IBM Cloud VPC where you want to create your instance. |
| Resource group | Select a resource group for the instance. |
| Region | Select the location where you want your virtual server instance to be created. |
| Subnets | Select the subnets where you want to create your instance group. To maximize the availability of your applications, select subnets in different zones. |
| Instance template |  Select the instance template that you want to use for provisioning the virtual server instances in your auto-scale instance group. All virtual server instances in the group are provisioned with the same instance template. |
| Scaling method | Select whether you want to use a dynamic or static scaling method. With the dynamic scaling method, instances are added or removed based on the metric targets that you specify. With the static scaling method, you can specify a fixed number of instances that you always want to maintain. |
| Instance group size | For a static group, enter the number of instances that you want to constantly have in this instance group. For a dynamic group, enter the minimum and maximum number of instances for your group. The number of instances will scale automatically within that range based on the target metrics that you define. |
| Aggregation window (seconds) | This value determines the time period that the auto scale manager monitors each instance and determines the average utilization. |
| Cooldown period (seconds) | The period of time in seconds to pause further scaling actions after scaling has taken place. |
{: caption="Table 2. Instance group selections" caption-side="top"}

### Creating scaling policies
{: #creating-scaling-policies}

For the dynamic scaling method, you define certain metrics (like CPU utilization percent) and the desired target utilization for that metric. Together, the metric and the average target utilization, determine when your instance group should dynamically add or remove virtual server instances from your group. 

To add scaling policies, complete the following fields on the **New instance group for VPC** page. If you need to add policies after your instance group is already created, see [add policies](/docs/vpc?topic=vpc-managing-instance-group#creating-target-policies).

| Field | Value |
|-------|-------|
| Metric type | Select the metric type that you want to associate with a target utilization value to use for adding or removing instances from your group. You can choose one of these metrics: CPU utilization (%), RAM utilization (%), Network in (Mbps), Network out (Mbps). You can define more than one target metric policy, but only one policy for each type of metric. |
| Average target utilization | Specify a desired average utilization for the metric that you select. This target value defines when the auto scale manager should scale up instances or scale down instances in your group. At the end of each aggregation window, the auto scale manager adds the current utilization of each instance and divides it by this target utilization value to determine the membership count. |
{: caption="Table 3. Scaling policies selections" caption-side="top"}
