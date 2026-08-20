---

copyright:
  years: 2019, 2026
lastupdated: "2026-08-20"

keywords: view instance details, restart virtual server, stop, details, delete

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing virtual server instances
{: #managing-virtual-server-instances}

Manage your {{site.data.keyword.vsi_is_full}} instances by completing tasks such as start, stop, restart, and delete virtual server instances.
{: shortdesc}

## Managing instances from the Virtual server instances page
{: #managing-virtual-server-instances-list-ui}
{: ui}

You can manage your {{site.data.keyword.vsi_is_full}} instances from the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console.

To manage your instances, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
2. On the **Virtual server instances** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance that you want to manage. You can select from the following actions:

| Action | Description |
|--------|-------------|
| Rename | Change the name of the instance. |
| Stop | Stop the instance. |
| Start | Start an instance that is stopped. This action is not available if the instance status is Running. |
| Reboot | Immediately powers off a running instance and then powers it back on again. |
| Open VNC console | Open a VNC console session for the instance. |
| Open serial console | Open a serial console session for the instance. |
| Resize | Vertically scale virtual server instances to any supported profile size. For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance). |
| Create image | Create a custom image from the instance. |
| Reload OS | You can reload the operating system (OS) on a device at any time to restore a device to its original working order. |
| Delete | To delete an instance, the instance must have a powered off status. If the instance has a floating IP address, it must be unassociated or released before the instance is deleted. The delete action permanently removes an instance and its connected vNIC, and data from your account. If auto-delete is enabled, the associated boot volume is also deleted. |
{: caption="Actions available for virtual server instances" caption-side="bottom"}

## Renaming a virtual server instance in the console
{: #rename-virtual-server-instances-ui}
{: ui}

You can rename a virtual server instance in the console.

1. On the **Virtual server instances** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance that you want to rename, then click **Rename**.
2. Enter the new name for the virtual server instance, then click **Rename**.

## Listing virtual server instances from the CLI
{: #list-virtual-server-instances-cli}
{: cli}

Before you can manage a virtual server instance, you need its name or ID. To list all virtual server instances in your account, use the following command.


```sh
ibmcloud is instances
```
{: pre}

For a full list of command options, see [ibmcloud is instances](/docs/vpc?topic=vpc-vpc-reference#instances-list).

## Listing virtual server instances with the API
{: #list-virtual-server-instances-api}
{: api}

Before you can manage a virtual server instance, you need its ID. To list all virtual server instances in your account, make the following request.

```sh
curl -X GET "$vpc_api_endpoint/v1/instances?version=2021-06-22&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

For more information, see [List all instances](/docs/apis/vpc/latest#list-instances) in the VPC API.

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
curl -X GET "$vpc_api_endpoint/v1/instance/profiles/$profile_name?version=2021-06-22&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

For more information, see the [Retrieve an instance profile](/docs/apis/vpc/latest#get-instance-profile) in the VPC API.

## Viewing instance details by using Terraform
{: #viewing-virtual-server-instances-terraform}
{: terraform}

You can retrieve information about an existing virtual server instance by using Terraform.

The following example retrieves information about a virtual server instance by name:

```terraform
data "ibm_is_instance" "example" {
  name        = "my-instance"
}
```
{: codeblock}

For more information, see [ibm_is_instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_instance){: external}.

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
   curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-29&generation=2" -H "Authorization: Bearer $iam_token" -d '{"name": "my-instance"}'
   ```
   {: pre}

## Renaming a virtual server instance by using Terraform
{: #rename-virtual-server-instances-terraform}
{: terraform}

You can rename a virtual server instance in your {{site.data.keyword.vpc_short}} by using Terraform. Update the `name` argument in the `ibm_is_instance` resource.

The following example renames a virtual server instance to `my-renamed-instance`:

```terraform
resource "ibm_is_instance" "example" {
  name    = "my-renamed-instance"
  # ... other required arguments
}
```
{: codeblock}

For more information, see the [`name` argument](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance#argument-reference){: external} in the `ibm_is_instance` resource documentation.

## Stopping and starting a virtual server instance
{: #stop-start-virtual-server-instances}

The stop action shuts down the guest operating system and then the virtual server instance is deprovisioned. This change releases the instance resources that were being used. The virtual server instance goes into the stopped state. If the instance is stopped, it remains in the stopped state and must be started manually. Billing is [suspended](/docs/vpc?topic=vpc-suspend-billing) for some compute resources while the instance is stopped. You cannot interact with an instance if it is stopped, but volumes remain provisioned. When the instance is started, normal interaction and billing continue.

A _Force stop_ action triggers a power cycle reset of the virtual server instance.
{: note}

The start action starts a virtual server instance that is in a stopped state.

When a virtual server is stopped, it is removed from the host. When the virtual server is started again later, it might be started on a new host. Capacity for a specific virtual server profile is not guaranteed or reserved; for example, capacity might be limited, or not available, for profile families such as GPU (Accelerated) or Storage Optimized.

_For z/OS virtual server instances only_: You must shut down all the subsystems in the z/OS system to halt the virtual server instance. For more information, see [Shutting down z/OS virtual server instances](https://www.ibm.com/docs/en/wazi-aas/1.1.0?topic=vpc-shutting-down-zos-virtual-server-instances){: external}.
{: note}

## Stopping and starting a virtual server instance in the console
{: #stop-start-virtual-server-instances-ui}
{: ui}

From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, click **Stop** or **Start**.

## Stopping a virtual server instance from the CLI
{: #stop-virtual-server-instances-cli}
{: cli}

You can stop the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI).

To stop the virtual server instance, use the **`ibmcloud is instance-stop`** command. Specify the ID or name of the virtual server instance that you want to stop with the `INSTANCE` variable.

```sh
ibmcloud is instance-stop INSTANCE
```
{: pre}

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
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/d6c3902d-1ecf-3a2c-b7ab-eb9143581000/actions?version=2021-06-22&generation=2" -H "Authorization: Bearer $iam_token" -d '{"type": "stop"}'
```
{: pre}

For more information, see the [Create an instance action](/docs/apis/vpc/latest#create-instance-action) in the VPC API.

## Starting a virtual server instance with the API
{: #start-virtual-server-instances-api}
{: api}

You can start a virtual server instance that is stopped in your {{site.data.keyword.vpc_short}} by using the API.

The following example starts a virtual server instance with an instance ID of `d6c3902d-1ecf-3a2c-b7ab-eb9143581000`.

```sh
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/d6c3902d-1ecf-3a2c-b7ab-eb9143581000/actions?version=2021-06-22&generation=2" -H "Authorization: Bearer $iam_token" -d '{"type": "start"}'
```
{: pre}

For more information, see the [Create an instance action](/docs/apis/vpc/latest#create-instance-action) in the VPC API.

## Stopping a virtual server instance by using Terraform
{: #stop-virtual-server-instances-terraform}
{: terraform}

You can stop a virtual server instance by using Terraform.

The following example stops a virtual server instance:

```terraform
resource "ibm_is_instance_action" "example" {
  action       = "stop"
  force_action = true
  instance     = ibm_is_instance.example.id
}
```
{: codeblock}

Set `force_action` to `true` to force the stop immediately and delete all queued actions.
{: note}

For more information, see [ibm_is_instance_action](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_action){: external}.

## Starting a virtual server instance by using Terraform
{: #start-virtual-server-instances-terraform}
{: terraform}

You can start a virtual server instance that is stopped by using Terraform.

The following example starts a virtual server instance:

```terraform
resource "ibm_is_instance_action" "example" {
  action   = "start"
  instance = ibm_is_instance.example.id
}
```
{: codeblock}

For more information, see [ibm_is_instance_action](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_action){: external}.

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
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/d6c3902d-1ecf-3a2c-b7ab-eb9143581000/actions?version=2021-06-22&generation=2" -H "Authorization: Bearer $iam_token" -d '{"type": "reboot"}'
```
{: pre}

The reboot action triggers a guest operating system reboot. The virtual server instance remains in a running state while the guest operating system is rebooting. Billing continues.

A _Force reboot_ action triggers a power cycle reset of the virtual server instance.
{: note}

For more information, see the [Create an instance action](/docs/apis/vpc/latest#create-instance-action) in the VPC API.

## Rebooting a virtual server instance by using Terraform
{: #reboot-virtual-server-instances-terraform}
{: terraform}

You can reboot a virtual server instance by using Terraform.

The following example reboots a virtual server instance:

```terraform
resource "ibm_is_instance_action" "example" {
  action       = "reboot"
  force_action = true
  instance     = ibm_is_instance.example.id
}
```
{: codeblock}

The reboot action triggers a guest operating system reboot. The virtual server instance remains in a running state while the guest operating system is rebooting. Billing continues.

A _Force reboot_ action triggers a power cycle reset of the virtual server instance. Set `force_action` to `true` to force the action immediately and delete all queued actions.
{: note}

For more information, see [ibm_is_instance_action](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_action){: external}.

## Resizing a virtual server instance in the console
{: #resize-virtual-server-instances-ui}
{: ui}

You can increase or decrease the amount of vCPU and RAM that is available for greater flexibility in workload management to address resource requirement changes, optimize cost, or workload performance. After the resize is complete, you are billed for the hourly rate of the new instance profile that you selected.

For the steps to resize a virtual server instance in the console, see [Resizing a virtual server instance by using the UI](/docs/vpc?topic=vpc-resizing-an-instance#resizing-a-virtual-server-UI).

## Resizing a virtual server instance from the CLI
{: #resize-virtual-server-instances-CLI}
{: cli}

You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management to address resource requirement changes, optimize cost, or workload performance. After the resize is complete, you are billed for the hourly rate of the new instance profile selected.

For the steps to resize a virtual server instance, see [Resizing a virtual server instance by using the CLI](/docs/vpc?topic=vpc-resizing-an-instance#resizing-a-virtual-server-CLI).

## Resizing a virtual server instance with the API
{: #resize-virtual-server-instances-API}
{: api}

You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management to address resource requirement changes, optimize cost, or workload performance. After the resize is complete, you are billed for the hourly rate of the new instance profile selected.

For the steps to resize a virtual server instance, see [Resizing a virtual server instance by using the API](/docs/vpc?topic=vpc-resizing-an-instance#resizing-a-virtual-server-API).

## Resizing a virtual server instance by using Terraform
{: #resize-virtual-server-instances-terraform}
{: terraform}

You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management by updating the `profile` argument in the `ibm_is_instance` resource. After the resize is complete, you are billed for the hourly rate of the new instance profile.

The following example updates the profile of a virtual server instance:

```terraform
resource "ibm_is_instance" "example" {
  name    = "my-instance"
  profile = "bx2-4x16"
  # ... other required arguments
}
```
{: codeblock}

For more information, see the [`profile` argument](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance#argument-reference){: external} in the `ibm_is_instance` resource documentation.

## Reloading the OS
{: #reload-operating-system-instances}

You can reload the operating system (OS) on a device at any time to restore a device to its original working order. Or, you can reconfigure a device with a different operating system or software. An OS reload removes all data from the device and applies a "like new" configuration, as specified during the configuration process of the OS reload setup. Because OS reloads clear all data from the device, if the data is not backed up before the reload, the data is permanently deleted.

If you want to retain your data, back up all data before you start an OS reload.
{: tip}

### Before you begin
{: #byb-reload-os-vpc}

* Go to the console device menu.
* Make sure that you have any necessary account permissions and device access. Only the account owner, or a user with the Manage users Classic infrastructure permission, can adjust the permissions.
* The virtual server must be stopped.

### Reloading the OS by using the UI
{: #reloading-os-software-vpc-ui}
{: ui}

Use the following steps to reload the OS.

1. From the Device list, click the virtual server that needs an OS reload to show the **Device details** page and make sure that the server is stopped.
1. From the Actions menu, select **Reinitialize**.
1. Determine whether you want to reload the existing configuration or reload the device with a new configuration.

   | Reload type | Steps |
   |-------------|-------|
   | If you want to reload a new configuration... | Click **Change image** to select a new operating system. software from the **Select software** drop-down list. |
   | If you want to reload by using the existing configuration... | Proceed to the next step. |
   | If you want to change the operating system... | Click **Edit OS** > **Change version or manufacturer**. |
   {: caption="OS and software reload options" caption-side="top"}

1. Determine whether to apply one or more SSH keys to the device. RSA keys are required for Windows.
1. Make the applicable selections for the options that you want to apply to the device during or after the OS reload. Options vary based on device. Not all options are available for every device.

   | Options | Description |
   | --- | --- |
   | Postinstallation script | Adds an existing or new postinstallation script. |
   | SSH key | Adds an SSH key to the device upon the reload action. An RSA key is required for Windows. |
   | OS reload with disk preservation | This option configures your current primary disk as a secondary disk, and creates a new primary disk. The OS is installed on the new primary disk. |
   {: caption="Post OS reload options" caption-side="top"}

1. Click **Reload with configuration** to proceed to review. Or, you can click **Cancel** to cancel the changes to the device.
1. Verify that all details in the _New configuration_ section are correct.
1. Click **Confirm OS reload** to confirm and initiate the OS reload. Or, you can click **Cancel** to cancel the reload. The reload can't be stopped nor undone.

When the reload is complete, a new admin password is provided, if applicable.

When you confirm that OS reload, the public network for the server is disabled and all data that is on the primary disk is permanently deleted. IBM Cloud isn't responsible for any lost data.
{: important}

If you receive an error during the OS reload process, check whether the operating system is supported or obsolete. For more help, [open a support case](/docs/support?topic=support-using-avatar).

## Reloading the OS by using the CLI
{: #reloading-os-software-vpc-cli}
{: cli}

You can reload the OS by using the CLI. Use the following CLI command to reload the OS.

If you want to retain your data, back up all data before you start an OS reload.
{: tip}

This example reinitializes the `INSTANCE` by using the `IMAGE` with the boot volume options contained in `BOOT_VOLUME_JSON`. It uses `KEY1` and `KEY2` as the keys, `DEFAULT_TRUSTED_PROFILE` as the default trusted profile, and `DATA` as the user data. The default trusted profile is auto linked because `--default-trusted-profile-auto-link` is set as `true`.

```sh
ibmcloud is instance-reinitialize INSTANCE --image IMAGE --boot-volume BOOT_VOLUME_JSON --keys KEY1,KEY2 --default-trusted-profile DEFAULT_TRUSTED_PROFILE --default-trusted-profile-auto-link true --user-data DATA
```
{: pre}

## Reloading the OS by using the API
{: #reloading-os-software-vpc-api}
{: api}

You can reload the OS by using the API. Use the follow API call to reload the OS.

If you want to retain your data, back up all data before you start an OS reload.
{: tip}

This example reinitializes the instance `instance_id` by using the image `image_id` with the boot volume attachment options that are provided. It uses `key_id` as the key, `some_data` for the user data, and `profile_id` as the default trusted profile. The default trusted profile is linked because `auto_link` is set as `true`.

```sh
curl -X POST "https://us-south.iaas.cloud.ibm.com/v1/instances/instance_id/reinitialize?version=2026-07-01&generation=2" -H "Authorization: Bearer $iam_token" -d
'{
    "image": {
         "id": "image_id"
    },
    "boot_volume_attachment": {
      "volume":  {
        "profile": {
          "name": "general-purpose"
          }
        }
      },"keys": [{
        "id": "key_id"
	}],
	"user_data": "some_data",
	"default_trusted_profile": {
      "target": {
	    "id": "profile_id"
	   },
	   "auto_link": true
	}
}'
```
{: pre}

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
curl -X DELETE "$vpc_api_endpoint/v1/instances/$instance_id?version=2021-06-22&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

The delete action permanently removes an instance, its connected vNIC, and data from your account. The instance boot volume is also deleted if the volume auto-delete setting is enabled. If an existing boot volume is attached as part of provisioning a virtual server instance, the volume is preserved by default when the instance is deleted. If a boot volume was created as part of provisioning a virtual server instance, the volume deletes by default when the instance is deleted. After you confirm the delete action, the process to delete the instance and its associated vNIC, boot volume, and data begins. The delete action can take up to 30 minutes, but when the process is complete, the instance no longer appears on the virtual server instances page.

For more information, see the [Delete an instance](/docs/apis/vpc/latest#delete-instance) in the VPC API.

## Managing instances from the instance details page
{: #managing-virtual-server-instances-detail-ui}
{: ui}

To display an instance's details page, take the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

1. In the list on the _Virtual server instances_ page, locate the instance that you want to view. Click the instance name to display its details. The instance details page is organized into five tabs: **Overview**, **Networking**, **Storage**, **Monitoring**, and **Integrations**.

## Viewing instance details in the console
{: #viewing-virtual-server-instances-ui}
{: ui}

Click the name of a virtual server instance on the _Virtual server instances_ page to open its details page. The details page is organized into the following tabs.

### Overview tab
{: #viewing-vsi-overview-tab}

The Overview tab is divided into the following sections.

Instance details
:   Displays the instance name (editable by clicking the pencil icon), instance ID, resource group, location, created date, virtual private cloud, and the name of the provisioned SSH keys.

Image details
:   Displays the image name, image type (stock or custom), version, image ID, architecture, and image status.

Profile details
:   Displays the profile name, processor type, number of vCPUs, memory, NUMA count, bandwidth allocation, whether spot instances and burstable instances are supported, and threads per core.

Reservation details
:   Displays the attachment policy type and the associated reservation.

Metadata details
:   Displays the access disabled toggle, hop limit, secure access toggle, and default trusted profile.

Resilience & business continuity
:   Displays the host failure auto-restart toggle, which you can enable or disable.

Monitoring preview
:   Displays CPU usage and memory usage as bar graphs. Click **Monitoring** to open the full monitoring UI.

### Networking tab
{: #viewing-vsi-networking-tab}

The Networking tab displays a list of network attachments with the following information for each: virtual network interface name, VNI name (links to the VNI overview page), subnet (links to the subnet overview page), reserved IP, floating IP, and security group.

Click **Actions** to create a new network attachment or attach an existing one. Click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) in a network attachment row to edit details, edit security groups, edit floating IPs, or edit secondary IPs.

### Storage tab
{: #viewing-vsi-storage-tab}

The Storage tab displays a list of volumes with the following information for each: type (boot or data), name (links to the volume details page), device ID, size (GB), IOPS, encryption (provider managed or customer managed), generation, auto-delete toggle, and tags.

Click **Actions** to attach an existing volume or create a new one.

### Monitoring tab
{: #viewing-vsi-monitoring-tab}

The Monitoring tab displays usage graphs for CPU, volumes by OS device name, volumes by IBM Cloud Block Storage for VPC, memory, network, and running state. The default time range is the last 7 days. You can switch to the last 6 hours, 24 hours, 48 hours, 14 days, or a custom date range.

### Integrations tab
{: #viewing-vsi-integrations-tab}

The Integrations tab provides quick access to related IBM Cloud services:

- Click **Monitoring** to open Infrastructure Monitoring.
- Click **CSPM** to open Cloud Security Posture Management.
- Click **View logging** to open the logging UI.
- Click **View activity tracking** to open the Activity Tracking UI.

## Changing the auto-deletion setting of volumes that are attached to an instance in the console
{: #auto-delete-toggle-ui}
{: ui}

During instance provisioning, a boot volume is created with the auto-delete option that is enabled by default. When this feature is enabled, the volume is deleted when the instance is deleted.

The opposite is true for data volumes that are created during instance provisioning, the auto-delete feature is disabled for them. Data volumes are meant to be detached but not deleted by default so your data can persist beyond the virtual server instance lifecycle.

You can change this setting on the Edit boot volume panel when you create an instance, or later in the instance details page. For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui) and [Updating the auto-delete setting of a volume](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#auto-delete-ui).

## Changing the auto-deletion setting of volumes that are attached to an instance from the CLI
{: #auto-delete-toggle-cli}
{: cli}

During instance provisioning, a boot volume is created with the auto-delete option that is enabled by default. When this feature is enabled, the volume is deleted when the instance is deleted.

The opposite is true for data volumes that are created during instance provisioning, the auto-delete feature is disabled for them. Data volumes are meant to be detached but not deleted by default so your data can persist beyond the virtual server instance lifecycle.

You can change this setting by specifying the `auto_delete` property when you create the instance or update the boot volume attachment. For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli) and [Updating a volume attachment from the CLI](/docs/vpc?topic=vpc-managing-block-storage&interface=cli#update-vol-attachment-cli).

## Changing the auto-deletion setting of volumes that are attached to an instance with the API
{: #auto-delete-toggle-api}
{: api}

During instance provisioning, a boot volume is created with the auto-delete option that is enabled by default. When this feature is enabled, the volume is deleted when the instance is deleted.

The opposite is true for data volumes that are created during instance provisioning, the auto-delete feature is disabled for them. Data volumes are meant to be detached but not deleted by default so your data can persist beyond the virtual server instance lifecycle.

You can change this setting by specifying the `delete_volume_on_instance_delete` property when you [create the instance](/docs/apis/vpc/latest#create-instance) or update the [volume attachment](/docs/apis/vpc/latest#update-instance-volume-attachment). For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api) and [Updating a volume attachment with the API](/docs/vpc?topic=vpc-managing-block-storage&interface=api#update-vol-attachment-api).

## Changing the auto-deletion setting of volumes that are attached to an instance by using Terraform
{: #auto-delete-toggle-terraform}
{: terraform}

During instance provisioning, a boot volume is created with the `auto_delete_volume` option enabled by default. When this option is enabled, the boot volume is deleted when the instance is deleted.

You can change this setting by updating the `auto_delete_volume` argument in the `ibm_is_instance` resource.

The following example disables auto-deletion of the boot volume:

```terraform
resource "ibm_is_instance" "example" {
  name              = "my-instance"
  auto_delete_volume = false
  # ... other required arguments
}
```
{: codeblock}

For more information, see the [`auto_delete_volume` argument](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance#argument-reference){: external} in the `ibm_is_instance` resource documentation.

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
  -H "Authorization: Bearer $iam_token" \
  -d '{
      "total_volume_bandwidth": 500
      }'
```
{: pre}

To see the new bandwidth allocation, you must either stop and start the instance, or detach and reattach data volumes. Bandwidth allocation for individual volumes is updated when you add a data volume by using the `POST / volume_attachments` method or delete a volume by using the `DELETE volume_attachments` method.

## Adjusting instance bandwidth allocation by using Terraform
{: #adjusting-bandwidth-allocation-terraform}
{: terraform}

You can adjust the allocation of your instance's total bandwidth between network bandwidth and storage bandwidth by updating the `total_volume_bandwidth` argument in the `ibm_is_instance` resource. Total storage bandwidth (in megabits per second) is the total bandwidth allocated for the boot and attached data volumes. Increasing total storage bandwidth results in a corresponding decrease in network bandwidth. The minimum network bandwidth is 500 Mbps.

The following example sets total storage bandwidth to 500 Mbps:

```terraform
resource "ibm_is_instance" "example" {
  name                   = "my-instance"
  total_volume_bandwidth = 500
  # ... other required arguments
}
```
{: codeblock}

To see the new bandwidth allocation, you must either stop and start the instance, or detach and reattach data volumes.

For more information, see the [`total_volume_bandwidth` argument](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance#argument-reference){: external} in the `ibm_is_instance` resource documentation.

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

You can programmatically change the storage QoS mode of a virtual server instance that uses one of the [General purpose - Flex](/docs/vpc?topic=vpc-flexible-profiles-virtual-servers), [General purpose Gen 3](/docs/vpc?topic=vpc-general-purpose-vsi-profiles-gen3-intel), [Accelerated Gen 3](/docs/vpc?topic=vpc-accelerated-profile-family ), or [Confidential Computing - Gen 3](/docs/vpc?topic=vpc-confidential-computing-vsi-profiles-gen3-x86) profiles by calling the `/instances/{id}` method in the [VPC API](/docs/apis/vpc/latest#update-instance){: external} as shown in the following sample request.

The following example request enables dynamic bandwidth allocation for data volumes that are attached to a 3rd generation virtual server.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-07-11&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
      "storage_qos_mode": "pooled"
      }'
```
{: pre}

The following example request reverts the storage QoS mode back to weighted.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-07-11&generation=2" \
  -H "Authorization: Bearer $iam_token" \
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
   - This property provides the status of the virtual server instance through the [Retrieve an instance](/docs/apis/vpc/latest#get-instance) request. The values that `status` returns are specialized for virtual server instances and indicate whether it is running, stopped, or transitioning. For more information, see the [Virtual Private Cloud API](/docs/apis/vpc/latest#about-vpc-api).
- `lifecycle_state`
   - This property provides the state of a resource through the [Retrieve an instance](/docs/apis/vpc/latest#get-instance) request. The values that `lifecycle_state` provide are generic and are meant to apply to various resources, such as [Placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc). `lifecycle_state` can return values that overlap with `status`. `lifecycle_state` also includes values that detail if a resource is suspended.

## Setting the host failure auto restart in the console
{: #set-recovery-policy-ui}
{: ui}

To set the host failure auto restart for an existing instance, complete the following steps.

   1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com){: external}, click **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
   1. On the **Virtual server instances** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the instance that you want to manage.
   1. From the instance details page, locate 'Host failure auto restart'. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") and choose Enabled or Disabled to toggle the status of the host recovery policy on or off.

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

During an instance [update](/docs/apis/vpc/latest#update-instance), the `host_failure` subproperty can be used to set the host failure `availability_policy` of the virtual server instance.

## Setting the host failure recovery policy by using Terraform
{: #set-recovery-policy-terraform}
{: terraform}

You can set the host failure recovery policy for a virtual server instance by updating the `availability_policy_host_failure` argument in the `ibm_is_instance` resource. The default value is `restart`.

The following example sets the policy to `stop`:

```terraform
resource "ibm_is_instance" "example" {
  name                              = "my-instance"
  availability_policy_host_failure  = "stop"
  # ... other required arguments
}
```
{: codeblock}

For more information, see the [`availability_policy_host_failure` argument](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance#argument-reference){: external} in the `ibm_is_instance` resource documentation.

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

For more information, see the [update an instance action](/docs/apis/vpc/latest#update-instance) in the {{site.data.keyword.vsi_is_short}} API.

## Setting the confidential compute value by using Terraform
{: #set-confidential-compute-terraform}
{: terraform}

[Select availability]{: tag-green}

Confidential computing with Intel SGX for VPC and Confidential computing with Intel TDX for VPC are available in the Dallas (us-south), Washington DC (us-east), and Frankfurt (eu-de) regions.
{: preview}

You can update the confidential compute mode of a virtual server instance by updating the `confidential_compute_mode` argument in the `ibm_is_instance` resource.

The following example enables SGX confidential computing:

```terraform
resource "ibm_is_instance" "example" {
  name                     = "my-instance"
  confidential_compute_mode = "sgx"
  # ... other required arguments
}
```
{: codeblock}

For more information, see the [`confidential_compute_mode` argument](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance#argument-reference){: external} in the `ibm_is_instance` resource documentation.

## Disable or enable secure boot in the console
{: #disable-secure-boot-ui}
{: ui}

When you select a [confidential computing instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles), the secure boot option is enabled by default. You can disable secure boot on your virtual server instance, but you must first stop the virtual server instance. After you disable secure boot, you can then restart your virtual server instance.

1. From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, select the virtual server instance.
1. From **Actions**, click **Stop**.
1. In **Advanced configuration details**, toggle secure boot to **Disabled**.
1. From **Actions**, click **Start**.

If you decide to re-enable secure boot, follow these same steps and toggle the option back to **Enabled**

## Enabling and disabling secure boot from the CLI
{: #set-secure-boot-cli}
{: cli}

When you select a [confidential computing instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles), the secure boot option is enabled by default. You can disable secure boot on your virtual server instance, but you must first stop the virtual server instance. After you disable secure boot, you can then restart your virtual server instance.

You can update an instance and change the `enable-secure-boot` by using the command-line interface (CLI). Use the `ibmcloud is instance-update` command. For INSTANCE, specify the ID or name of the instance and set the `--enable-secure-boot` property to `false`.

```sh
ibmcloud is instance-update INSTANCE --enable-secure-boot false
```
{: pre}

If you decide to re-enable secure boot, follow these same steps and set the `--enable-secure-boot` property to `true`.

## Enabling and disabling secure boot value from the API
{: #set-secure-boot-API}
{: api}

When you select a [confidential computing instance profile](/docs/vpc?topic=vpc-profiles&interface=ui#confidential-computing-profiles), the secure boot option is enabled by default. You can disable secure boot on your virtual server instance, but you must first stop the virtual server instance. After you disable secure boot, you can then restart your virtual server instance.

You can update a virtual server instance and change the `enable_secure_boot` property by using the API. Make a `PATCH /instances` request and specify a boolean value for the `enable_secure_boot` property. The following example disables secure boot:

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-10-17&generation=2" -H "Authorization: Bearer $iam_token" -d '{"enable_secure_boot": false}'
```
{: pre}

If you decide to re-enable secure boot, follow these same steps and set the `enable_secure_boot` property to `true`.

For more information, see the [update an instance action](/docs/apis/vpc/latest#update-instance) in the {{site.data.keyword.vsi_is_short}} API.

## Detaching a server from a reservation in the console
{: #removing-adding-server-reserved-capacity-ui-vpc}
{: ui}

You can detach a virtual server from a reservation in the console.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation menu** icon ![the menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Reservations**.
1. From your virtual server list or in the Reservation details page, click the server that you want to detach and then click **Actions** > **Detach**.
1. To confirm, click **Detach**.

## Detaching a server from a reservation from the CLI
{: #removing-adding-server-reserved-capacity-cli-vpc}
{: cli}

For the steps to detach a virtual server from a reservation by using the CLI, see [Managing a reservation for VPC](/docs/vpc?topic=vpc-managing-reserved-capacity-vpc&interface=cli).

## Detaching a server from a reservation with the API
{: #removing-adding-server-reserved-capacity-api-vpc}
{: api}

For the steps to detach a virtual server from a reservation by using the API, see [Managing a reservation for VPC](/docs/vpc?topic=vpc-managing-reserved-capacity-vpc&interface=api).

## Adding CSPM in the console
{: #cloud-security-posture-management}
{: ui}

When you select this option, a workload protection instance is created with the configurations to provide CSPM to all the resources. If a workload protection instance exists, this option is not available. For more information, see [About IBM Cloud Security Posture Management (CSPM)](/docs/workload-protection?topic=workload-protection-about&interface=ui).

You can add CSPM (Cloud Security Posture Management) on the _Virtual server instances_ page.

1. From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, click an individual instance name to view details.
1. From the details page, go to the **Integrations** tab.
1. In **Cloud security posture management**, click **Add CSPM**.
1. On the **Add cloud security posture management (CSPM)**, click **Create**.

This setting adds CSPM to an existing Workload Protection instance for your account. For more information, see [Implementing CSPM (Cloud Security Posture Management) for IBM Cloud](/docs/workload-protection?topic=workload-protection-cspm-implement&interface=ui).

## Viewing the software attachments of a virtual server instance using the CLI
{: #view-software-attachments-virtual-server-instances-cli}
{: cli}

You can view the software attachments of a virtual server instance that was created from a catalog image by using the CLI.

- To list all software attachments for an instance, use [`ibmcloud is instance-software-attachments`](/docs/vpc?topic=vpc-vpc-reference#instance-software-attachments-list. The `INSTANCE` variable is the ID or name of the instance.

   ```sh
   ibmcloud is instance-software-attachments INSTANCE [--output JSON] [-q, --quiet]
   ```
   {: pre}

   The following example lists all software attachments for the instance `my-instance`.

   ```sh
   ibmcloud is instance-software-attachments my-instance
   ```
   {: pre}

- To view the details of a specific software attachment for an instance, use [`ibmcloud is instance-software-attachment`](/docs/vpc?topic=vpc-vpc-reference#instance-software-attachment-view). The `INSTANCE` variable is the ID or name of the instance. The `SWAC` variable is the instance software attachment ID or name.

   ```sh
   ibmcloud is instance-software-attachment INSTANCE SWAC [--output JSON] [-q, --quiet]
   ```
   {: pre}

   The following example shows the details of the software attachment `my-software-attachment` for the instance `my-instance`.

   ```sh
   ibmcloud is instance-software-attachment my-instance my-software-attachment
   ```
   {: pre}

## Updating the software attachments of a virtual server instance using the CLI
{: #update-software-attachments-virtual-server-instances-cli}
{: cli}

You can update the software attachments of a virtual server instance that was created from a catalog image by using the CLI.

To update the name of a software attachment for an instance, use [`ibmcloud is instance-software-attachment-update`](/docs/vpc?topic=vpc-vpc-reference#instance-software-attachment-update). The `INSTANCE` variable is the ID or name of the instance. The `SWAC` variable is the instance software attachment ID or name. The `--name` value is the new name of the instance software attachment.

```sh
ibmcloud is instance-software-attachment-update INSTANCE SWAC --name NEW_NAME [--output JSON] [-q, --quiet]
```
{: pre}

The following example renames the software attachment `$software_attachment_id`to `my-renamed-software-attachment` for the instance `my-instance`.

```sh
ibmcloud is instance-software-attachment-update my-instance my-software-attachment --name my-renamed-software-attachment
```
{: pre}

## Viewing the software attachments of a virtual server instance with the API
{: #view-software-attachments-virtual-server-instances-api}
{: api}

When you create a virtual server instance sourced from a catalog image, you can use a catalog image that is specificially configured with a defined software billing plan. After you create your virtual server instance, you can view the software attachments that are now a part of the virtual server instance.

To view the software attachments by using the API, make a `GET /instances/{instance_id}/software_attachments` request.

```sh
curl -X GET "https://us-south.iaas.cloud.ibm.com/v1/instances/{instance_id}/software_attachments?version=2026-04-21" -H "Authorization: Bearer $iam_token"
```
{: pre}

## Listing instance software attachments with the API
{: #list-instance-software-attachments-api}
{: api}

You can programmatically list all software attachments associated with a virtual server instance by calling the VPC API.

The following example lists all software attachments for an instance with an instance ID of `$instance_id`.

```sh
curl -X GET "https://us-south.iaas.cloud.ibm.com/v1/instances/$instance_id/software_attachments?version=2024-06-23&generation=2" -H "accept: application/json" -H "Authorization: Bearer $iam_token"
```
{: pre}

For more information, see [List instance software attachments associated with an instance](/docs/apis/vpc/latest#list-instance-software-attachments) in the VPC API.

## Retrieving an instance software attachment with the API
{: #get-instance-software-attachment-api}
{: api}

You can retrieve a single instance software attachment by making a `GET  /instances/{instance_id}/software_attachments/{id}` request and specifying the instance software attachment ID and the instance ID.

The following example retrieves a specific software attachment with an ID of `$software_attachment_id` for an instance with an instance ID of `$instance_id`.

```sh
curl -X GET "https://us-south.iaas.cloud.ibm.com/v1/instances/$instance_id/software_attachments/$software_attachment_id?version=2024-06-23&generation=2" -H "accept: application/json" -H "Authorization: Bearer $iam_token"
```
{: pre}

For more information, see [Retrieve an instance software attachment](/docs/apis/vpc/latest#get-instance-software-attachment) in the VPC API.

## Updating an instance software attachment with the API
{: #update-instance-software-attachment-api}
{: api}

You can update an instance software attachment by making a `PATCH  /instances/{instance_id}/software_attachments/{id}` request and specifying the instance software attachment ID and the instance ID.

The following example updates a software attachment with an ID of `$software_attachment_id` for an instance with an instance ID of `$instance_id`.

```sh
curl -X PATCH "https://us-south.iaas.cloud.ibm.com/v1/instances/$instance_id/software_attachments/$software_attachment_id?version=2024-06-23&generation=2" -H "accept: application/json" -H "Authorization: Bearer $iam_token" -H "Content-Type: application/merge-patch+json" -d "{\"name\":\"my-software-attachment-patch\"}"
```
{: pre}

For more information, see [Update an instance software attachment](/docs/apis/vpc/latest#update-instance-software-attachment) in the VPC API.

## Managing a software attachment for a virtual server instance by using Terraform
{: #manage-software-attachments-virtual-server-instances-terraform}
{: terraform}

You can't directly create a software attachment. The software attachment is automatically created when a virtual server instance is provision from a software licensed catalog image. For more information regarding software attachments and catalog images, see [Getting started with Catalog images on VPC](/docs/vpc?topic=vpc-getting-started-images-on-vpc-catalog).


You use the `ibm_is_instance_software_attachment` resource to manage an existing attachment by adopting it and setting its name. The `instance_id` argument is the virtual server instance identifier. The `instance_software_attachment_id` argument is the identifier of the attachment that was created for the `instance_id`. you can reference it from the instance's software_attachments list. The name argument is the name to set on the instance software attachment.

The following example sets the name of the software attachment.

```terraform
resource "ibm_is_instance" "example" {
  name    = "my-instance"
  profile = "cx2-2x4"
  vpc     = ibm_is_vpc.example.id
  zone    = "us-south-1"
  keys    = [ibm_is_ssh_key.example.id]

  image = "r006-90c63ab2-f216-4b50-aa9f-0cca03ab0ac2"

  primary_network_interface {
    virtual_network_interface {
      subnet = ibm_is_subnet.example.id
    }
  }
}

resource "ibm_is_instance_software_attachment" "is_instance_software_attachment_instance" {
  instance_id                         = ibm_is_instance.example.id
  instance_software_attachment_id     = ibm_is_instance.example.software_attachments.0.id
  name                                = "my-software-attachment"
}
```
{: codeblock}

After you define the resource, run `terraform apply` to adopt the attachment and apply the name.

For more information, see [ibm_is_instance_software_attachment](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_software_attachment){: external}.

## Updating a software attachment for a virtual server instance by using Terraform
{: #update-software-attachments-virtual-server-instances-terraform}
{: terraform}

You can update the name of a software attachment by using Terraform. You can only update `name` argument. When the `name` is updated, a new resource is created with that `instance_id` and `instance_software_attachment_id`. The `name` argument is the updated name for the instance software attachment.

The following example updates the name of a software attachment:

```terraform
resource "ibm_is_instance_software_attachment" "is_instance_software_attachment_instance" {
  instance_id                        = ibm_is_instance.example.id
  instance_software_attachment_id = ibm_is_instance.example.software_attachments.0.id
  name                               = "my-software-attachment-update"
}
```
{: codeblock}

After you update the resource configuration, run `terraform plan` to preview the changes, then run `terraform apply` to apply them.

For more information, see [ibm_is_instance_software_attachment](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_software_attachment){: external}.

## Removing a software attachment for a virtual server instance by using Terraform
{: #remove-software-attachments-virtual-server-instances-terraform}
{: terraform}

You can't delete a software attachment using API and Terraform. The lifecycle of a software attachment is linked to the instance it is attached to. Running `terraform destroy` on the `ibm_is_instance_software_attachment` resource only removes it from the Terraform state. The`ibm_is_instance_software_attachment` resource remaines linked to it's `instance_id`.

The following example shows the resource that is removed from Terraform state when you destroy it.

```terraform
resource "ibm_is_instance_software_attachment" "is_instance_software_attachment_instance" {
  instance_id                         = ibm_is_instance.example.id
  instance_software_attachment_id     = ibm_is_instance.example.software_attachments.0.id
  name                                = "my-software-attachment-update"
}
```
{: codeblock}

Run `terraform destroy` to stop managing the software attachment with Terraform. To remove the attachment, delete its virtual server instance.

For more information, see [ibm_is_instance_software_attachment](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance_software_attachment){: external}.

## Editing Threads per core in the console
{: #edit-threads-per-core-ui-vpc}
{: ui}

Threads per core offers you the flexibility to optimize your workload. While threads per core is displayed for all profiles, you can edit it only for the [High Frequency](/docs/vpc?topic=vpc-high-frequency-profile-family) profile family. For High Frequency profiles, the default value is to 2. You can adjust this to 1.

While threads per core is displayed for all profiles, you can edit it only for the [High Frequency](/docs/vpc?topic=vpc-high-frequency-profile-family) profile family. Before you change the `threads_per_core` property, you must stop the virtual server instance.
{: note}

1. From the _Virtual server instances_ page in {{site.data.keyword.cloud_notm}} console, click an individual instance name to view details.
1. From the details page, go to `Profiles details` and then `Threads per core`. Click `Edit`.
1. From the Threads per core page, edit the Threads per core value.
1. Click `Save`.

## Editing Threads per core using the CLI
{: #edit-threads-per-core-cli-vpc}
{: cli}

Threads per core offers you the flexibility to optimize your workload. While threads per core is displayed for all profiles, you can edit it only for the [High Frequency](/docs/vpc?topic=vpc-high-frequency-profile-family) profile family. For High Frequency profiles, the default value is to 2. You can adjust this to 1.

Before you can change the `threads_per_core` property, you must stop the virtual server instance.
{: note}

You can edit the `--threads-per-core` value for the virtual server instance in your {{site.data.keyword.vpc_short}} by using the command-line interface (CLI). To update the `--threads-per-core` value, use the `ibmcloud is instance-update` command. Specify the ID or name of the virtual server instance that you want to update by using the `INSTANCE` variable. For the `--threads-per-core` option, enter either `1` or `2`.

```sh
ibmcloud is instance-update INSTANCE --threads-per-core [1]
```
{: pre}

## Editing Threads per core with the API
{: #edit-threads-per-core-api-vpc}
{: api}

Threads per core offers you the flexibility to optimize your workload. While threads per core is displayed for all profiles, you can edit it only for the [High Frequency](/docs/vpc?topic=vpc-high-frequency-profile-family) profile family. For High Frequency profiles, the default value is to 2. You can adjust this to 1.

Before you change the `threads_per_core` property, you must stop the virtual server instance.
{: note}

You edit the `threads_per_core` property for the  virtual server instance in your {{site.data.keyword.vpc_short}} by using the API. Use the `update-instance` command. Make a `PATCH /instances` request and specify a new value for the `threads_per_core` property.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/instances/$instance_id?version=2024-10-17&generation=2" -H "Authorization: Bearer $iam_token" -d '{"threads_per_core": "1"}'
```
{: pre}

## Editing Threads per core using Terraform
{: #edit-threads-per-core-terraform-vpc}
{: terraform}

Threads per core offers you the flexibility to optimize your workload. While threads per core is displayed for all profiles, you can edit it only for the [High Frequency](/docs/vpc?topic=vpc-high-frequency-profile-family) profile family. For High Frequency profiles, the default value is to 2. You can adjust this to 1.

Before you change the `threads_per_core` property, you must stop the virtual server instance.
{: note}

You can edit the `threads_per_core` property for a virtual server instance in your {{site.data.keyword.vpc_short}} by using the [`ibm_is_instance`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_instance){: external} Terraform resource.

```terraform
resource "ibm_is_instance" "example" {
  name             = "example-instance"
  profile          = "hx4da-64x1408"
  zone             = "us-south-1"
  threads_per_core = 1
  # ... other required arguments
}
```
{: codeblock}
