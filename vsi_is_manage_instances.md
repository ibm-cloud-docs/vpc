---

copyright:
  years: 2019, 2026
lastupdated: "2026-01-14"

keywords: view instance details, restart virtual server, stop, details, delete

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing virtual server instances
{: #managing-virtual-server-instances}

Manage your {{site.data.keyword.vsi_is_full}} instances by performing tasks such as start, stop, restart, and delete virtual server instances.
{: shortdesc}

If you don't know the name or ID of the virtual server instance that you want to manage, run **`ibmcloud is instances`** to list virtual server instances in your account.
{: tip}
{: cli}

## Managing virtual server instances in the console
{: #managing-virtual-server-instances-ui}
{: ui}

You can view and manage your {{site.data.keyword.vsi_is_full}} instances from the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console.

To manage your instances, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
2. On the **Virtual server instances** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance that you want to manage. You can select from the following actions:

| Action | Description |
|--------|-------------|
| Rename | Change the name of the instance. |
| Stop| Stop the instance. |
| Start | Start an instance that is stopped. This action is not available if the instance status is Running. |
| Reboot | Immediately powers off a running instance and then powers it back on again. |
| Resize | Vertically scale virtual server instances to any supported profile size. For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance).
| Delete | To delete an instance, the instance must have a powered off status. If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The delete action permanently removes an instance and its connected vNIC, and data from your account. If auto-delete is enabled, the associated boot volume is also deleted. |
| Host failure auto restart | Toggles the host failure restart policy for an instance on or off. For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=api). |
{: caption="Actions available for virtual server instances" caption-side="bottom"}

