---

copyright:
  years: 2018, 2019

lastupdated: "2019-08-28"

keywords: vsi, virtual server instance, connecting, windows

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

After you have created your Windows instance, you can connect to it by completing these steps.
{:shortdesc}

## Before you begin
{: #before_vsi_connecting_windows}

Complete the following prerequisites:

1. Make sure you have the private key associated with the public SSH key that was used to create the Windows instance. This private key must be stored locally in `~/.ssh/id_rsa`.
4. Have Microsoft Remote Desktop client software available.
1. Make sure the security group associated with the instance allows inbound and outbound Remote Desktop Protocol traffic (TCP port 3389).
1. Make sure you reserve and associate a floating IP address to your Windows instance.
2. Install openssl (on Mac: `brew install openssl`) and note the location of the executable (for example, `/usr/local/opt/openssl/bin/openssl`)

You must run OpenSSL (not LibreSSL). For more information, see [OpenSSL Downloads ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.openssl.org/source/){: new_window}.
{:important}


## Connecting to your Windows instance
{: #vsi_connecting_windows_instance}

After you create your Windows instance and complete the prerequisites, complete the following steps to connect to your Windows instance. 
  
1. Determine the encrypted password of the instance:
    1. In the navigation pane of the {{site.data.keyword.cloud_notm}} console, click **Compute > Virtual server instances** and click your instance to view its details.
    1. Scroll down to the **Encrypted password** field. Copy the value and paste it into a text file, for example, encrypted_pwd.txt.

  You can also get the encrypted password by using the API or CLI. For more information, see [Retrieve configuration used to initialize the instance API](https://{DomainName}/apidocs/vpc#retrieve-configuration-used-to-initialize-the-inst) and [instance-initialization-values](/docs/vpc?topic=vpc-cli-reference#instance-initialization-values).
  {:tip}

1. Decode the encrypted password and store it in a new file (for example, decoded_pwd.txt) by running the following command: `cat encrypted_pwd.txt | base64 -D > decoded_pwd.txt`
1. Decrypt the decoded password by using the following openssl command: `/<location_of_openssl_executable> pkeyutl -in decoded_pwd.txt -decrypt -inkey ~/.ssh/id_rsa`
1. Use the returned value as the Administrator password in Remote Desktop. Enter the public IP address of the Windows instance into the Remote Desktop client.
