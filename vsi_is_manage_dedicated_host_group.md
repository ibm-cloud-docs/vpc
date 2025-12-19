---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-19"

keywords: manage dedicated host, manage dedicated group, delete dedicated host, disable placement

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing dedicated hosts and groups
{: #manage-dedicated-hosts-groups}

With a dedicated group and dedicated host, you can manage your compute resources. You can create more dedicated hosts, provision virtual server instances, or delete resources. You can also disable the placement of new instances on a dedicated host. Disabling placement of new instances is required if you need to delete a dedicated host.
{: shortdesc}

## Managing dedicated hosts
{: #manage-dedicated-hosts}

When you have a dedicated host, you can create virtual server instances on the host or disable the placement of instances on the host. If you no longer need a dedicated host or dedicated group, you can delete the resources.

To complete management tasks on your dedicated hosts, complete the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Dedicated hosts**.
2. On the Dedicated hosts tab of the Dedicated hosts for VPC page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the dedicated host that you want to manage. You can select from the following actions:

| Action | Description |
|--------|-------------|
| New instance | Create a virtual server instance on this dedicated host. |
| Disable placement | Temporarily prevent new virtual server instances from being provisioned on this host. No instances can be created on the host until placement is enabled again. You must disable placement before you can delete a dedicated host. |
| Enable placement | Indicate that new virtual server instances can be provisioned on this host. If placement is disabled, it must be enabled before instances can be created on the host. |
| Delete | If you no longer need the dedicated host, you can delete it to permanently remove it from your account. Before you can delete the dedicated host, you must first delete any virtual server instances on the host. (Click the name of the dedicated host to access the Host detail page that displays instances on the host.) Then, you must disable the placement of any new virtual server instances on the dedicated host. This action prevents any new instances from being created before you can delete the dedicated host. If the dedicated host is in a failed state, you can proceed directly with the delete action. |
{: caption="Actions available for dedicated hosts" caption-side="bottom"}

## Viewing dedicated host details
{: #viewing-host-details}

On the **Dedicated hosts** tab of the Dedicated hosts for VPC page, click the name of the dedicated host about which you want to see. The following information is shown:

- The dedicated group that the host belongs to.
- The ID of the host.
- The profile that was used to create the host.
- The remaining vCPU and memory on the host.

## Managing dedicated groups
{: #manage-dedicated-groups}

When you have a dedicated group, you can create dedicated hosts within the group. You can also create virtual server instances on the hosts in the group. If you no longer need a dedicated group, you can delete it.

To complete management tasks on your dedicated groups, complete the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Dedicated hosts**.
2. Click the **Dedicated groups** tab on the Dedicated hosts for VPC page. Then, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the dedicated host that you want to manage. You can select from the following actions:

| Action | Description |
|--------|-------------|
| New instance | Create a virtual server instance in this dedicated group. The virtual server instance is provisioned on any dedicated host in the group that has space available. |
| New dedicated host | Create a dedicated host within this dedicated group.|
| Delete | If you no longer need the dedicated group, you can delete it to permanently remove it from your account. Before you can delete the dedicated group, you must first delete all dedicated hosts and virtual server instances within the group. |
{: caption="Actions available for dedicated groups" caption-side="bottom"}

## Viewing dedicated group details
{: #viewing-group-details}

On the **Dedicated groups** tab of the Dedicated hosts for VPC page, click the name of the dedicated group to you want to see. The following information is shown:

- The ID of the group. 
- The profile family that is assigned to the group.
- The hosts that are part of the group.

### Determining the capacity available in a group
{: #group-capacity}

On the Dedicated group details page, you can view the capacity of each dedicated host in the group that is currently allocated
for vCPU and memory. To determine the space that is available for provisioning virtual server instances, complete the following steps:

1. On the Dedicated group details page, locate the existing hosts in the group under the heading, Dedicated hosts in *group*.
2. Check the vCPU allocated and Memory allocated columns to see what capacity is already used and how much is remaining on the individual hosts.
3. A virtual server instance cannot span more than one dedicated host, so the profile for an instance must be able to be provisioned on the remaining capacity of a single dedicated host.

### Dedicated host maintenance
{: #managing-dh-maintenance}

{{site.data.keyword.cloud}} performs periodic maintenance on the hosts and dedicated hosts.
Maintenance includes the live migration of the servers on the host or dedicated host. Host and dedicated host maintenance events can address load balancing, host failures, or other updates.

For more information, see [Host and dedicated host maintenance](/docs/vpc?topic=vpc-about-cloud-maintenance#types-of-maintenance-host).
