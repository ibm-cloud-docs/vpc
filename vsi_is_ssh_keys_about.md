---

copyright:
  years: 2018, 2023
lastupdated: "2023-12-18"

keywords: ssh public keys, OpenSSH, add ssh key, ssh key, manage ssh key, generate ssh key, locate ssh key

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with SSH keys
{: #ssh-keys}

When you create a server, you must select an existing SSH key or generate an SSH key. SSH keys are used by servers to identify a user or device through public-key cryptography. SSH keys are made up of an alpha-numeric combination and are unique to the user or device to which they are assigned. You can create an SSH key by using the UI. You can add, edit, or delete SSH keys by using the {{site.data.keyword.cloud}} console, CLI, and API. By adding an SSH key to a server, you can access the server with the corresponding private SSH key instead of a password. You can add SSH keys to a server only when you initially create the server. After a Linux&reg; server is created, you can edit keys directly in the `~/.ssh/` directory of the server.
{: shortdesc}

Creating a server with a password option for connecting isn't supported. You must specify an SSH key when you provision the server and use the private key to connect to the server.
{: note}

## Supported SSH key types: RSA and Ed25519 in the UI
{: #ssh-key-types-ui}
{: ui}

{{site.data.keyword.vpc_full}} supports two different types of public SSH keys.

* RSA
* Ed25519

On {{site.data.keyword.vpc_short}}, RSA is the default SSH key type. You can select to change the key type to Ed25519. The Ed25519 SSH key type enables a slightly higher performance benefit because it can give the same level of security as the RSA SSH key type with a smaller key. You can create virtual server instances and bare metal servers with a mix of RSA and Ed25519 SSH keys.

In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, you can go to **menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > SSH keys** to manage your SSH keys. From here you can create, rename, or delete keys. If you select to create a key, that key must be an RSA SSH key type. You can upload an Ed25519 SSH key type, you just can't generate one within VPC.

You can generate new RSA key pairs using the UI. Pre-existing RSA and Ed25519 SSH keys can be uploaded. Ed25519 can be used only if the operating system supports this key type. Ed25519 can't be used with Windows or VMware images. 
{: note}

You can generate new RSA key pairs using the UI. Pre-existing RSA and Ed25519 SSH keys can be uploaded. 

## SSH key types: RSA and Ed25519 in the CLI
{: #ssh-key-types-cli}
{: cli}

{{site.data.keyword.vpc_full}} supports two different types of public SSH keys.

* RSA
* Ed25519

On {{site.data.keyword.vpc_short}}, RSA is the default SSH key type. You can select to change the key type to Ed25519. The Ed25519 SSH key type enables a slightly higher performance benefit because it can give the same level of security as the RSA SSH key type with a smaller key. You can create virtual server instances and bare metal servers with a mix of RSA and Ed25519 SSH keys.

* For Windows or VMware images, you must use the RSA SSH key type. The Ed25519 SSH key type can't be used with Windows or VMware images.
* For Linux images, the Ed25519 SSH key type can be used only if the SSH server for the operating system supports that key type.

In the CLI, you can specify which type of key by using the `--key-type` option. You can't create SSH keys within the CLI, you can import only an existing SSH key. The default `--key-type` is RSA. If you try to import a Ed25519 SSH key and don't specify the `ed25519` key type, the process fails.

```sh
--key-type ed25519
```
{: pre}

## SSH key types: RSA and Ed25519 in the API
{: #ssh-key-types-api}
{: api}

{{site.data.keyword.vpc_full}} supports two different types of public SSH keys.

* RSA
* Ed25519

On {{site.data.keyword.vpc_short}}, RSA is the default SSH key type. You can select to change the key type to Ed25519. The Ed25519 SSH key type enables a slightly higher performance benefit because it can give the same level of security as the RSA SSH key type with a smaller key. You can create virtual server instances and bare metal servers with a mix of RSA and Ed25519 SSH keys.

* For Windows or VMware images, you must use the RSA SSH key type. The Ed25519 SSH key type can't be used with Windows or VMware images.
* For Linux images, the Ed25519 SSH key type can be used only if the SSH server for the operating system supports that key type.

You can't create SSH keys within the API, you can import only an existing SSH key. You can generate a new RSA SSH key pair within the UI. You have the option when you create an SSH key to copy the API code snippet for that key.

In the API, you can specify which type of key by using the `type` variable. The default `type` is RSA. If you try to import a Ed25519 SSH key and don't specify the `ed25519` key type, the process fails.

```sh
"type":"ed25519"
```
{: pre}


## SSH key types: RSA and Ed25519 in the Terraform
{: #ssh-key-types-terraform}
{: terraform}

{{site.data.keyword.vpc_full}} supports two different types of public SSH keys.

* RSA
* Ed25519

On {{site.data.keyword.vpc_short}}, RSA is the default SSH key type. You can select to change the key type to Ed25519. The Ed25519 SSH key type enables a slightly higher performance benefit because it can give the same level of security as the RSA SSH key type with a smaller key. You can create virtual server instances and bare metal servers with a mix of RSA and Ed25519 SSH keys.

* For Windows or VMware images, you must use the RSA SSH key type. The Ed25519 SSH key type can't be used with Windows or VMware images.
* For Linux images, the Ed25519 SSH key type can be used only if the SSH server for the operating system supports that key type.

You can't create SSH keys within Terraform, you can import only an existing SSH key. You can generate a new RSA SSH key pair within the UI.

In Terraform, you can specify which type of key by using the `type` variable. The default type is `rsa`. If you try to import the Ed25519 SSH key and don't specify the `ed25519` key type, the process fails.

```terraform
type = "ed25519"
```
{: pre}

## Before you begin
{: #ssh-key-prereqs}

You can generate an SSH key in {{site.data.keyword.vpc_full}} when you create the SSH key on the **SSH keys for VPC** page. However, if you choose to import an existing SSH key file, keep the following limitations in mind.

* Your SSH key must be an RSA or Ed25519 key type with a key size of either 2048 bits or 4096 bits.
* If your Mac system generates a key size of 3072 bits (by default), run one of the following commands to make sure that the generated key is a supported size.
   * For RSA SSH key type, issue:  `ssh-keygen -t rsa -b 4096 -C "user_ID"`
   * For Ed25519 SSH key type, issue:  `ssh-keygen -t ed25519 -b 4096 -C "user_ID"`
* SSH keys are generated as key pairs; one is a public key, and the other is a private key. Select the public key when you import an SSH key to your VPC. The corresponding private key remains on your local workstation and is not imported.
* When you copy an SSH key from a terminal to add the public key to your VPC, sometimes extra line breaks are introduced which cause a parsing error. To avoid this issue, first paste your public key into a text editor and remove any extra line breaks. Then, copy the public key from text editor and paste it into the {{site.data.keyword.vpc_short}} UI, CLI, or API.

### Locating your existing SSH key
{: #locate-ssh-key}

If you choose to import an existing SSH key file, look for a file that is called `id_rsa.pub` or `id_ed25519.pub` that contains your public key. The file might be in an `.ssh` directory under your home directory, for example, `/Users/<USERNAME>/.ssh/id_rsa.pub`. The content of the public key file for an RSA SSH key typically starts with `ssh-rsa`, whereas the contents of the public key file for an Ed25519 SSH key typcially starts with `ssh-ed25519`. The file extension, `.pub`, indicates which file contains the public key.

### Generating an external SSH key
{: #generating-ssh-keys}

If you want to generate an SSH key outside of {{site.data.keyword.vpc_short}}, run the `ssh-keygen` command and follow the prompts. For example, you can generate an RSA SSH key on your Linux or Mac system by running the command `ssh-keygen -t rsa -C "user_ID"`.

Press **Enter** to accept the default location for the file. The command generates two files. The generated public key is in the `<your key>.pub` file. For Windows&reg;, you can use [PuTTYgen](https://www.ssh.com/ssh/putty/windows/puttygen){: external} to generate an SSH key.

If you are using OpenSSH version 7.8 or higher and plan to access a Windows server, use the following command to generate the key in PEM format. `ssh-keygen -m PEM -t rsa -C "user_ID"`
{: important}

## Locating your existing SSH keys by using the UI
{: #locating-ssh-keys-ui}
{: ui}

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > SSH keys**.
2. Any existing SSH keys are displayed.

## Listing your existing SSH keys by using the CLI
{: #locating-ssh-keys-cli}
{: cli}

To view a list of all SSH keys by using the CLI, use the **ibmcloud is keys** command. Specify either the `RESOURCE_GROUP_ID` or `RESOURCE_GROUP_NAME` variable. If you want to list all your resource groups, specify `--all-resource-groups`.

```sh
ibmcloud is keys [--all-resource-groups]
```
{: pre}

For more information, see [ibmcloud is keys](/docs/vpc?topic=vpc-vpc-reference#keys-list) in the VPC CLI reference.

## Listing your existing SSH keys by using the API
{: #locating-ssh-keys-api}
{: api}

To list all SSH keys by using the API, use [List all keys](/apidocs/vpc/latest#list-keys).

```sh
curl -X GET "$vpc_api_endpoint/v1/keys?version=2023-03-30&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

## Listing your existing SSH keys by using Terraform
{: #locating-ssh-keys-terraform}
{: terraform}

To list all SSH keys by using Terraform, use [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_ssh_keys){: external}.

```terraform
data "ibm_is_ssh_keys" keys {}
```
{: pre}

## Creating your SSH key by using the UI
{: #generate-ssh-keys-ui}
{: ui}

Use the following steps to create an SSH key. You can create only an RSA SSH key. For Ed25519 SSH keys, you must upload the SSH key.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > SSH keys**.
1. Click **Create** and enter the information that is in Table 1.
   | Field | Value |
   | --- | --- |
   | Location | Locations are composed of regions (specific geographic areas) and zones (fault-tolerant data centers within a region). Select the location where you want to create your SSH key. |
   | Name  | A name is required for your SSH key. |
   | Resource group | Select a resource group for the SSH key. |
   | Tags | You can assign a user tag to the SSH key so that you can easily filter a list of SSH keys. For more information, see [Working with tags](/docs/account?topic=account-tag&interface=ui).|
   | Access management tags | Access management tags help you apply flexible access policies on specific resources. For more information, see the [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial) UI tutorial. |
   | SSH key type | The default value is `rsa`.You can generate new RSA key pairs using the UI. Pre-existing RSA and Ed25519 SSH keys can be uploaded. Ed25519 can be used only if the operating system supports this key type. Ed25519 can't be used with Windows or VMware images. |
   {: caption="Table 1. Creating an SSH key for VPC selections" caption-side="bottom"}

1. Select **Generate a key pair for me**.
1. Click **Save private key**.
1. Click **Save public key**.
1. Optionally, click **Get sample API call** to get an API code with all your SSH key information that you can copy.
1. Click **Create**.

Your SSH key is now displayed in the list of SSH keys on the UI.

## Importing an SSH key by using the UI
{: #import-ssh-keys-ui}
{: ui}

You can import an SSH key in two ways. You can upload a public key from a local file. Or you can paste in your public key into the UI.

Use the following steps to import an SSH key from a local file.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > SSH keys**.
1. Click **Create** and enter the information that is in Table 1.
   | Field | Value |
   | --- | --- |
   | Location | Locations are composed of regions (specific geographic areas) and zones (fault-tolerant data centers within a region). Select the location where you want to create your SSH key. |
   | Name  | A name is required for your SSH key. |
   | Resource group | Select a resource group for the SSH key. |
   | Tags | You can assign a user tag to the SSH key so that you can easily filter a list of SSH keys. For more information, see [Working with tags](/docs/account?topic=account-tag&interface=ui).|
   | Access management tags | Access management tags help you apply flexible access policies on specific resources. For more information, see the [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial) UI tutorial. |
   {: caption="Table 1. Creating an SSH key for VPC selections" caption-side="bottom"}

1. Select **Upload a public key**.
1. Click **Upload a public key file**.
1. Select the public key file and click **Open**. The file extension, `.pub`, typically indicates which file contains the public key.
1. Optionally, click **Get sample API call** to get an API code with all your SSH key information that you can copy.
1. Click **Create**.

Use the following steps to import an SSH key by pasting in the public key material into the UI.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > SSH keys**.
1. Click **Create** and enter the information that is in Table 1.
   | Field | Value |
   | --- | --- |
   | Location | Locations are composed of regions (specific geographic areas) and zones (fault-tolerant data centers within a region). Select the location where you want to create your SSH key. |
   | Name  | A name is required for your SSH key. |
   | Resource group | Select a resource group for the SSH key. |
   | Tags | You can assign a user tag to the SSH key so that you can easily filter a list of SSH keys. For more information, see [Working with tags](/docs/account?topic=account-tag&interface=ui).|
   | Access management tags | Access management tags help you apply flexible access policies on specific resources. For more information, see the [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial) UI tutorial. |
   | SSH key type | Select a key type for the SSH key. SSH key type `ed25519` can't be used for Windows&reg; instances. |
   {: caption="Table 1. Creating an SSH key for VPC selections" caption-side="bottom"}

1. Select **Paste public key material**.
1. Paste in the public key information.
1. Optionally, click **Get sample API call** to get an API code with all your SSH key information that you can copy.
1. Click **Create**.

When you copy an SSH key from a terminal to add the key to your VPC, sometimes extra line breaks are introduced which cause a parsing error. To avoid this issue, first paste your SSH key into a text editor and remove any extra line breaks. Then, copy the SSH key from text editor and paste it into the VPC UI, CLI, or API.
{: tip}

Your imported SSH key is now displayed in the list of SSH keys on the UI.

## Importing your SSH key by using the CLI
{: #generate-ssh-keys-cli}
{: cli}

To import an SSH key by using the CLI, use the **ibmcloud is key-create** command. You also must specify a `KEY_NAME` and the `KEY`. The SSH key file is either the `id_rsa.pub` or `id_ed25519.pub` depending on which key type you are importing. Optionally, you can specify the resource by using either the `RESOURCE_GROUP_ID` or `RESOURCE_GROUP_NAME` variable. See [ibmcloud is key-create](/docs/vpc?topic=vpc-vpc-reference#key-create) in the VPC CLI reference guide.

```sh
ibmcloud is key-create KEY_NAME (KEY | @KEY_FILE) [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME]
```
{: pre}

## Importing your SSH key by using the API
{: #import-ssh-keys-api}
{: api}

To import an SSH key by using the API, use the [Create a key](/apidocs/vpc/latest#create-key). You can't create an SSH key by using the API. However, you can create an SSH key by using the UI and from the UI, generate the API code snippet that you need that includes the new SSH key. If you use the UI, make sure you save the SSH key that you create in the UI before you generate the API code snippet.

For the `name` property, specify the name of the SSH key. For `public_key` property, enter in the public key information. For the `type` property, specify either `rsa` or `ed25519` for the SSH key type.

```sh
curl -X POST "$vpc_api_endpoint/v1/keys?version=2023-03-30&generation=2" -H "Authorization: Bearer $iam_token" -d '{
      "name":"my-key-1",
      "public_key":"AAAAB3NzaC1yc2EAAAADAQABAAABAQDDGe50Bxa5T5NDddrrtbx2Y4/VGbiCgXqnBsYToIUKoFSHTQl5IX3PasGnneKanhcLwWz5M5MoCRvhxTp66NKzIfAz7r+FX9rxgR+ZgcM253YAqOVeIpOU408simDZKriTlN8kYsXL7P34tsWuAJf4MgZtJAQxous/2byetpdCv8ddnT4X3ltOg9w+LqSCPYfNivqH00Eh7S1Ldz7I8aw5WOp5a+sQFP/RbwfpwHp+ny7DfeIOokcuI42tJkoBn7UsLTVpCSmXr2EDRlSWe/1M/iHNRBzaT3CK0+SwZWd2AEjePxSnWKNGIEUJDlUYp7hKhiQcgT5ZAnWU121oc5En",
      "type":"rsa"
    }'
```
{: pre}

## Importing your SSH key by using Terraform
{: #import-ssh-keys-terraform}
{: terraform}

Create a ssh-key resource or use an existing ssh-key by referring to the ssh-key data source. For more information, see the Terraform documentation on [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_ssh_keys){: external}

Create a ssh-key resource or use an existing ssh-key by referring to the ssh-key data source. For the `name` property, specify the name of the SSH key. For `public_key` property, enter in the public key information. For the `type` property, specify either `rsa` or `ed25519` for the SSH key type. The default value is `rsa`. You must specify `ed25519` to import an `ed25519` SSH key.

   ```terraform
   resource "ibm_is_ssh_key" "example_sshkey" {
      name       = "example-sshkey"
      public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
      type = "rsa"
   }
   ```
   {: codeblock}

## Next steps
{: #next-steps-ssh}

After you locate or generate an SSH key, it's time to plan for and create a server.

Virtual servers

* [Planning for virtual servers](/docs/vpc?topic=vpc-vsi_best_practices)
* [Creating a virtual server by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating a virtual server by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli)

Bare metal servers

* [Planning for Bare Metal Servers on VPC](/docs/vpc?topic=vpc-planning-for-bare-metal-servers)
* [Creating Bare Metal Servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=ui)

Troubleshooting SSH keys

* [How do I resolve an SSH key error?](/docs/vpc?topic=vpc-troubleshooting-your-virtual-servers-for-vpc#troubleshooting-ssh-keys-errors)
* [Why do I receive an SSH key permission denied error?](/docs/vpc?topic=vpc-troubleshooting-your-virtual-servers-for-vpc#troubleshooting-ssh-key-permission-denied-error)
