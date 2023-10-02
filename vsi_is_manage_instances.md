---

copyright:
  years: 2019, 2023
lastupdated: "2023-06-16"

keywords: view instance details, restart, stop, instance details, delete

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Managing virtual server instances
{: #managing-virtual-server-instances}

Manage your {{site.data.keyword.vsi_is_full}} instances by performing tasks such as start, stop, restart, and delete virtual server instances.
{: shortdesc}

## Managing virtual server instances using the UI
{: #managing-virtual-server-instances-ui}
{: ui}

You can view and manage your {{site.data.keyword.vsi_is_full}} instances from the *Virtual server instances* page in {{site.data.keyword.cloud_notm}} console.

To manage your instances, complete the following steps.
1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. On the **Virtual server instances** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance that you want to manage. You can select from the following actions:

| Action | Description |
|--------|-------------|
| Rename | Change the name of the instance. |
| Stop| Stop the instance. |
| Start | Start an instance that is stopped. This action is not available if the instance status is Running. |
| Reboot | Immediately powers off a running instance and then powers it back on again. |
| Resize | Vertically scale virtual server instances to any supported profile size. For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance).
| Delete | To delete an instance, the instance must have a powered off status. If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The delete action permanently removes an instance and its connected vNIC, boot volume, and data from your account. If auto-delete is enabled, the associated boot volume is also deleted. |
| Host failure auto restart | Toggle on or off the host failure restart policy for an instance. For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=api). |
{: caption="Table 1. Actions available for virtual server instances" caption-side="top"}