## Renaming a virtual server instance in the console
{: #rename-virtual-server-instances-ui}
{: ui}

You can rename a virtual server instance in the console.

1. From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, click **Rename**.
2. Enter the new name for the virtual server instance, then click **Rename**.

## Renaming a virtual server instance from the CLI
{: #rename-virtual-server-instances-CLI}
{: cli}

You can rename a virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

1. To rename a virtual server instance, use the `ibmcloud is instance-update INSTANCE` command. Specify the new name of the instance with the `--name NEW_NAME` option.

   The following example renames a virtual server with the name of `my-instance-name` to `my-instance-name-new`.

   ```sh
   ibmcloud is instance-update my-instance-name --name my-instance-name-new
   ```
   {: pre}

   For a full list of command options, see [ibmcloud is instance-update](/docs/vpc?topic=vpc-vpc-reference#instance-update).

## Renaming a virtual server instance with the API
{: #rename-virtual-server-instances-API}
{: api}

You can rename the virtual server instance in your {{site.data.keyword.vpc_short}} by using the API.

The following example renames a virtual server instance from `name` to `my-instance`.

   ```sh
   curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-29&generation=2" -H "Authorization: $iam_token" -d '{"name": "my-instance"}'
   ```
   {: pre}

## Stopping and starting a virtual server instance in the console
{: #stop-start-virtual-server-instances-ui}
{: ui}

The stop action shuts down the guest operating system and then the virtual server instance is deprovisioned. This change releases the instance resources that were being consumed. The virtual server instance goes into the Stop state. If the instance is stopped, the instance remains in the stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped, but volumes remain provisioned. If the instance is started, normal interaction and billing continue.

A _Force stop_ action triggers a power cycle reset of the virtual server instance.
{: note}

The start action starts a virtual server instance that is in a stopped state.

When a virtual server is stopped, it is removed from the host. When the virtual server is started again later, it might be started on a new host. Capacity for a specific virtual server profile is not guaranteed or reserved; for example, capacity might be limited, or not available, for profile families such as GPU (Accelerated) or Storage Optimized.

From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, click **Stop** or **Start**.

_For z/OS virtual server instances only_. You must shut down all the subsystems in the z/OS system to halt the virtual server instance. For more information, see [Shutting down z/OS virtual server instances](https://www.ibm.com/docs/en/wazi-aas/1.1.0?topic=vpc-shutting-down-zos-virtual-server-instances){: external}.
{: note}

## Stopping a virtual server instance from the CLI
{: #stop-virtual-server-instances-cli}
{: cli}

You can stop the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To stop the virtual server instance, use the **`ibmcloud is instance-stop`** command. Specify the ID or name of the virtual server instance that you want to stop with the `INSTANCE` variable.

```sh
ibmcloud is instance-stop INSTANCE
```
{: pre}

The stop action shuts down the guest operating system and then the virtual server instance is deprovisioned. This change releases the instance resources that were being consumed. The virtual server instance goes into the Stop state. If the instance is stopped, the instance remains in the stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped, but volumes remain provisioned. If the instance is started, normal interaction and billing continue.

The following example stops an instance without requesting confirmation. The virtual server instance has an ID of `0777_e7af506a-35d4-451d-aa9e-59330e62b77e`. The `--force` option indicates that the request for confirmation is skipped.

```sh
ibmcloud is instance-stop 0777_e7af506a-35d4-451d-aa9e-59330e62b77e --force
```
{: pre}

A _Force stop_ action triggers a power cycle reset of the virtual server instance.
{: note}

If you have an instance that gets stuck in a _stopping_ state, you can use the following example command with the `--force` and `--no-wait` options that are specified to stop the instance immediately without confirmation. The instance has an ID of `0757_5446c277-3190-48dd-ac67-5f02fab39ed5`. The `--force` option indicates that the request for confirmation is skipped. The `--no-wait` option runs the command immediately, dropping any queued actions.

```sh
ibmcloud is instance-stop 0757_5446c277-3190-48dd-ac67-5f02fab39ed5 --force --no-wait
```
{: pre}

For a full list of command options, see [ibmcloud is instance-stop](/docs/vpc?topic=vpc-vpc-reference#instance-stop).

## Starting a virtual server instance from the CLI
{: #start-virtual-server-instances-cli}
{: cli}

You can start a virtual server instance that is stopped in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To start the virtual server instance, use the **`ibmcloud is instance-start`** command. Specify the ID or name of the virtual server instance that you want to start by using the `INSTANCE` variable.

```sh
ibmcloud is instance-start INSTANCE
```
{: pre}

For a full list of command options, see [ibmcloud is instance-start](/docs/vpc?topic=vpc-vpc-reference#instance-start) on the VPC CLI reference page.

## Stopping a virtual server instance with the API
{: #stop-virtual-server-instances-api}
{: api}

You can stop the virtual server instance in your {{site.data.keyword.vpc_short}} by using the API.

The following example stops a virtual server instance with an instance ID of `d6c3902d-1ecf-3a2c-b7ab-eb9143581000`.

```sh
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/d6c3902d-1ecf-3a2c-b7ab-eb9143581000/actions?version=2021-06-22&generation=2" -H "Authorization: $iam_token" -d '{"type": "stop"}'
```
{: pre}

The stop action shuts down the guest operating system and then the virtual server instance is deprovisioned. This change releases the instance resources that were being used. The virtual server instance goes into the Stop state. If the instance is stopped, the instance remains in the stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped, but volumes remain provisioned. If the instance is started, normal interaction and billing continue.

A _Force stop_ action triggers a power cycle reset of the virtual server instance.
{: note}

For more information, see the [Create an instance action](/apidocs/vpc/latest#create-instance-action) in the VPC API.

## Starting a virtual server instance with the API
{: #start-virtual-server-instances-api}
{: api}

You can start a virtual server instance that is stopped in your {{site.data.keyword.vpc_short}} by using the API.

The following example stops a virtual server instance with an instance ID of `d6c3902d-1ecf-3a2c-b7ab-eb9143581000`.

```sh
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/d6c3902d-1ecf-3a2c-b7ab-eb9143581000/actions?version=2021-06-22&generation=2" -H "Authorization: $iam_token" -d '{"type": "start"}'
```
{: pre}

The stop and start action remotely turns an instance off or on. If the instance is stopped, the instance remains in the stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped. If the instance is started, normal interaction and billing continue.

For more information, see the [Create an instance action](/apidocs/vpc/latest#create-instance-action) in the VPC API.

## Rebooting a virtual server instance in the console
{: #reboot-virtual-server-instances-ui}
{: ui}

The reboot action triggers a guest operating system reboot. The virtual server instance remains in a running state while the guest operating system is rebooting. Billing continues.

A _Force reboot_ action triggers a power cycle reset of the virtual server instance.
{: note}

From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, click **Reboot**.

## Rebooting a virtual server instance from the CLI
{: #reboot-virtual-server-instances-cli}
{: cli}

The reboot action triggers a guest operating system reboot. The virtual server instance remains in a running state while the guest operating system is rebooting. Billing continues.

A Force reboot action, by using the `--force` option, triggers a power cycle reset of the virtual server instance.
{: note}

You can reboot the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To reboot the virtual server instance, use the **`ibmcloud is instance-reboot`** command. Specify the ID or name of the virtual server instance that you want to reboot by using the `INSTANCE` variable.

```sh
ibmcloud is instance-reboot INSTANCE
```
{: pre}

For a full list of command options, see [ibmcloud is instance-reboot](/docs/vpc?topic=vpc-vpc-reference#instance-reboot).

## Rebooting a virtual server instance with the API
{: #reboot-virtual-server-instances-api}
{: api}

You can reboot the virtual server instance in your {{site.data.keyword.vpc_short}} by using the API. The following example reboots the specified virtual server.

```sh
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/d6c3902d-1ecf-3a2c-b7ab-eb9143581000/actions?version=2021-06-22&generation=2" -H "Authorization: $iam_token" -d '{"type": "reboot"}'
```
{: pre}

The reboot action triggers a guest operating system reboot. The virtual server instance remains in a running state while the guest operating system is rebooting. Billing continues.

A _Force reboot_ action triggers a power cycle reset of the virtual server instance.
{: note}

For more information, see the [Create an instance action](/apidocs/vpc/latest#create-instance-action) in the VPC API.

## Resizing a virtual server instance in the console
{: #resize-virtual-server-instances-ui}
{: ui}

You can increase or decrease the amount of vCPU and RAM that is available for greater flexibility in workload management to address resource requirement changes, optimize cost, or workload performance. After the resize is complete, you are billed the hourly rate of the new instance profile that you selected.

For the steps to resize a virtual server instance in the console, see [Resizing a virtual server instance by using the UI](/docs/vpc?topic=vpc-resizing-an-instance#resizing-a-virtual-server-UI).

## Resizing a virtual server instance from the CLI
{: #resize-virtual-server-instances-CLI}
{: cli}

You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management to address resource requirement changes, optimize cost or workload performance. After the resize is complete, you are billed the hourly rate of the new instance profile selected.

For the steps to resize a virtual server instance, see [Resizing a virtual server instance by using the CLI](/docs/vpc?topic=vpc-resizing-an-instance#resizing-a-virtual-server-CLI).

## Resizing a virtual server instance with the API
{: #resize-virtual-server-instances-API}
{: api}

You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management to address resource requirement changes, optimize cost or workload performance. After the resize is complete, you are billed the hourly rate of the new instance profile selected.

For the steps to resize a virtual server instance, see [Resizing a virtual server instance by using the API](/docs/vpc?topic=vpc-resizing-an-instance#resizing-a-virtual-server-API).

## Deleting a virtual server instance in the console
{: #delete-virtual-server-instances-ui}
{: ui}

You can delete the virtual server instance that is in your {{site.data.keyword.vpc_short}} in the console.

If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The account that is associated with the floating IP continues to be charged if it is not released.
{: important}

From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, click **Delete**.

The delete action permanently removes an instance, its connected vNIC, and data from your account. The instance boot volume is also deleted if the volume auto-delete setting is enabled. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. If a boot volume was created as part of provisioning a virtual server instance, the volume is deleted by default when the instance is deleted. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30 minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

## Deleting a virtual server instance from the CLI
{: #delete-virtual-server-instances-cli}
{: cli}

You can delete the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The account that is associated with the floating IP continues to be charged if it is not released.
{: important}

To delete the virtual server instance, use the **`ibmcloud is instance-delete`** command. Specify the ID or name of the virtual server instance that you want to delete by using the `INSTANCE` variable.

```sh
ibmcloud is instance-delete INSTANCE
```
{: pre}

For a full list of command options, see [ibmcloud is instance-delete](/docs/vpc?topic=vpc-vpc-reference#instance-delete).

The delete action permanently removes an instance, its connected vNIC, and data from your account. The instance boot volume is also deleted if the volume auto-delete setting is enabled. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. If a boot volume was created as part of provisioning a virtual server instance, the volume is deleted by default when the instance is deleted. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30 minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

## Deleting a virtual server instance with the API
{: #delete-virtual-server-instances-api}
{: api}

You can delete the virtual server instance in your {{site.data.keyword.vpc_short}} by using the API.

If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The account that is associated with the floating IP continues to be charged if it is not released.
{: important}

The following example deletes an instance.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-22&generation=2" -H "Authorization: $iam_token"
```
{: pre}

The delete action permanently removes an instance, its connected vNIC, and data from your account. The instance boot volume is also deleted if the volume auto-delete setting is enabled. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. If a boot volume was created as part of provisioning a virtual server instance, the volume deletes by default when the instance is deleted. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30 minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

For more information, see the [Delete an instance](/apidocs/vpc/latest#delete-instance) in the VPC API.

## Changing the auto-deletion setting of volumes that are attached to an instance in the console
{: #auto-delete-toggle-ui}
{: ui}

During instance provisioning, a boot volume is created with the auto-delete option enabled by default. When this feature is enabled, the volume is deleted when the instance is deleted.

The opposite is true for data volumes that are created during instance provisioning, the auto-delete feature is disabled for them. Data volumes are meant to be detached but not deleted by default so your data can persist beyond the virtual server instance lifecycle.

You can change this setting on the Edit boot volume panel when you create an instance, or later in the instance details page. For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui) and [Updating the auto-delete setting of a volume](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#auto-delete-ui).

## Changing the auto-deletion setting of volumes that are attached to an instance from the CLI
{: #auto-delete-toggle-cli}
{: cli}

During instance provisioning, a boot volume is created with the auto-delete option enabled by default. When this feature is enabled, the volume is deleted when the instance is deleted.

The opposite is true for data volumes that are created during instance provisioning, the auto-delete feature is disabled for them. Data volumes are meant to be detached but not deleted by default so your data can persist beyond the virtual server instance lifecycle.

You can change this setting by specifying the `auto_delete` property when you create the instance or update the boot volume attachment. For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli) and [Updating a volume attachment from the CLI](/docs/vpc?topic=vpc-managing-block-storage&interface=cli#update-vol-attachment-cli).

## Changing the auto-deletion setting of volumes that are attached to an instance with the API
{: #auto-delete-toggle-api}
{: api}

During instance provisioning, a boot volume is created with the auto-delete option enabled by default. When this feature is enabled, the volume is deleted when the instance is deleted.

The opposite is true for data volumes that are created during instance provisioning, the auto-delete feature is disabled for them. Data volumes are meant to be detached but not deleted by default so your data can persist beyond the virtual server instance lifecycle.

You can change this setting by specifying the `delete_volume_on_instance_delete` property when you [create the instance](/apidocs/vpc/latest#create-instance) or update the [volume attachment](/apidocs/vpc/latest#update-instance-volume-attachment). For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api) and [Updating a volume attachment with the API](/docs/vpc?topic=vpc-managing-block-storage&interface=api#update-vol-attachment-api).

## Viewing instance details in the console
{: #viewing-virtual-server-instances-ui}
{: ui}

You can view a summary of all instances on the _Virtual server instances_ page. You can access the details page for an instance by clicking an individual instance name to view details and make changes. From the instance details page, you can also view the associated network interface, access its subnet, toggle the auto-delete setting, and reserve or release a floating IP address.

Select [compute profiles](/docs/vpc?group=profile-details) support dynamic bandwidth allocation for data volumes. You can see whether the feature is enabled or disabled on the instance details page.

## Viewing instance details from the CLI
{: #viewing-virtual-server-instances-cli}
{: cli}

You can view the virtual server instance details in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To view the virtual server instance details, use the **`ibmcloud is instance`** command. Specify the ID or name of the virtual server instance that you want to view by using the `INSTANCE` variable.

```sh
ibmcloud is instance INSTANCE
```
{: pre}

## Viewing instance details with the API
{: #viewing-virtual-server-instances-api}
{: api}

You can view the virtual server instance details in your {{site.data.keyword.vpc_short}} by using the API.

The following example displays the virtual server instance details for an instance profile with a profile name of `version=2021-06-22&generation=2`.

```sh
curl -X GET "https://us-south.iaas.cloud.ibm.com/v1/instance/profiles/$profile_name?version=2021-06-22&generation=2" -H "Authorization: $iam_token"
```
{: pre}

For more information, see the [Retrieve an instance profile](/apidocs/vpc/latest#get-instance-profile) in the VPC API.

## Adjusting instance bandwidth allocation in the console
{: #adjusting-bandwidth-allocation-ui}
{: ui}

You can adjust the allocation of your instance's total bandwidth between network bandwidth and storage bandwidth in the console.

To adjust the bandwidth of an instance, use the following steps.

1. Go to a virtual service instance.
2. Select **Bandwidth allocation**.
3. On the **Edit bandwidth allocation** screen, adjust the value for **Storage bandwidth**. You can increase the bandwidth that is allocated for your Block Storage boot and attached data volumes. For more information about storage bandwidth allocation, see [Bandwidth allocation for Block Storage volumes](/docs/vpc?topic=vpc-block-storage-bandwidth). After you set storage bandwidth, network bandwidth is automatically adjusted so the total bandwidth of the instance equals the displayed Total bandwidth value. The value for Network bandwidth or Storage bandwidth cannot be set to less than 500 Mbps.

To view the new bandwidth allocation, you must either stop and start the instance, or detach and reattach secondary volumes.

## Adjusting instance bandwidth allocation from the CLI
{: #adjusting-bandwidth-allocation-cli}
{: cli}

You can adjust the allocation of your instance's total bandwidth between network bandwidth and storage bandwidth by using the CLI.

To reallocate instance bandwidth, run the `instance-update` command and specify the total storage bandwidth in megabits per second (Mbps) for the `total-volume-bandwidth` parameter. Use the following syntax. Use the ID or name of the instance for INSTANCE.

```sh
ibmcloud is instance-update INSTANCE --total-volume-bandwidth VALUE
```
{: pre}

Total storage bandwidth (in megabits per second) is the total bandwidth that is allocated for the boot and attached data volumes. Increasing total storage bandwidth results in a corresponding decrease in network bandwidth. The minimum network bandwidth that you can have is 500 Mbps, so adjust the total storage bandwidth as needed.

To see the new bandwidth allocation, you must either stop and start the instance, or detach and reattach data volumes.

## Adjusting instance bandwidth allocation with the API
{: #adjusting-bandwidth-allocation-api}
{: api}

You can adjust the total storage bandwidth for an existing instance with the API. Make a `PATCH /instances` request and specify a new value for `total_volume_ bandwidth`. Total storage bandwidth (in megabits per second) is the total bandwidth that is allocated for primary boot and secondary attached data volumes. Increasing total storage bandwidth results in a corresponding decrease in network bandwidth. The minimum network bandwidth that you can have is 500 Mbps, so adjust the total storage bandwidth as needed. For example,

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-22&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
      "total_volume_bandwidth": 500
      }'
```
{: pre}

To see the new bandwidth allocation, you must either stop and start the instance, or detach and reattach data volumes. Bandwidth allocation for individual volumes is updated when you add a data volume by using the `POST / volume_attachments` method or delete a volume by using the `DELETE volume_attachments` method.

## Adjusting storage QoS mode in the console
{: #updating-qos-mode-ui}
{: ui}

You can change the Storage QoS mode of a virtual server instance that uses one of the [General purpose - Flex](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers), [General purpose Gen 3](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen3-intel), [Accelerated Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family ), or [Confidential Computing - Gen 3](/docs/vpc?topic=vpc-confidential-computing-vsi-profiles-gen3-x86) profiles to enable dynamic bandwidth allocation for attached data volumes.

1. Go to a virtual service instance.
2. On the virtual server instance details page, click the toggle to switch the QoS mode from _weighted_ to _pooled_.

For the QoS mode change to take effect, you must stop and start the virtual server instance.
{: important}

## Adjusting storage QoS mode from the CLI
{: #updating-qos-mode-cli}
{: cli}

You can change the Storage QoS mode of a virtual server instance that uses one of the [General purpose - Flex](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers), [General purpose Gen 3](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen3-intel), [Accelerated Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family ), or [Confidential Computing - Gen 3](/docs/vpc?topic=vpc-confidential-computing-vsi-profiles-gen3-x86) profiles to enable dynamic bandwidth allocation for attached data volumes by using the `ibmcloud is instance-update` command.

```sh
ibmcloud is instance-update INSTANCE --storage_qos_modes pooled
```
{: pre}

You can use the same command to revert to the weighted QoS mode.

```sh
ibmcloud is instance-update INSTANCE --storage_qos_modes weighted
```
{: pre}

For the QoS mode change to take effect, you must first stop the virtual server instance. Run the `instance-update` command. Then, start the instance.
{: important}

## Adjusting storage QoS mode with the API
{: #updating-qos-mode-api}
{: api}

You can programmatically change the storage QoS mode of a virtual server instance that uses one of the [General purpose - Flex](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers), [General purpose Gen 3](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen3-intel), [Accelerated Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family ), or [Confidential Computing - Gen 3](/docs/vpc?topic=vpc-confidential-computing-vsi-profiles-gen3-x86) profiles by calling the `/instances/{id}` method in the [VPC API](/apidocs/vpc/latest#update-instance){: external} as shown in the following sample request.

The following example request enables dynamic bandwidth allocation for data volumes that are attached to a 3rd generation virtual server.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-07-11&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
      "storage_qos_mode": "pooled"
      }'
```
{: pre}

The following example request reverts the storage QoS mode back to weighted.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-07-11&generation=2" \
  -H "Authorization: $iam_token" \
  -d '{
      "storage_qos_mode": "weighted"
      }'
```
{: pre}

For the QoS mode change to take effect, you must first stop the virtual server instance. Make the PATCH request. Then, start the instance.
{: important}

## Retrieving the virtual server instance identifier
{: #retrieve-VSI-instance-identifer}

When a virtual server instance is created, that instance is automatically assigned an instance identifier (ID), which includes the SMBIOS system-uuid as a portion of the ID. The ID can be up to 64 bytes and consists of digits, lowercase letters, underscores, and dashes.

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
   - This property provides the status of the virtual server instance through the [Retrieve an instance](/apidocs/vpc/latest#get-instance) request. The values that `status` returns are specialized for virtual server instances and indicate whether it is running, stopped, or transitioning. For more information, see the [Virtual Private Cloud API](/apidocs/vpc/latest#about-vpc-api).
- `lifecycle_state`
   - This property provides the state of a resource through the [Retrieve an instance](/apidocs/vpc/latest#get-instance) request. The values that `lifecycle_state` provide are generic and are meant to apply to various resources, such as [Placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc). `lifecycle_state` can return values that overlap with `status`. `lifecycle_state` also includes values that detail if a resource is suspended.

## Setting the host failure auto restart in the console
{: #set-recovery-policy-ui}
{: ui}

To set the host failure auto restart for an existing instance, complete the following steps.

   1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, click **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
   2. On the **Virtual server instances** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance that you want to manage.
   3. From the instance details page, locate 'Host failure auto restart'. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") and choose Enabled or Disabled to toggle the status of the host recovery policy on or off.

   For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=api).

## Setting the host failure recovery policy from the CLI
{: #set-recovery-policy-cli}
{: cli}

You can update an instance in your {{site.data.keyword.cloud}} VPC with and change the availability policy on host failure by using the command-line interface (CLI). Run the ibmcloud `instance-update` command and set the `--host-failure-policy` property to `start` or `stop`. The host failure policy service is set to `restart` by default. Here, INSTANCE can be the ID or Name of the Instance.

```sh
ibmcloud is instance-update INSTANCE --total-volume-bandwidth VALUE --host-failure-policy stop
```
{: pre}

## Setting the host failure recovery policy with the API
{: #set-policy-api}
{: api}

During an instance [update](/apidocs/vpc/latest#update-instance), the `host_failure` subproperty can be used to set the host failure `availability_policy` of the virtual server instance.

## Setting the confidential compute value from the CLI
{: #set-confidential-compute-cli}
{: cli}

[Select availability]{: tag-green}

Confidential computing with Intel SGX for VPC and Confidential computing with Intel TDX for VPC are available in the Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de) regions.
{: preview}

You can update an instance and change the `confidential-compute-mode` by using the command-line interface (CLI). Use the `ibmcloud is instance-update` command. For INSTANCE, specify the ID or name of the instance and set the `--confidential-compute-mode` property to `sgx` or `tdx`.

```sh
ibmcloud is instance-update INSTANCE --confidential-compute-mode sgx
```
{: pre}

## Setting the confidential compute value from the API
{: #set-confidential-compute-API}
{: api}

[Select availability]{: tag-green}

Confidential computing with Intel SGX for VPC and Confidential computing with Intel TDX for VPC are available in the Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de) regions.
{: preview}

You can update a virtual server instance and change the `confidential_compute_mode` property by using the API. Use the ibmcloud `update-instance` command. Make a `PATCH /instances` request and specify a new value for the `confidential_compute_mode` property. To enable confidential computing, change this value to `sgx` or `tdx`.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-10-17&generation=2" -H "Authorization: Bearer $iam_token" -d '{"confidential_compute_mode": "sgx"}'
```
{: pre}

For more information, see the [update an instance action](/apidocs/vpc/latest#update-instance) in the {{site.data.keyword.vsi_is_short}} API.

## Disable or enable secure boot in the console
{: #disable-secure-boot-ui}
{: ui}

When you select a [confidential computing instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles), the secure boot option is enabled by default. You can disable secure boot on your virtual server instance, but you must first stop the virtual server instance. After disabling secure boot, you can then restart your virtual server instance.

1. From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, select the virtual server instance.
1. From **Actions**, click **Stop**.
1. In **Advanced configuration details**, toggle secure boot to **Disabled**.
1. From **Actions**, click **Start**.

If you decide to re-enable secure boot, follow these same steps and toggle the option back to **Enabled**

## Enabling and disabling secure boot from the CLI
{: #set-secure-boot-cli}
{: cli}

When you select a [confidential computing instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles), the secure boot option is enabled by default. You can disable secure boot on your virtual server instance, but you must first stop the virtual server instance. After disabling secure boot, you can then restart your virtual server instance.

You can update an instance and change the `enable-secure-boot` by using the command-line interface (CLI). Use the `ibmcloud is instance-update` command. For INSTANCE, specify the ID or name of the instance and set the `--enable-secure-boot` property to `false`.

```sh
ibmcloud is instance-update INSTANCE --enable-secure-boot false
```
{: pre}

If you decide to re-enable secure boot, follow these same steps and set the `--enable-secure-boot` property to `true`.

## Enabling and disabling secure boot value from the API
{: #set-secure-boot-API}
{: api}

When you select a [confidential computing instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles), the secure boot option is enabled by default. You can disable secure boot on your virtual server instance, but you must first stop the virtual server instance. After disabling secure boot, you can then restart your virtual server instance.

You can update a virtual server instance and change the `enable_secure_boot` property by using the API. Use the `update-instance` command. Make a `PATCH /instances` request and specify a new value for the `enable_secure_boot` property. To enable confidential secure boot, change this value to `false`.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-10-17&generation=2" -H "Authorization: Bearer $iam_token" -d '{"enable_secure_boot": "false"}'
```
{: pre}

If you decide to re-enable secure boot, follow these same steps and set the `enable_secure_boot` property to `true`.

For more information, see the [update an instance action](/apidocs/vpc/latest#update-instance) in the {{site.data.keyword.vsi_is_short}} API.

## Detaching a server from a reservation in the console
{: #removing-adding-server-reserved-capacity-ui-vpc}
{: ui}

You can detach a virtual server from a reservation in the console.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon ![the menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Reservations**.
1. From your virtual server list or in the Reservation details page, click the server that you want to detach and then click **Actions** > **Detach**.
1. To confirm, click **Detach**.

## Adding CSPM in the console
{: #cloud-security-posture-management}
{: ui}

When you select this option, a workload protection instance is created with the configurations to provide CSPM to all the resources. If a workload protection instance already exists, this option is not available. For more information, see [About IBM Cloud Security Posture Management (CSPM)](/docs/workload-protection?topic=workload-protection-about&interface=ui).

You can add CSPM (Cloud Security Posture Management) on the _Virtual server instances_ page.

1. From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, click an individual instance name to view details.
1. From the details page, go to the **Integrations** tab.
1. In **Cloud security posture management**, click **Add CSPM**.
1. On the **Add cloud security posture management (CSPM)**, click **Create**.

This setting adds CSPM to an existing Workload Protection instance for your account. For more information, see [Implementing CSPM (Cloud Security Posture Management) for IBM Cloud](/docs/workload-protection?topic=workload-protection-cspm-implement&interface=ui).
