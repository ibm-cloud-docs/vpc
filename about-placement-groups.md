---
copyright:
  years: 2021, 2024
lastupdated: "2025-06-27"

keywords: virtual private cloud, private cloud network, cloud-native, workloads, high availability, generation 2, placement group, host spread, power spread

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About placement groups
{: #about-placement-groups-for-vpc}

## Overview
{: #overview-placement-groups-for-vpc}

Placement groups for {{site.data.keyword.vpc_full}} are used to create placement strategies for managing high availability workloads. A placement group contains virtual server instances that share a common placement strategy. Placement strategies influence the physical placement of select VPC resources to meet certain workload demands.
{: shortdesc}

Placement groups and their assigned resources can be managed by using the UI, CLI, API, and Terraform. A placement group can have 1 of two placement strategies: [host spread](#host-spread-placement-groups-for-vpc) or [power spread](#power-spread-placement-groups-for-vpc). With a VPC resource called `placement-groups`, you can create a policy for placing groups of instances together. The `placement-groups` resource is then added to the service `is.placement-groups`. For more information about actions that are related to placement group resources, see the placement group events section in [Activity tracking events](/docs/vpc?topic=vpc-at_events#events-placement-group).

After the placement group is created, a selected virtual server instance or a group of virtual server instances are assigned to the placement group. When you provision these virtual server instances, the virtual server instances are then placed on a compute host in the appropriate zone for the instance based on the placement group strategy. The same placement group can be used for instances that are provisioned on shared public servers in different VPCs.

### Benefits
{: #placement-groups-benefits}

Placement groups give you a measure of control over the host on which a new public virtual server is placed in relation to other virtual servers in the same placement group.

They support high availability workloads by making sure that virtual server instances in the group do not share a physical host or power supply. This technology is an added layer of assuredness for the resiliency of your virtual server instances if an unexpected power disruption or host failure occurs.

You can build a highly available application within a zone knowing that your virtual servers are isolated from each other. You can be assured that your applications are provisioned on the cloud zone infrastructure to maximize availability with unique host server hardware.

## Understanding placement strategies
{: #understand-placement-strategies}

Placement groups for VPC have two different anti-affinity strategies for high availability. Using placement strategies minimizes the chance of service disruption with virtual server instances that are placed on different hosts or into an infrastructure with separate power and network supplies.

### Host spread strategy (host anti-affinity) for high availability.
{: #host-spread-placement-groups-for-vpc}

A host spread placement group strategy guarantees that each instance is placed on separate compute hosts. This placement group strategy avoids the host from being a single point of failure. This placement policy supports a maximum of 12 instances per placement group.

Instance provisioning fails if an instance cannot be placed on a different node than all other instances in the placement group.

### Power spread strategy (power anti-affinity) for high availability.
{: #power-spread-placement-groups-for-vpc}

A power spread placement group strategy guarantees each instance is placed on computer hosts with separate power and network supplies to minimize the chances of service disruption. This placement group strategy supports a maximum of four instances per placement group.

Instance provisioning fails if the instance cannot be placed on a different power supply than all other instances in the placement group.

If a placement group is not specified when the instance is provisioned, the default placement strategy is to distribute customer instances among as many different physical hosts as possible. This approach is a best effort basis and does not guarantee the instances are placed on different physical hosts. To guarantee different physical hosts, specify a placement group during instance provisioning.
{: note}

## Using placement groups with instance groups
{: #placement-instance-groups}

Instance groups (also known as autoscale groups) support instances that are provisioned with a placement group specification. Instance templates (used by instance groups) inherit the new placement group attribute. Any instances that are started within an autoscale group with a specified placement group are placed according to the placement group strategy. The instance group size is limited to the maximum size of the placement group.

Since placement groups for high availability strategies can use instances from multiple zones, you can use instance groups to support instances with subnets that span zones. For more information about instance groups, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

## Placement groups when an instance is resized
{: #instance-resize-placement-groups-for-vpc}

When an instance is resized, the instance is stopped, the profile is updated, and the instance is restarted. When the instance is stopped, it is removed from its assigned node. When the instance is started again, the instance is then placed according to its defined placement group strategy, if a placement strategy exists for that instance. For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance).

## Common use cases
{: #placement-group-use-cases}

* Workload needs highest availability for critical application instances.
   * Issue: Distributed application or database experiences an outage if any one of the application parts experience a failure.
   * Solution: Use power spread placement group policy for independent power and network.
* Workload needs to optimize topology-aware applications.
   * Issue: Modern database uses triple replication for redundancy therefore must make sure that performance for database components while also optimizing high availability for replicate parts of the database.
   * Solution: Use power spread placement group policy to group database components on different power supplies.

## Limitations
{: #limitations-placement-groups-for-vpc}

* The VPC must exist before you create a placement group. If the VPC isn't created before you create a placement group, you receive an error and the placement group isn't created.
* The quotas have a set limit and can't be adjusted. For more information about placement group quotas, see the placement group quotas section in [Quotas and service limits](/docs/vpc?topic=vpc-quotas#placement-group-quotas).

## Restrictions
{: #restrictions-placement-groups-for-vpc}

The following are the restrictions for placement groups:

- The placement group placement strategy can't be modified after the placement group is created.
- The placement group must be deleted and created with a new placement strategy.
- A placement group can't be deleted if it is attached to one or more instances.

The following are the restrictions for instances that are attached to a placement group:

- An instance can be in only one placement group.
- Instances that are provisioned with placement group strategies do not work with dedicated hosts.
- After an instance is placed, the assigned placement of that instance does not change based on placement of other instances.
- After an instance is started, the associated placement group strategy can't be changed.
- Instances can't be removed from a placement group or assigned to a different placement group. Instances must be deleted to remove the instance from the placement group.
- If you define a placement group for an instance, the instance provision fails if the placement can't be completed according to the defined placement group strategy.
- If an instance fails to provision due to lack of capacity, the instance placement is not automatically retried. You can stop and then start the instance to retry provisioning.

For more information about deleting a virtual private cloud instance and its associated resources, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting).