## Rename a virtual server instance using the UI
{: #rename-virtual-server-instances-ui}
{: ui}

From the *Virtual server instances* page in {{site.data.keyword.cloud_notm}} console, click **Rename**. Enter the new name for the virtual server instance. Click **Rename**.

## Rename a virtual server instance using the CLI
{: #rename-virtual-server-instances-CLI}
{: cli}

You can rename a virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To rename a virtual server instance by using the CLI, use the `ibmcloud is instance-update INSTANCE` command. Specify the new name of the instance with the `--name NEW_NAME` option.

The following example renames a virtual server with the name of `my-instance-name` to `my-instance-name-new`.

```sh
ibmcloud is instance-update my-instance-name --name my-instance-name-new
```
{: pre}

For a full list of command options, see [ibmcloud is instance-update](/docs/vpc?topic=vpc-vpc-reference#instance-update).

## Rename a virtual server instance using the API
{: #rename-virtual-server-instances-API}
{: api}

You can rename the virtual server instance in your {{site.data.keyword.vpc_short}} by using the API.

The following example renames a virtual server instance from `name` to `my-instance`.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-29&generation=2" -H "Authorization: $iam_token" -d '{"name": "my-instance"}'
```
{: pre}

## Stop and start a virtual server instance using the UI
{: #stop-start-virtual-server-instances-ui}
{: ui}

The stop action shuts down the guest operating system and then the virtual server instance is deprovisioned. This change frees the instance resources that were being consumed. The virtual server instance is transitioned to the Stop state. If the instance is stopped, the instance remains in the stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped, but volumes remain provisioned. If the instance is started, normal interaction and billing continues.

A Force stop action triggers a power cycle reset of the virtual server instance.
{: note}

The start action starts a virtual server instance that is in a stopped state.

From the *Virtual server instances* page in {{site.data.keyword.cloud_notm}} console, click **Stop** or **Start**.

_For z/OS virtual server instances only_: You must shut down all the subsystems in the z/OS system to halt the virtual server instance. For more information, see [Shutting down z/OS virtual server instances](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=vpc-shutting-down-zos-virtual-server-instances){: external}.
{: note}

## Stop a virtual server instance using the CLI
{: #stop-virtual-server-instances-cli}
{: cli}

You can stop the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To stop the virtual server instance by using the CLI, use the **`ibmcloud is instance-stop`** command. Specify the ID or name of the virtual server instance that you want to stop with the `INSTANCE` variable.

```sh
ibmcloud is instance-stop INSTANCE
```
{: pre}

The stop action shuts down the guest operating system and then the virtual server instance is deprovisioned. This change frees the instance resources that were being consumed. The virtual server instance is transitioned to the Stop state. If the instance is stopped, the instance remains in the stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped, but volumes remain provisioned. If the instance is started, normal interaction and billing continues.

The following example stops an instance without requesting confirmation. The virtual server instance has an ID of `0777_e7af506a-35d4-451d-aa9e-59330e62b77e`. The `--force` option indicates that the request for confirmation is skipped. 

```sh
ibmcloud is instance-stop 0777_e7af506a-35d4-451d-aa9e-59330e62b77e --force
```
{: pre}

A Force stop action triggers a power cycle reset of the virtual server instance.
{: note}

If you have an instance that gets stuck in a *stopping* state, you can use the following example command with the `--force` and `--no-wait` options specified to stop the instance immediately without confirmation. The instance has an ID of `0757_5446c277-3190-48dd-ac67-5f02fab39ed5`. The `--force` option indicates that the request for confirmation is skipped. The `--no-wait` option runs the command immediately, dropping any queued actions. 

```sh
ibmcloud is instance-stop 0757_5446c277-3190-48dd-ac67-5f02fab39ed5 --force --no-wait
```
{: pre}

For a full list of command options, see [ibmcloud is instance-stop](/docs/vpc?topic=vpc-vpc-reference#instance-stop).

## Start a virtual server instance using the CLI
{: #start-virtual-server-instances-cli}
{: cli}

You can start a virtual server instance that is stopped in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To start the virtual server instance by using the CLI, use the **`ibmcloud is instance-start`** command. Specify the ID or name of the virtual server instance that you want to start by using the `INSTANCE` variable.

```sh
ibmcloud is instance-start INSTANCE 
```
{: pre}

For a full list of command options, see [ibmcloud is instance-start](/docs/vpc?topic=vpc-vpc-reference#instance-start).

## Stop a virtual server instance using the API
{: #stop-virtual-server-instances-api}
{: api}

You can stop the virtual server instance in your {{site.data.keyword.vpc_short}} by using the API.

The following example stops a virtual server instance with an instance ID of `d6c3902d-1ecf-3a2c-b7ab-eb9143581000`.

```sh
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/d6c3902d-1ecf-3a2c-b7ab-eb9143581000/actions?version=2021-06-22&generation=2" -H "Authorization: $iam_token" -d '{"type": "stop"}'
```
{: pre}

The stop action shuts down the guest operating system and then the virtual server instance is deprovisioned. This change frees the instance resources that were being consumed. The virtual server instance is transitioned to the Stop state. If the instance is stopped, the instance remains in the stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped, but volumes remain provisioned. If the instance is started, normal interaction and billing continues.

A Force stop action triggers a power cycle reset of the virtual server instance.
{: note}

For more information, see the [Create instance action](/apidocs/vpc#create-instance-action) in the Virtual Private Cloud API documentation.

## Start a virtual server instance using the API
{: #start-virtual-server-instances-api}
{: api}

You can start a virtual server instance that is stopped in your {{site.data.keyword.vpc_short}} by using the API.

The following example stops a virtual server instance with an instance ID of `d6c3902d-1ecf-3a2c-b7ab-eb9143581000`.

```sh
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/d6c3902d-1ecf-3a2c-b7ab-eb9143581000/actions?version=2021-06-22&generation=2" -H "Authorization: $iam_token" -d '{"type": "start"}'
```
{: pre}

The stop and start action remotely turns an instance off or on. If the instance is stopped, the instance remains in the
stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for
some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped. If the instance is started, normal interaction and billing continues.

For more information, see the [Create instance action](/apidocs/vpc#create-instance-action) in the Virtual Private Cloud API documentation.

## Reboot a virtual server instance using the UI
{: #reboot-virtual-server-instances-ui}
{: ui}

The reboot action triggers a guest operating system reboot. The virtual server instance remains in a running state while the guest operating system is rebooting. Billing continues.

A Force reboot action triggers a power cycle reset of the virtual server instance.
{: note}

From the *Virtual server instances* page in {{site.data.keyword.cloud_notm}} console, click **Reboot**.

## Reboot a virtual server instance using the CLI
{: #reboot-virtual-server-instances-cli}
{: cli}

The reboot action triggers a guest operating system reboot. The virtual server instance remains in a running state while the guest operating system is rebooting. Billing continues.

A Force reboot action, by using the `--force` option, triggers a power cycle reset of the virtual server instance.
{: note}

You can reboot the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To reboot the virtual server instance by using the CLI, use the **`ibmcloud is instance-reboot`** command. Specify the ID or name of the virtual server instance that you want to reboot by using the `INSTANCE` variable.

```sh
ibmcloud is instance-reboot INSTANCE 
```
{: pre}

For a full list of command options, see [ibmcloud is instance-reboot](/docs/vpc?topic=vpc-vpc-reference#instance-reboot).

## Reboot a virtual server instance using the API
{: #reboot-virtual-server-instances-api}
{: api}

You can reboot the virtual server instance in your {{site.data.keyword.vpc_short}} by using the API. The following example reboots the specified virtual server.

```sh
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/d6c3902d-1ecf-3a2c-b7ab-eb9143581000/actions?version=2021-06-22&generation=2" -H "Authorization: $iam_token" -d '{"type": "reboot"}'
```
{: pre}

The reboot action triggers a guest operating system reboot. The virtual server instance remains in a running state while the guest operating system is rebooting. Billing continues.

A Force reboot action triggers a power cycle reset of the virtual server instance.
{: note}

For more information, see the [Create instance action](/apidocs/vpc#create-instance-action) in the Virtual Private Cloud API documentation.

## Resize a virtual server instance using the UI
{: #resize-virtual-server-instances-ui}
{: ui}

You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management to address resource requirement changes, optimize cost or workload performance. Once resizing an instance is complete, you are billed the hourly rate of the new instance profile selected.

For the steps to resize a virtual server instance using the UI, see [Resizing a virtual server instance using the UI](/docs/vpc?topic=vpc-resizing-an-instance#resizing-a-virtual-server-UI).

## Resize a virtual server instance using the CLI
{: #resize-virtual-server-instances-CLI}
{: cli}

You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management to address resource requirement changes, optimize cost or workload performance. Once resizing an instance is complete, you are billed the hourly rate of the new instance profile selected.

For the steps to resize a virtual server instance using the CLI, see [Resizing a virtual server instance using the CLI](/docs/vpc?topic=vpc-resizing-an-instance#resizing-a-virtual-server-CLI).

## Resize a virtual server instance using the API
{: #resize-virtual-server-instances-API}
{: api}

You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management to address resource requirement changes, optimize cost or workload performance. Once resizing an instance is complete, you are billed the hourly rate of the new instance profile selected.

For the steps to resize a virtual server instance using the CLI, see [Resizing a virtual server instance using the API](/docs/vpc?topic=vpc-resizing-an-instance#resizing-a-virtual-server-API).

## Delete a virtual server instance using the UI
{: #delete-virtual-server-instances-ui}
{: ui}

From the *Virtual server instances* page in {{site.data.keyword.cloud_notm}} console, click **Delete**.

If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The account associated with the floating IP will continue to be charged if it is not released.
{: important}

The delete action permanently removes an instance and its connected vNIC, and data from your account. The instance boot volume is also deleted if the volume auto-delete setting is configured to be deleted when the attached instance is deleted. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. If a boot volume created as part of provisioning a virtual server instance, the volume will be deleted by default when the instance is deleted. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30  minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

## Delete a virtual server instance using the CLI
{: #delete-virtual-server-instances-cli}
{: cli}

You can delete the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The account associated with the floating IP will continue to be charged if it is not released.
{: important}

To delete the virtual server instance by using the CLI, use the **`ibmcloud is instance-delete`** command. Specify the ID or name of the virtual server instance that you want to delete by using the `INSTANCE` variable.

```sh
ibmcloud is instance-delete INSTANCE 
```
{: pre}

For a full list of command options, see [ibmcloud is instance-delete](/docs/vpc?topic=vpc-vpc-reference#instance-delete). 

The delete action permanently removes an instance and its connected vNIC, and data from your account. The instance boot volume is also deleted if the volume auto-delete setting is configured to be deleted when the attached instance is deleted. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. If a boot volume created as part of provisioning a virtual server instance, the volume will be deleted by default when the instance is deleted. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30  minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

## Delete a virtual server instance using the API
{: #delete-virtual-server-instances-api}
{: api}

You can delete the virtual server instance in your {{site.data.keyword.vpc_short}} by using the API.

If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The account associated with the floating IP will continue to be charged if it is not released.
{: important}

The following example deletes an instance.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-22&generation=2" -H "Authorization: $iam_token"
```
{: pre}

The delete action permanently removes an instance and its connected vNIC, and data from your account. The instance boot volume is also deleted if the volume auto-delete setting is configured to be deleted when the attached instance is deleted. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. If a boot volume created as part of provisioning a virtual server instance, the volume will be deleted by default when the instance is deleted. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30  minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

For more information, see the [Delete an instance](/apidocs/vpc#delete-instance) in the Virtual Private Cloud API documentation.

## Toggle the auto-deletion of boot volumes attached to an instance using the UI
{: #auto-delete-toggle-ui}
{: ui}

By default, a boot volume created as part of provisioning a virtual server instance is deleted when the instance is deleted. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. You can control this by setting the auto-delete option on the Edit boot volume panel when creating an instance. For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui).

## Toggle the auto-deletion of boot volumes attached to an instance using the CLI
{: #auto-delete-toggle-cli}
{: cli}

By default, a boot volume created as part of provisioning a virtual server instance is deleted when the instance is deleted. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. You can control this by specifying the `auto_delete` property when creating the instance or updating the boot volume attachment. For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli).

## Toggle the auto-deletion of boot volumes attached to an instance using the API
{: #auto-delete-toggle-api}
{: api}

By default, a boot volume created as part of provisioning a virtual server instance is deleted when the instance is deleted. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. You can control this by specifying the `delete_volume_on_instance_delete` property when [creating the instance](/apidocs/vpc/latest#create-instance) or updating the [boot volume attachment](/apidocs/vpc/latest#update-instance-volume-attachment). For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api).

## Viewing instance details using the UI
{: #viewing-virtual-server-instances-ui}
{: ui}

You can view a summary of all instances on the *Virtual server instances* page. You can access the details page for an instance clicking an individual instance name to view details and make changes. From the instance details page, you can also view the associated network interface, access its subnet, toggle the auto-delete setting, and reserve or release a floating IP address.

## Viewing instance details using the CLI
{: #viewing-virtual-server-instances-cli}
{: cli}

You can view the virtual server instance details in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI). 

To view the virtual server instance details by using the CLI, use the **`ibmcloud is instance`** command. Specify the ID or name of the virtual server instance that you want to view by using the `INSTANCE` variable.

```sh
ibmcloud is instance INSTANCE 
```
{: pre}

## Viewing instance details using the API
{: #viewing-virtual-server-instances-api}
{: api}

You can view the virtual server instance details in your {{site.data.keyword.vpc_short}} by using the API.

The following example displays the virtual server instance details for instance profile with a profile name of `version=2021-06-22&generation=2`.

```sh
curl -X GET "https://us-south.iaas.cloud.ibm.com/v1/instance/profiles/$profile_name?version=2021-06-22&generation=2" -H "Authorization: $iam_token"
```
{: pre}

For more information, see the [Retrieve an instance profile](/apidocs/vpc#get-instance-profile) in the Virtual Private Cloud API documentation.

## Adjusting bandwidth allocation using the UI
{: #adjusting-bandwidth-allocation-ui}
{: ui}

You can adjust the allocation of your instance's total bandwidth between network bandwidth and storage bandwidth using the UI.

To adjust the bandwidth of an instance:

1. Navigate to a virtual service instance.
2. Select Bandwidth allocation.
3. On the Edit bandwidth allocation screen, adjust the value for Storage bandwidth. You can increase the bandwidth allocated for your Block Storage boot and attached data volumes. For information about how storage bandwidth is allocated, see [Bandwidth allocation for Block Storage volumes](/docs/vpc?topic=vpc-block-storage-bandwidth).

After you set storage bandwidth, network bandwidth is automatically adjusted so the total bandwidth of the instance equals the displayed Total bandwidth value. The value for Network bandwidth or Storage bandwidth cannot be set less than 500 Mbps.

To view the new bandwidth allocation, you must either stop and start the instance, or detach and reattach secondary volumes.
{: note}

## Adjusting bandwidth allocation using the CLI
{: #adjusting-bandwidth-allocation-cli}
{: cli}

You can adjust the allocation of your instance's total bandwidth between network bandwidth and storage bandwidth using the CLI.

To reallocate instance bandwidth by using the CLI, run the `instance-update` command and specify the total storage bandwidth in megabits per second (Mbps) for the `total-volume-bandwidth` parameter. Here, INSTANCE can be ID or Name of the instance. Use this syntax:

```sh
ibmcloud is instance-update INSTANCE --total-volume-bandwidth VALUE
```
{: pre}

## Adjusting total storage bandwidth allocation from the API
{: #adjusting-bandwidth-allocation-api}
{: api}

You can adjust total storage bandwidth for an existing instance. Make a `PATCH /instances` call and specify `total_volume_ bandwidth`. Total storage bandwidth (in megabits per second) is the total bandwidth allocated for primary boot and secondary attached data volumes. Increasing total storage bandwidth results in a corresponding decrease in network bandwidth. The minimum network bandwidth you can have is 500 mbps, so adjust total storage bandwidth accordingly. For example,

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-22&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
      "total_volume_bandwidth": 500
      }'
```
{: pre}

Bandwidth allocation is also realized when you add a secondary volume using `POST / volume_attachments` or delete a volume using `DELETE volume_attachments`.

## Retrieving the virtual server instance identifier
{: #retrieve-VSI-instance-identifer}

When a virtual server instance is created, that instance is automatically assigned an instance identifier (ID), which includes the SMBIOS system-uuid as a portion of the ID. The ID can be up to 64 bytes in length and it consists of digits, lower-case letters, underscores, and dashes.

IDs are immutable, globally unique, and never reused, so the ID uniquely identifies a particular instantiation of a virtual server instance across all of IBM Cloud. The ID, including the SMBIOS system-uuid portion, is static and persists for the lifecycle of the virtual server instance until that virtual server instance is deleted.

From within your virtual server, you can retrieve the instance identifier in one of the following ways:

Linux

```sh
dmidecode -s system-family
```
{: pre}

Windows

```sh
Get-WmiObject Win32_ComputerSystem | Select-Object -ExpandProperty SystemFamily
```
{: pre}

From within your virtual server, you can retrieve the SMBIOS system-uuid in one of the following ways:

Linux

```sh
dmidecode -s system-uuid
```
{: pre}

Windows

```sh
Get-WmiObject Win32_ComputerSystemProduct | Select-Object -ExpandProperty UUID
```
{: pre}

For z/OS virtual server instances, you can SSH into the instance, and then run the `DISPLAY IPLINFO` command. When the [IEE254I](https://www.ibm.com/docs/en/zos/2.5.0?topic=iee399i-iee254i){: external} message is displayed, the ID is included in the `VM EXT NAME`.


## Viewing instance status and lifecycle_state in the API
{: #instance-status-api}
{: api}

- `status`
   - This property provides the current status of the virtual server instance through the [Retrieve an instance](/apidocs/vpc/latest#get-instance) request. The values that `status` returns are specialized for virtual server instances and indicate if it is running, stopped, or transitioning. For more information, see the [Virtual Private Cloud API](/apidocs/vpc/latest) content.
- `lifecycle_state`
   - This property provides the current state of a resource through the [Retrieve an instance](/apidocs/vpc/latest#get-instance) request. The values that `lifecycle_state` provides are generic and are meant to apply to a variety resources, such as [Placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc). `lifecycle_state` can return values that overlap with `status`. `lifecycle_state` also includes values that detail if a resource is suspended.

## Setting the host failure auto restart using the UI
{: #set-recovery-policy-ui}
{: ui}

To set the host failure auto restart for an existing instance, complete the following steps.
   1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
   2. On the **Virtual server instances** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance that you want to manage.
   3. From the instance details page, locate 'Host failure auto restart'. Click the pencil icon and choose Enabled or Disabled to toggle the status of the host recovery policy on or off.

For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=api).

## Setting the host failure recovery policy by using the CLI
{: #set-recovery-policy-cli}
{: cli}

You can update an instance in your IBM Cloud VPC with and change the availability policy on host failure by using the command-line interface (CLI). Run the ibmcloud `instance-update` command and set the `--host-failure-policy` property to `start` or `stop`. The host failure policy service is set to `restart` by default. Here, INSTANCE can be ID or Name of the Instance.

```sh
ibmcloud is instance-update INSTANCE --total-volume-bandwidth VALUE --host-failure-policy stop
```
{: pre}

## Setting the host failure recovery policy by using the API
{: #set-policy-api}
{: api}

During instance [update](/apidocs/vpc#update-instances), the `host_failure` sub-property can be used to set the host failure `availability_policy` of the virtual server instance.
