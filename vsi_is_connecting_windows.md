---

copyright:
  years: 2018, 2023

lastupdated: "2023-06-22"

keywords: connecting, windows

subcollection: vpc


---

{{site.data.keyword.attribute-definition-list}}

# Connecting to Windows instances
{: #vsi_is_connecting_windows}

After you created your Windows virtual server instance, you can connect to it by completing these steps.
{: shortdesc}

Windows instances are not supported for LinuxONE (s390x processor architecture).
{: note}

## Before you begin
{: #before_vsi_connecting_windows}

Complete the following prerequisites:

1.  Make sure that you have the private key that is associated with the public SSH key that was used to create the Windows server. If multiple SSH keys were added when the server was created, you must use the first SSH key that was added.
     <!--ed25519 note is shared with several other files bare_metal_server_connect_windows.md, vsi)is_connecting_windows.md, bare_metal_server_connect.md, vsi_is_ssh_keys_about.md -->
     {{_include-segments/ed25519-ssh-key-type-note.md}}
1. Ensure that you downloaded, installed, and initialized the following CLI plug-ins:
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plug-in
    * For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
1. Have Microsoft Remote Desktop client software available.
1. Make sure the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).

   For example, to allow all SSH traffic (3389) and ping traffic (ICMP type 8):

   | Protocol | Source Type | Source | Value |
   |-----------|------|------|------|
   | TCP| Any | <cidr_range> | 3389 |
   | UDP| Any | <cidr_range> | 3389 |
   | ICMP | Any | <cidr_range> | Type: 8, Code: Any|
   {: caption="Table 1. Configuration information for inbound rules" caption-side="bottom"}

   Then, configure outbound rules that allow all TCP traffic:

   | Protocol | Destination Type | Source | Value |
   |-----------|------|------|------|
   | TCP| Any | <cidr_range> | Any port|
   {: caption="Table 2. Configuration information for outbound rules" caption-side="bottom"}

1. Make sure that you reserve and associate a floating IP address to your Windows instance.

## Connecting to your Windows instance
{: #vsi_connecting_windows_instance}

After you create your Windows instance and complete the prerequisites, complete the following steps to connect to your Windows instance.

1. Query the status of your instance by running the following command. `INSTANCE` is the ID or Name for the instance that you want to connect:

    ```sh
    ibmcloud is instance INSTANCE
    ```
    {: pre}

    When the instance shows that it's `running`, you are ready to retrieve the initialization values to get your password.

2. Run the following command to initialize your instance and obtain your instance password. Specify your instance ID or Name for the `INSTANCE` variable and your private key for the `KEY` or `KEY_FILE` variable:

    ```sh
    ibmcloud is instance-initialization-values INSTANCE [--private-key (KEY | @KEY_FILE)]
    ```
    {: pre}

    This command decodes and decrypts your password, which is automatically generated when you create an instance by using a Windows image. The password is decoded and decrypted based on the public SSH Key that you used at instance create time and the associated private SSH key that you specify in the `instance-initialization-values` command. For more information, see the [CLI command reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-initialization-values).

    The following command shows example usage for the `instance-initialization-values` command where `0xx4e27x-33xx-4e7x-a08b-bexx2ac3xx0c` is the instance ID and `~/.ssh/id_rsa` is the location of the user's private key file:

    ```sh
    ibmcloud is instance-initialization-values 0xx4e27x-33xx-4e7x-a08b-bexx2ac3xx0c --private-key @~/.ssh/id_rsa
    ```
    {: pre}

    You can also use the API to get the encrypted password, which returns the decoded and decrypted password. For more information, see [Retrieve configuration that is used to initialize the instance API](/apidocs/vpc/latest#retrieve-configuration-used-to-initialize-the-inst).
    {: tip}

3. After you obtain your instance password, you can optionally associate a floating IP address to your Windows instance so you can connect to it from an internet location. Run the following command to associate a floating IP address to your instance, where `NIC` is the ID or Name of the target network interface (for example, `eth0`).   

   ```sh
   ibmcloud is floating-ip-reserve <FLOATING_IP_NAME> --nic <NIC>
   ```
   {: pre}

4. You now have what you need to connect to your Windows instance: a decrypted password and a floating IP address. Use your preferred Remote Desktop client to connect to your instance. To connect to your instance, provide the floating IP address and the decrypted password. The username is `Administrator` by default. If you're connecting from a client that is running the Windows Administrator account, use `.\administrator` as the user ID to log on to RDP.

## Next steps
{: #next-manage-vsi-win}

After you connect to your instance, you can [manage your instances](/docs/vpc?topic=vpc-managing-virtual-server-instances).


<!-- OLD METHOD 8/24/20 1. Retrieve the encrypted password of the instance:
    1. In the navigation pane of the {{site.data.keyword.cloud_notm}} console, click **Compute > Virtual server instances** and click your instance to view its details.
    1. Scroll down to the **Encrypted password** field. Copy the value and paste it into a text file, for example, encrypted_pwd.txt.

    You can also use the API to get the encrypted password, or the CLI, which returns the decoded and decrypted password. For more information, see [Retrieve configuration used to initialize the instance API](/apidocs/vpc/latest#retrieve-configuration-used-to-initialize-the-inst) and [instance-initialization-values](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-initialization-values).
    {: tip}

1. Decode the encrypted password and store it in a new file (for example, decoded_pwd.txt) by running the following command: `cat encrypted_pwd.txt | base64 -d > decoded_pwd.txt`
1. Decrypt the decoded password by using the following openssl command: `/<location_of_openssl_executable> pkeyutl -in decoded_pwd.txt -decrypt -inkey ~/.ssh/id_rsa`
1. Use the returned value as the Administrator password in Remote Desktop. Enter the public IP address of the Windows instance into the Remote Desktop client. -->
