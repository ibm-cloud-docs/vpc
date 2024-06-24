---

copyright:
  years: 2021, 2024
lastupdated: "2024-06-24"

keywords: bare metal servers, managing, operation, manage bare metal server, manage bare metal, manage server, restart bare metal, stop bare metal, delete bare metal, reboot bare metal, restart server, stop server, delete server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing Bare Metal Servers for VPC
{: #managing-bare-metal-servers}

You can manage your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by performing tasks such as start, stop, update firmware, reboot, and delete bare metal server.
{: shortdesc}

 

You can perform the following actions by using the UI, CLI, and API.

| Action | Description |
|--------|-------------|
| Stop| Stop the server using a soft stop or a hard stop.  \n - Soft stop can take a few seconds to several minutes to stop shut down the server, depending on the state of the operating system. If the operating system is not responding and the soft stop can not complete, a hard stop is required.  \n - Hard stop shuts down the bare metal server immediately. This prevents the operating system from shutting down gracefully. |
| Start | Start a stopped server. This action is not available if the status is Running. |
| Update firmware | If the server is stopped and a firmware update is available, this option is visible. \n \n If you select to update the firmware, a prompt is displayed giving you additional details about the firmware update. There is an option to start the server when the update completes. This option is selected by default. \n \n You can select to either proceed with the firmware update or to cancel.  \n **Important** It is recommended to back up your server before any firmware update. |
| Reboot | Immediately powers off a running server and then powers it back on. |
| Delete | To delete a server, the server must be powered off. If the server has a floating IP address, the floating IP address must be unassociated or released before the server is deleted. The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account. |
{: caption="Table 1. Actions available for bare metal servers" caption-side="bottom"}
   
## Managing bare metal servers by using the UI
{: #managing-bare-metal-servers-ui}
{: ui}

You can view and manage your bare metal server from the *Bare metal servers* page in the {{site.data.keyword.cloud_notm}} console.

To manage your servers, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} dashboard](https://cloud.ibm.com){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.
2. On the **Bare metal servers** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the server that you want to manage. You can perform the following actions:

### Viewing your bare metal servers by using the UI
{: #viewing-bare-metal-server-ui}

You can view the summary of all bare metal server on the bare metal server page, or you can click an individual server name to view details and make changes. From the details page, you can also view the associated network interface, access its subnet, and reserve or delete a floating IP address.



1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to view.

### Updating the firmware for a bare metal server by using the UI
{: #update-firmware-bare-metal-servers-ui}

This action is only displayed if the server is stopped and a firmware update is available. It is recommended to back up your server before any firmware update.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to reboot.
1. Click **Actions...**, then click **Update firmware**.

   You receive the following message when you select to update the firmware.

   > This option updates BIOS and BMC firmware on your server. Backup your server before proceeding. The server will be stopped and unavailable during the process.

   **Start server after update completes** is selected by default. Remove the check if you don't want the server to restart.

1. Click **Proceed** to start the firmware update. Click **Cancel** to cancel the update.

### Rebooting a bare metal server by using the UI
{: #reboot-bare-metal-servers-ui}

The reboot action immediately powers off and powers on the bare metal server.

1. In the [{{site.data.keyword.cloud_notm}} dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to reboot.
1. Click **Actions...**, then click **Reboot**.

### Stopping and starting a bare metal server by using the UI
{: #stop-start-bare-metal-servers-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to start or stop.
1. Click **Actions...**, then click **Stop** or **Start**. When selecting **Stop**, select hard stop or soft stop. The instance will soft stop by default.

Billing continues after the bare metal server is stopped.
{: note}

### Deleting a bare metal server by using the UI
{: #delete-bare-metal-servers-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**
1. Click the name of the bare metal server that you want to delete.
1. Click **Actions...**, then click **Delete**.

The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account.
{: important}

## Managing your bare metal server by by using the CLI
{: #managing-bare-metal-servers-cli}
{: cli}

### Viewing your bare metal servers by using the CLI
{: #viewing-bare-metal-servers-cli}

You can list all your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the command-line interface (CLI).

To list all the bare metal servers by using the CLI, use the `ibmcloud is bare-metal-servers` command. To retrieve a specific bare metal server, specify the ID or name of the bare metal server with the `SERVER` variable.

   ```sh
   ibmcloud is bare-metal-server SERVER
   ```
   {: pre}

For a full list of command options, see [ibmcloud is bare-metal-server](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-view).

### Rebooting a bare metal server by using the CLI
{: #reboot-bare-metal-servers-cli}

You can reboot {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the command-line interface (CLI).

To reboot your bare metal server, use the `ibmcloud is bare-metal-server-restart` command. Specify the ID or name of the bare metal server with the `SERVER` variable.

```sh
ibmcloud is bare-metal-server-restart SERVER
```
{: pre}

The `[-f, --force]` flag forces the operation without confirmation.
{: tip}

### Stopping and starting a bare metal server by using the CLI
{: #stop-start-bare-metal-servers-cli}

You can stop and start your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the command-line interface (CLI)

To stop and start your bare metal server, use the `ibmcloud is bare-metal-server-stop` or `ibmcloud is bare-metal-server-start` command. Specify the ID or name of the bare metal server with the `SERVER` variable.

You must specify the `type` for the stop action in the data payload. `soft` tells the running operating system to stop and shut down cleanly. `hard` immediately stops the bare metal server.
{: important}

To stop the bare metal server:

```sh
ibmcloud is bare-metal-server-stop SERVER --type soft
```
{: pre}

To start the bare metal server:

```sh
ibmcloud is bare-metal-server-start SERVER
```
{: pre}

Billing continues after the bare metal server is stopped.
{: note}

For a full list of command options, see [ibmcloud is bare-metal-server-restart](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-restart).



### Updating the firmware for a bare metal server by using the CLI
{: #update-firmware-bare-metal-servers-cli}

You can update the firmware for your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the command-line interface (CLI). You can only update the firmware on a bare metal server that is stopped and has firmware updates available. It is recommended to back up your server before any firmware update.

To update the firmware for the bare metal server by using the CLI, use the `ibmcloud is bare-metal-server-firmware-update` command. Specify the ID or name of the baremetal server with the`SERVER` variable.

The default value for `auto-start` is `true`. If you don't want the bare metal server to start after the firmware is updated, you must change this value to `false`.

```sh
ibmcloud is bare-metal-server-firmware-update SERVER --auto-start true
```
{: pre}

After issuing the above command, you receive a message to verify whether or not to proceed with the firmware update.

> This option upgrades BIOS and BMC firmware on your server. Backup your server before proceeding. The server will be unavailable during the process. Proceed (y/n)?

For a full list of command options, see [ibmcloud is bare-metal-server-firmware-update](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-firmware-update).

### Deleting a bare metal server by using the CLI
{: #delete-bare-metal-servers-cli}
{: cli}

You can delete your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the command-line interface (CLI).

To delete your bare metal server, use the `ibmcloud is bare-metal-server-delete` command. Specify the ID or name of the baremetal server with the`SERVER` variable.

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

To manage bare metal servers using the application programming interface (API), you need an IAM role that includes the following actions. For more information, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).

   - is.bare-metal-server.bare-metal-server.list
   - is.bare-metal-server.bare-metal-server.read
   - is.bare-metal-server.bare-metal-server.delete
   - is.bare-metal-server.bare-metal-server.update
   - is.bare-metal-server.bare-metal-server.operate
   - is.key.key.operate
   - is.image.image.operate

### Viewing your bare metal servers by using the API
{: #viewing-bare-metal-servers-api}

You can view a list of all your bare metal servers for your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the API. To list all bare metal servers by using the API, use [List all bare metal servers](/apidocs/vpc/latest#list-bare-metal-servers).

Specify a `GET /bare_metal_servers` request to list all the bare metal servers.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers?version=2021-03-09&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

### Retrieve a bare metal servers by using the API
{: #retrieve-bare-metal-servers-api}

You can retrieve a specific bare metal server for your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the API. To retrieve a bare metal servers by using the API, use [Retrieve a bare metal server](/apidocs/vpc/latest#get-bare-metal-server).

Specify a `GET /bare_metal_servers/{id}` request retrieve a specific bare metal server where `id` is the identifer of the bare metal server you are retrieving.

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

For more information of the API requests, see [List all bare metal servers](/apidocs/vpc/latest#list-bare-metal-servers) and [Retrieve a bare metal server](/apidocs/vpc/latest#get-bare-metal-server).

### Rebooting the bare metal server by using the API
{: #reboot-bare-metal-servers-api}

You can reboot a bare metal server for your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the API. To reboot a bare metal servers by using the API, use [Restart a bare metal server](/apidocs/vpc/latest#restart-bare-metal-server).

Specify a `POST /bare_metal_servers/{id}/restart` request to restart a specific bare metal server where `id` is the identifer of the bare metal server you are restarting.

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/restart?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

For more information of the API request, see [Restart a bare metal server](/apidocs/vpc/latest#restart-bare-metal-server).

### Stopping and starting a bare metal server by using the API
{: #stop-start-bare-metal-servers-api}

You can stop or start a bare metal server on your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the API. To stop or start a bare metal servers by using the API, use [Stop a bare metal server](/apidocs/vpc/latest#stop-bare-metal-server) and [Start a bare metal server](/apidocs/vpc/latest#start-bare-metal-server).

Use the following API requests to stop or start a bare metal server.

#### Stopping the bare metal server by using the API
{: #stop-bm-api}

Specify a `POST /bare_metal_servers/{id}/stop` request to stop a specific bare metal server where `id` is the identifer of the bare metal server you are stopping.

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

Specify a `POST /bare_metal_servers/{id}/start` request to start a specific bare metal server where `id` is the identifer of the bare metal server you are starting.

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/start?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

For more information about the API request, see [Start a bare metal server](/apidocs/vpc/latest#start-bare-metal-server).



### Updating the firmware for a bare metal server by using the API
{: #update-firmware-bare-metal-servers-API}

You can update the firmware for your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the API. You can only update the firmware on a bare metal server that is stopped and has firmware updates available. It is recommended to back up your server before any firmware update. To update the firmware using API, use [Update firmware for a bare metal server](/apidocs/vpc-beta/initial#update-firmware-for-bare-metal-server).

Specify a `POST /bare_metal_servers/{id}/firmware/update` request to update the firmware for a specific bare metal server where `id` is the identifer of the bare metal server that you are updating.

The default value for `auto_start` is `true`. If you don't want the bare metal server to start after the firmware is updated, you must change this value to `false`.

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/firmware/update?version=$tomorrow&generation=2&maturity=development" -H "Authorization: Bearer $iam_token" -d '{
  "auto_start": false
}'
```
{: pre}

### Deleting a bare metal server by using the API
{: #delete-bare-metal-servers-api}

You can delete a bare metal server in your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by using the API. To delete a bare metal servers by using the API, use [Delete a bare metal server](/apidocs/vpc/latest#delete-bare-metal-server).

Specify a `DELETE /bare_metal_servers/{id}` request delete a specific bare metal server where `id` is the identifer of the bare metal server you are deleting.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account.
{: important}
