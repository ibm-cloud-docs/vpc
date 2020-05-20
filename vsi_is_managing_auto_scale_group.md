---

copyright:
  years: 2020 
lastupdated: "2019-05-20"

keywords: manage auto scale group, auto scale, autoscale, UI, console

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
{:table: .aria-labeledby="caption"}

# Managing an instance group (Beta)
{: #managing-instance-group}

When you have an instance group created, you can duplicate it, delete it, or edit it (scaling method, group size, and target policies for 
scaling).   
{:shortdesc}

To manage your instance group from the Instance groups for VPC page, complete the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) >
VPC Infrastructure > Auto scale > Instance groups**.
2. On the Instance groups for VPC page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance group that you want to manage and select from the options, **Duplicate** or **Delete**. 
3. To edit an instance group, click the instance group that you want to edit to access the **Instance group details** page. 

## Duplicating an instance group
{: #duplicating-instance-group}

Duplicating an instance group makes a copy of the current settings and presents you with the New instance group page, with fields pre-populated. You can make any necessary changes and then create a new instance group based off of the instance group that you duplicated.

## Deleting an instance group
{: #deleting-instance-group}

If you no longer need the instance group, you can delete the instance group to permanently remove it from your account.


## Editing an instance group 
{: #editing-instance-group}

You can access the instance group details page to make changes to the instance group. You can change the scaling method between static and dynamic methods. You can adjust the instance group size. And you can edit or add target policies for scaling. 


### Changing the membership count of a static instance group
{: #changing-membership-count}

If you are using the static method for your instance group, the instance group works to always maintain the number of instances you have set. If any instances fail, the instance group automatically replaces those failed instances in the instance group. By maintaining the instances in the group, the availibility of the applications running in those instances is also enhanced. 

You can manually scale the instances in the group at any time by editing the membership count. 

- On the Instance group details page, edit the membership count and specify the number of instances you desire to have in your instance group. 

### Changing the scaling method for an instance group
{: #changing-scaling-method}

If you start out with a static membership in your instance group, for example, you can change it to use the dynamic scaling method. (You can also switch from dynamic scaling method to the static scaling method.) With the dynamic scaling method, you set a minimum and maximum number of instances for the instance group. Then, you specify target policies that define the desired utilization of a metric (such as vCPU) that you want the instance group to maintain.

To change an instance group from using static membership to dynamic scaling, complete the following steps:

1. From the menu in the upper right-hand corner of the instance group details page, select **Switch scaling method**. 
2. On the **Switch to dynamic** page, click **Switch** to confirm that you want to change to the dynamic scaling method.
3. Specify the minimum and maximum number of instances that you want to have running in your instance group at any given time.
4. Specify the aggregation window. The aggregation window is the time period in seconds that the auto scale manager monitors each instance and determines the average utilization.
5. Specify the cool down period, the number of seconds to pause further scaling actions after scaling has taken place.
6. Create one or more target policies that define a metric and target utilization that the auto scale manager will use for auto scaling in your instance group. See Table 1 for more information. 

| Field | Value |
|-------|-------|
| Metric type | Select the metric type that you want to associate with a target utilization value to use for adding or removing instances from your group. You can choose one of these metrics: CPU utilization (%), RAM utilization (%), Network in (Mbps), Network out (Mbps). You can define more than one target scaling policy, but only one policy for each metric type. |
| Average target utilization | Specify a desired average utilization for the metric that you select. This target value defines when the auto scale manager should scale up instances or scale down instances in your group. At the end of each aggregation window, the auto scale manager adds the current utilization of each instance and divides it by this target utilization value to determine the membership count. |
{: caption="Table 1. Target policies selections" caption-side="top"}

### Creating target scaling policies
{: #creating-target-policies}

For the dynamic scaling method, you define certain metrics (like vCPU) and the desired target utilization for that metric. Together, the metric and the average target utilization, determine when your instance group should dynamically add or remove virtual server instances from your group. 

To add scaling policies, complete the following steps. You can define more than one target scaling policy, but only one policy for each type of metric.

1. Make sure that the dynamic scaling method is enabled for the instance group. **Auto scale** will display under **Scaling method**. If the scaling method shows that the instance group is unmanaged, you can [change the scaling method](/docs/vpc?topic=vpc-managing-instance-group#changing-scaling-method) to dynamic.
2. Under **Target policies**, click **Add a policy** and complete the information in Table 2.

| Field | Value |
|-------|-------|
| Metric type | Select the metric type that you want to associate with a target utilization value to use for adding or removing instances from your group. You can choose one of these metrics: CPU utilization (%), RAM utilization (%), Network in (Mbps), Network out (Mbps). You can define more than one target scaling policy, but only one policy for each type of metric. |
| Average target utilization | Specify a desired average utilization for the metric that you select. This target value defines when the auto scale manager should scale up instances or scale down instances in your group. At the end of each aggregation window, the auto scale system adds the current utilization of each instance and divides it by this target utilization value to determine the membership count. |
{: caption="Table 2. Target policies selections" caption-side="top"}
