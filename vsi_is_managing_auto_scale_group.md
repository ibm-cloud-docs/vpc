---

copyright:
  years: 2020, 2024
lastupdated: "2024-03-18"

keywords: auto scale, autoscale, UI, console

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing an instance group
{: #managing-instance-group}

After you create an instance group, you can duplicate it, delete it, or edit it to change the scaling method, group size, or target policies for scaling.
{: shortdesc}

To manage your instance group from the Instance groups for VPC page, complete the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Instance groups**.
2. On the Instance groups for VPC page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance group that you want to manage and select from **Duplicate** or **Delete**.
3. To edit or view the details of an instance group, click the instance group that you want to edit or view to access the **Instance group details** page.

## Duplicating an instance group
{: #duplicating-instance-group}

Duplicating an instance group makes a copy of the current settings and presents you with the new instance group page, with fields pre-populated. You can make any necessary changes and then create a new instance group that is based on of the instance group that you duplicated.

## Deleting an instance group
{: #deleting-instance-group}

If you no longer need the instance group, you can delete the instance group to permanently remove it from your account. All instances that are part of the instance group are deleted as well.

## Editing an instance group
{: #editing-instance-group}

You can access the instance group details page to change the instance group. You can change the scaling method between static and dynamic methods. You can adjust the instance group size. And you can edit or add target policies for scaling.

### Editing the details for an instance group
{: #editing-details-group}

On the Instance group details page, you can use the pencil icon to change the following items:

* **Name**
* **Subnets**: Subnet IP address capacity is validated against the size of the instance group. The membership count of the group cannot exceed the total number of IP addresses that are available to assign across your subnets. Currently, 6 IP addresses per subnet are allotted as overhead and not available to assign to instances.
* **Load balancer pool**: If the membership count of an instance group is set to 0, you can change the load balancer pool that is associated with the instance group. You can select a different load balancer that is available, or select **none** to stop using an assigned load balancer.
* **Instance template**: You can select a different instance template to be used to provision any new instances that are created for the instance group. Existing instances are not updated with the new instance template.
* **Membership count**: For more information, see [Changing the membership count of a static instance group](/docs/vpc?topic=vpc-managing-instance-group#changing-membership-count). You cannot manually adjust the membership count for an instance group that is using the dynamic auto scale method, unless dynamic auto scaling is disabled.

### Changing the membership count of a static instance group
{: #changing-membership-count}

If you are using the static method for your instance group, the instance group works to always maintain the number of instances that you set. If any instances fail, the instance group automatically replaces those failed instances in the instance group. By maintaining the instances in the group, the availability of the applications that are running in those instances is also enhanced.

You can manually scale the instances in the group at any time by editing the membership count.

* On the Instance group details page, edit the membership count and specify the number of instances that you want in your instance group.

### Changing the scaling method for an instance group
{: #changing-scaling-method}

If you start out with a static membership in your instance group, for example, you can change it to use the dynamic scaling method. (You can also switch from dynamic scaling method to the static scaling method.) With the dynamic scaling method, you set a minimum and maximum number of instances for the instance group. Then, you specify target policies that define the utilization of a metric (such as CPU) that you want the instance group to maintain.

To change an instance group from using static membership to dynamic scaling, complete the following steps:

1. From the menu in the upper-right of the instance group details page, select **Switch scaling method**.
2. On the **Switch to dynamic** page, click **Switch** to confirm that you want to change to the dynamic scaling method.
3. Specify the minimum and maximum number of instances that you want to run in your instance group at any time. You can edit these numbers later.
4. Specify the aggregation window. The aggregation window is the time period in seconds that the instance group manager monitors each instance and determines the average utilization.
5. Specify the cool down period, the number of seconds to pause further scaling actions after scaling takes place.
6. Create one or more target policies that define a metric and target utilization that the instance group manager can use for auto scaling in your instance group. For more information, see Table 1 for more information.

| Field | Value |
|-------|-------|
| Metric type | Select the metric type that you want to associate with a target utilization value to use for adding or removing instances from your group. You can choose one of these metrics: CPU utilization (%), RAM utilization (%), Network in (Mbps), Network out (Mbps). You can define more than one target scaling policy, but only one policy for each metric type. |
| Average target utilization | Specify the average utilization for the metric that you select. This target value defines when the instance group manager needs to scale up instances or scale down instances in your group. At the end of each aggregation window, the instance group manager adds the current utilization of each instance and divides it by this target utilization value to determine the membership count. |
{: caption="Table 1. Target policies selections" caption-side="bottom"}

### Creating target scaling policies
{: #creating-target-policies}

For the dynamic scaling method, you define certain metrics (like CPU) and set the target utilization for that metric. Together, the metric and the average target utilization determine when your instance group needs to dynamically add or remove virtual server instances from your group.

To add scaling policies, complete the following steps. You can define more than one target scaling policy, but only one policy for each type of metric.

1. Make sure that the dynamic scaling method is enabled for the instance group. **Auto scale** is under the **Scaling method**. If the scaling method shows that the instance group is unmanaged, you can [change the scaling method](/docs/vpc?topic=vpc-managing-instance-group#changing-scaling-method) to dynamic.
2. Under **Target policies**, click **Add a policy** and complete the information in Table 2.

| Field | Value |
|-------|-------|
| Metric type | Select the metric type that you want to associate with a target utilization value to use for adding or removing instances from your group. You can choose one of these metrics: CPU utilization (%), RAM utilization (%), Network in (Mbps), Network out (Mbps). You can define more than one target scaling policy, but only one policy for each type of metric. |
| Average target utilization | Specify the average utilization for the metric that you select. This target value defines when the instance group manager needs to scale up instances or scale down instances in your group. At the end of each aggregation window, the instance group manager adds the current utilization of each instance and divides it by this target utilization value to determine the membership count. |
{: caption="Table 2. Target policies selections" caption-side="bottom"}

For existing target scaling policies, you can edit them by using the pencil icon.
{: tip}

### Viewing and managing memberships
{: #managing-memberships}

From the Instance group details page, you can see the memberships, or virtual server instances that are part of the instance group. In the navigation pane, click **Memberships** to view memberships for an instance group.

You can complete the following tasks from the Memberships page:

* Access the Instance details page for a membership by clicking the name of the instance.
* Access the Instance template details page for the instance template that was used to provision the membership instance.
* Delete a membership and its associated virtual server instance. Click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the membership that you want to delete and select **Delete**.

### Disabling and enabling auto scale
{: #disabling-auto-scale}

If you are using the dynamic method for auto scale, you can temporarily disable dynamic auto scaling for an instance group. You might choose to pause the dynamic scaling method to perform maintenance on an instance group to save money at the end of a billing cycle, or to create all new instances that use a new instance template. When auto scale is disabled, instances are not automatically added or removed. (You can still manually adjust the membership count of your instance group.)

* In the **Scaling method** section of the Instance group details page, you can use the Auto scale slider to change from green **Enabled** to **Disabled**.
* When auto scale is **Disabled**, you can change it to **Enabled**.

#### Pausing auto scale to apply a new instance template
{: #pausing-for-maint}

You can temporarily disable dynamic auto scaling for an instance group to create all new instances with a new instance template. Complete the following steps to remove existing instances in the group and create new instances with a new instance template:

1. When the dynamic auto scale scaling method is enabled, move the slider in the **Scaling method** section from **Enabled** to change it to **Disabled**.
2. With Auto scale set to **Disabled**, change your **Membership count** for the instance group to 0. All of your existing instances in the group are removed.
3. For the **Instance template** field, use the pencil icon to edit and select a new instance template to use for creating instances in the instance group.
4. Change the **Membership count** for the instance group to the number of instances that you want to provision with the new instance template. For example, you can specify a membership count of _5_.
5. When you are ready to use the dynamic scaling method again, move the slider in the **Scaling method** section from Disabled** to change it to **Enabled**. The instance group creates five instances that use the new instance template.

## Instance groups and reservations
{: #instance-group-and-reservations}

Keep the following information in mind when you use an instance group with a reservation. For more information about reservations, see [About Reservations for VPC](/docs/vpc?topic=vpc-about-reserved-virtual-servers-vpc).

The instance prototype definition, and thus the instance template includes a new reservation property to enable an instance to attach to a specific reservation.

An instance template resource is required when you create an autoscale group. If the instance template is created with a reservation identifier, any instances that are started by the autoscale group attempt to attach to the specified reservation.

## Best practices for autoscale groups with reservations
{: #best-practices-autoscale-with-reservations}

The size of the autoscale group does not need to match the size (quantity) of the reservation capacity. The autoscale group membership can't exceed the reservation size. For example, you can create a reservation for 100 instances, but the autoscale group can be configured to a maximum membership size of 50. Meaning, that the autoscale group consumes at most 50 units of the reserved capacity.

The autoscale group can be created or updated to have a larger size than the reservation capacity, but this larger size is not recommended. Instances that are created by the scale group might fail to provision if the group is larger than the reservation.

If the reservation does not have available capacity for an autoscale instance to start, the existing autoscale self-healing feature retries the instance provisioning to attain the autoscale membership count. After the retries expire (10 minutes), the autoscale group status reports as _unhealthy_. The autoscale error array reports the instance status failure messages.

If the available capacity of a reservation increases after the autoscale self-healing expires, the autoscale group doesn't automatically retry to attain the membership count.

You can create autoscale groups with up to three subnets, one for each zone. However, the instance template that is associated with the autoscale group can target only a single reservation, which is scoped to exactly one zone. Therefore, if you use an instance template that targets a reservation, it is recommended that the autoscale group contain only a single subnet and that subnet's zone needs to match that of the reservation. If multiple subnets are used, the instances that are created in zones that are not the same as the reservation's zone fail.
