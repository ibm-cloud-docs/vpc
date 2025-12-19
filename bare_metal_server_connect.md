---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-19"

keywords: bare metal server connect esxi, connect to esxi, connect to esxi, bare metal connect esxi, bare metal esxi, windows serial console, connect to windows console, connect to windows serial console, serial console, connect to serial console

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to ESXi with Bare Metal Servers on VPC
{: #connect-to-ESXi-bare-metal-servers}

After your bare metal server is running, you can connect to the ESXi direct console user interface (DCUI) and the ESXi web client. You can use the VNC or serial console to access ESXi DCUI. You can access the ESXi web client through a floating IP.
{: shortdesc}

For more information about VMWare deployments, see the [VMware Cloud Foundation overview](/docs/vmwaresolutions?topic=vmwaresolutions-vpc-vcf-ovw).

## Obtaining your ESXi account name and password
{: #prereq}

You need to enter the account name and password to access both ESXi DCUI and the ESXi web client. You can retrieve the account information by using the CLI.

The password is automatically generated and encrypted by using the first SSH key that was provided when the bare metal server was created. You need to decrypt the password by using this SSH key.


{{_include-segments/ed25519-ssh-key-type-note.md}}

1. Use the following command to retrieve the account name and identify the SSH key that you use to decrypt the password.

   ```sh
   ibmcloud is bare-metal-server-initialization-values $bare_metal_server_id
   ```
   {: pre}

3. In the response, locate the `User accounts` and `SSH keys` fields. The value of “User accounts” is your account name. You need to use the first SSH key to decrypt the password.

4. After you have the SSH key, run the following command to obtain your password:

   ```sh
   ibmcloud is bare-metal-server-initialization-values $bare_metal_server_id [--private-key (KEY | @KEY_FILE)]
   ```
   {: pre}

5. Add either `'<content of the private key file>'` or `@private_key_file_path` for the `--private-key` flag to decode and decrypt your password.

After you retrieve the account name and password, you can use them to access the ESXi DCUI and web client.

## Accessing ESXi DCUI by using a VNC or serial console
{: #access-esxi-dcui-using-console}

You can access the VMware&reg; ESXi DCUI by connecting to a VNC or serial console.

To connect to a console, you need to be assigned **Operator** (or greater) and **Bare Metal Console Administrator** roles for the bare metal server in {{site.data.keyword.iamlong}} (IAM). If you are an administrator of your account, you also need to self-assign the **Bare Metal Console Administrator** role.

ESXi DCUI doesn’t output from a serial console by default. You can enable it by following [Connecting to a serial console](/docs/vpc?topic=vpc-connect-to-ESXi-bare-metal-servers#connect-to-serial-console).

## Connecting to a VNC console
{: #connect-to-vnc-console}

You can use the UI, [CLI](#connect-to-vnc-console-cli), or [API](#connect-to-vnc-console-api) to connect to a VNC console.

### Connecting to a VNC console by using the UI
{: #connect-to-vnc-console-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**

2. Click the overflow icon of the target bare metal server, then click **Open VNC Console**.

### Connecting to a VNC console by using the CLI
{: #connect-to-vnc-console-cli}

1. Run the following command to connect to a console:

   ```sh
   ibmcloud is bare-metal-server-console $bare_metal_server_id --vnc
   ```
   {: pre}

   The access token is invalid after 3 minutes.
   {: note}

2. Save the value of "href" in the response.

3. Open the [noVNC portal ![External link icon](../icons/launch-glyph.svg "External link icon")](https://novnc.com/noVNC/vnc.html) in a browser.

4. Click **Setting**, then expand **Advanced > WebSocket**.

5. Check **Encrypt**, paste the URL API endpoint portion that you saved in Step 2 to **Host:**. Don't include "wss://", set **Port** to "443". Paste the URL's path portion that you saved in Step 2 to **Path**.
   * Example API endpoint: `us-south.iaas.cloud.ibm.com`
   * Example path: `v1/bare_metal_servers/<bare_metal_server_id>/console?access_token=<access_token>&version=2021-05-26&generation=2`

6. Click **Connect**.

### Connecting to a VNC console by using the API
{: #connect-to-vnc-console-api}

1. Run the following API call to connect the VNC console:

   ```sh
   curl -X POST \
   "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/console_access_token?version=2020-05-26&generation=2" \
   -H "Authorization: $token" \
   -d '{"console_type":"vnc"}'
   ```
   {: pre}

2. Follow Steps 2 - 6 in [Connecting to a VNC console by using the CLI](#connect-to-vnc-console-cli) to connect to the VNC console.

## Logging in to ESXi DCUI
{: #login-esxi-dcui}

1. Open the ESXi DCUI in a VNC console.
2. Press F2 from the DCUI main page to access the **System Customization** menu.
3. In the open window, enter the account name and password, then press Enter.

You can reset the password in the **Configure Password** section.
{: tip}

## Enabling SSH for the bare metal server (optional)
{: #enable-ssh}

By default, you don't have SSH access to the ESXi bare metal server. You can enable SSH access in DCUI with one of the following options.

### Enabling SSH from the DCUI
{: #enable-shh-dcui-option-1}

1. Press F2 from the DCUI main page to access the System Customization menu. You are prompted to enter your account name and password.

2. From the **System Customization** menu, select **Troubleshooting Options**.

3. Under the **Troubleshooting Mode Options** menu, select **Enable SSH** and toggle it on.

4. SSH is enabled. You can now access DCUI by using SSH protocol.

### Enabling SSH by using a script
{: #enable-ssh-script-option-2}

You can also enable SSH when you provision a bare metal server by passing in the following script content as user data:

```sh
vim-cmd hostsvc/enable_ssh
vim-cmd hostsvc/start_ssh
```
{: pre}

## Connecting to a serial console
{: #connect-to-serial-console}

You can optionally use a serial console to access ESXi DCUI. You must first manually switch to the serial console mode by using the following steps:

1. Reboot the bare metal server. Then, immediately open a VNC console. Follow the steps in [Accessing ESXi DCUI by using a VNC or serial console](/docs/vpc?topic=vpc-connect-to-ESXi-bare-metal-servers#access-esxi-dcui-using-console).

2. After you click **Connect** on the noVNC connect window, wait until the **Loading VMware Hypervisor** window appears, then press **shift**+**O** to edit boot options.

3. Enter `gdbPort=none logPort=none tty2Port=com1` in the input field, then press Enter. To use com2 instead, replace `com1` with `com2`.

4. When the loading completes, close the noVNC window.

5. Open a serial console by clicking **Open serial console** on the IBM Cloud UI. A new tab opens in your terminal.

6. Press **ESC** if ESXi DCUI doesn’t show up in the terminal. You see ESXi DCUI in your terminal.

You can use other methods to enable the serial console. For more information, see [VMware Docs Home](https://techdocs.broadcom.com/us/en/vmware-cis.html){: external}.
{: note}

## Accessing the ESXi web client by using floating IP
{: #access-esxi-client-using-fip}

You can access the ESXi web client through the floating IP.

## Attaching a floating IP to the network interface of the bare metal server
{: #attaching-fip-to-nic}

Before you can access the bare metal server through the public internet, you need to attach a floating IP to its primary PCI network interface. You attach a floating IP by using the UI, [CLI](#attaching-fip-to-nic-cli), or [API](#attaching-fip-to-nic-api).

### Attach a floating IP by using the UI
{: #attaching-fip-to-nic-ui}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Bare metal servers**

2. Click the name of the bare metal server.

3. On the **Bare metal server details** page, scroll to the **Network Interfaces** section and click **Edit** of a network interface.

4. On **Edit network interface**, select a reserved floating IP or **Reserve a new floating IP**.

5. Click **Save**

### Attach a floating IP by using the CLI
{: #attaching-fip-to-nic-cli}

You need the following information to attach a floating IP to the bare metal server:

* Bare metal server ID
* Network interface ID
* Floating IP ID

Use the [List the network interfaces](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-network-interface-view) command to find the ID of the network interface. Use the [ibmcloud is floating-ips](/docs/vpc?topic=vpc-vpc-reference#floating-ip-view) command to find the reserved floating IP IDs, or, use the [ibmcloud is floating-ip-reserve](/docs/vpc?topic=vpc-vpc-reference#floating-ip-reserve) command to reserve a new one.
{: tip}

After you collect all the required information, use the following command to attach the floating IP to the bare metal server:

```sh
ibmcloud is bare-metal-server-network-interface-floating-ip-add $bare_metal_server_id $network_interface_id $floating_ip_id
```
{: pre}

### Attach a floating IP by using the API
{: #attaching-fip-to-nic-api}

You need the following information to attach a floating IP to a network interface of the bare metal server:

* Bare metal server ID
* Network interface ID
* Floating IP ID

Use the [List all network interfaces](/apidocs/vpc/latest#list-bare-metal-server-network-interfaces) command to find the ID of the network interface. Use the [List all floating IP](/apidocs/vpc/latest#list-floating-ips) command to find the reserved floating IP IDs, or use the [Reserve a floating IP](/apidocs/vpc/latest#create-floating-ip) command to reserve a new one.
{: tip}

After you collect all the required information, use the following API request to attach the floating IP to the bare metal server:

```sh
curl -X PUT "$vpc_api_endpoint/v1/bare_metal_servers/$bare_metal_server_id/network_interfaces/$network_interface_id/floating_ips/$floating_ip_id?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

### Accessing the ESXi web client
{: #login-esxi-client}

1. After you complete the previous steps, you can now access the ESXi web client by entering the floating IP in the address bar of your browser.
2. In the login window, enter the account name and password that you previously retrieved.

## Notes on the console service
{: #access-console-notes}

Keep the following notes in mind when you're using a console service.

- The console session expires after 10 minutes of idle time. It closes after 60 minutes, regardless of activity.
- The number of active VNC consoles per server is limited to two. The number of active serial consoles per server is limited to one.
