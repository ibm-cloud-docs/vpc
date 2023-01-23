---

copyright:
  years: 2018, 2022
lastupdated: "2022-05-18"

keywords: ssh public keys, OpenSSH, add ssh key, ssh key, manage ssh key, generate ssh key, locate ssh key

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# SSH keys
{: #ssh-keys}
[comment]: # (linked help topic)

When you create a server, you must select an existing SSH key or generate an SSH key. SSH keys are used by servers to identify a user or device through public-key cryptography. SSH keys are made up of an alpha-numeric combination and are unique to the device to which they are assigned. You can add, edit, or delete SSH keys by using the {{site.data.keyword.cloud}} console. By adding an SSH key to a server, you can access the server with the corresponding SSH key instead of a password. You can add SSH keys to a server only when you initially create the server. After a Linux&reg; server is created, you can edit keys directly in the `~/.ssh/` directory of the server.
{: shortdesc}

Creating a server with a password option for connecting isn't supported. You must specify an SSH key when you provision the server and use the private key to connect to the server.
{: note}

## Locating or generating your SSH key
{: #locating-ssh-keys}

Before you can add an SSH key in the {{site.data.keyword.cloud_notm}} console, you must have your SSH key available. Your SSH key must be an RSA key with a key size of either 2048 bits or 4096 bits.

To locate your SSH key or generate an SSH key, complete one of the following steps.

* Locate an SSH key: Look for a file called `id_rsa.pub`. It might be in an `.ssh` directory under your home directory, for example, `/Users/<USERNAME>/.ssh/id_rsa.pub`. The content of the file typically starts with `ssh-rsa` and ends with your user ID.  

* Generate an SSH key: If you don't have a public SSH key or if you forgot the password of an SSH key, generate a new one by running the `ssh-keygen` command and following the prompts. 

    For example, you can generate an SSH key on your Linux or Mac system by running the command `ssh-keygen -t rsa -C "user_ID"`.

    If your Mac system generates a key size of 3072 bits (by default), run the following command to make sure that the generated key is a supported size: `ssh-keygen -t rsa -b 4096 -C "user_ID"`.
    {: tip}

    Press **Enter** to accept the default location for the file. The command generates two files. The generated public key is in the `<your key>.pub` file. For Windows&reg;, you can use [PuTTYgen](https://www.ssh.com/ssh/putty/windows/puttygen){: external} to generate an SSH key.

    If you are using OpenSSH version 7.8 or higher and plan to access a Windows server, use the following command to generate the key in PEM format. `$ssh-keygen -m PEM -t rsa -C "user_ID"`
    {: important}

When you copy an SSH key from a terminal to add the key to your VPC, sometimes extra line breaks are introduced which cause a parsing error. To avoid this issue, first paste your SSH key into a text editor and remove any extra line breaks. Then, copy the SSH key from text editor and paste it into the VPC UI, CLI, or API.
{: tip}

## Next steps
{: #next-steps-ssh}

After you locate or generate an SSH key, it's time to plan for and create a server. 

Virtual servers
* [Planning for virtual servers](/docs/vpc?topic=vpc-vsi_best_practices)
* [Creating a virtual server by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating a virtual server by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli)

Bare metal servers
* [Planning for Bare Metal Servers on VPC](/docs/vpc?topic=vpc-planning-for-bare-metal-servers)
* [Creating Bare Metal Servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=ui)
