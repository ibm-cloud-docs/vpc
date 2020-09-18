---

copyright:
  years: 2018, 2020

lastupdated: "2020-08-24"

keywords: connecting, windows

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Connecting to Windows instances
{: #vsi_is_connecting_windows}

After you created your Windows instance, you can connect to it by completing these steps.
{:shortdesc}

## Before you begin
{: #before_vsi_connecting_windows}

Complete the following prerequisites:

1. Make sure that you have the private key that is associated with the public SSH key that was used to create the Windows instance. This private key must be stored locally, for example, in `~/.ssh/id_rsa`. The private key might also be named your `username`. For more information, see [Locating or generating your SSH key](/docs/vpc?topic=vpc-ssh-keys#locating-ssh-keys).
1. Ensure that you downloaded, installed, and initialized the following CLI plug-ins:
    * {{site.data.keyword.cloud_notm}} CLI
    * The infrastructure-service plug-in
   For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
1. Have Microsoft Remote Desktop client software available.
1. Make sure the security group that is associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).
1. Make sure that you reserve and associate a floating IP address to your Windows instance.

## Connecting to your Windows instance
{: #vsi_connecting_windows_instance}

After you create your Windows instance and complete the prerequisites, complete the following steps to connect to your Windows instance. 
  
1. Query the status of your instance by running the following command:
  
  ```
  $ ibmcloud is instance <INSTANCE_ID>
  ```
  {:codeblock}
  
  When the instance shows that it's `running`, you are ready to retrieve the initialization values to get your password.

2. Run the following command to initialize your instance and obtain your instance password:

    ```
    ibmcloud is instance-initialization-values INSTANCE [--private-key (KEY | @KEY_FILE)] [--json]
    ```
    {:pre}

    This command decodes and decrypts your password, which is automatically generated when you create an instance using a Windows image based on the public SSH Key you uploaded and the associated private SSH key file. For more information, see the [CLI command reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-initialization-values).

    You can also use the API to get the encrypted password, which returns the decoded and decrypted password. For more information, see [Retrieve configuration used to initialize the instance API](https://{DomainName}/apidocs/vpc#retrieve-configuration-used-to-initialize-the-inst).
    {:tip}

3. After you obtain your instance password, you can optionally associate a floating IP address to your Windows instance so you can connect to it from an internet location. Run the following command to associate a floating IP address to your instance:

   ```
   ibmcloud is floating-ip-reserve <FLOATING_IP_NAME> --nic-id <NIC_ID>
   ```
   {:pre}

4. You now have what you need in order to connect to your Windows instance: a decrypted password and a floating IP address. Use your preferred Remote Desktop client to connect to your instance. To connect to your instance, provide the floating IP address and the decrypted password. The username is `Administrator` by default. (If you are connecting from a client that is running the Windows Administrator account, use `.\Administrator` as the user ID to log on to RDP.)

## Next steps
{: #next-manage-vsi}

After you are connected to your instance, you can [manage your instances](/docs/vpc?topic=vpc-managing-virtual-server-instances). 
