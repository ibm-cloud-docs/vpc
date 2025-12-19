---

copyright:
  years: 2020, 2025
lastupdated: "2025-12-19"

keywords: resource attribute, iam access policy, terraform, cli

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC resource attributes
{: #resource-attributes}

When you use Terraform or the {{site.data.keyword.cloud}} CLI to create, update, or delete {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) access policies, you can specify the target VPC resource by using resource attributes.
{: shortdesc}

Resource attributes are in the form of `name=value,name=value...`.

You can select a resource object by entering the ID of the object. Or, you can enter the wildcard `*` in `value` to denote all applicable objects. For example, the attribute `vpcId:*` set the access policy to be applicable to all the VPCs in the account. You can also specify which resource group the policy is applied to in the command.

The following example CLI command gives the user `name@example.com` `Viewer` role for all the VPCs in the current account:

```sh
ibmcloud iam user-policy-create name@example.com --roles Viewer --service-name is --attributes "vpcId=*"
```
{: pre}

For more information about using the CLI to create and modify IAM access policy, see [ibmcloud iam user-policy-create](/docs/account?topic=account-ibmcloud_commands_iam#ibmcloud_iam_user_policy_create).

For more information about using Terraform to create IAM access policies, see the `resources` attribute for the following IAM policies:

* [`ibm_iam-access_group_policy`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_access_group_policy){: external}
* [`ibm_iam_service_policy`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_service_policy){: external}
* [`ibm_iam_user_policy`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_user_policy){: external}
* [`ibm_iam_user_invite`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_user_invite){: external}
* (`iam_policy.resource.attributes`)

See Table 1 for the full list of VPC resource attributes.

{{site.data.content.vpc-resource-attributes-table}}
