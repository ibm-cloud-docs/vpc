---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-18"

keywords: connecting, windows, bare metal, bare metal server

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Connecting to a Windows bare metal server
{: #bare_metal_server_connecting_windows}

After you create your Windows bare metal server, you can connect to it by using the following steps.
{: shortdesc}



## Before you begin
{: #bare_metal_connecting_windows_before_you_begin}

Complete the following prerequisites:

1.  Make sure that you have the private key that is associated with the public SSH key that was used to create the Windows server. If multiple SSH keys were added when the server was created, you must use the first SSH key that was added.
     
     {{_include-segments/ed25519-ssh-key-type-note.md}}
1. Make sure that you download, install, and initialize the following CLI plug-ins:
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plug-in
    * For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
1. Have Microsoft Remote Desktop client software available.
1. Make sure that the security group that is associated with the server allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).

   For example, to allow all SSH traffic (3389) and ping traffic (ICMP type 8):

   | Protocol | Source Type | Source | Value |
   |-----------|------|------|------|
   | TCP| Any | `<cidr_range>` | 3389 |
   | UDP| Any | `<cidr_range>` | 3389 |
   | ICMP | Any | `<cidr_range>` | Type: 8, Code: Any|
   {: caption="Configuration information for inbound rules" caption-side="bottom"}

   Then, configure outbound rules to allow all TCP traffic:

   | Protocol | Destination Type | Source | Value |
   |-----------|------|------|------|
   | TCP| Any | `<cidr_range>` | Any port|
   {: caption="Configuration information for outbound rules" caption-side="bottom"}

## Connecting to your Windows bare metal server
{: #bare_metal_server_connect_windows}

After you create your Windows server and complete the prerequisites, use the following steps to connect to it.

1. Query the status of your server by running the following command. `SERVER_ID` is the ID for the server that you want to connect:

   ```sh
   ibmcloud is bare-metal-server SERVER
   ```
   {: pre}

    When the server shows `running`, you are ready to retrieve the initialization values to get your password.

2. Run the following command to initialize your server and obtain your password. Specify your server ID for the `SERVER_ID` variable and your private key for the `KEY` or `KEY_FILE` variable:

   ```sh
   ibmcloud is  bare-metal-server-initialization-values SERVER_ID [--private-key (KEY | @KEY_FILE)]
   ```
   {: pre}

   This command decodes and decrypts your password, which is automatically generated when you create a server by using a Windows image. The password is decoded and decrypted based on the public SSH Key that you used at creating time and the associated private SSH key that you specify in this `bare-metal-server-initialization-values` command. For more information, see the [CLI command reference](/docs/vpc?topic=vpc-vpc-reference#bare-metal-server-initialization-values).

   The following command shows example usage for the `bare-metal-server-initialization-values` command where `0xx4e27x-33xx-4e7x-a08b-bexx2ac3xx0c` is the server ID and `~/.ssh/id_rsa` is the location of the user's private key file:

   ```sh
   ibmcloud is bare-metal-server-initialization-values 0xx4e27x-33xx-4e7x-a08b-bexx2ac3xx0c --private-key @~/.ssh/id_rsa
    ```
   {: pre}

   You can also use the API to get the encrypted password, which returns the decoded and decrypted password. For more information, see [Retrieve initialization configuration for a bare metal server](/apidocs/vpc/latest#get-bare-metal-server-initialization).
   {: tip}

3. After you obtain your password, you can optionally associate a floating IP address to your Windows server so you can connect to it from an internet location. Run the following command to associate a floating IP address to your server, where `NIC_ID` is the ID of the target network interface (for example, `eth0`).

   ```sh
   ibmcloud is floating-ip-reserve <FLOATING_IP_NAME> --nic-id <NIC_ID>
   ```
   {: pre}

4. You now have a decrypted password and a floating IP address to connect to your Windows server. Use your preferred Remote Desktop client to connect to your server. To connect to your server, provide the floating IP address and the decrypted password. The username is `Administrator` by default. (If you are connecting from a client that is running the Windows Administrator account, use `.\administrator` as the user ID to log on to RDP.)

## Next steps
{: #next-manage-vsi-bms-win}

After you connect to your server, you can [manage your server](/docs/vpc?topic=vpc-managing-bare-metal-servers).
