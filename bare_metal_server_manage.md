---

copyright:
  years: 2021, 2026
lastupdated: "2026-02-24"

keywords: bare metal servers, managing, operation, manage bare metal server, manage bare metal, manage server, restart bare metal, stop bare metal, delete bare metal, reboot bare metal, restart server, stop server, delete server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing Bare Metal Servers for VPC
{: #managing-bare-metal-servers}

You can manage your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by performing tasks such as start, stop, update firmware, reboot, reinitialize and delete bare metal server.
{: shortdesc}

You can perform the following actions by using the UI, CLI, and API.

| Action | Description |
|--------|-------------|
| Stop| Stop the server by using a soft stop or a hard stop.  \n - Soft stop can take a few seconds to several minutes to shut down the server, depending on the state of the operating system. If the operating system is not responding and the soft stop can’t complete, a hard stop is required.  \n - Hard stop shuts down the bare metal server immediately. This method prevents the operating system from shutting down gracefully. |
| Start | Start a stopped server. This action is not available if the status is Running. |
| Update firmware | If the server is stopped and a firmware update is available, this option is visible. \n \n If you select to update the firmware, a prompt is displayed giving you extra details about the firmware update. You can start the server when the update completes. This option is selected by default. \n \n You can select to either proceed with the firmware update or to cancel.  \n **Important** It is recommended to back up your server before any firmware update. |
| Reboot | Immediately powers off a running server and then powers it back on. |
| Reinitialize | You can reinitialize the server only if the server is stopped. Or, you can reinitialize if the server status is `failed` and the lifecycle state has a status reason of `cannot_reinitialize`. When the bare metal server is reinitialized, the contents of the boot disk are wiped and the specified operating system is installed. The server retains the same physical node, interfaces, IP addresses, and resource IDs. Data on secondary drives is preserved. |
| Delete | To delete a server, the server must be powered off. If the server has a floating IP address, the floating IP address must be unassociated or released before the server is deleted. The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account. |
{: caption="Actions available for bare metal servers" caption-side="bottom"}

To manage your bare metal servers, you need an IAM role that includes the following actions. For more information, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).

   - is.bare-metal-server.bare-metal-server.list
   - is.bare-metal-server.bare-metal-server.read
   - is.bare-metal-server.bare-metal-server.delete
   - is.bare-metal-server.bare-metal-server.update
   - is.bare-metal-server.bare-metal-server.operate
   - is.key.key.operate
   - is.image.image.operate
   - is.bare-metal-server.bare-metal-server-firmware.update (required to update firmware)
   - is.bare-metal-server.initialization.update (required to reinitialize server)

