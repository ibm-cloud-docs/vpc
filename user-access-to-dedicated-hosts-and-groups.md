---

copyright:
  years: 2021
lastupdated: "2021-04-06"

keywords: dedicated host, dedicated hosts, dedicated host group, access, user access,

subcollection: vpc

content-type: tutorial
account-plan: paid
completion-time: 6m

---

{:shortdesc: .shortdesc}
{:screen: .screen}  
{:codeblock: .codeblock}  
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}
{:step: data-tutorial-type='step'}

# Assigning separate user access to dedicated hosts and dedicated groups
{: #user-access-to-dedicated-hosts-and-groups}
{: toc-content-type="tutorial"}
{: toc-completion-time="6m"}

This tutorial demonstrates how to use resource groups and access groups to give users access to resources on dedicated hosts without allowing them to see or interact with these hosts directly.
{:shortdesc}

The fictional user profile Employee Example is used to show how to give a user access to resources in a dedicated host group without giving them the ability to interact directly with the dedicated hosts in the group. For example, users can provision an instance on the dedicated host group, and the instance is automatically assigned to a dedicated host within the group. However, in this example of resource group configuration, users don't have authority to create or delete a dedicated host.

## Objectives
{: objectives}

- Assign users the required permissions in a dedicated host group to provision instances.
- Restrict users from interacting directly with the dedicated hosts.
- Allow a user to provision instances through a dedicated host group without interacting directly with dedicated hosts.

## Before you begin
{: dedicated-before-you-begin}

This tutorial requires the following prerequisites.
- An IBM Cloud billable account.
- An IBM Cloud [VPC](/docs/vpc?topic=vpc-getting-started).
- Active IBM Cloud accounts for users receiving access.

## Create resource groups
{: #create-resource-group}
{: step}

You use resource groups to organize dedicated hosts and dedicate host groups. The first step in our example is to create two separate resource groups. One resource group for dedicated host groups and one resource group for dedicated hosts. Later, we are creating an access group so that you can assign specific employees the Operator role.

### Creating a dedicated host resource group
{: creating-a-dedicated-host-resource-group}

The following steps explain how to create a resource group. This group will be used to organize the dedicated host.

1. Login to the [{{site.data.keyword.Bluemix_notm}} console](https://{DomainName}){: external}.
2. Click **Manage** > **Account**. ![**Manage** dropdown menu](/images/click manage.png){: caption="Figure 1. Manage dropdown menu" caption-side="bottom"}
3. From the *Account* page, click **Resource groups** > **Create**.
4. Give the resource group a unique name such as *Admin resources*.
5. Click **Add**.

### Creating a dedicated host group resource group
{: #creating-a-dedicated-host-group-resource-group}

The following steps explain how to create a resource group. This group will be used to organize the dedicated host group.

1. Click **Manage** > **Account**.
4. From the *Account* page click **Resource groups** > **Create**.
7. Give the resource group a unique name such as *Users resources*.
8. Click **Add**.

## Creating a dedicated host and dedicated host group
{: #create-dedicated-host}
{: step}

Dedicated hosts must be assigned to a resource group. Dedicated hosts must also be assigned to a dedicated host group. This allows permissions to be applied seperately.

1. Click the **Menu icon** ![Menu icon](../icons/icon_hamburger.svg) > **VPC Infrastructure**.
3. From the *VPC Infrastructure* page, click **Dedicated hosts** > **Create**.
5. Give the dedicated host a unique name such as *Example Dedicated Host*.
6. Change the selected resource group from *Default* to *Admin resources*.
7. Click **New dedicated group** to begin creation of a dedicated host group. ![Add a dedicated group](/images/new dedicated group.png){: caption="Figure 2. New dedicated group" caption-side="bottom"}
8. On the pop-up window, give the dedicated host group a unique name such as *Users resources*.
9. Change the resource group from *Default* to *Users access*.
10. Click **Create** > **Create dedicated host** to create the dedicated host.

## Creating an access group for the dedicated host group
{: #create-an-access-group
{: step}

To simplify changes in permissions, you can add users to an access group. Access groups apply permissions to a user account. You can add multiple users to an access group. Use the following steps to create an access group.

1. Click **Manage** > **Access (IAM)**.
2. On the *Access (IAM)* page, click **Access groups** > **Create**.
3. On the pop-up window, enter a unique name for the access group such as *User access*.
4. Enter *Provides access to resources on the dedicated host group.* in the description text box.
5. On the *User access* page, click the **Access policies** tab > **Assign access**.
6. On the *Assign access to User access*, click **IAM services**. ![Access policies](/images/user access.png){: caption="Figure 4. Access policies" caption-side="bottom"}
7. In *Which service do you want to assign access to?*, click **VPC Infrastructure Services**.
8. In *How do you want to scope the access?*, select **Resources based on selected attributes**.
9. Click the **Resource group** box.
10. Select the *User resources* resource group.
11.	In *Platform access* select the **Viewer** and **Operator** options.
12.	Click **Assign** > **Yes**.

Clicking the numbers on the permission options shows what actions these permissions allow. For more information on IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).
{: note}

## Inviting and adding users
{: #invite-users}
{: step}

Users with an active IBM Cloud account must be invited for permissions to be applied. This is done using the email associated with the account. Once invited, these users can be added to access groups.

1. Go to the *IAM users* page in the IBM Cloud console by clicking **Manage** > **Access (IAM)**.
3. From the *Manage access and users* page, click **Users** > **Invite users**.
5. To invite *Employee Example*, enter *example@ibm.com* in the text box.
6. Click **Add** on *User access* to select the access group.
7. Click **Invite**.

## Related content
{: #related-content}

For more information on dedicated hosts, see these links.

- [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)
- [Creating instances on dedicated hosts](/docs/vpc?topic=vpc-creating-instance-on-dh)
- [Managing dedicated hosts and groups](/docs/vpc?topic=vpc-manage-dedicated-hosts-groups)
