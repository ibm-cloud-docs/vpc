---

copyright:
  years: 2024, 2026
lastupdated: "2026-01-22"

keywords: virtual network interfaces, hazardous change remediation, mitigation

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attention: Mitigating behavior changes to virtual network interfaces, instances, bare metal servers, and file shares
{: #vni-api-introduction}

On 12 March 2024, a feature that expands the support for [virtual network interfaces](/docs/vpc?topic=vpc-vni-about) was made generally available in the Virtual Private Cloud (VPC) service. If used in your account, this feature introduces behavior changes to instances, bare metal servers, and file shares. These changes could affect your automation or workflows for managing these resources.
{: shortdesc}

You can choose to defer access to this feature through [IBM Support](/unifiedsupport/supportcenter). Users in an account that has deferred access will not be able to create instances or bare metal servers with virtual network interfaces. If you need more time to assess, remediate, and test changes for virtual network interfaces, request deferral for your production accounts while you complete the mitigations using your test accounts.
{: important}

If you have not deferred access to this feature, read the provided guidance even if you do not plan to use virtual network interfaces.
{: tip}

## What changed?
{: #vni-api-changes}

The following features are available for virtual network interfaces:

- You can create [instances](/apidocs/vpc/latest#create-instance) and [bare metal servers](/apidocs/vpc/latest#create-bare-metal-server) with virtual network interfaces attached to new child resources called network attachments. You can specify a `primary_network_attachment` (instead of a `primary_network_interface`) and provide either the identity of an already-created virtual network interface, or a subnet to create a new virtual network interface for the instance or bare metal server.
- Virtual network interfaces have lifecycles that are independent of the resources they are attached to. You can [update](/apidocs/vpc/latest#update-virtual-network-interface) the `auto_delete` property to `false` to allow a virtual network interface to persist beyond the lifecycle of its original bare metal server or instance, and be re-attached to another bare metal server or instance.
- Virtual network interfaces support [secondary IP addresses](/docs/vpc?topic=vpc-vni-about-secondary-ip). You can [add](/apidocs/vpc/latest#add-virtual-network-interface-ip) and [remove](/apidocs/vpc/latest#remove-virtual-network-interface-ip) reserved IPs to and from a virtual network interface.
- For compatibility with existing clients, instances and bare metal servers with virtual network interfaces include a read-only representation of their network attachments and virtual network interfaces as old-style network interface child resources. Learn about [support for old API clients](/docs/vpc?topic=vpc-vni-about&interface=ui#vni-old-api-clients).
- For instances and bare metal servers with virtual network interfaces, the IAM permissions for options to allow IP spoofing or disable infrastructure NAT are managed on their attached virtual network interfaces. When [creating](/apidocs/vpc/latest#create-virtual-network-interface) or [updating](/apidocs/vpc/latest#update-virtual-network-interface) a virtual network interface, you can set non-default values for the `allow_ip_spoofing` and `enable_infrastructure_nat` properties only if you have the `is.virtual-network-interface.virtual-network-interface.manage-ip-spoofing` and `is.virtual-network-interface.virtual-network-interface.manage-infrastructure-nat` IAM permissions respectively.
- You can use [flow log collectors](/apidocs/vpc/latest#create-flow-log-collector) to target instance network attachments and virtual network interfaces. There is no support for flow logs for bare metal servers and share mount targets.

## Why did we make this change?
{: #vni-api-change-justification}

A virtual network interface provides a mechanism for consolidating network policies in one resource that can be preserved and reused, with a lifecycle independent of the resources it attaches to. Network-specific IAM permissions are managed separately from compute IAM permissions. When new network features are released, they will be made available through virtual network interfaces, allowing the new network features to support all resources that virtual network interfaces can attach to.

## What are the effects of this change?
{: #vni-api-affected}

Unless you have deferred access to this feature, your account is affected by this change if you have API clients (such as custom automations, auditing scripts, or dashboards) that interact with instances, bare metal servers, network interfaces, or file shares.

These operations could cause API clients and workflows to fail.

## What actions can you take to avoid a failure?
{: #vni-api-actions-preparing}

Behavior changes described in this section represent the types of hazards that may affect your code if your account has not deferred access to this feature. Someone in your account might start performing operations that could break your automation.

Compare these changes to any client automation you've created. Use this guidance to mitigate behavior changes that could lead to API client and workflow failures by updating and testing the design for your automation.

### Virtual network interfaces remediation
{: #vni-api-vni-changes}

You can create instances and bare metal servers with virtual network interfaces attached to new child resources called network attachments. Virtual network interfaces have lifecycles that are independent of the resources they are attached to, and network-specific IAM actions are managed on virtual network interfaces.

Behavior change 1
:   For an instance or bare metal server created with network attachments, the instance's or bare metal server's network interface child resources are [read-only representations](/docs/vpc?topic=vpc-vni-about&interface=ui#vni-old-api-clients) of its network attachments (and their associated virtual network interfaces).

:   _Possible failure_: Clients that attempt to update these read-only resources will fail.

:   _Mitigation_: Review client code that retrieves and updates the `network_interfaces` child resources of instances and bare metal servers. Update the code, as needed, to check the instance or bare metal server for the presence of a `primary_network_attachment` property. Change the client logic to instead update the `network_attachments` child resources and their associated virtual network interfaces.

Behavior change 2
:   Virtual network interfaces have lifecycles that are independent of the resources they are attached to. A virtual network interface with `auto_delete` set to `false` will persist after its target is deleted, and the reserved IPs bound to the virtual network interface remain bound. The virtual network interface can subsequently be attached to another resource.

:   _Possible failure_: Workflows that delete instances, and then create new instances that reuse the reserved IPs from the deleted instances, may fail. Failure occurs when attempting to reuse a reserved IP that was not released because it is still bound to a virtual network interface that outlived a deleted instance.

:   _Mitigation_: Review client code for any assumptions that reserved IPs used by instances and bare metal servers are bound to `network_interfaces` child resources and will always be unbound immediately following the deletion of an instance or bare metal server. Change the client to do one of the following:

    - Set the `auto_delete` property to `true` on the virtual network interfaces associated with the `network_attachments` of the instance or bare metal server before it is deleted
    - Separately delete all virtual network interfaces before their reserved IPs are reused
    - Reuse the virtual network interfaces, which reuses all the properties of the virtual network interfaces, such as the `primary_ip`, any secondary `ips`,  any `security_groups`, and so on

Behavior change 3
:   For instances and bare metal servers with virtual network interfaces, the IAM permissions for options to allow IP spoofing or to disable infrastructure NAT are managed on their associated virtual network interfaces.

:   _Possible failure_: Clients and workflows that update properties on child network interfaces to allow IP spoofing or disable infrastructure NAT will fail when the instances or bare metal servers were created with network attachments.

:   _Mitigation_: Review client code that updates the `allow_ip_spoofing` or `enable_infrastructure_nat` properties on `network_interfaces` child resources. Update the code to check for the presence of a `primary_network_attachment` property, which signifies that the `network_interfaces` child resources are read only. Update client logic to instead update the `allow_ip_spoofing` or `enable_infrastructure_nat` properties as needed on the virtual network interfaces associated with the `network_attachments` of the instance or bare metal server.

### IP address remediation
{: #vni-api-ip-address-changes}

Behavior change
:   Virtual network interfaces support secondary IP addresses.

:   _Possible failure_: A client that attempts to enumerate IP addresses for an instance by retrieving the `primary_ip.address` on the `network_interfaces` child resources will miss the secondary IP addresses if the `network_interfaces` are backward-compatible representations of the `network_attachments` and their associated virtual network interfaces.

:   _Mitigation_: Review your client code that enumerates or audits IP addresses for instances and bare metal servers. Update your client code to check for the presence of a `primary_network_attachment` property, which signifies that the instance or bare metal server has `network_attachments` with associated virtual network interfaces. Update client logic to instead retrieve all the `address` properties of the `ips` array on each of the associated virtual network interfaces.

### Flow log collector remediation
{: #vni-api-flow-log-collector-changes}

Behavior change
:   Flow log collectors can target instance network attachments and virtual network interfaces. Flow logs are collected for a virtual network interface when the interface is bound to an instance network attachment, and the [Cloud Object Storage (COS) naming convention](/docs/vpc?topic=vpc-fl-analyze#flow-log-object-format) for the collected flow logs has the instance network attachment identifier in the `vnic-id` field. Over its lifetime, the virtual network interface may be attached to more than one instance.

:   _Possible failure_: Tooling, audits, or troubleshooting procedures that analyze collected flow logs from {{site.data.keyword.cos_full}} buckets may fail to correlate the logs with a virtual network interface.

:   _Mitigation_: Review and update your tooling, auditing, or troubleshooting procedures that create flow log collectors and analyze flow logs. When targeting a virtual network interface with a flow log collector, make sure that your procedures to analyze flow logs take into account that the collector may collect flow logs for more than one instance over the lifetime of the virtual network interface. The identifier of the instance is in the `instance-id` section of the name of the [flow log Object Storage bucket](/docs/vpc?topic=vpc-fl-analyze#flow-log-object-format). Therefore, a new flow log Object Storage bucket is created when the `target` of a virtual network interface is updated by associating the virtual network interface with a new instance.

### Activity tracking event remediation
{: #vni-api-at-event-changes}

Behavior change
:   Expanded support for virtual network interfaces adds many new [activity tracking events](/docs/vpc?topic=vpc-at_events#events-vni):

    - [Bare metal events](/docs/vpc?topic=vpc-at_events#events-compute-bm)
    - [Floating IP events](/docs/vpc?topic=vpc-at_events&interface=api#events-network-floatingIP)
    - [Subnet events](/docs/vpc?topic=vpc-at_events&interface=api#events-network-subnet)

:   _Possible failures_: Client tools and auditing processes that have not been updated for the new activity tracking events will report incomplete event sequences.

:   _Mitigation_: Review your tooling that consumes activity tracking events, and update your tooling, as needed, to include the new events. Take into account that no activity tracking events are generated for [read-only representations](/docs/vpc?topic=vpc-vni-about&interface=ui#vni-old-api-clients) of network attachments.

### Instance template remediation
{: #vni-api-instance-template-changes}

Behavior change 1
:   An instance template can be either for instances with old-style `network_interfaces` child resources or for instances with `network_attachments` child resources and associated virtual network interfaces. When creating an instance from an instance template, the network interface or attachment type must match that of the source template.

:   _Possible failure_: Clients and workflows that create instances from an instance template and override the `primary_network_interface` or `network_interfaces` properties will fail if the template was for instances with a `primary_network_attachment` or `network_attachments`.

:   _Mitigation_: Before creating an instance from a template, retrieve the instance template and determine whether the template has `primary_network_interface` and `network_interfaces` properties, or `primary_network_attachment` and `network_attachments` properties. When creating an instance from the template, match the instance properties, as needed, with those of the template.

Behavior change 2
:   An instance template can be either for instances with old-style `network_interfaces` child resources or for instances with `network_attachments` child resources and associated virtual network interfaces. When creating an instance template from an instance template, the network interface or attachment type must match that of the source template.

:   _Possible failure_: Clients and workflows that create instance templates from another instance template and override the `primary_network_interface` or `network_interfaces` properties will fail if the source template was for instances with a `primary_network_attachment` and `network_attachments`.

:   _Mitigation_: Before creating an instance template from a source template, retrieve the source template and determine whether the source template has `primary_network_interface` and `network_interfaces` properties, or `primary_network_attachment` and `network_attachments` properties. When creating a new template from the source template, match the new template properties, as needed, with those of the source template.

    **Note:** You are affected by these template behavior changes if either of the following is true:

    - An API client or a user in your account creates a template that has network attachments and a client attempts to use the new template to create instances or templates with network interfaces
    - A client attempts to use a template that has network interfaces to create instances or templates with network attachments

### File share remediation
{: #vni-api-file-share-changes}

Behavior change
:   Virtual network interfaces attached to file share mount targets may have their `auto_delete` property set to `false`. When the file share mount target associated with such a virtual network interface is deleted, the virtual network interface is not automatically deleted. The virtual network interface can subsequently be reused for another resource, such as another file share mount target.

:   _Possible failure_: Workflows that delete file share mount targets, and then delete subnets, may fail. Failure occurs when the subnets contain virtual network interfaces that were not automatically deleted.

:   _Mitigation_: Review client code for any assumptions that `auto_delete` is always `true`, and update the client's deletion logic, as needed, to look up the `virtual_network_interface` associated with share mount target. Then select one of the following actions:

    - Set the `auto_delete` property of the virtual network interface to `true` before deleting the share mount target
    - Delete the virtual network interface in a follow-up step
    - Preserve the virtual network interface for later reuse with new share mount target
