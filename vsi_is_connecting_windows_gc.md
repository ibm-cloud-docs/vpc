---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-29"

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

# Connecting to your Windows instance
{: #vsi_is_connecting_windows}

After you have created your Windows instance, you can connect to it by completing these steps.
{:shortdesc}

## Before you begin
{: #before_vsi_connecting_windows}

Make sure to complete the following prerequisites before you begin:

1. Ask your account administrator to grant you access to retrieve the password from your virtual server instance. For more information, review [user permissions](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).
2. Create a new security group or add a rule to the default security group to enable inbound access for the Remote Desktop default port, 3389. For more information, see [Using Security Groups](/docs/vpc?topic=vpc-using-security-groups).
4. Verify that you have OpenSSL installed. To successfully decrypt your password, you must run OpenSSL and not LibreSSL. For more information, see [OpenSSL Downloads ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.openssl.org/source/){: new_window}.

LibreSSL isn't compatible for the decryption of your password. You must run OpenSSL to decrypt your password.
{:important}

## Connecting to your Windows instance
{: #vsi_connecting_windows_instance}

After you create your Windows instance and complete the prerequisites, complete the following steps to connect to your Windows instance. 
  
1. Query the status of your instance by running the following command:
  ```
  $ ibmcloud is instance-status <instance id>
  ```
  {:pre}
  
  When the instance shows that it's `running`, you are ready to retrieve the initialization values the instance to get your password. 

2. Run the following command to initialize your instance:

  ```
  $ ibmcloud is instance-initialization-values <instance id>
  ```
  {:pre}
  
  This command displays your encrypted password, which is automatically generated when you create an instance by using a Windows image.

3. You now need to decrypt your password through a manual decryption process. To decrypt your password, run the following command:

  ```
  # Decode the encrypted password
  cat ~/examplepwd | base64 --decode > ~/examplepwd64
  # Decrypt the decoded password using the RSA private key
  openssl pkeyutl -in examplepwd64 -decrypt -inkey private.pem -pkeyopt rsa_padding_mode:oaep -pkeyopt rsa_oaep_md:sha256
  -pkeyopt rsa_mgf1_md:sha256
  ```
  {:pre}
  
  where `~/examplepwd` is the file where you saved your encrypted password as referenced in step 2.  
  
  LibreSSL, which is included with macOS, doesn't support SHA2 hashing algorithms that are needed to decrypt the password, resulting in `Public Key operation error` errors. You can obtain standard OpenSSL libraries by using a package management tool or by installing them manually. 
  {:note}

4. After you decrypt your password, you can optionally associate a floating IP address to your Windows instance so you can connect to it from an internet location. Run the following command to associate a floating IP address to your instance:

  ```
  ibmcloud is fipc --nic <instance nic id>
  ```
  {:pre}

5. You now have what you need in order to connect to your Windows instance: decrypted password and floating IP address. Use your preferred Remote Desktop client to connect to your instance. To connect to your instance, provide the floating IP address and the decrypted password. The username is `Administrator` by default. 



