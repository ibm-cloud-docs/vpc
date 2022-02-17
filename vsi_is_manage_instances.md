---

copyright:
  years: 2019, 2022
lastupdated: "2022-02-17"

keywords: view instance details, restart, stop, instance details, delete

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:preview: .preview} 
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

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
| Delete | To delete an instance, the instance must have a powered off status. If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The delete action permanently removes an instance and its connected vNIC, boot volume, and data from your account. |
{: caption="Table 1. Actions available for virtual server instances" caption-side="top"}

## Rename a virtual server instance using the UI
{: #rename-virtual-server-instances-ui}
{: ui}

From the *Virtual server instances* page in {{site.data.keyword.cloud_notm}} console, click **Rename**. Enter the new name for the virtual server instance. Click **Rename**.

## Rename a virtual server instance using the CLI
{: #rename-virtual-server-instances-CLI}
{: cli}

You can rename the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To rename the virtual server instance by using the CLI, use the **ibmcloud is instance-update** command. Specify the new name of the instance disk in `--name`.

The following example renames an instance with a current ID of `72251a2e-d6c5-42b4-97b0-b5f8e8d1f479` to a new name of `my-instance`.

```
ibmcloud is instance-update 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --name my-instance
```
{: pre}

For more information, see [ibmcloud is instance-update](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-update) in the VPC CLI reference page.

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

## Stop a virtual server instance using the CLI
{: #stop-virtual-server-instances-cli}
{: cli}

You can stop the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To stop the virtual server instance by using the CLI, use the **ibmcloud is instance-stop** command. Specify the ID of the instance that is being stopped.

The following example stops an instance with an ID of `INSTANCE`.

```
ibmcloud is instance-stop INSTANCE [--no-wait] [-f, --force] [--output JSON] [-q, --quiet]
```
{: pre}

The stop action shuts down the guest operating system and then the virtual server instance is deprovisioned. This change frees the instance resources that were being consumed. The virtual server instance is transitioned to the Stop state. If the instance is stopped, the instance remains in the stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped, but volumes remain provisioned. If the instance is started, normal interaction and billing continues.

The following example stops an instance without requesting confirmation. The instance has an ID of `0777_e7af506a-35d4-451d-aa9e-59330e62b77e`. The `--force` option indicates that the request for confirmation is skipped. 

```
ibmcloud is instance-stop 0777_e7af506a-35d4-451d-aa9e-59330e62b77e --force
```
{: pre}

A Force stop action triggers a power cycle reset of the virtual server instance.
{: note}

If you have an instance that gets stuck in a *stopping* state, you can use the following example command with the `--force` and `--no-wait` options specified to stop the instance immediately without confirmation. The instance has an ID of `0757_5446c277-3190-48dd-ac67-5f02fab39ed5`. The `--force` option indicates that the request for confirmation is skipped. The `--no-wait` option runs the command immediately, dropping any queued actions. 

```
ibmcloud is instance-stop 0757_5446c277-3190-48dd-ac67-5f02fab39ed5 --force --no-wait
```
{: pre}

For more information, see [ibmcloud is instance-stop](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-stop) on the VPC CLI reference page.

## Start a virtual server instance using the CLI
{: #start-virtual-server-instances-cli}
{: cli}

You can start a virtual server instance that is stopped in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To start the virtual server instance by using the CLI, use the **ibmcloud is instance-start** command. Specify the ID of the instance that is being started.

The following example starts an instance with an ID of `INSTANCE`.

```
ibmcloud is instance-start INSTANCE [--output JSON] [-q, --quiet]
```
{: pre}

For more information, see [ibmcloud is instance-start](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-start) on the VPC CLI reference page.

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

A Force reboot action triggers a power cycle reset of the virtual server instance.
{: note}

You can reboot the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To reboot the virutal server instance by using the CLI, use the **ibmcloud is instance-reboot** command. Specify the ID of the instance that is being rebooted.

The following example reboots an instance with an ID of `INSTANCE`.

```sh
ibmcloud is instance-reboot INSTANCE [--no-wait] [-f, --force] [--output JSON] [-q, --quiet]
```
{: pre}

For more information, see [ibmcloud is instance-reboot](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-reboot) on the VPC CLI reference page.

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

The delete action permanently removes an instance and its connected vNIC, and data from your account. The instance boot volume is also deleted if it was configured to be deleted when the attached instance is deleted. If the instance has one or more attached data volumes, those volumes are preserved unless the default setting is changed to auto-delete. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30  minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

## Delete a virtual server instance using the CLI
{: #delete-virtual-server-instances-cli}
{: cli}

You can delete the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To delete the virtual server instance by using the CLI, use the **ibmcloud is instance-delete** command. Specify the ID of the instance that is being deleted.

The following example deletes 2 instances with an IDs of `INSTANCE1` and `INSTANCE2`.

```
ibmcloud is instance-delete (INSTANCE1 INSTANCE2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

The delete action permanently removes an instance and its connected vNIC, and data from your account. The instance boot volume is also deleted if it was configured to be deleted when the attached instance is deleted. If the instance has one or more attached data volumes, those volumes are preserved unless the default setting is changed to auto-delete. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30  minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

## Delete a virtual server instance using the API
{: #delete-virtual-server-instances-api}
{: api}

You can delete the virtual server instance in your {{site.data.keyword.vpc_short}} by using the API.

The following example deletes an instance.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-22&generation=2" -H "Authorization: $iam_token"
```
{: pre}

The delete action permanently removes an instance and its connected vNIC, and data from your account. The instance boot volume is also deleted if it was configured to be deleted when the attached instance is deleted. If the instance has one or more attached data volumes, those volumes are preserved unless the default setting is changed to auto-delete. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30  minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

For more information, see the [Delete an instance](/apidocs/vpc#delete-instance) in the Virtual Private Cloud API documentation.

## Viewing instance details using the UI
{: #viewing-virtual-server-instances-ui}
{: ui}

You can interact with instances by viewing the summary of all instances on the *Virtual server instances* page, or by clicking an individual instance name to view details and make changes. From the instance details page, you can also view the associated network interface, access its subnet, and reserve or delete a floating IP address.

## Viewing instance details using the CLI
{: #viewing-virtual-server-instances-cli}
{: cli}

You can view the virtual server instance details in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To view the virtual server instance details by using the CLI, use the **ibmcloud is instance** command. Specify the ID of the instance that is being viewed.

The following example views the details of the virtual server instance with the ID `INSTANCE`.

```
ibmcloud is instance INSTANCE [--output JSON] [-q, --quiet]
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
3. On the Edit bandwidth allocation screen, adjust the value for Storage bandwidth. You can increase the bandwidth allocated for your block storage boot and attached data volumes. For information about how storage bandwidth is allocated, see [Bandwidth allocation for block storage volumes](/docs/vpc?topic=vpc-block-storage-bandwidth).
After you set storage bandwidth, network bandwidth is automatically adjusted so the total bandwidth of the instance equals the displayed Total bandwidth value. The value for Network bandwidth or Storage bandwidth cannot be set less than 500 Mbps.

To view the new bandwidth allocation, you must either stop and start the instance, or detach and reattach secondary volumes.
{: note}

## Adjusting bandwidth allocation using the CLI
{: #adjusting-bandwidth-allocation-cli}
{: cli}

You can adjust the allocation of your instance's total bandwidth between network bandwidth and storage bandwidth using the CLI.

To reallocate instance bandwidth by using the CLI, run the `instance-update {id}` command and specify the total storage bandwidth in megabits per second (Mbps) for the `total-volume-bandwidth` parameter. Use this syntax:
```
ibmcloud is instance-update {id} --total-volume-bandwidth VALUE
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

```
dmidecode -s system-family
```
{: pre}

Windows

```
Get-WmiObject Win32_ComputerSystem | Select-Object -ExpandProperty SystemFamily
```
{: pre}

From within your virtual server, you can retrieve the SMBIOS system-uuid in one of the following ways:

Linux

```
dmidecode -s system-uuid
```
{: pre}

Windows

```
 Get-WmiObject Win32_ComputerSystemProduct | Select-Object -ExpandProperty UUID
 ```
{: pre}
