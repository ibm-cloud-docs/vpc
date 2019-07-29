---

copyright:
  years: 2019

lastupdated: "2019-07-29"

keywords: resource, access, role, role-based, authorization, policy, access group, resource group, permission, assign, administrator, operator, editor, viewer, user, team, scenario, manage, create, IAM

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Managing user permissions for VPC resources
{: #managing-user-permissions-for-vpc-resources}

{{site.data.keyword.vpc_full}} uses role-based access control that enables account administrators to control their users' access to VPC resources.
{:shortdesc}

This document shows you how the administrator of the account can add users to the account and give them the correct permissions to manage VPC infrastructure resources. It covers two common scenarios for a VPC administrator:

* **Simple access scenario:** Shows how to assign an access policy to a user, so the user can create and use VPC infrastructure resources (including VPCs).

* **Team access scenario:** Shows how to set up resource groups and access policies that allow two separate teams to create and use the VPC resources assigned to their team.

Changes to IAM access policies for VPC may take up to 10 minutes to take effect.
{: tip}

## Simple access scenario
{: #simple-access-scenario}

This scenario covers the basic steps needed to set up an individual user. We will cover two cases, inviting a new user to the account and modifying an existing user's permissions.

### Inviting a new user to create or manage VPC resources
{: #inviting-a-new-user-to-create-or-manage-vpc-resources}

Invite an IBM Cloud user to your account and give them access to `VPC Infrastructure` so they can have access to view, create, and update VPC resources. This section gives a quick overview of the IAM steps. Further information is available through the links in the **Related Links** section near the end of this document.

To invite users to VPC services and resources:

1. Navigate to the [IAM Users ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam#/users){: new_window} page in the IBM Cloud console and click **Invite users**.
3. In the **Users** section of the Invite users page, enter the email addresses of the users that you want to invite in the **Email address** field.
4. In the **Access** section, expand the **Services** area and complete the following tasks:
  * From the **Assign access to** list, select **Resource group**.
  * From the **Resource group** list, select the account's default resource group.
  * Make sure the **Assign access to a resource group** option is set to **Viewer**. 
  * From the **Services** list, select **VPC Infrastructure**.
  * From the **Resource type** list, select a particular VPC resource or all resource types.
  * Select the platform access role to assign to the users: **Administrator**, **Editor**, **Operator**, or **Viewer**.
  * Scroll to the end of the page and click **Invite users**.


### Giving an existing user permission to manage VPC resources
{: #giving-an-existing-user-permission-to-manage-vpc-resources}

This scenario covers the basic steps needed to give an existing user in your account permission to edit all VPC resources.

In the steps that follow, you'll create two IAM policies. Both policies are needed before your user can create and use VPC infrastructure resources. All of the resources must reside within the account's default resource group.

1. Navigate to the [IAM Users ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam#/users){: new_window} page in the IBM Cloud console and select the user whose authorization you're enabling.
3. On the **Access policies** tab, select **Assign access**.
4. Click **Assign access within a resource group** and select the account's default resource group.
6. Make sure the **Assign access to a resource group** option is set to **Viewer**.
7. For the service, select **VPC Infrastructure**.
8. Make sure the **Resource type** value is set to **All resource types**.
9. In the **Assign platform access roles** area, select **Editor**.
10. Click **Assign**.

The user is now authorized to create and use all VPC resources in the account's default resource group. The first policy (steps 1-4) allows the user to view the account's default resource group. The second policy (steps 5-7) assigns the user the **Editor** role for VPC infrastructure resources, but access is limited to resources in the account's default resource group. 

## Viewing your user's permissions
{: #viewing-your-user-s-permissions}

Policies can be viewed in the user's **Access policies** tab.

You can use the following CLI commands to validate the resource group permissions assigned to your user, by policy or by access group:

**By policy**
```
ibmcloud iam user-policies <username>
```
**By access group**
```
ibmcloud iam access-groups -u <username>
```

Changes to IAM access policies for VPC may take up to 10 minutes to take effect.
{: note}

## Team access scenario
{: #team-access-scenario}

This scenario covers how an account administrator can assign authorization so that different teams have access to separate VPC resources. The example uses _resource groups_ to set up separate resource access for two teams. For the purposes of this example, resources are not shared across teams.

The example takes you through the process of creating resource groups, creating access groups, and assigning the appropriate policies to provide your teams with access to separate VPC resources.

Imagine you're setting up two different project teams to use two separate Virtual Private Clouds. You'll assign access to team members so that each team has access to their team's VPC resources (only).

* Your first team is a test team. You've decided to assign that team access to VPCs in a resource group named `test_vpcs`.
* The second team is your production team. They'll be assigned access to VPCs in a resource group named `production_vpcs`.

This strategy can be used to assign separate VPC resources to any number of teams. However all resources share the same VPC quotas for the account. For more information about quotas and limits, see [VPC quotas](/docs/vpc?topic=vpc-quotas#quotas).
{: tip}

### Step 1: Create resource groups
{: #step-1-create-resource-groups}

By default, account administrators have access to create new resource groups. Other users first must be assigned the **Editor** role for **All Account Management Services**, which allows them to create resource groups.

Your first task is to create resource groups that will contain each of your teams' VPC resources.

1. Create a resource group called `test_team`.  
2. Create a resource group called `production_team`.  

For more information on how to create resource groups, see [Managing resource groups](/docs/resources?topic=resources-rgs).

### Step 2: Create access groups
{: #step-2-create-access-groups}

Resource access can be assigned to individual users, or to groups of users. Groups of users with the same access permissions are called _access groups_. In this scenario, you'll create an access group to represent each grouping of team members who require a specific type of VPC access, a total of 4 unique access groups.

**Task:** Create four access groups with the following names: `test_team_manage_vpcs`, `test_team_view_vpcs`, `production_team_manage_vpcs`, `production_team_view_vpcs`.

For help creating access groups, see [Create access groups](/docs/iam?topic=iam-groups#create_ag).

### Step 3: Add IAM policies to the access groups you just created
{: #step-3-add-iam-policies-to-the-access-groups-you-just-created}

Add the necessary VPC access policies so that the `test_team` access group can manage (create, update and delete) VPC resources.  

1. Navigate to the [IAM Group UI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam#/groups){: new_window} in the IBM Cloud Console.
2. Select the desired access group (start with: `test_team_manage_vpcs`).
3. On the **Access policies** tab, click **Assign access**.
4. Select **Assign access within a resource group**.
5. Select the desired resource group (start with: `test_team`)
6. Make sure **Assign access to a resource group** remains set to **Viewer**.
7. Select the service **VPC Infrastructure**.
8. Make sure **Resource type** remains set to **All resource types**.
9. In the **Assign platform access roles** area, select **Editor**.
10. Select **Assign**.

These steps also assign **Viewer** access to the `test_team` resource group. Viewer access is required to update, create, and delete resources inside the `test_team` resource group.
{: tip}

Repeat the previous steps for the remaining three access groups. You'll accomplish these tasks for matching up access groups with resource groups:

* Assign the `test_team_view_vpcs` access group the **Viewer** role for VPC infrastructure resources in the `test_team` resource group.
* Assign the `production_team_manage_vpcs` access group **Editor** role to VPC infrastructure resources in the `production_team` resource group.
* Assign the `production_team_view_vpcs` access group **Viewer** role to VPC infrastructure resources in the `production_team` resource group.

Users with **Editor** access to VPC resources also can view them. It isn't necessary to add members to the **Editor** AND **Viewer** access groups.

#### Setting up Viewer access
{: #setting-up-viewer-access)

VPC infrastructure `Floating IP` resources are created in the account's default resource group. Therefore, users who need to manage `Floating IPs` need **Viewer** access to the account's default resource group.
{: tip}

To create **Viewer** access:

1. Navigate to the [IAM Group UI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam#/groups){: new_window} in the IBM Cloud Console.
2. Select the desired access group (start with `test_team_manage_vpcs`).
3. On the **Access policies** tab, click **Assign access**.
4. Select **Assign access to account management services**.
7. Select service **All Account Management Services**.
9. Select the **Viewer** role.
10. Select **Assign**.

Repeat the previous steps to assign the `production_team_manage_vpcs` access group the **Viewer** role to **All Account Management Services**.

### Step 4: Add users to the access groups
{: #step-4-add-users-to-the-access-groups}

Now you can assign team members (users) to the appropriate access groups. Follow these steps to add each member of the test team to the access group that allows test team VPC management:

1. Navigate to the [IAM Group UI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/iam#/groups){: new_window} in the IBM Cloud Console.
2. Select the desired access group (start with: `test_team_manage_vpcs`).
3. On the **Users** tab, click **Add users**.
4. Select each user you want to add to the access group.
5. Click **Add to group**.

Repeat the previous steps to accomplish these tasks:
* Assign the desired users to the `test_team_view_vpcs` access group.
* Assign the desired users to the `production_team_manage_vpcs` access group.
* Assign the desired users to the `production_team_view_vpcs` access group.

It isn't necessary to assign users **Viewer** access to VPC resources if they have manage access.
{: tip}

## Next steps
{: #permissions-next-steps}

The two example teams are now set up to use VPCs. At this point, members of the `test_team_manage_vpcs` and `production_team_manage_vpcs` access groups can create VPCs in their assigned resource groups.


## Related links
{: #permissions-related-links}

* [Managing identity and access](/docs/iam?topic=iam-getstarted)
* [Managing users and access](/docs/iam?topic=iam-iamuserinv)
* [What is IAM](/docs/iam?topic=iam-iamoverview)

