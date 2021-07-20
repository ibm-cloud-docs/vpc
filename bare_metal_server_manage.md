---

copyright:
  years: 2021
lastupdated: "2021-05-14"

keywords: bare metal servers, managing, operation, tutorial

subcollection: vpc

---

{:beta: .beta}
{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing bare metal servers - draft
{: #managing-bare-metal-servers}

You can view your bare metal servers and perform lifecycle operations including reboot, stop, and start. When you finish using the bare metal server, you can delete it.

## Viewing details of the bare metal server
{: #viewing-bare-metal-servers}

You can view details of the bare metal server by using the IBM Cloud console, API request, or IBM Cloud Command Line Interface (CLI).

### View the bare metal server using the UI
{: #managing-bare-metal-servers-ui}

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**

2. Click the name of the bare metal server that you want to view.

### View the bare metal server using CLI
{: #managing-bare-metal-servers-cli}

For the beta release, you must enable the bare metal servers CLI by running: `export IBMCLOUD_IS_FEATURE_BARE_METAL_SERVER=true`
{:important}

1. List all bare metal servers:

  ```
  ibmcloud is bare-metal-servers [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups]
  ```
  {:pre}

2. Retrieve a bare metal server:

  ```
  ibmcloud is bare-metal-server $bare_metal_server_id
  ```
  {:pre}

### View the bare metal server using API
{: #managing-bare-metal-servers-api}

1. List all bare metal servers:

  ```
  curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers?version=2021-03-09&generation=2" \
  -H "Authorization: $iam_token"
  ```
  {:pre}

2. Retrieve a bare metal server:

  ```
  curl -X GET "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
  -H "Authorization: $iam_token"
  For detailed information of the API requests, see List all bare metal servers and Retrieve a bare metal server.
  ```
  {:pre}

For more information of the API requests, see [List all bare metal servers](/apidocs/vpc-beta#list-bare-metal-servers) and [Retrieve a bare metal server](/apidocs/vpc-beta#get-bare-metal-server).

## Performing lifecycle operations on the bare metal server
{: #lifecycle-operation-bare-metal-servers}

The following operations are available for bare metal servers.

### Reboot
{: #reboot-bare-metal-servers}

The reboot action immediately powers off and powers on the bare metal server.

#### Reboot the bare metal server using the UI
{: #reboot-bare-metal-servers-ui}

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**

2. Click the name of the bare metal server that you want to reboot.

3. Click the **Actions...** button, then click **Reboot**.

#### Reboot using CLI
{: #reboot-bare-metal-servers-cli}

Use the following CLI command to reboot your bare metal server.

```
ibmcloud is bare-metal-server-restart $bare_metal_server_id [-f, --force] [-q, --quiet]
```

The `[-f, --force]` flag indicates to force the operation without confirmation.
{: note}

#### Reboot the bare metal server using API
{: #reboot-bare-metal-servers-api}

Use the following API request to reboot your bare metal server.

```
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/restart?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{:pre}

For more information of the API request, see [Restart a bare metal server](/apidocs/vpc-beta#create-bare-metal-server-restart).

### Stop and start
{: #stop-start-bare-metal-servers}

The stop action powers off the bare metal server and the start action powers it on.

Billing will continue after the bare metal server is stopped.
{: note}

#### Stop and start using the UI
{: #stop-start-bare-metal-servers-ui}

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**

2. Click the name of the bare metal server that you want to start or stop.

3. Click the **Actions...** button, then click **Stop** or **Start**.

#### Stop and start using CLI
{: #stop-start-bare-metal-servers-cli}

Use the following CLI command to stop and start your bare metal server.

To stop the bare metal server:

```
ibmcloud is bare-metal-server-stop $bare_metal_server_id [--type soft | hard] [-f, --force] [-q, --quiet]
```
{:pre}

To start the bare metal server:

```
ibmcloud is bare-metal-server-start $bare_metal_server_id [-q, --quiet]
```
{:pre}

#### Stop and start using API
{: #stop-start-bare-metal-servers-api}

To stop the bare metal server:

```
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/stop?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
-d '{
        "type": "soft"
}'
```
{:pre}

You must specify the `type` for the stop action in the data payload. `soft` signals the running operating system to quiesce and shutdown cleanly. `hard` indicates to immediately stop the bare metal server.
{: note}

To start the bare metal server:

```
curl -X POST "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/start?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{:pre}

For detailed information of the API requests, see [Stop a bare metal server](/apidocs/vpc-beta#create-bare-metal-server-stop) and [Start a bare metal server](/apidocs/vpc-beta#create-bare-metal-server-start).

## Deleting your bare metal server
{: #delete-bare-metal-servers}

When you no longer need the bare metal server, you can delete it.

This operation permanently removes the bare metal server and its attached network interfaces, boot volume and data from your account. After you confirm the delete action, the deletion process begins immediately and the instance will be removed from the bare metal servers page.

Before you can delete a bare metal server, you must:

  1. Make sure that the bare metal server is in the **Stopped** state.
  2. Detach the floating IP that is attached to the bare metal server.

### Delete the bare metal server using the UI
{: #delete-bare-metal-servers-ui}

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**

2. Click the name of the bare metal server that you want to delete.

3. Click the **Actions...** button, then click **Delete**.

### Delete the bare metal server using the CLI
{: #delete-bare-metal-servers-cli}

Use the following CLI command to delete your bare metal server.

```
ibmcloud is bare-metal-server-delete $bare_metal_server_id
```

### Delete the bare metal server using the API
{: #delete-bare-metal-servers-api}

Use the following API request to delete your bare metal server.

```
curl -X DELETE "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```

For more information of the API requests, see [Delete a bare metal server](/apidocs/vpc-beta#delete-bare-metal-server).
