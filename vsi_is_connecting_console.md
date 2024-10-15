---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-15"

keywords: vnc console, serial console, virtual server instance

subcollection: vpc

---
{{site.data.keyword.attribute-definition-list}}


# Accessing virtual server instances by using VNC or serial consoles
{: #vsi_is_connecting_console}

You can access your IBM Cloud virtual server instance by connecting to a VNC or serial console by using the IBM Cloud UI, API requests, or IBM Cloud Command Line Interface (CLI). The console service is a quick-and-easy way for you to interact with the instance without using a Secure Shell.
{: shortdesc}

_For z/OS virtual server instances only:_ Connecting a z/OS virtual server instance to a VNC console is not supported.
{: note}

It applies to situations where a boot failure or kernel crash occurred, especially when you use a custom image. When these situations happen, you can use the console service to examine the issue.

The VNC console provides a graphical user interface and accepts both mouse and keyboard input. The serial console provides a text-based console that accepts keyboard input.

You can use the console to access OS load and boot procedures such as GNU GRand Unified Bootloader (GRUB).

The console can be opened by using any of the [supported browsers](/docs/overview?topic=overview-prereqs-platform#browsers-platform).

## Before you begin
{: #vsi_is_connecting_console_prereq}

1. To connect to the consoles, you need to be assigned `Operator` (or greater) and `Console Administrator` roles for the virtual server instance in IBM Cloud Identity and Access Management (IAM).

    `Console Administrator` role is not applied automatically. If you are an administrator of your account, you also need to self-assign the `Console Administrator` role to use this feature.
    {: important}

    To check whether you are assigned the required roles, go to the [IAM Users](/iam/users){: external} page in the IBM Cloud console and select your account under **User**, then select **Access policies**. Make sure that you see an access policy that assigns you the `Operator` (or greater) role and the `VirtualServerConsoleAdmin` role to the **Resource Attributes** of the target virtual server instance. Otherwise, you would need to contact an administrator of your account to assign you the roles by using the following steps:

    1. Go to the [IAM Users](/iam/users){: external} page in the IBM Cloud console and select the target user.
    1. Click the **Access** tab, and scroll to **Access policies**.
    1. Click **Assign access**.
    1. Scroll to the **Create policy** section.
    1. In the **Service** section, select _VPC Infrastructure services_. Then, click **Next**.
    1. In the **Resources** section, select _All resources_.
    1. In the **Roles and actions, select the following Service access:
       - Console Administrator
    1. Then, select one of the following Platform accesses:
       - Operator
       - Editor
       - Administrator
    1. Click **Next**.
    1. Optionally, add a Condition.
    1. Click **Review**.
    1. Click **Add**.
    1. Review the **Access summary** side pane, and click **Assign**.

    For more information about the IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).
    {: tip}

2. The images that are provided by IBM Cloud typically do not have passwords. To successfully access the instance with the consoles, you might need to create a password for a Linux image, or retrieve the password for a Windows image in advance.

    * For Linux images, connect to the instance following [Connecting to Linux instances](/docs/vpc?topic=vpc-vsi_is_connecting_linux). On the instance, create a local password with the following command: `sudo passwd $(whoami)`

    * For Windows images, obtain the password following [Connecting to Windows instances](/docs/vpc?topic=vpc-vsi_is_connecting_windows).

    * For z/OS images, obtain the password following [Connecting to z/OS instances](/docs/vpc?topic=vpc-vsi_is_connecting_zos).

For the serial console, you can configure `getty` to log in automatically without a password by using the `-a root` flag.
{: tip}

To enable the serial console service for custom Linux images, make sure that the argument `console=ttyS0` is present on the kernel command line. For more information, see [Step 1 - Start with a single image file in qcow2 or VHD format](/docs/vpc?topic=vpc-create-linux-custom-image#single-image-qcow2) in [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image).
{: note}

## Using the IBM Cloud UI to connect to a console
{: #vsi_is_connecting_console_ui}
{: ui}

Follow these steps to connect to a console by using IBM Cloud UI.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instance**.
2. In the **Virtual server instances for VPC** list, click the overflow button of the instance that you need to access, then click **Open VNC Console** or **Open serial Console**. Alternatively, on the instance details page, click **Action** on the upper right then click **Open VNC Console** or **Open Serial Console**.
3. (For serial console only) If the serial console is being used, you are prompted to confirm whether to force open a session. This action disconnects the other user's session.
4. Enter the credentials and follow the prompts to log in to your instances.

You can stop or restart the instance by clicking **Shutdown instance** or **Reboot instance** on the upper right of the console's window.
{: note}

## Using API to connect to a console
{: #vsi_is_connecting_console_api}
{: api}

Before you can use the API requests to connect to a VNC or serial console, you need to get an IAM token, store the endpoint as a variable, and verify that you have access to the VPC API service. For more information, see [API prerequisites](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

### Using API to connect to a VNC console
{: #vsi_is_connecting_console_api_vnc}

1. Create a console access token for the instance. Specify `"console_type"："VNC"` in the payload.

    ```curl
      curl -X POST \
      "$vpc_api_endpoint/v1/instances/$instance_id/console_access_token?version=2021-01-26&generation=2" \
      -H "Authorization: $iam_token" \
      -d '{
            "console_type": "vnc"
      	  }'
    ```
    {: pre}

    The access token will be invalid after 3 minutes.
    {: note}

2. Save the value of "href" in the response.
3. Open the [noVNC portal](https://novnc.com/noVNC/vnc.html){: external} in a browser.
4. Click **Setting** and expand **Advanced > WebSocket**.
5. Check **Encrypt**, paste the URL's API endpoint portion you saved in step 2 to **Host:**, do not include "wss://", set **Port** to "443", paste the URL's path portion you saved in step 2 to **Path**.
    * Example API endpoint: `us-south.iaas.cloud.ibm.com`
    * Example path: `v1/instances/<instance_id>/console?access_token=<access_token>&version=2020-12-06&generation=2`
6. Click **Connect**.
7. Log in to the instance.

### Using API to connect to a serial console
{: #vsi_is_connecting_serial_console_api_vnc}

1. Create a console access token for the instance, specify `"console_type": "serial"` and `"force": true` in the payload.

    ```curl
      curl -X POST \
      "$vpc_api_endpoint/v1/instances/$instance_id/console_access_token?version=2020-01-26&generation=2" \
      -H "Authorization: $token" \
      -d '{
         "console_type":"serial",
         "force": true
      }'
    ```
    {: pre}

    By specifying `"force"` to `true`, you can connect to the serial console even when the console is used by other users. The default value is `false`, which means the connection can't be established if the console is being used.
    {: note}

2. Save the value of "href" in the response.
3. Start your serial console program by using the URL.

   If you use websocat, specify the `--binary` flag in your command. For example, `websocat --binary "wss://us-south.iaas.cloud.ibm.com/v1/instances/<instance_id>/console?access_token=<access_token>&version=2020-12-06&generation=2"`
   {: tip}

4. Enter the credentials and follow the prompts to log in to your instances.

## Using CLI to connect to a console
{: #vsi_is_connecting_console_cli}
{: cli}

Make sure that you set up the CLI environment by following [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

1. Run the following command to connect to a console:

   ```sh
   ibmcloud is instance-console $instance_id [-q, --quiet]
   ```
   {: pre}

   This command opens a serial console by default. To open a VNC console, add the `[--vnc]` flag to the command to retrieve URL of the console.
   {: note}

2. Depending on what console that you are using, do one of the following steps:
   * For VNC consoles, follow **Step 2** to **Step 7** in [Using API to connect to a VNC console](/docs/vpc?topic=vpc-vsi_is_connecting_console#vsi_is_connecting_console_api_vnc).
   * For serial consoles, enter the credentials and follow the prompts to log in to your instances.

## Disconnecting from the console
{: #vsi_is_connecting_console-disconnect}

When you are finished with the console, you can disconnect from it by closing the terminal or the browser.

## Notes on the console service
{: #vsi_is_connecting_console-notes}

1. The console session expires after 10 minutes of idle time. It closes after 60 minutes, regardless of activity.

   Some operating systems have a flashing cursor on the console, for example, Ubuntu 18.04. When you use the VNC console to access instances that use such operating systems, the flashing cursor causes the console session to remain active after it idles for 10 minutes. The console session will still close after 60 minutes regardless of activity.
   {: note}

1. The console disconnects if the instance is powered off. You can't reestablish the connection until the instance starts again.
1. Restart, reset, or any other operation that doesn't result in rescheduling of the instance maintains the console connection.
1. The number of active VNC consoles per instance is limited to two. The number of active serial consoles per instance is limited to one.
1. For information on troubleshooting a Linux virtual server instance using the serial console, see [How can I use Linux SysRq key to troubleshoot a Linux virtual server instance from the serial console?](/docs/vpc?topic=vpc-troubleshooting-your-virtual-servers-for-vpc#linux-vsi-serial-console).
