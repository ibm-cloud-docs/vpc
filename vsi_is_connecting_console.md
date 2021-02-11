---

copyright:
  years: 2020

lastupdated: "2021-02-11"

keywords: vnc console, serial console, virtual server instance

subcollection: vpc


---

{:beta: .beta}
{:codeblock: .codeblock}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:tip: .tip}



# Accessing virtual server instances by using VNC or serial consoles (Beta)
{: #vsi_is_connecting_console}

You can access your IBM Cloud virtual server instance by connecting to a VNC or serial console using the IBM Cloud UI, API requests, or IBM Cloud Command Line Interface (CLI). The console service is a quick-and-easy way for you to interact with the virtual server instance without using a Secure Shell.
{:shortdesc}

It applies to situations where a boot failure or kernel crash occurred, especially when you use a custom image. When these situations happen, you can use the console service to examine the issue.

The virtual server instance console is only available to accounts with special approval to preview this beta feature. Contact your IBM Sales representative if you are interested in getting access.
{:beta}

Limitation for the Beta release: For instances that were created before 2/13/2021, you must stop and re-start (not reboot) the instance before a console can be opened. Opening consoles for instances created after this time doesn't require the above operation.
{:important}

## Before you begin
{: #vsi_is_connecting_console_prereq}

1. To connect to the consoles, you need to be assigned `Operator` (or above) and `Console Administrator` roles for the instance in IBM Cloud Identity and Access Management (IAM).

2. The images that are provided by IBM Cloud typically do not have passwords. To successfully access the instance with the consoles, you might need to create a password for a Linux image, or retrieve the password for a Windows image in advance.

  * For Linux images, connect to the instance following [Connecting to Linux instances](/docs/vpc?topic=vpc-vsi_is_connecting_linux). On the instance, create a local password with the following command: `sudo passwd $(whoami)`

  * For Windows images, obtain the password following [Connecting to Windows instances](/docs/vpc?topic=vpc-vsi_is_connecting_windows).

For the serial console, you can configure `getty` to log in automatically without a password using the `-a root` flag.
{: tip}

To enable the serial console service for custom Linux images, you should make sure that the argument `console=ttyS0` is present on the kernel command line. For more information, see [Step 2 - Check kernel arguments](/docs/vpc?topic=vpc-create-linux-custom-image#kernel-args) in [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image).
{: note}

## Using the IBM Cloud UI to connect to a console
{: #vsi_is_connecting_console_ui}

Follow the steps below to connect to a console by using IBM Cloud UI.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instance**.
2. In the **Virtual server instances for VPC** list, click the overflow button of the instance that you need to access, then click **Open VNC Console** or **Open serial Console**. Alternatively, on the instance details page, click **Action** on the upper right then click **Open VNC Console** or **Open Serial Console**.
3. (For serial console only) If the serial console is being used, you will be prompted to confirm whether to force open a session. This will disconnect the other user's session.
4. Enter the credentials following the prompts to log in to your instances.

You can stop or reboot the instance by clicking **Shutdown instance** or **Reboot instance** on the upper right of the console's window.
{: note}

## Using API to connect to a console
{: #vsi_is_connecting_console_api}

Before you can use the API requests to connect to a VNC or serial console, you need to get an IAM token, store the endpoint as a variable, and verify that you have access to the VPC API service. For more information, see [API prerequisites](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

### Using API to connect to a VNC console
{: #vsi_is_connecting_console_api_vnc}

  1. Create a console access token for the instance. Specify `"console_type"："VNC"` in the payload.

    ```
      curl -X POST \
      "$vpc_api_endpoint/v1/instances/$instance_id/console_access_token?version=2021-01-26&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
            "console_type": "vnc" 
      	  }'
    ```
    {:pre}

    The access token will be invalid after 3 minutes.
    {: note} 

  2. Save the value of "href" in the response.
  3. Open the [noVNC portal ![External link icon](../icons/launch-glyph.svg "External link icon")](https://novnc.com/noVNC/vnc.html) in a browser.
  4. Click the **Setting** button, then expand **Advanced > WebSocket**.
  5. Check **Encrypt**, paste the URL's API endpoint portion you saved in step 2 to **Host:**, do not include "wss://", set **Port** to "443", paste the URL's path portion you saved in step 2 to **Path**.
      * Example API endpoint: `us-south.iaas.cloud.ibm.com`
      * Example path: `v1/instances/<instance_id>/console?access_token=<access_token>&version=2020-12-06&generation=2`
  6. Click **Connect**.
  7. Log in to the instance.

### Using API to connect to a serial console
{: #vsi_is_connecting_console_api_vnc}

  1. Create a console access token for the instance, specify `"console_type": "serial"` and `"force": true` in the payload.

    ```
      curl -X POST \
      "$vpc_api_endpoint/v1/instances/$instance_id/console_access_token?version=2020-01-26&generation=2" \
      -H "Authorization: $token" \
      -d '{
         "console_type":"serial",
         "force": true
      }' 
    ```
    {:pre}

    By specifying `"force"` to `true`, you can connect to the serial console even when the console is being used by other users. The default value is `false`, which means the connection will not be established if the console is being used.
    {: note}

  2. Save the value of "href" in the response.
  3. Start your serial console program by using the above URL.
  
   If you use websocat, specify the `--binary` flag in your command. For example: `websocat --binary "wss://us-south.iaas.cloud.ibm.com/v1/instances/<instance_id>/console?access_token=<access_token>&version=2021-01-26&generation=2"`
   {: tip}
  
  4. Enter the credentials following the prompts to log in to your instances.

## Using CLI to connect to a console
{: #vsi_is_connecting_console_cli}

Make sure you have set up the CLI environment following [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

1. Enable virtual server instance console CLI command by setting the following environment variable:

  ```
  export IBMCLOUD_IS_FEATURE_INSTANCE_CONSOLE=true
  ```
  {:pre}

2. Run the following command to connect to a console:

  ```
  ibmcloud is instance-console $instance_id [-q, --quiet]
  ```
  {:pre}
    
  This command will open a serial console by default. To open a VNC console, add the `[--vnc]` flag to the command to retrieve URL of the console.
  {: note}

3. Next:
  * For VNC consoles, follow **Step 2** to **Step 7** in [Using API to connect to a VNC console](/docs/vpc?topic=vpc-vsi_is_connecting_console#vsi_is_connecting_console_api_vnc).
  * For serial consoles, enter the credentials following the prompts to log in to your instances.

## Disconnecting from the console
{: #vsi_is_connecting_console-disconnect}

When you finish using the console, you can disconnect from it by closing the terminal or the browser.

## Notes on the console service
{: #vsi_is_connecting_console-notes}

1. The console session expires after idling for 10 minutes. It will be closed after 60 minutes, regardless of activity.
  
  Some operating systems have a blinking cursor on the console, for example, Ubuntu 18.04. When using the VNC console to access instances that use such operating systems, the blinking cursor will cause the console session to remain active after it idles for 10 minutes. The console session will still be closed after 60 minutes regardless of activity.
  {: note}
    
2. The console will be disconnected if the instance is powered off. You cannot reestablish the connection until the instance is started again.
3. Reboot, reset, or any other operation that doesn't result in rescheduling of the instance maintains the console connection.
4. The number of active VNC consoles per instance is limited to 2. The number of active serial consoles per instance is limited to 1.
