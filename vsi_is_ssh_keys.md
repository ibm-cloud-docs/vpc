---

copyright:
  years: 2018, 2021
lastupdated: "2021-05-12"

keywords: 
subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:note: .note}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}

# Managing SSH keys
{: #managing-ssh-keys}

To access {{site.data.keyword.vsi_is_full}} instances, you must have an SSH key available to use. You can add and delete SSH keys in {{site.data.keyword.cloud_notm}} console and by using the CLI. 
{: shortdesc}

Managing keys by using the {{site.data.keyword.cloud_notm}} console or CLI has no effect on keys in instances that are already created. (For an existing Linux instance, you can edit keys directly in the `~/.ssh/` directory of the instance.)

## Before you begin
{: #prereq-ssh-key-available}

To create a virtual server instance, you must have an SSH key uploaded and available so that you can connect to your instance after it's provisioned.

For more information about locating or generating an SSH key, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys).
{: tip}

## Managing SSH keys with IBM Cloud console
{: #managing-ssh-keys-with-ibm-cloud-console}

When you provision a virtual server, you can select from available SSH keys or upload a new one. You cannot generate SSH keys in {{site.data.keyword.cloud_notm}} console.


You can add and delete SSH keys by using the {{site.data.keyword.cloud_notm}} console.
1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc){: external}, navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > SSH keys**. Any existing SSH keys are displayed. You can use the **...** menu to copy the UUID of an SSH key or delete an SSH key.
2. To add an SSH key, click **Add SSH Key**.
3. On the Add SSH key page, enter a name for your SSH key, select a resource group, and select a region.

    You are shown a list of the available regions for your specific resource group.
    {: note}
  
4. Locate your public SSH key. It might be in an .ssh directory within your home directory, for example, `/Users/<USERNAME>/.ssh/id_rsa.pub`. For more information, see [Locating or generating your SSH key](/docs/vpc?topic=vpc-ssh-keys#locating-ssh-keys).
  
    The directory might contain two files with the same file name. The "public" SSH key, contains the extension `.pub`. The content of the public SSH key file typically begins with `ssh-rsa` and ends with your user name.
    {: tip}
  
5. You can open the public SSH key file with a text editor. Then, copy and paste the entire contents of the SSH file into the **Public key** space on the form.
6. Click **Add SSH key** to create the your SSH key in the IBM Cloud console. It now displays in **VPC Infrastructure > Compute > SSH keys**.

## Managing SSH keys by using the CLI
{: #managing-ssh-keys-by-using-the-cli}

You can also manage your SSH keys by using the CLI.

Make sure that you have the {{site.data.keyword.cloud_notm}} CLI `vpc-infrastructure` plug-in installed. For more information, see [IBM Cloud CLI for VPC](/docs/vpc?topic=vpc-set-up-environment).
{: tip}

| Action           | Command                     | What happens next |
| ---------------- | --------------------------- | ----------------- |
| Create SSH key   | `ibmcloud is key-create`    | After you create an SSH key, it is added to the list of keys. |
| View key details | `ibmcloud is key`           | You can view the name of the key and the ID of the key. |
| List keys        | `ibmcloud is keys`          | You can view all of your existing SSH keys. |
| Update key       | `ibmcloud is key-update`    | After you update an existing key, the key is renamed immediately. |
| Delete key       | `ibmcloud is key-delete`    | After you remove an SSH key, it can no longer be used when you provision a new instance or when you perform an OS reload on an existing instance. However, the key is still available on any instances that you have provisioned with it, and you can use it to log in. |
{: caption="Table 1. SSH key actions" caption-side="top"}
