---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-12"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating a network ACL
{: #acl-create-ui}

You can configure an ACL to limit inbound and outbound traffic to the subnet. By default, all traffic is allowed.
{: shortdesc}

Each subnet can be attached to only one ACL. However, each ACL can be attached to multiple subnets.
{: important}

Before you begin, ensure that you have created a VPC and subnet.

## Creating a network ACL in the console
{: #configuring-the-acl}
{: ui}

To configure an ACL in the {{site.data.keyword.cloud_notm}} console, follow these steps:

1. Go to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Access control lists**.
1. Click **Create +**.
1. Select the **Edit location** icon ![Edit location icon](../icons/edit-tagging.svg "Edit location") and enter values for the following fields:

   * **Geography** - Select a continent for your access control list.
   * **Region** - Select a region for your access control list.

1. Enter values for the following fields under details:

   * **Name** - Type a unique name for your access control list.
   * **Resource group** -  Select a resource group for your access control list. You can use the default group for this access control list, or select from the resource group list (if defined). For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).

   After provisioning is complete, you cannot change the resource group.
   {: important}

   * **Tags** - Add user tags. User tags are visible account-wide. For more information, see [Working with tags](/docs/account?topic=account-tag).
   * **Access management tags** - Add access management tags to help organize access control relationships. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Virtual private cloud** - Select a VPC. You can use the default VPC for this ACL, or select from the list (if defined). For more information, see [Getting started with Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started&interface=ui).

1. Under Rules, click **Create +** to configure inbound and outbound rules that define what traffic is allowed in or out of the subnet. For each rule, specify the following information:
      * Select whether to allow or deny the specified traffic.
      * Select the protocol to which the rule applies. For more information about using ANY IPv4 protocols in your ACL rules, see [Understanding internet communication protocols](/docs/vpc?topic=vpc-understanding-icp#understanding-icp).
      * For the source and destination of the rule, specify the IP range and ports for which the rule applies. For example, if you want all inbound traffic to be allowed to the IP range `192.168.0.0/24` in your subnet, specify **Any** as the source and `192.168.0.0/24` as the destination. However, if you want to allow inbound traffic only from `169.168.0.0/24` to your entire subnet, specify `169.168.0.0/24` as the source and **Any** as the destination for the rule.
      * Specify the rule's priority. Rules with lower numbers are evaluated first and override rules with higher numbers. For example, if a rule with priority `2` allows HTTP traffic and a rule with priority `5` denies all traffic, HTTP traffic is still allowed.
      * Click **Create**.
      * Select a subnet to attach to this ACL. Click **Attach** to attach additional subnets.

      If the subnet has an existing ACL connection, the ACL is replaced by the ACL being created.
      {: note}

1. View your Total estimated cost in the Summary menu in the lower right of the page.

## Creating a network ACL from the CLI
{: #cr-using-the-cli-acl}
{: cli}

Before you begin, [set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).

To create a network ACL from the CLI, run the following command:
{: shortdesc}

```sh
ibmcloud is network-acl-create ACL_NAME VPC \
[--rules (RULES_JSON|@RULES_JSON_FILE) | --source-acl-id SOURCE_ACL_ID] \
[--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] \
[--output JSON] [-q, --quiet]
```
{: codeblock}

Where:

`ACL_NAME` 
:   Name of the network ACL.
`VPC` 
:   ID of the VPC.
`--rules`
:   Rules for the ACL in JSON or JSON file.
`--source-acl-id`
:   ID of the network ACL to copy rules from.
`--resource-group-id`
:   ID of the resource group. This option is mutually exclusive with `--resource-group-name`.
`--resource-group-name` 
:   Name of the resource group. This option is mutually exclusive with `--resource-group-id`.
`--output`
:   Output in JSON format.
`-q, --quiet`
:   Suppresses verbose output.



For example:

- `ibmcloud is network-acl-create my-acl 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479`
- `ibmcloud is network-acl-create my-acl 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --source-acl-id 72b27b5c-f4b0-48bb-b954-5becc7c1dcb3`



## Creating a network ACL with the API
{: #cr-using-the-api-acl}
{: api}

To create a network ACL with the API, follow these steps:
{: shortdesc}

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the `VpcId` value in a variable to be used in the API command:

    ```sh
    export VpcId=<your_vpc_id>
    ```
    {: pre}

1. Create a network ACL:

   ```sh
   curl -X POST -sH "Authorization:${iam_token}" \
   "$vpc_api_endpoint/v1/network_acls?version=$api_version&generation=2" \
   -d '{"name": "testacl", "vpc":{"id": "'$VpcId'"},"resource_group": {"id": "'$ResourceGroupId'"}}' | jq
   ```
   {: pre}

## Example: Configuring rules
{: #example-configuring-rules}

For example, you can configure the following inbound rules:

* Allow HTTP traffic from the internet.
* Allow all inbound traffic from the subnet `10.10.20.0/24`.
* Deny all other inbound traffic.

| Priority | Allow/Deny | Protocol | Source | Destination |
|--------------|-----------|------|------|------|
| 1 | Allow | TCP | Any IP, ports 80 - 80 |Any IP, any port|
| 2 | Allow | ANY | `10.10.20.0/24`, any port |Any IP, any port|
| 3 | Deny| ANY | Any IP, any port |Any IP, any port|
{: caption="Information for configuring inbound rules." caption-side="bottom"}

Then, configure the following outbound rules:

* Allow HTTP traffic to the internet.
* Allow all outbound traffic to the subnet `10.10.20.0/24`.
* Deny all other outbound traffic.

| Priority | Allow/Deny | Protocol | Source | Destination |
|--------------|-----------|------|------|------|
| 1 | Allow | TCP | Any IP, any port |Any IP, ports 80 - 80  |
| 2 | Allow | ANY | Any IP, any port | `10.10.20.0/24`, any port |
| 3 | Deny| ANY | Any IP, any port |Any IP, any port|
{: caption="Information for configuring outbound rules." caption-side="bottom"}
