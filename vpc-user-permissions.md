---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-19"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, team, scenario, manage, create, IAM

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Granting user permissions for VPC resources
{: #managing-user-permissions-for-vpc-resources}

{{site.data.keyword.vpc_full}} uses role-based access control that enables account administrators to control their users' access to VPC resources. Access can be assigned to individual users or to groups of users by using {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM).
{: shortdesc}

For more information about IAM access policies and IAM roles and actions, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started).
{: note}

This document shows examples of how the account administrator can use the {{site.data.keyword.cloud_notm}} console to grant the correct permissions for managing VPC infrastructure resources. It covers the following scenarios:

* **Full-access scenario:** Assign an access policy so a new user can create and use all VPC infrastructure resources (including VPCs).
* **Limited access scenario:** Assign an access policy so an existing user can create and use only virtual server instances.
* **Team access scenario:** Set up resource groups and access groups to allow two separate teams to create and use the VPC resources that are assigned to their team.

You can also manage permissions through the CLI or API. For more information, see [How do I use IBM Cloud IAM](/docs/account?topic=account-iamoverview#howto).
{: tip}

## Full-access scenario
{: #inviting-new-user-to-create-or-manage-vpc-resources}

This scenario shows how to invite a new IBM Cloud user to your account and give them access to VPC infrastructure so they can view, create, and update all VPC resources in the Default resource group.

To give a new user access to all VPC infrastructure resources:

1. Go to the [IAM Users](/iam/users){: external} page in the IBM Cloud console and click **Invite users**.
1. Enter the email addresses of the users that you want to invite in the **Enter Email addresses** section.
1. In the **Assign users additional access** section, select **IAM services** and complete the following tasks:
    * From the **What type of access do you want to assign?** list, select **VPC Infrastructure   Services**.
    * From the **Resource type** list, select **All resource types**.
    * In the **Platform access** area, select **Editor**.
    * In the **Resource group access** area, select **Viewer**.
    * In the **Service access** area, select **Console Administrator**.
    * Scroll to the end of the page and click **Add**.
    * In the **Access summary** side panel, review the details and click **Invite**.

## Limited access scenario
{: #giving-existing-user-permission-to-create-vsis}

This scenario shows how to give an existing user permission to create and manage only virtual server instances in the Default resource group. Before the user can create an instance and associate a floating IP, the user also needs access to related resources, such as the VPC and subnet in which the instance will be created.

1. Go to the [IAM Users](/iam/users){: external} page in the IBM Cloud console and select the user whose access you want to configure.
1. On the **Access policies** tab, click **Assign access**.
1. In the **Assign users additional access** section, select **IAM services** and complete the following tasks:
    * From the **What type of access do you want to assign?** list, select **VPC Infrastructure Services**.
    * From the **in** list, select **Resource group: default**.
    * From the **Resource type** list, select **All resource types**.
    * In the **Platform access** area, select **Editor**.
    * Make sure that the **Resource group access** option is set to **Viewer**.
    * In the **Service access** area, select **Console Administrator**.
    * Scroll to the end of the page and click **Add**.
    * Review the **Access summary** side pane, and click **Assign**.

## Team access scenario
{: #team-access-scenario}

This scenario shows how an account administrator can assign authorization so that different teams have access to separate VPC resources. The example uses _resource groups_ to set up separate resource access for two teams. For the purposes of this example, resources are not shared across teams.

The example takes you through the process of creating resource groups, creating access groups, and assigning the appropriate policies to provide your teams with access to separate VPC resources.

In this scenario, you're setting up two different project teams to use separate VPCs. You'll assign access so that each team has access to their team's VPC resources only.

* Your first team is a test team. You've decided to assign them access to VPCs in a resource group named `test_vpcs`.
* The second team is your production team. They'll be assigned access to VPCs in a resource group named `production_vpcs`.

This strategy can be used to assign separate VPC resources to any number of teams. However, all resources share the same VPC quotas for the account. For more information about quotas and limits, see [VPC quotas](/docs/vpc?topic=vpc-quotas#quotas).
{: tip}

### Step 1: Create resource groups
{: #step-1-create-resource-groups}

Create resource groups that contain each of your teams' VPC resources.

1. Create a resource group called `test_team`.
2. Create a resource group called `production_team`.

For more information about how to create resource groups, see [Managing resource groups](/docs/account?topic=account-rgs).

By default, account administrators can create new resource groups. Other users must be assigned the **Editor** role for **All Account Management Services**, which allows them to create resource groups.
{: tip}

### Step 2: Create access groups
{: #step-2-create-access-groups}

Resource access can be assigned to groups of users. Groups of users with the same access permissions are called _access groups_. In this scenario, the account administrator creates an access group to represent each grouping of team members who require a specific type of VPC access, a total of four unique access groups.

Create four access groups with the following names, and assign the appropriate users to each access group:

* `test_team_manage_vpcs`
* `test_team_view_vpcs`
* `production_team_manage_vpcs`
* `production_team_view_vpcs`

For more information about how to create access groups and assign users to the access groups, see [Create access groups](/docs/account?topic=account-groups&interface=ui#create_ag).

### Step 3: Add IAM policies to the access groups
{: #step-3-add-iam-policies-to-the-access-groups}

Add the necessary VPC access policies for each access group. For example, add a policy so members of the `test_team_manage_vpcs` access group can create, update, and delete all VPC resources in the `test_team` resource group.

1. Go to the [IAM Group UI](/iam/groups){: external} in the IBM Cloud console.
1. Select an access group. Let's start with the `test_team_manage_vpcs` access group.
1. On the **Access policies** tab, click **Assign access**.
1. In the **Assign access group additional access** section, select **IAM services**
1. From the **What type of access do you want to assign?** list, select **VPC Infrastructure Services**.
1. From the **in** list, select **Resource group: test_team**.
1. From the **Resource type** list, select **All resource types**.
1. In the **Platform access** area, select **Editor**.
1. In the **Resource group access** area, select **Viewer**.
1. In the **Service access** area, select **Console Administrator**.
1. Scroll to the end of the page and click **Add**.
1. In the **Access summary** side panel, review the details and click **Assign**.

Because floating IP resources and the boot volume that's automatically attached to an instance are created in the Default resource group, you must also add access policies for the Default resource group.

| Access group | Resource group | Resource type | Platform access role | Service access role |
| -------------- | -------------- | -------------- | -------------- | -------------- |
|test_team_manage_vpcs|Default|Block Storage for VPC| Editor|   |
|test_team_manage_vpcs|Default|Floating IP for VPC| Editor|   |
{: caption="Access policies for the default resource group" caption-side="bottom"}

Repeat the previous steps to add access policies for the remaining three access groups.

| Access group | Resource group |  Resource type | Platform access role|  Service access role |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| test_team_view_vpcs | test_team | All resource types | Viewer |   |
| test_team_view_vpcs | Default | Block Storage for VPC| Viewer|   |
| test_team_view_vpcs | Default | Floating IP for VPC| Viewer|   |
| production_team_manage_vpcs | production_team| All resource types| Editor| Console Administrator |
| production_team_manage_vpcs | Default | Block Storage for VPC | Editor |   |
| production_team_manage_vpcs | Default | Floating IP for VPC | Editor |    |
| production_team_view_vpcs | production_team | All resource types | Viewer |   |
| production_team_view_vpcs | Default | Block Storage for VPC | Viewer |   |
| production_team_view_vpcs | Default | Floating IP for VPC | Viewer |   |
{: caption="Access policies for the remaining access groups" caption-side="bottom"}

The teams are now set up to use VPCs. Members of the `test_team_manage_vpcs` and `production_team_manage_vpcs` access groups can now create VPCs in their assigned resource groups (that is, in the `test_team` and `production_team` resource groups).

When you create a VPC or other resources, make sure that you specify the resource group in which to create the resource. If you don't specify a resource group, the resource is created in the Default resource group.
{: tip}

## Viewing user's permissions
{: #viewing-user-permissions}

Policies can be viewed in the user's **Access policies** tab.

You can use the following CLI commands to validate the resource group permissions that are assigned to your user, by policy or by access group:

### Validate By policy
{: #validate-by-policy}

```sh
ibmcloud iam user-policies <username>
```
{: pre}

### Validate by access group
{: #validate-by-access-group}

```sh
ibmcloud iam access-groups -u <username>
```
{: pre}

Changes to IAM access policies for VPC can take up to 10 minutes to take effect.
{: note}

## Related links
{: #permissions-related-links}

* [Managing identity and access](/docs/vpc?topic=vpc-iam-getting-started)
* [Managing users and access](/docs/account?topic=account-iamuserinv)
* [What is IAM](/docs/account?topic=account-iamoverview)
