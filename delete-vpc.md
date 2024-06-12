---

copyright:
  years: 2019, [{CURRENT_YEAR}]
lastupdated: "[{LAST_UPDATED_DATE}]"

keywords: delete, resources

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Considerations when deleting a VPC
{: #deleting}

An {{site.data.keyword.vpc_full}} resource cannot be deleted if it contains other resources, or if it is attached to another resource. This document describes the relationships between VPC resources, and it provides information about deleting a VPC.
{: shortdesc}

The following table summarizes the types of VPC resources and the relationships to consider when you delete those resources.

| Resource | Before it can be deleted... | After it is deleted... |
| ---------------- | ----------------------------------------- | --------------------------- |
| VPC | All subnets and public gateways in the VPC must be deleted. | All security groups and address prefixes are deleted automatically. |
| Subnet | All instances and any network interfaces in subnet must be deleted. | Any public gateway that serves the subnet is detached. Any network ACL associated with the subnet is detached. |
| Instance | ---- | All network interfaces are deleted automatically and the boot volume is deleted with the instance. Secondary data volumes are preserved unless the default setting is changed to auto-delete. A floating IP address must be unassociated or released before the instance is deleted.  |
| Network interface | ---- | Any floating IP attached to the network interface is released. |
| Key | ---- | After you delete a key, it can no longer be used to provision a new instance, or to perform an OS reload on an existing instance. However, the key is still available on any instances that you provisioned with it, and you can continue to use it to log in.  |
| Image | ---- | The image cannot be used to provision a new instance, but existing instances with the image are not affected. |
| Volume | The volume must be detached from all instances. | ---- |
| Network ACL | The network ACL must be detached from all subnets. Default network ACLs for a VPC cannot be deleted.  | ---- |
| Security group | The security group must be detached from all network interfaces. The default security group for a VPC cannot be deleted. | ---- |
| Floating IP | The floating IP must be detached from any network interface. | ---- |
| Public gateway | The public gateway must be detached from all subnets. |  ---- |
| Load balancer | ---- | ---- |
| VPN gateway | ---- | Because IKE and IPsec policies can be shared between gateways, these policies are not deleted when a VPN Gateway is deleted. These policies must be removed manually. |
{: caption="Table 1. Types of resources and relationships that affect deletions" caption-side="bottom"}

## VPC resources cannot be deleted in a transient state
{: #deleting-status}

VPC resources cannot be deleted when they are in a transient state, such as `pending`. All resources must be in a "final" state, such as `available` or `failed`, before they can be deleted. You must wait for the resource to get to `available` or `failed` before you try to delete the resource. [Contact support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) if a resource does not show an `available` or `failed` status within 30 minutes.

After you request to delete a resource, the resource enters a transient state of `deleting`. Wait for the resource to disappear from the list before you try to delete the parent or attached resource. In some situations, the delete operation might fail, and the resource enters a `failed` status within a few minutes. Contact support if a resource doesn't disappear or show a `failed` status within 30 minutes after you made a delete request. After the resource is in `failed` status, you can attempt to delete it again. [Contact support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc) if you are unable to delete a resource in `failed` status.

## Follow this order when you delete a VPC
{: #deleting-order}

A VPC contains subnets and public gateways. A public gateway can be attached to one or more subnets in the VPC. A subnet also can contain virtual server instances, and one virtual server instance can have multiple network interfaces in different subnets within the VPC. Before a subnet can be deleted, any network interfaces in the subnet must first be deleted. A VPC contains security groups as well, but they are deleted automatically when the VPC is deleted. Address prefixes are attributes of a VPC, and they are deleted automatically, when the VPC is deleted.

Follow this order when you delete a VPC:

1. Delete all VPN gateways that are provisioned in any subnet of the VPC.
1. Detach all load balancers that are provisioned in any subnet of the VPC.
1. Delete all instances that have network interfaces in any subnet of the VPC. Any Floating IP associated with a network interface is automatically detached from the network interface when the instance is deleted.
1. Delete all subnets in the VPC. Any public gateway attached to a subnet is automatically detached from the subnet when the subnet is deleted.
1. Delete all public gateways in the VPC.
1. When the VPC has no subnets and no public gateways, you can delete the VPC. All Address Prefixes in the VPC are automatically deleted when the VPC is deleted.

## VPC resource relationships
{: #deleting-relationships}

Some VPC resources are contained within other VPC resources (as "children"), but other VPC resources are contained at the account level, outside of any specific VPC. All child VPC resources must be deleted before the parent resource can be deleted. For account-level VPC resources, all links to existing resources must be deleted before the account resource can be deleted.

In some cases, deleting a VPC resource removes the link to existing resources automatically. Table 2 and Table 3 describe the expected behavior.

### VPC
{: #deleting-details}

VPCs contain subnets and public gateways. Load balancers, VPN gateways, and instances are provisioned in a subnet. Before a VPC can be deleted, all subnets and public gateways in the VPC must be deleted. In other words, all public gateways, load balancers, VPN gateways, and instances in the VPC must first be deleted.

A VPC also contains address prefixes and security groups, but these resources are deleted automatically when the VPC is deleted.

A VPC must have a default security group and a default network ACL. If you created a VPC without specifying a default security group and network ACL, a default security group and network ACL were created automatically for the VPC. The default security group and default network ACL cannot be deleted. The default network ACL is deleted automatically when the VPC is deleted. All security groups in the VPC are deleted automatically when the VPC is deleted.

| In VPC | Can contain | Can attach to | Has Status? | Automatically deleted when VPC is deleted | Automatically detaches when deleted |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Subnet | Instances, load balancers, VPN gateways | Public gateway, network ACL | Yes| No | Yes|
| Public gateway| --- | Subnets, floating IP | Yes | No | No to subnets, Yes to floating IP |
| Security Groups | ---  | Instances (NIC), VPC as default | No| Yes | No |
{: caption="Table 2. Requirements for deleting subnets, public gateways, and security groups" caption-side="bottom"}

### Subnet
{: #deleting-subnet}

All resources in a subnet must be deleted before the subnet can be deleted. Subnets contain the instance's network interfaces, load balancers, and VPN gateways. The network interface that is associated with the subnet must be deleted before the subnet can be deleted, along with any load balancers or VPN gateway provisioned in the subnet.

An instance can have multiple network interfaces, and those network interfaces can belong to multiple subnets of the VPC. Before a subnet can be deleted, any network interface in the subnet must first be deleted. The instance's primary network interface cannot be deleted or moved to another subnet. All instances with a primary network interface in the subnet must be deleted before the subnet can be deleted.

| In Subnet | Can contain | Can attach to | Has Status? | Automatically deletes when subnet is deleted | Automatically detached when deleted |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Instance (network interface) | Multiple network interfaces | Volume attachments, security groups | Yes| No  | Yes|
| VPN | --- | ---| Yes | No  | --- |
| Load Balancer | ---  | --- | Yes | No | ---  |
{: caption="Table 3. Requirements for deleting network interfaces" caption-side="bottom"}

### Instance
{: #deleting-instance}

No prerequisites are required for deleting an instance. When the instance is deleted, all its network interfaces are deleted automatically. The instance's boot volume and all of its volume attachments are deleted. Any Floating IP addresses attached to any of its network interfaces are released automatically. Any Block Storage volume attached to the instance is deleted automatically if the volume was created with the flag `delete_volume_on_instance_delete` set to true. Otherwise, the instance is detached from the volume, but the volume remains. If any security group is attached to any of the instance's network interfaces, the security group is detached automatically as well, when the network interfaces are deleted.

| In Instance | Can contain | Can attach to | Has Status? | Automatically deleted when instance is deleted | Automatically detached when deleted |
| ---------------- | ----------------------------------------- | --------------------------- | ------ | ---------------------------------------------- | ----------------------------------- |
| Network interface | --- | Subnets, floating IP, security groups | No | Yes | Yes |
{: caption="Table 4. Information for deleting instances" caption-side="bottom"}

### Load Balancer
{: #deleting-lb}

No prerequisites are required for deleting a load balancer. When the load balancer is deleted, all listeners, pools, and pool members that are part of the load balancer are deleted automatically.

Deleting a load balancer can take up to 30 minutes. The delete request immediately changes the provisioning status of the load balancer to `deleting`. However, the load balancer is not deleted until it disappears from the list query.
{: important}

### VPN
{: #deleting-vpn}

No prerequisites are required for deleting a VPN gateway. When the VPN gateway is deleted, its associated connections are also deleted automatically. IKE and IPsec policies are not deleted when a VPN gateway is deleted.

Deleting a VPN gateway can take up to 30 minutes. The delete request immediately changes the provisioning status of the VPN gateway to `deleting`. However, the VPN gateway is not deleted until it disappears from the list query.
{: important}

### Floating IP
{: #deleting-floating-ip}

A floating IP exists outside of a VPC, at the account level. If the floating IP is bound to an instance, the floating IP must be released before the floating IP can be deleted.

If you delete a resource to which the floating IP is bound, such as an instance's network interface, the floating IP is released automatically.

### Volume
{: #deleting-volume}

Two types of volumes can exist in a VPC: a Block Storage boot volume and a Block Storage data volume. These volumes are deleted differently.

#### Deleting boot volumes
{: #deleting-boot-volumes}

Boot volumes exist within an instance, and they are not considered to be separate entities from that instance. When you create an instance, a boot volume also is created and attached to the instance. The boot volume is deleted automatically when you delete the instance. You can't delete the boot volume without deleting the instance.

#### Deleting data volumes
{: #deleting-data-volumes}

Block Storage data volumes can be provisioned and managed separately from their associated virtual server instances. You can attach a data volume as auxiliary storage to one virtual server instance. You can't delete a data volume that is attached to an instance (that is, if the volume is "active"). You must first detach the volume from the instance, and then you can delete the volume.

A data volume has an attribute (or flag) called `delete_volume_on_instance_delete` in the API and `Auto Delete` in the CLI and UI. If this flag is set to `true` (`Enabled` in the UI), when the instance with the attached volume is deleted, the volume is detached and deleted automatically. If the volume's flag is set to `false` (`Disabled` in the UI), the instance is detached from the volume, but the volume is not deleted when the attached instance is deleted. The volume can be attached to another instance.

A Block Storage volume can be attached to only one virtual server at a time.
{: tip}

### Security groups
{: #deleting-secgroup}

A security group cannot be deleted if it is being used by a network interface, or if it is the default security group of a VPC. Before you delete the security group, remove all network interfaces from that security group. Also, make sure that the security group is not being used as the default security group of the VPC.

Deleting a VPC deletes all security groups in that VPC, automatically.

### Network ACLs
{: #deleting-netacl}

A network ACL cannot be deleted if it is being used by a subnet, or if it is the default network ACL of a VPC. Before you delete the network ACL, detach the network ACL from all subnets, and make sure that the network ACL is not being used as the default network ACL of a VPC.

When any VPC is created, it requires a default network ACL. If an existing network ACL is not specified as the default when the VPC is created, a new network ACL is created and set as the default. This default network ACL is deleted automatically when the VPC is deleted, if the ACL is not in use anywhere else.

Unlike security groups, network ACLs can be assigned across VPCs. Therefore, deleting a VPC does not delete the network ACLs.
{: note}

## Next steps
{: #deleting-nextsteps}

The following topics provide more examples on how to delete VPC resources by using the IBM Cloud Console, CLI, or API.

* [Deleting a VPC and its associated resources](/docs/vpc?topic=vpc-deleting-vpc-resources&interface=ui)
