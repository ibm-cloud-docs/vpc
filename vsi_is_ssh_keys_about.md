---

copyright:
  years: 2018, 2019
lastupdated: "2019-09-30"

keywords: ssh keys, vsi, virtual server instance

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:tip: .tip}

# SSH keys
{: #ssh-keys}
[comment]: # (linked help topic)

When you create a virtual server instance, you must select an existing SSH key or add a new SSH key. SSH keys are used by servers to identify a user or device through public-key cryptography. SSH keys are made up of an alpha-numeric combination and are unique to the device to which they are assigned. You can add, edit, or delete SSH keys by using the {{site.data.keyword.cloud}} console.
{:shortdesc}

By adding an SSH key to an instance, the instance can be accessed with the corresponding key instead of a password. You can add SSH keys to an instance only when you initially create the instance. After a Linux instance is created, you can edit keys directly in the `~/.ssh/` directory of the instance.

Logging in to your instance with a password isn't supported. 
{:note}

## Locating or generating your SSH key
{: #locating-ssh-keys}

Before you can add a key in the {{site.data.keyword.cloud}} console, you must have your SSH key available. To locate your SSH key or generate an SSH key, complete one of the following steps.

 * Locate an SSH key: Look for a file called `id_rsa.pub` under an `.ssh` directory under your home directory, for example, `/Users/<USERNAME>/.ssh/id_rsa.pub`. The file starts with `ssh-rsa` and ends with your email address.

* Generate an SSH key: If you do not have a public SSH key or if you forgot the password of an existing one, generate a new one by running the `ssh-keygen` command and following the prompts. For example, you can generate an SSH key on your Linux server by running the command `ssh-keygen -t rsa -C "user_ID"`. That command generates two files. The generated public key is in the `<your key>.pub` file.