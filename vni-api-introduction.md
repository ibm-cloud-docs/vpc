---

copyright:
  years: 2024
lastupdated: "2024-02-02"

keywords: virtual network interfaces, hazardous change remediation, mitigation

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attention: Preparing for behavior changes to virtual network interfaces, instances, bare metal servers, and file shares
{: #vni-api-introduction}

Starting 12 March 2024, a new feature will be enabled in the Virtual Private Cloud (VPC) service that expands the support for [virtual network interfaces](/docs/vpc?topic=vpc-vni-about). If used in your account, this feature will introduce behavior changes to instances, bare metal servers, and file shares. These changes may affect your automation or workflows for managing these resources.
{: preview}

If you have workflows or automation for managing your instances, bare metal servers, and file share mount targets, read this guidance regardless of whether you plan to use virtual network interfaces.
{: important}

## What are we changing?
{: #vni-api-changes}

When this feature becomes available on 12 March 2024:

- You will be able to create [instances](/apidocs/vpc/latest#create-instance) and [bare metal servers](/apidocs/vpc/latest#create-bare-metal-server) with virtual network interfaces attached to new child resources called network attachments. You'll be able to specify a `primary_network_attachment` (instead of a `primary_network_interface`) and provide either the identity of an already-created virtual network interface, or a subnet to create a new virtual network interface for the instance or bare metal server.
- Virtual network interfaces will have lifecycles that are independent of the resources they are attached to. You will be able to [update](/apidocs/vpc/latest#update-virtual-network-interface) the `auto_delete` property to `false` to allow a virtual network interface to persist beyond the lifecycle of its original bare metal server or instance, and be re-attached to another bare metal server or instance.
- Virtual network interfaces will support [secondary IP addresses](/docs/vpc?topic=vpc-vni-about-secondary-ip). Reserved IPs can then be [added](/apidocs/vpc/latest#add-virtual-network-interface-ip) to and [removed](/apidocs/vpc/latest#remove-virtual-network-interface-ip) from a virtual network interface.
- For compatibility with existing clients, instances and bare metal servers with virtual network interfaces will include a read-only representation of their network attachments and virtual network interfaces as legacy network interface child resources. Learn about [support for old API clients](/docs/vpc?topic=vpc-vni-about&interface=ui#vni-old-api-clients).
- For instances and bare metal servers with virtual network interfaces, the IAM permissions for options to allow IP spoofing or disable infrastructure NAT will be managed on their attached virtual network interfaces. When [creating](/apidocs/vpc/latest#create-virtual-network-interface) or [updating](/apidocs/vpc/latest#update-virtual-network-interface) a virtual network interface, you will be able to set non-default values for the `allow_ip_spoofing` and `enable_infrastructure_nat` properties only if you have the `is.virtual-network-interface.virtual-network-interface.manage-ip-spoofing` and `is.virtual-network-interface.virtual-network-interface.manage-infrastructure-nat` IAM permissions respectively.
- [Flow log collectors](/apidocs/vpc/latest#create-flow-log-collector) will be able to target instance network attachments and virtual network interfaces. There is currently no support for flow logs for bare metal servers and share mount targets.

## Why are we making this change?
{: #vni-api-change-justification}

A virtual network interface provides a mechanism for consolidating network policies in one resource that can be preserved and reused, with a lifecycle independent of the resources it attaches to. Network-specific IAM permissions are managed separately from compute IAM permissions. When new network features are released, they will be made available through virtual network interfaces, allowing the new network features to support all resources that virtual network interfaces can attach to.

## What are the effects of this change?
{: #vni-api-affected}

Your account will be affected by this change where both of the following conditions apply:

- You have API clients (such as custom automations, auditing scripts, or dashboards) that interact with instances, bare metal servers, network interfaces, or file shares.
- After the feature is available, users or clients create new instances or bare metal servers with virtual network interfaces, or change the `auto_delete` property of a virtual network interface attached to a file share.

Users in your account will be able to create instances and bare metal servers with network attachments and virtual network interfaces. These operations could cause API clients and workflows to fail.

## How could the upcoming changes cause a failure?
{: #vni-api-actions-preparing}

Until this feature becomes generally available, you won't be able to perform mitigation steps or testing. However, review this section now to prepare for the behavior changes that could lead to API client and workflow failures. When the feature becomes generally available, revisit this topic for guidance on how to mitigate these hazards.  If you do not want your account to be affected by expanded support for virtual network interfaces when it becomes generally available, you'll have the option to temporarily defer this feature.
{: important}

### Virtual network interfaces changes
{: #vni-api-vni-changes}

You will be able to create instances and bare metal servers with virtual network interfaces attached to new child resources called network attachments. Virtual network interfaces will have lifecycles that are independent of the resources they are attached to, and network-specific IAM actions will be managed on virtual network interfaces.

Behavior change 1
:   For an instance or bare metal server created with network attachments, the instance's or bare metal server's network interface child resources will be [read-only representations](/docs/vpc?topic=vpc-vni-about&interface=ui#vni-old-api-clients) of its network attachments (and their associated virtual network interfaces).

:   _Possible failure_: Clients that attempt to update these read-only resources will fail.

Behavior change 2
:   Virtual network interfaces will have lifecycles that are independent of the resources they are attached to. A virtual network interface with `auto_delete` set to `false` will persist after its target is deleted, and the reserved IPs bound to the virtual network interface remain bound. The virtual network interface can subsequently be attached to another resource.

:   _Possible failure_: Workflows that delete instances, and then create new instances that reuse the reserved IPs from the deleted instances, will fail. Failure occurs when reserved IPs were not released or are still bound to the child network interface.

Behavior change 3
:   For instances and bare metal servers with virtual network interfaces, the IAM permissions for options to allow IP spoofing or to disable infrastructure NAT will be managed on their associated virtual network interfaces.

:   _Possible failure_: Clients and workflows that update properties on child network interfaces to allow IP spoofing or disable infrastructure NAT will fail when the instances or bare metal servers were created with network attachments.

### IP address changes
{: #vni-api-ip-address-changes}

Behavior change
:   Virtual network interfaces will support secondary IP addresses.

:   _Possible failure_: A client that attempts to enumerate IP addresses for an instance by retrieving the IP addresses on the child network interfaces will miss the secondary IP addresses if the child network interface is a backward-compatible representation of a network attachment and its associated virtual network interface.

### Flow log collector changes
{: #vni-api-flow-log-collector-changes}

Behavior change
:   Flow log collectors will be able to target instance network attachments and virtual network interfaces. Flow logs are collected for a virtual network interface when the interface is bound to an instance network attachment, and the [Cloud Object Storage (COS) naming convention](/docs/vpc?topic=vpc-fl-analyze#flow-log-object-format) for the collected flow logs has the instance network attachment identifier in the `vnic-id` field. Over its lifetime, the virtual network interface may be attached to more than one instance.

:   _Possible failure_: Tooling, audits, or troubleshooting procedures that analyze collected flow logs from COS buckets may fail to correlate the logs with a virtual network interface.

### Activity tracker event changes
{: #vni-api-at-event-changes}

Behavior change
:   Many new [activity tracker events](/docs/vpc?topic=vpc-at-events#events-network-virtual-network-interface) will be associated with virtual network interfaces. Also, some existing activity tracker events have been [corrected](/docs/vpc?topic=vpc-release-notes#vpc-dec1923):

    - [Bare metal events](/docs/vpc?topic=vpc-at-events#events-compute-bm)
    - [Floating IP events](/docs/vpc?topic=vpc-at-events&interface=api#events-network-floatingIP)
    - [Subnet events](/docs/vpc?topic=vpc-at-events&interface=api#events-network-subnet)

:   _Possible failures_: Client tools and auditing processes that look for the corrected activity tracker events may miss events or report incorrect event sequences. Client tools and auditing processes that have not been updated for the new activity tracker events will report incomplete event sequences.

### Instance template changes
{: #vni-api-instance-template-changes}

Behavior change 1
:   An instance template can be either for instances with old-style child network interfaces or for instances with network attachments and virtual network interfaces. When creating an instance from an instance template, the network interface type must match that of the source template.

:   _Possible failure_: Clients and workflows that create instances from an instance template and override the network interface properties will fail if the template was for instances with network attachments and virtual network interfaces.

Behavior change 2
:   An instance template can be either for instances with old-style child network interfaces or for instances with network attachments and virtual network interfaces. When creating an instance template from another instance template, the network interface type must match that of the source template.

:   _Possible failure_: Clients and workflows that create instance templates from another instance template and override the network interface properties will fail if the source template was for instances with network attachments and virtual network interfaces.

### File share changes
{: #vni-api-file-share-changes}

Behavior change
:   Virtual network interfaces attached to file share mount targets will be allowed to have their `auto_delete` property set to `false`. When the file share mount target to which a virtual network interface is attached is deleted, the virtual network interface is not automatically deleted. The virtual network interface can subsequently be reused for another resource, such as another file share mount target.

:   _Possible failure_: Workflows that delete file share mount targets, and then delete subnets, may fail. Failure occurs when the subnets are no longer referenced by any resources.

## What actions can you take to avoid a disruption?

Bookmark this guidance. When the feature becomes generally available, this page will be updated to include guidance on how to mitigate hazards introduced by the changes.

Behavior changes introduced by this feature represent the types of hazards you should evaluate in your code. Starting 12 March 2024, someone in your account might start performing operations that could break your automation. Before the feature becomes generally available, compare these changes to any client automation you've created and assess any risk. Prepare for behavior changes that could lead to API client and workflow failures by starting to update the design for your automation, even if you won't be able to test it yet.

If you need more time to test expanded support for virtual network interfaces, or if you do not want your account to be affected by this feature when it becomes generally available, you'll have the option to temporarily defer this feature.