## Managing bare metal servers by using the UI
{: #managing-bare-metal-servers-ui}
{: ui}

You can view and manage a bare metal server from the *Bare metal servers* page in the {{site.data.keyword.cloud_notm}} console.

To manage your servers, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} dashboard](https://cloud.ibm.com){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.
2. On the **Bare metal servers** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the server that you want to manage. You can perform the following actions:

### Viewing your bare metal servers by using the UI
{: #viewing-bare-metal-server-ui}

You can view the summary of all bare metal server on the bare metal server page, or you can click an individual server name to view details and make changes. From the details page, you can also view the associated network interface, access its subnet, change the network bandwidth on Sapphire Rapids servers, and reserve or delete a floating IP address.

Adjustable network bandwidth for Sapphire Rapids bare metal servers is only available in US South (Dallas).
{: preview}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to view.

### Updating the firmware for a bare metal server by using the UI
{: #update-firmware-bare-metal-servers-ui}

This action is only displayed if the server is stopped and a firmware update is available. It is recommended to back up your server before any firmware update.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to reboot.
1. Click **Actions...**, then click **Update firmware**.

   You receive the following message when you select to update the firmware.

   > This option updates BIOS and BMC firmware on your server. Backup your server before proceeding. The server is stopped and unavailable during the process.

   **Start server after the update completes** is selected by default. Remove the check if you don't want the server to restart.

1. Click **Proceed** to start the firmware update. Click **Cancel** to cancel the update.

### Rebooting a bare metal server by using the UI
{: #reboot-bare-metal-servers-ui}

The reboot action immediately powers off and powers on the bare metal server.

1. In the [{{site.data.keyword.cloud_notm}} dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to reboot.
1. Click **Actions...**, then click **Reboot**.

### Stopping and starting a bare metal server by using the UI
{: #stop-start-bare-metal-servers-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to start or stop.
1. Click **Actions...**, then click **Stop** or **Start**. When you select **Stop**, select hard stop or soft stop. The instance soft-stops by default.

Billing continues after the bare metal server is stopped.
{: note}

### Reinitialize a bare metal server by using the UI
{: #reinitialize-bare-metal-servers-ui}
{: ui}

You can reinitialize the server only if the server is stopped and provisioned with local storage. Or, you can reinitialize if the server status is `failed` and the lifecycle state has a status reason of `cannot_reinitialize`. When the bare metal server is reinitialized, the contents of the boot disk are wiped and the specified operating system is installed. The server retains the same physical node, interfaces, IP addresses, and resource IDs. Data on secondary drives is preserved.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**

2. Click the name of the bare metal server that you want to reinitialize.

3. Click **Actions...**, then click **Reinitialize**.

### Deleting a bare metal server by using the UI
{: #delete-bare-metal-servers-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to delete.
1. Click **Actions...**, then click **Delete**.

The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account.
{: important}

## Adding CSPM in the console
{: #cloud-security-posture-management-ui}
{: ui}

When you select this option, a workload protection bare metal server is created with the configurations to provide CSPM to all the resources. If a workload protection bare metal server already exists, this option is not available. For more information, see [About IBM Cloud Security Posture Management (CSPM)](/docs/workload-protection?topic=workload-protection-about&interface=ui).


1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.
1. Click the name of the bare metal server that you want to add CSPM to.
1. From the details page, go to the **Integrations** tab.
1. In **Cloud security posture management**, click **Add CSPM**.
1. On the **Add cloud security posture management (CSPM)**, click **Create**.

This setting adds CSPM to an existing Workload Protection instance for your account. For more information, see [Implementing CSPM (Cloud Security Posture Management) for IBM Cloud](/docs/workload-protection?topic=workload-protection-cspm-implement&interface=ui).

## Managing your bare metal server by using the CLI
{: #managing-bare-metal-servers-cli}
{: cli}

### Viewing your bare metal servers by using the CLI
{: #viewing-bare-metal-servers-cli}

To list all the bare metal servers by using the CLI, use the `ibmcloud is bare-metal-servers` command. To retrieve a specific bare metal server, specify the ID or name of the bare metal server with the `SERVER` variable.

   ```sh
   ibmcloud is bare-metal-server SERVER
   ```
   {: pre}

For a full list of command options, see [ibmcloud is bare-metal-server](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-view).

### Rebooting a bare metal server by using the CLI
{: #reboot-bare-metal-servers-cli}

To reboot your bare metal server by using the CLI, use the `ibmcloud is bare-metal-server-restart` command. Specify the ID or name of the bare metal server with the `SERVER` variable.

```sh
ibmcloud is bare-metal-server-restart SERVER
```
{: pre}

The `[-f, --force]` flag forces the operation without confirmation.
{: tip}

### Stopping and starting a bare metal server by using the CLI
{: #stop-start-bare-metal-servers-cli}

To stop and start your bare metal server by using the CLI, use the `ibmcloud is bare-metal-server-stop` or `ibmcloud is bare-metal-server-start` command. Specify the ID or name of the bare metal server with the `SERVER` variable.

You must specify the `type` for the stop action in the data payload. `soft` tells the running operating system to stop and shut down cleanly. `hard` immediately stops the bare metal server.
{: important}

To stop the bare metal server, use the following command.

```sh
ibmcloud is bare-metal-server-stop SERVER --type soft
```
{: pre}

To start the bare metal server, use the following command.

```sh
ibmcloud is bare-metal-server-start SERVER
```
{: pre}

Billing continues after the bare metal server stops.
{: note}

For a full list of command options, see [ibmcloud is bare-metal-server-restart](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-restart).

### Updating a bare metal server by using the CLI
{: #updating-bare-metal-servers-cli}

To update your bare metal server by using the CLI, use the `ibmcloud is bare-metal-server-update` command.

Specify the following variables to use when you reinitialize the bare metal server.
- `SERVER` specifies the name of the bare metal server
- `NAME` ID, name, or CRN of the trusted profile
- `METADATA` specifying `true` or `false` enables or disables the metadata service
- `PROTOCOL` specifies which protocol for the metadata service.

```sh
ibmcloud is bare-metal-server-update SERVER --name NAME --enable-secure-boot true --metadata-service true --metadata-service-protocol https
```
{: codeblock}

For a full list of command options, see [ibmcloud is bare-metal-server-update](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-update).

### Reinitialize a bare metal server by using the CLI
{: #reinitialize-bare-metal-servers-cli}
{: cli}

You can reinitialize the server only if the server is stopped and provisioned with local storage. Or, you can reinitialize if the server status is `failed` and the lifecycle state has a status reason of `cannot_reinitialize`. When the bare metal server is reinitialized, the contents of the boot disk are wiped and the specified operating system is installed. The server retains the same physical node, interfaces, IP addresses, and resource IDs. Data on secondary drives is preserved.

To reinitialize a bare metal server by using the CLI, use the `ibmcloud is bare-metal-server-initialization-replace` command.

```sh
ibmcloud is bare-metal-server-initialization-replace SERVER --image IMAGE ---keys KEYS --user-data DATA [--default-trusted-profile DEFAULT_TRUSTED_PROFILE [--default-trusted-profile-auto-link true]]
```
{: pre}

Specify the following variables to use when you reinitialize the bare metal server.
- `SERVER` specifies the name of the bare metal server.
- `IMAGE` specifies the operating system image.
- `KEYS` specifies the SSH keys.
- `DATA` specifies any optional user data.
- `PROFILE` ID, name, or CRN of the trusted profile.
- `DEFAULT_TRUSTED_PROFILE` changes the default trusted profile auto-link parameter.

When using a default_trusted_profile, reinitializing the bare metal server includes the same auto_link parameter defined for the default_trusted_profile to determine whether the links the trusted profile to the bare metal server as part of the reinitialization process. Trusted profiles previously linked to the bare metal server before the reinitialization process remain linked. The reinitialization process doesn't remove the link to IAM trusted profiles. To prevent the trusted profile from remaining linked to the bare metal server, change the "auto_link" parameter to false.

For a full list of command options, see [ibmcloud is bare-metal-server-initialization-replace](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-initialization-replace-view).

### Updating the firmware for a bare metal server by using the CLI
{: #update-firmware-bare-metal-servers-cli}

You can update the firmware for your bare metal servers by using the command-line interface (CLI). You can update the firmware on only a bare metal server that is stopped and has firmware updates available. It is recommended to back up your server before any firmware update.

To update the firmware for the bare metal server by using the CLI, use the `ibmcloud is bare-metal-server-firmware-update` command. Specify the ID or name of the bare metal server with the`SERVER` variable.

The default value for `auto-start` is `true`. If you don't want the bare metal server to start after the firmware is updated, you must change this value to `false`.

```sh
ibmcloud is bare-metal-server-firmware-update SERVER --auto-start true
```
{: pre}

After you issue this command, you receive a message to verify whether to proceed with the firmware update.

> This option upgrades the BIOS and BMC firmware on your server. Back up your server before you proceed. The server is unavailable during the process. Proceed (y/n)?

For a full list of command options, see [ibmcloud is bare-metal-server-firmware-update](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-firmware-update).

### Deleting a bare metal server by using the CLI
{: #delete-bare-metal-servers-cli}
{: cli}

To delete your bare metal server by using the CLI, use the `ibmcloud is bare-metal-server-delete` command. Specify the ID or name of the bare metal server with the`SERVER` variable.

```sh
ibmcloud is bare-metal-server-delete SERVER
```
{: pre}

The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account.
{: important}

For a full list of command options, see [ibmcloud is bare-metal-server-delete](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-delete).

## Managing your bare metal server by using the API
{: #managing-bare-metal-servers-api}
{: api}

### Viewing your bare metal servers by using the API
{: #viewing-bare-metal-servers-api}

To list all bare metal servers by using the API, use [List all bare metal servers](/apidocs/vpc/latest#list-bare-metal-servers).

Specify a `GET /bare_metal_servers` request to list all the bare metal servers.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers?version=2021-03-09&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

### Retrieve a bare metal server by using the API
{: #retrieve-bare-metal-servers-api}

To retrieve a bare metal server by using the API, use [Retrieve a bare metal server](/apidocs/vpc/latest#get-bare-metal-server).

Specify a `GET /bare_metal_servers/{id}` request retrieve a specific bare metal server where `id` is the identifier of the bare metal server you are retrieving.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

For more information of the API requests, see [List all bare metal servers](/apidocs/vpc/latest#list-bare-metal-servers) and [Retrieve a bare metal server](/apidocs/vpc/latest#get-bare-metal-server).

### Rebooting the bare metal server by using the API
{: #reboot-bare-metal-servers-api}

To reboot a bare metal server by using the API, use [Restart a bare metal server](/apidocs/vpc/latest#restart-bare-metal-server).

Specify a `POST /bare_metal_servers/{id}/restart` request to restart a specific bare metal server where `id` is the identifier of the bare metal server you are restarting.

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/restart?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

For more information of the API request, see [Restart a bare metal server](/apidocs/vpc/latest#restart-bare-metal-server).

### Stopping and starting a bare metal server by using the API
{: #stop-start-bare-metal-servers-api}

To stop or start a bare metal server by using the API, use [Stop a bare metal server](/apidocs/vpc/latest#stop-bare-metal-server) and [Start a bare metal server](/apidocs/vpc/latest#start-bare-metal-server).

Use the following API requests to stop or start a bare metal server.

#### Stopping the bare metal server by using the API
{: #stop-bm-api}

Specify a `POST /bare_metal_servers/{id}/stop` request to stop a specific bare metal server where `id` is the identifier of the bare metal server you are stopping.

You must specify the `type` for the stop action in the data payload. `soft` tells the running operating system to stop and shut down cleanly. `hard` immediately stops the bare metal server.
{: important}

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/stop?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
-d '{
        "type": "soft"
}'
```
{: pre}

Billing continues after the bare metal server is stopped.
{: note}

For more information about the API request, see [Stop a bare metal server](/apidocs/vpc/latest#stop-bare-metal-server).

#### Starting the bare metal server by using the API
{: #start-bm-api}

Specify a `POST /bare_metal_servers/{id}/start` request to start a specific bare metal server where `id` is the identifier of the bare metal server you are starting.

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/start?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

For more information about the API request, see [Start a bare metal server](/apidocs/vpc/latest#start-bare-metal-server).

### Updating  a bare metal server by using the API
{: #update-bare-metal-servers-api}

Specify a `PATCH /bare_metal_servers/{id}` request to update a specific bare metal server where `id` is the identifier of the bare metal server you are updating.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2025-05-13&generation=2" -H "Authorization: Bearer $iam_token" -d '{
  "name": "my-bare-metal-server-updated"
  "metadata_service": {
     "enabled": true,
     "protocol": "https"
  }
}'
```
{: codeblock}

Specify the following properties values to use when you update the bare metal server.
- `name` specifies the name of the bare metal server
- `metadata_service` specifies whether the service is enabled and which protocol to use.

For more information about the API request, see [Update a bare metal server](/apidocs/vpc/latest#update-bare-metal-server).

### Reinitialize a bare metal server by using the API
{: #reinitialize-bare-metal-servers-api}
{: api}

To reinitialize your bare metal server by using the API, use [Bare metal servers initialization](/apidocs/vpc/latest#replace-bare-metal-server-initialization).

You can reinitialize the server only if the server is stopped and provisioned with local storage. Or, you can reinitialize if the server status is `failed` and the lifecycle state has a status reason of `cannot_reinitialize`. When the bare metal server is reinitialized, the contents of the boot disk are wiped and the specified operating system is installed. The server retains the same physical node, interfaces, IP addresses, and resource IDs. Data on secondary drives is preserved.

Specify a `PUT /bare_metal_servers/{id}/initialization` request to reinitialize the bare metal server.

```sh
curl -X PUT "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/initialization?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token" \
-d '{
	  "image": {
		"id": $image
	  },
	  "keys": [
		{
			"id": $key
		}
	  ],
	  "default_trusted_profile": {
		"auto_link": true,
		"target": {
			"id": $trusted_profile
		}
	  }
}'
```
{: pre}

Specify the following properties values to use when you reinitialize the bare metal server.
- `name` specifies the name of the bare metal server
- `image` specifies the operating system image
- `keys` specifies the SSH keys
- `user_data` specifies any optional user data
- `default_trusted_profile` changes or removes the default trusted profile

When using a `default_trusted_profile`, reinitializing the bare metal server includes the same `"auto_link"` parameter defined for the `default_trusted_profile` to determine whether the links the trusted profile to the bare metal server as part of the reinitialization process. Trusted profiles previously linked to the bare metal server before the reinitialization process remain linked. The reinitialization process doesn't remove the link to IAM trusted profiles. To prevent the trusted profile from remaining linked to the bare metal server, change the `"auto_link"` parameter to `false`.

### Updating the firmware for a bare metal server by using the API
{: #update-firmware-bare-metal-servers-API}

You can update the firmware on only a bare metal server that is stopped and has firmware updates available. It is recommended that you back up your server before you update the firmware. To update the firmware by using the API, use [Update firmware for a bare metal server](/apidocs/vpc-beta/initial#update-firmware-for-bare-metal-server).

Specify a `POST /bare_metal_servers/{id}/firmware/update` request to update the firmware for a specific bare metal server where `id` is the identifier of the bare metal server that you are updating.

The default value for `auto_start` is `true`. If you don't want the bare metal server to start after the firmware update, you must change this value to `false`.

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/firmware/update?version=$tomorrow&generation=2&maturity=beta" -H "Authorization: Bearer $iam_token" -d '{
  "auto_start": false
}'
```
{: pre}

### Deleting a bare metal server by using the API
{: #delete-bare-metal-servers-api}

To delete a bare metal server by using the API, use [Delete a bare metal server](/apidocs/vpc/latest#delete-bare-metal-server).

Specify a `DELETE /bare_metal_servers/{id}` request delete a specific bare metal server where `id` is the identifier of the bare metal server you are deleting.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account.
{: important}

## Managing bare metal servers by using Terraform
{: #managing-bare-metal-servers-terraform}
{: terraform}

Make sure that you set up [Terraform for VPC](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest).

### Viewing your bare metal servers by using Terraform
{: #viewing-bare-metal-server-terraform}
{: terraform}

To view a bare metal server by using the Terraform, use the `ibm_is_bare_metal_server` resource.

```terraform
data "ibm_is_bare_metal_server" "example" {
  identifier        = "9328-9849-9849-9849"
}
```
{: pre}

For more information, see [ibm_is_bare_metal_server](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_bare_metal_server).

To view a list of all the bare metal servers, use the `ibm_is_bare_metal_servers` resource.

```terraform
data "ibm_is_bare_metal_servers" "example" {
}
```
{: pre}

For more information, see [ibm_is_bare_metal_servers](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_bare_metal_servers).

### Restarting a bare metal server by using Terraform
{: #reboot-bare-metal-servers-terraform}
{: terraform}

To restart a bare metal server by using the Terraform, you need to use the `ibm_is_bare_metal_server_action` resource.

```terraform
resource "ibm_is_bare_metal_server_action" "bms_action" {
  bare_metal_server = SERVER
  action            = "restart"
}
```
{: pre}

Specify the following variables to use when you reinitialize the bare metal server.
- `SERVER` specifies the name of the bare metal server
- `restart` specifies the action for the server

For a full list of command options, see [ibm_is_bare_metal_server_action](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_bare_metal_server_action).

### Stopping and starting a bare metal server by using Terraform
{: #stop-start-bare-metal-servers-terraform}
{: terraform}

To stop a bare metal server by using the Terraform, you need to use the `ibm_is_bare_metal_server_action` resource.

```terraform
resource "ibm_is_bare_metal_server_action" "bms_action" {
  bare_metal_server = SERVER
  action            = "stop"
  stop_type         = "hard"
}
```
{: pre}

To start a bare metal server by using the Terraform, you need to use the `ibm_is_bare_metal_server_action` resource.

```terraform
resource "ibm_is_bare_metal_server_action" "bms_action" {
  bare_metal_server = SERVER
  action            = "stop"
  delete_type       = "hard"
}
```
{: pre}

Specify the following variables to use when you reinitialize the bare metal server.
- `SERVER` specifies the name of the bare metal server
- `stop` or `start` specifies the action for the server
- `hard` is specific to stopping the server and indicates the type of stop. Specifying `hard` immediately stops the server and `soft` shuts down the operating system.

For a full list of command options, see [ibm_is_bare_metal_server_action](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_bare_metal_server_action).

### Reinitializing a bare metal server by using Terraform
{: #reinitialize-bare-metal-servers-terraform}
{: terraform}

You can reinitialize the server only if the server is stopped and provisioned with local storage. Or, you can reinitialize if the server status is `failed` and the lifecycle state has a status reason of `cannot_reinitialize`. When the bare metal server is reinitialized, the contents of the boot disk are wiped and the specified operating system is installed. The server retains the same physical node, interfaces, IP addresses, and resource IDs. Data on secondary drives is preserved.

To reinitialize a bare metal server by using the Terraform, you need to use the resource command `ibm_is_bare_metal_server_initialization`.

```terraform
resource "ibm_is_bare_metal_server_initialization" "reinitialize" {
  bare_metal_server = SERVER
  user_data         = DATA
  keys              = KEYS
  image             = "IMAGE"
  default_trusted_profile {
      auto_link = true
      target {
          id = "Profile-a2075c9d-c69f-404f-8488-7962a383059c"
      }
  }
}
```
{: codeblock}

Specify the following variables to use when you reinitialize the bare metal server.
- `SERVER` specifies the name of the bare metal server
- `DATA` specifies any optional user data
- `KEYS` specifies the SSH keys
- `IMAGE` specifies the operating system image
- `Default Trusted Profile` specified if auto link is enable

While you can retain the same physical node, interfaces, IP addresses, and resource IDs, you can also select to avoid these changes by using the `lifecycle` property.

For a full list of command options, see [ibm_is_bare_metal_server_initialization](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_bare_metal_server_initialization).

```terraform
resource "ibm_is_bare_metal_server_initialization" "reinitialize" {
  bare_metal_server = SERVER
  user_data         = DATA
  keys              = KEYS
  image             = "IMAGE"
}
## to avoid changes on the ibm_is_bare_metal_server resource, use lifecycle meta argument ignore_changes
resource "ibm_is_bare_metal_server" "bms" {
  ....
  lifecycle{
    ignore_changes = [ image, keys, user_data ]
  }
}
```
{: codeblock}

### Deleting a bare metal server by using Terraform
{: #delete-bare-metal-servers-terraform}
{: terraform}

To delete a bare metal server resource by using Terraform, you need to use the `ibm_is_bare_metal_server` resource.

```terraform
resource "ibm_is_bare_metal" "server" {
  delete_type          = "hard"
}
```
{: pre}

Specify the following variables to use when you delete the bare metal server.
- `hard` specifies a `hard` delete that immediately stops and deletes the server. The other option is `soft` delete which shutdowns the operating system.

For a full list of command options, see [ibm_is_bare_metal_server](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_bare_metal_server).
