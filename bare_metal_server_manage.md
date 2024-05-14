---

copyright:
  years: 2021, 2024

lastupdated: "2024-05-16"

keywords: bare metal servers, managing, operation, manage bare metal server, manage bare metal, manage server, restart bare metal, stop bare metal, delete bare metal, reboot bare metal, restart server, stop server, delete server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing Bare Metal Servers for VPC
{: #managing-bare-metal-servers}

You can manage your {{site.data.keyword.cloud}} Bare Metal Servers for VPC by performing tasks such as start, stop, restart, and delete bare metal server.
{: shortdesc}

Updating the firmware for a bare metal server is a beta feature that is available for evaluation and testing purposes.
{: beta}

You can perform the following actions by using the UI, CLI, and API.

| Action | Description |
|--------|-------------|
| Stop| Stop the server using a soft stop or a hard stop.  \n - Soft stop can take a few seconds to several minutes to stop shut down the server, depending on the state of the operating system. If the operating system is not responding and the soft stop can not complete, a hard stop is required.  \n - Hard stop shuts down the bare metal server immediately. This prevents the operating system from shutting down gracefully. |
| Start | Start a stopped server. This action is not available if the status is Running. |
| Reboot | Immediately powers off a running server and then powers it back on. |
| Delete | To delete a server, the server must be powered off. If the server has a floating IP address, the floating IP address must be unassociated or released before the server is deleted. The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account. |
{: caption="Table 1. Actions available for bare metal servers" caption-side="bottom"}

## Managing bare metal servers by using the UI
{: #managing-bare-metal-servers-ui}
{: ui}

You can view and manage your bare metal server from the *Bare metal servers* page in the {{site.data.keyword.cloud_notm}} dashboard.

To manage your servers, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} dashboard](https://cloud.ibm.com){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**.
2. On the **Bare metal servers** page, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for the server that you want to manage. You can perform the following actions:

### Viewing your bare metal servers
{: #viewing-bare-metal-server-ui}

You can view the summary of all bare metal server on the bare metal server page, or you can click an individual server name to view details and make changes. From the details page, you can also view the associated network interface, access its subnet, and reserve or delete a floating IP address.

1. In the [{{site.data.keyword.cloud_notm}} dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**

2. Click the name of the bare metal server that you want to view.

### Rebooting a bare metal server
{: #reboot-bare-metal-servers-ui}

The reboot action immediately powers off and powers on the bare metal server.

1. In the [{{site.data.keyword.cloud_notm}} dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**

2. Click the name of the bare metal server that you want to reboot.

3. Click **Actions...**, then click **Reboot**.

### Stopping and starting a bare metal server
{: #stop-start-bare-metal-servers-ui}

1. In the [{{site.data.keyword.cloud_notm}} dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**

2. Click the name of the bare metal server that you want to start or stop.

3. Click **Actions...**, then click **Stop** or **Start**. When selecting **Stop**, select hard stop or soft stop. The instance will soft stop by default.

Billing continues after the bare metal server is stopped.
{: note}

### Deleting a bare metal server
{: #delete-bare-metal-servers-ui}
{: ui}

1. In the [{{site.data.keyword.cloud_notm}} dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**

2. Click the name of the bare metal server that you want to delete.

3. Click **Actions...**, then click **Delete**.

The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account.
{: important}

## Managing your bare metal server by using the CLI
{: #managing-bare-metal-servers-cli}
{: cli}

### Viewing your bare metal servers
{: #viewing-bare-metal-servers-cli}

Use the following command to list all bare metal servers:

   ```sh
   ibmcloud is bare-metal-servers [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups]
   ```
   {: pre}

Use the following command to retrieve a bare metal server:

   ```sh
   ibmcloud is bare-metal-server $bare_metal_server_id
   ```
   {: pre}

### Rebooting a bare metal server
{: #reboot-bare-metal-servers-cli}

Use the following CLI command to reboot your bare metal server.

```sh
ibmcloud is bare-metal-server-restart $bare_metal_server_id [-f, --force] [-q, --quiet]
```

The `[-f, --force]` flag forces the operation without confirmation.
{: tip}

### Stopping and starting a bare metal server
{: #stop-start-bare-metal-servers-cli}

Use the following CLI command to stop and start your bare metal server.

To stop the bare metal server:

```sh
ibmcloud is bare-metal-server-stop $bare_metal_server_id [--type soft | hard] [-f, --force] [-q, --quiet]
```
{: pre}

To start the bare metal server:

```sh
ibmcloud is bare-metal-server-start $bare_metal_server_id [-q, --quiet]
```
{: pre}

Billing continues after the bare metal server is stopped.
{: note}

### Deleting a bare metal server by using the CLI
{: #delete-bare-metal-servers-cli}
{: cli}

Use the following CLI command to delete your bare metal server.

```sh
ibmcloud is bare-metal-server-delete $bare_metal_server_id
```

The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account.
{: important}

## Managing your bare metal server by using the API
{: #managing-bare-metal-servers-api}
{: api}

### Viewing your bare metal servers
{: #viewing-bare-metal-servers-api}

Use the following API request to list all bare metal servers:

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers?version=2021-03-09&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

Use the following API request to retrieve a bare metal server:

   ```sh
   curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
   -H "Authorization: $iam_token"
   ```
   {: pre}

For more information of the API requests, see [List all bare metal servers](/apidocs/vpc/latest#list-bare-metal-servers) and [Retrieve a bare metal server](/apidocs/vpc/latest#get-bare-metal-server).

### Rebooting the bare metal server
{: #reboot-bare-metal-servers-api}

Use the following API request to reboot your bare metal server.

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/restart?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

For more information of the API request, see [Restart a bare metal server](/apidocs/vpc/latest#create-bare-metal-server).

### Stopping and starting a bare metal server
{: #stop-start-bare-metal-servers-api}

Use the following API requests to stop or start a bare metal server.

#### Stopping the bare metal server
{: #stop-bm-api}

Use the following API request to stop your bare metal server.

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

For more information about the API request, see [Stop a bare metal server](/apidocs/vpc/latest#create-bare-metal-server).

#### Starting the bare metal server
{: #start-bm-api}

Use the following API request to start your bare metal server.

```sh
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/start?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

For more information about the API request, see [Start a bare metal server](/apidocs/vpc/latest#create-bare-metal-server).

### Deleting a bare metal server by using the API
{: #delete-bare-metal-servers-api}

Use the following API request to delete your bare metal server.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```

The delete action permanently removes a server and its connected vNIC, boot volume, and data from your account.
{: important}

For more information about the API request, see [Delete a bare metal server](/apidocs/vpc/latest#delete-bare-metal-server).
