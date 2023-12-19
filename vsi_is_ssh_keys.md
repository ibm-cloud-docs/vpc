---

copyright:
  years: 2018, 2023
lastupdated: "2023-12-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing SSH keys
{: #managing-ssh-keys}

To access {{site.data.keyword.vsi_is_full}} instances, you must have an SSH key available to use. You can, create, update, and delete SSH keys in {{site.data.keyword.cloud_notm}} console, CLI, and API.
{: shortdesc}

Managing keys by using the {{site.data.keyword.cloud_notm}} console or CLI has no effect on keys in instances that are already created. (For an existing Linux instance, you can edit keys directly in the `~/.ssh/` directory of the instance.)

{{site.data.keyword.vpc_full}} supports two different types of public SSH keys.

* RSA
* Ed25519

You can generate new RSA key pairs using the UI. Pre-existing RSA and Ed25519 SSH keys can be uploaded. Ed25519 can be used only if the operating system supports this key type. Ed25519 can't be used with Windows or VMware images. 
{: note}

## Before you begin
{: #prereq-ssh-key-available}

To create a virtual server instance, you must create or upload an SSH key and have it available so that you can connect to your instance after it is provisioned.

## Managing SSH keys with IBM Cloud console
{: #managing-ssh-keys-with-ibm-cloud-console}
{: ui}

When you provision a virtual server, you can create a new SSH key, select from an available list of existing SSH keys or upload a new one.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > SSH keys**. Any existing SSH keys are displayed.
1. On the **SSH keys for VPC** tab, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for an SSH key and select from the available options.
   | Action            | Description            |
   | ----------------- | --------------------------- |
   | Rename | After you update an existing SSH key, the key is renamed immediately. |
   | Delete | After you remove an SSH key, it can no longer be used when you provision an instance or when you perform an OS reload on an existing instance. However, the key is still available on any instances that you previously provisioned with it, and you can use it to log in. |
   {: caption="Table 1. SSH key actions" caption-side="bottom"}

   You are shown a list of the available regions for your specific resource group.
   {: note}

## Managing SSH keys by using the CLI
{: #managing-ssh-keys-by-using-the-cli}
{: cli}

You can also manage your SSH keys by using the CLI.

Make sure that the {{site.data.keyword.cloud_notm}} CLI `vpc-infrastructure` plug-in is installed. For more information, see [IBM Cloud CLI for VPC](/docs/vpc?topic=vpc-set-up-environment).
{: tip}

| Action            | Command                     | What happens next |
| ----------------- | --------------------------- | ----------------- |
| Create SSH key.   | [`ibmcloud is key-create`](/docs/vpc?topic=vpc-vpc-reference&interface=cli#key-create) | After you create an SSH key, it is added to the list of keys. |
| View key details. | [`ibmcloud is key`](/docs/vpc?topic=vpc-vpc-reference&interface=cli#key-view) | You can view the name of the key and the ID of the key. |
| List keys.        | [`ibmcloud is keys`](/docs/vpc?topic=vpc-vpc-reference&interface=cli#keys-list) | You can view all of your existing SSH keys. |
| Update key.       | [`ibmcloud is key-update`](/docs/vpc?topic=vpc-vpc-reference&interface=cli#key-update) | After you update an existing key, the key is renamed immediately. |
| Delete key.       | [`ibmcloud is key-delete`](/docs/vpc?topic=vpc-vpc-reference&interface=cli#key-delete) | After you remove an SSH key, it can no longer be used when you provision an instance or when you perform an OS reload on an existing instance. However, the key is still available on any instances that you previously provisioned with it, and you can use it to log in. |
{: caption="Table 1. SSH key actions" caption-side="bottom"}

## Managing SSH keys by using the API
{: #managing-ssh-keys-by-using-the-api}
{: api}

You can also manage your SSH keys by using the API. For more information about the `$vpc_api_endpoint` and `$iam_token` variables in the following examples, see the Authentication and Endpoint URLs sections in [Virtual Private Cloud API Introduction](/apidocs/vpc/latest#about-vpc-api).

## Managing SSH keys by using Terraform
{: #managing-ssh-keys-by-using-terraform}
{: terraform}

You can also manage your SSH keys by using Terraform. See [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_ssh_keys){: external} for more information.

## Listing all your SSH keys by using the UI
{: #viewing-all-ssh-keys-ui}
{: ui}

To list all your SSH keys, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > SSH keys**.
2. Any existing SSH keys are displayed.

## Listing all your SSH keys by using the CLI
{: #viewing-all-ssh-keys-cli}
{: cli}

To view all of your existing SSH keys, use the **ibmcloud is keys** command.

Use the `--all-resource-groups` option to list the SSH keys for all available resource groups. Optionally, you can filter the list to include only SSH keys for a specific resource group. Specify the resource group by using the `RESOURCE_GROUP_ID` or `RESOURCE_GROUP_NAME` variable. For more information, see [ibmcloud is keys](/docs/vpc?topic=vpc-vpc-reference#keys-list).

```sh
ibmcloud is keys [--all-resource-groups]
```
{: pre}

## Listing all your SSH keys by using the API
{: #viewing-all-ssh-keys-api}
{: api}

To list all SSH keys by using the API, use [List all keys](/apidocs/vpc/latest#list-keys).

```sh
curl -X GET "$vpc_api_endpoint/v1/keys?version=2023-03-30&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

## Listing all your SSH keys by using Terraform
{: #viewing-all-ssh-keys-terraform}
{: terraform}

To list all SSH keys by using Terraform, use [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_ssh_keys){: external}.

```terraform
data "ibm_is_ssh_keys" keys {}
```
{: pre}

## Viewing the details of your SSH key by using the UI
{: #viewing-ssh-keys-ui}
{: ui}

You can view the following details of an SSH key.

* Name
* Resource group
* Fingerprint
* Type
* Length
* Created date (Local)

To view details for an SSH key, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > SSH keys**.
1. On the **SSH keys for VPC** page, a list of all existing SSH keys is displayed.
1. From the **Actions** menu, you can **Rename** or **Delete** the SSH key.

## Viewing the details of your SSH key by using the CLI
{: #viewing-ssh-keys-cli}
{: cli}

You can view the name of the key and the ID of the key by using the **ibmcloud is key** command. Specify the name of the SSH key by using the `KEY` variable. For more information, see [ibmcloud is key](/docs/vpc?topic=vpc-vpc-reference#key-view) in the VPC CLI reference guide.

```sh
ibmcloud is key KEY
```
{: pre}

## Viewing details of your SSH key by using the API
{: #viewing-ssh-keys-api}
{: api}

To retrieve information for a specific key by using the API, use [Retrieve a key](/apidocs/vpc/latest#get-key).

For the `$id` variable, specify the name of the SSH key for which you want to display details.

```sh
curl -X GET "$vpc_api_endpoint/v1/keys/$id?version=2023-03-30&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

## Viewing details of your SSH key by using Terraform
{: #viewing-ssh-keys-terraform}
{: terraform}

To retrieve information for a specific key by using Terraform, use [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_ssh_key){: external}.

For the `name` variable, specify the name of the SSH key for which you want to display details.

```terraform
data "ibm_is_ssh_key" "example" {
  name = "example-ssh-key"
}
```
{: pre}

## Creating an SSH key by using the UI
{: #generating-ssh-keys-ui}
{: ui}

Use the following steps to create a new SSH key. You can create only RSH SSH key types. To use an Ed25519 SSH key, that SSH key must be imported. For more information, see [Importing an SSH key by using the UI](#importing-ssh-keys-ui).

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > SSH keys**.
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

Your new SSH key is now displayed in the list of SSH keys on the UI.

## Importing an SSH key by using the UI
{: #importing-ssh-keys-ui}
{: ui}

You can import an SSH key in two ways. You can upload a public key from a local file. Or you can copy and paste your public key information into the UI.

Use the following steps to import an SSH key from a local file.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > SSH keys**.
1. Click **Create** and enter the information that is in Table 1.
   | Field | Value |
   | --- | --- |
   | Location | Locations are composed of regions (specific geographic areas) and zones (fault-tolerant data centers within a region). Select the location where you want to create your SSH key. |
   | Name  | A name is required for your SSH key. |
   | Resource group | Select a resource group for the SSH key. |
   | Tags | You can assign a user tag to the SSH key so that you can easily filter a list of SSH keys. For more information, see [Working with tags](/docs/account?topic=account-tag&interface=ui).|
   | Access management tags | Access management tags help you apply flexible access policies on specific resources. For more information, see the [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial) UI tutorial. |
   | SSH key type | Select a key type for the SSH key. The default value is `rsa`. The `ed25519` SSH key type can be used only to create instances if the operating system supports this key type. This key type can't be used with Windows or VMware images. |
   {: caption="Table 1. Creating an SSH key for VPC selections" caption-side="bottom"}

1. Select **Upload a public key**.
1. Click **Upload a public key file**.
1. Select the public key file and click **Open**. The file extension, `.pub`, typically indicates which file contains the public key.
1. Optionally, click **Get sample API call** to get an API code with all your SSH key information that you can copy.
1. Click **Create**.

Use the following steps to import an SSH key by pasting in the public key material into the UI.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > SSH keys**.
1. Click **Create** and enter the information that is in Table 1.
   | Field | Value |
   | --- | --- |
   | Location | Locations are composed of regions (specific geographic areas) and zones (fault-tolerant data centers within a region). Select the location where you want to create your SSH key. |
   | Name  | A name is required for your SSH key. |
   | Resource group | Select a resource group for the SSH key. |
   | Tags | You can assign a user tag to the SSH key so that you can easily filter a list of SSH keys. For more information, see [Working with tags](/docs/account?topic=account-tag&interface=ui).|
   | Access management tags | Access management tags help you apply flexible access policies on specific resources. For more information, see the [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial) UI tutorial. |
   | SSH key type | Select a key type for the SSH key. The default value is `rsa`. The `ed25519` SSH key type can be used only to create instances if the operating system supports this key type. This key type can't be used with Windows or VMware images.|
   {: caption="Table 1. Creating an SSH key for VPC selections" caption-side="bottom"}

1. Select **Paste public key material**.
1. Paste in the public key information.
1. Optionally, click **Get sample API call** to get an API code with all your SSH key information that you can copy.
1. Click **Create**.

When you copy an SSH key from a terminal to add the key to your VPC, sometimes extra line breaks are introduced which cause a parsing error. To avoid this issue, first paste your SSH key into a text editor and remove any extra line breaks. Then, copy the SSH key from text editor and paste it into the VPC UI, CLI, or API.
{: tip}

Your imported SSH key is now displayed in the list of SSH keys on the UI.

## Importing your SSH key by using the CLI
{: #generating-ssh-keys-cli}
{: cli}

To import an SSH key by using the CLI, use the **ibmcloud is key-create** command. The file that you import is `id_rsa.pub` or `id_ed25519.pub`, which contains your public key. You must specify the name or ID of the SSH key by using the `KEY_NAME` variable and the public SSH key you are importing by using the `KEY` variable. Specify the SSH key type with the `--key-type` option. Optionally, you can specify a resource group when you create the SSH key. Specify the resource group by using either the `RESOURCE_GROUP_ID` or `RESOURCE_GROUP_NAME` variable. See [ibmcloud is key-create](/docs/vpc?topic=vpc-vpc-reference#key-create) in the VPC CLI reference guide.

```sh
ibmcloud is key-create KEY_NAME (KEY | @KEY_FILE) [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--key-type KEY_TYPE]
```
{: pre}

## Importing your SSH key by using the API
{: #importing-ssh-keys-api}
{: api}

To import a new SSH key by using the API, use the [Create a key](/apidocs/vpc/latest#create-key). The file that you import is `id_rsa.pub` or `id_ed25519.pub`, which contains your public key. You can't create a brand-new SSH key by using the API. However, you can create a new SSH key by using the UI and from the UI, generate the API code snippet that you need that includes the new SSH key. For more information, see [Creating an SSH key by using the UI](#generating-ssh-keys-ui).

For the `name` property, specify the name of the SSH key. For `public_key` property, enter in the public key information. For the `type` property, specify either `rsa` or `ed25519` for the SSH key type.

The Ed25519 SSH key type can be used only to create instances if the operating system supports this key type. This key type can't be used with Windows or VMware images.
{: important}

```sh
curl -X POST "$vpc_api_endpoint/v1/keys?version=2023-03-30&generation=2" -H "Authorization: Bearer $iam_token" -d '{
      "name":"my-key-1",
      "public_key":"AAAAB3NzaC1yc2EAAAADAQABAAABAQDDGe50Bxa5T5NDddrrtbx2Y4/VGbiCgXqnBsYToIUKoFSHTQl5IX3PasGnneKanhcLwWz5M5MoCRvhxTp66NKzIfAz7r+FX9rxgR+ZgcM253YAqOVeIpOU408simDZKriTlN8kYsXL7P34tsWuAJf4MgZtJAQxous/2byetpdCv8ddnT4X3ltOg9w+LqSCPYfNivqH00Eh7S1Ldz7I8aw5WOp5a+sQFP/RbwfpwHp+ny7DfeIOokcuI42tJkoBn7UsLTVpCSmXr2EDRlSWe/1M/iHNRBzaT3CK0+SwZWd2AEjePxSnWKNGIEUJDlUYp7hKhiQcgT5ZAnWU121oc5En",
      "type":"rsa"
    }'
```
{: pre}

## Importing your SSH key by using Terraform
{: #importing-ssh-keys-terraform}
{: terraform}

To import a new SSH key by using Terraform, use [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_ssh_key#import){: external}.

To import a new SSH key, complete the following steps.

1. Create a resource block type of `ibm_is_ssh_key` with the required values. For the `name` attribute, specify the dummy name of the SSH key. For `public_key` attribute, enter in the dummy public key information.

   ```terraform
   resource "ibm_is_ssh_key" "example_sshkey" { 
     name = "my-key"
     public_key = "public-key"
   }
   ```
   {: pre}

1. After creating the resource, run the following Terraform command to import the SSH key.

  ```terraform
   terraform import ibm_is_ssh_key.example_sshkey d7bec597-4726-451f-8a63-e62e6f19c32c
   ```
   {: pre}

1. Improve the configuration to match the state. Copy over the resource block details to avoid replacing the resource block.

   ```terraform
   resource "ibm_is_ssh_key" "example_sshkey" {
      name       = "example-sshkey"
      public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
      type = "rsa"
   }
   ```
   {: codeblock}


## Updating your SSH key by using the CLI
{: #updating-ssh-keys-cli}
{: cli}

You can update an existing key by using the **ibmcloud is key-update** command. After you update an existing key, the key is renamed immediately. For more information, see [ibmcloud is key-delete](/docs/vpc?topic=vpc-vpc-reference#key-delete). You must specify both the current SSH key name by using the `KEY` variable and the new SSH key name by using the `NEW_NAME` variable in the `--name` option.

```sh
ibmcloud is key-update KEY [--name NEW_NAME]
```
{: pre}

## Updating your SSH key by using the API
{: #updating-ssh-keys-api}
{: api}

To update an existing SSH key by using the API, use [Update a key](/apidocs/vpc/latest#update-key). After you update an existing key, the key is renamed immediately.

For the `$id` variable, specify the current name of the SSH key. For the `name` property, specify the new name for the SSH key.

```sh
curl -X PATCH "$vpc_api_endpoint/v1/keys/$id?version=2023-03-30&generation=2" -H "Authorization: Bearer $iam_token" -d '{ "name": "my-key-1-updated" }'
```
{: pre}

## Updating your SSH key by using Terraform
{: #updating-ssh-keys-terraform}
{: terraform}

To update an existing SSH key by using the Terraform, use [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_ssh_key){: external}. After you update an existing key, the key is renamed immediately.


1. Update the SSH key resource block. For the `name` property, specify the new name for the SSH key.

   ```terraform
      resource "ibm_is_ssh_key" "example_sshkey" {
      name       = "new-example-sshkey"
      public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCKVmnMOlHKcZK8tpt3MP1lqOLAcqcJzhsvJcjscgVERRN7/9484SOBJ3HSKxxNG5JN8owAjy5f9yYwcUg+JaUVuytn5Pv3aeYROHGGg+5G346xaq3DAwX6Y5ykr2fvjObgncQBnuU5KHWCECO/4h8uWuwh/kfniXPVjFToc+gnkqA+3RKpAecZhFXwfalQ9mMuYGFxn+fwn8cYEApsJbsEmb0iJwPiZ5hjFC8wREuiTlhPHDgkBLOiycd20op2nXzDbHfCHInquEe/gYxEitALONxm0swBOwJZwlTDOB7C6y2dzlrtxr1L59m7pCkWI4EtTRLvleehBoj3u7jB4usR"
      type = "rsa"
   }
    ```
   {: codeblock}

1. Run `terraform apply` to update the resource with the new name. 

## Deleting your SSH key by using the CLI
{: #deleting-ssh-keys-cli}
{: cli}

To delete one or more SSH keys by using the CLI, use the **ibmcloud is key-delete** command. For more information, see [ibmcloud is key-delete](/docs/vpc?topic=vpc-vpc-reference#key-delete). Specify name of each SSH key that you delete by using the `KEY` variable.

```sh
ibmcloud is key-delete (KEY1 KEY2 ...)
```
{: pre}

## Deleting your SSH key by using the API
{: #deleting-ssh-keys-api}
{: api}

To delete one or more SSH keys by using the API, use [Delete a key](/apidocs/vpc/latest#delete-key).

For the `$id` variable, specify the name of the SSH key you want to delete.

```sh
curl -X DELETE "$vpc_api_endpoint/v1/keys/$id?version=2023-03-30&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

## Deleting your SSH key by using Terraform
{: #deleting-ssh-keys-terraform}
{: terraform}

To delete your SSH key by using the Terraform, use [ibm_is_ssh_keys](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_ssh_key){: external}.

For the `example_sshkey` attribute, replace this with the SSH key you want to delete.

   ```terraform
   terraform destroy --target ibm_is_ssh_keys.example_sshkey
   ```
   {: pre}
