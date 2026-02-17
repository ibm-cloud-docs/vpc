---

copyright:
  years: 2025, 2026
lastupdated: "2026-02-16"

keywords: spot virtual server, spot virtual server instance, spot instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Spot instances
{: #spot-instances-virtual-servers}

Spot instances are highly discounted versions of the standard instances. They are designed to use available compute resources for interruptible or stateless workloads. Spot instances can be preempted (or evicted) at any time.
{: shortdesc}

With Spot instances, you can choose the best compatible profile that fits your workload characteristics, performance needs, and price point. Spot instances are ideal for fault-tolerant, interruptible workloads because IBM Cloud VPC preempts these instances based on resource demand.

Spot instances are ideal for, but not limited to, the following workloads.

* Development test servers
* Data analytics
* Image rendering
* Containerization
* Batch processing

## Spot instance benefits
{: #spot-instance-benefits}

Spot instances have the following benefits.

* Cost-efficient
* Scalable - you can add or remove Spot instances through [Auto scale for VPC](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui#auto-scale-vpc)]
* Spot instances don't use the standard instance quota. Spot instances have their own quotas (the default is 100).
* No minimum runtime
* Resizable (instance must be stopped to resize)

## Spot instance limitations
{: #spot-instances-limitations}

Spot instances aren't compatible with the following features.

* Burstable instances
* Dedicated hosts
* Reservations

## Spot instances details
{: #spot-instances-details}

* Spot instances support only [GPU profiles](/docs/vpc?topic=vpc-accelerated-profile-family) and [Flex profiles](/docs/vpc?topic=vpc-profiles&interface=ui#flexible-profiles).
* [Auto Scale](/docs/vpc?topic=vpc-creating-auto-scale-instance-group) and instance groups support Spot instances.
* Placement groups are supported, but keep in mind that using placement groups can cause capacity errors.

Spot instances aren't guaranteed available capacity. If capacity isn't available, you can try another zone or region or try again later.
{: note}

Instance profiles have a property that is called availability_class that is either "standard", "spot", or both. If "spot" is in the array of values, you can use the profile to create a spot instance.

When you create an instance, you need to set a couple of new fields:

* availability.class needs to be "spot"
* availability_policy.preemption can be "stop" or "delete" (the default is "stop")

See the following example of the properties in an API request.

```curl
{
   "availability": {
       "class": "spot"
   },
   "availability_policy": {
       "preemption": "delete"
   },
   ...
}
```

### Preemption
{: #spot-instances-preemption}

Preemption of a Spot instance is when the platform determines it is necessary to evict the spot instance. At the start of preemption, the preemption policy is applied and a best-effort preemption grace period of 30 seconds begins. At the end of this grace period, the spot instance is forcefully shut down.

The preemption policy determines the behavior of the Spot instance when the instance is preempted. The `availability_policy.preemption` property when added to supported instances has a value of "stop" or "delete". This property applies only to spot instances where `availability.class = spot`.

* "stop" - the instance goes into the stopped state when the preemption completes. You can start the instance again or delete it.
* "delete" - the instance is deleted when after preemption.

Changes that are made to the preemption policy during a preemption aren't applied until the next preemption.

You are notified of a preemption in two ways.

* A graceful shutdown command is issued within the operating system of the Spot instance. This shutdown command can be programmatically responded to within the guest.
* When a spot instance is selected for preemption, a user-facing log is sent at the beginning of the preemption process. Example: "instance.00002": "Spot instance {{.instanceID}} is selected for preemption". After 30 seconds and if the spot instance is not yet stopped, a forced shutdown is issued within the guest. For more information about logging, see [Logging for VPC](/docs/vpc?topic=vpc-logging).

Spot instances that are deleted due to scaling down with an instance group aren't considered preempted so the preemption policy is ignored.
{: note}

When a spot instance is preempted, a graceful shutdown command is sent to the spot instance operating system. This shutdown request can be intercepted and a shutdown script can run by creating the shutdown script and adding it to a `systemd` service file. An example script is at `usr/local/bin/ibm-cloud-shutdown-script.sh`.

```bash
#!/bin/bash
OUTPUT_FILE="/tmp/counter.out"
counter=0
while true; do
    echo "$counter" > "$OUTPUT_FILE"
    ((counter++))
    sleep 1
done
```
{: codeblock}

This script is a placeholder for running a shutdown. To run the script from the `systemd` service file, add the path to your script in the **ExecStop** field of the service file as shown in the following example.

Example `systemd` service file that is at `/etc/systemd/system/ibm-cloud-shutdown-script.service`:

```text
[Unit]
Description=IBM Cloud Shutdown Script
Wants=network-online.target rsyslog.service
After=network-online.target rsyslog.service
[Service]
Type=oneshot
ExecStart=/bin/true
RemainAfterExit=true
ExecStop=/usr/local/bin/ibm-cloud-shutdown-script.sh
TimeoutStopSec=0
KillMode=process
[Install]
WantedBy=multi-user.target
```
{: codeblock}

The time between the graceful shutdown command and the subsequent force stop command is 30 seconds. Therefore, if a shutdown script is still running after the 30 seconds, it is interrupted by the force stop. This 30-second grace period is a best-effort attempt and can vary depending on circumstances.

## Re-creating a spot instance by using an instance group
{: #recreate-spot-instance}

When a spot instance is preempted, if it's a member of an instance group, then the instance group attempts to create a spot instance to replace the preempted spot instance. The new spot instance is created and attached to one of the subnets that are specified in the instance group subnet array.

If you want to automatically re-create a spot instance after preemption, you can use an instance group to re-create these spot instances.

1. [Create an instance template](/docs/vpc?topic=vpc-creating-auto-scale-instance-group) that fits your needs with instance availability.class to `spot` and the availability_policy.preemption to `delete`.
1. [Create an instance group](/docs/vpc?topic=vpc-creating-auto-scale-instance-group) with the instance template that you created.

   To increase the likelihood of finding capacity for your spot instance (if capacity is constrained in one zone), specify multiple subnets that are spread across zones in the region for high-availability.
   { :tip}

To keep your preempted instances in a stopped state, then you need to specify `stop` for the availability_policy.preemption_ configuration. After the spot instance is created by the instance group, you need to submit a [PATCH /instance_groups/{instance_group_id}/memberships/{id}](/apidocs/vpc/latest#update-instance-group-membership) and specify `false`for the [delete_instance_on_membership_delete](/apidocs/vpc/latest#delete-instance-group-memberships) property.
