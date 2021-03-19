---

copyright:
  years: 2021
lastupdated: "2021-03-19"

keywords: custom image, creating a custom image, migrating a custom image, rbac, permissions, granular, granular permissions, rbac role-based access control

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
{:external: target="_blank" .external}
{:download: .download}

# Using granular RBAC permissions for custom images
{: #using-granular-RBAC-permissions-for-custom-images}

The VPC image service allows the [creation of custom images](/docs/vpc?topic=vpc-managing-images). These images are only visible and useable within a single account. Custom images are assigned to a [Resource group](/docs/account?topic=account-rgs) when created. Users can have multiple resource groups to host custom images with different access control rights or assign permissions directly to an image, restricting their access to specific users.

The custom resource group that hosts the images can contain a set of images. The resource group can
control either a single image or multiple images. It is common for a family of images to be hosted within a resource group. This makes management of the control of the images easier as the access rules are attached to the resource group.

The process is described using the User Interface. However, this function can also be carried
out by the [API](https://test.cloud.ibm.com/apidocs/iam-identity-token-api) and
[CLI](/docs/account?topic=account-rgs#rgs_cli).
{:shortdesc}

## Creating a custom resource group for image access controls
You can use an existing Resource group or create a new one.
To create a new Resource group:
1.	Go to **Manage> Account> Account Resources> Resource groups**.
2.	Enter a name for your resource group and click **Create**.

## Adding role based access control to the Resource group
[Create an access group](/docs/account?topic=account-access-getstarted#create-access-group) within the resource group that will be given rights to create images in your resource group. 
1.	Go to **Manage> Access (IAM)> Access Groups** and click **Create**.
2.	Enter the name and description for your access group and click **Create**.

### Assigning permissions
Now that an access group has been created. A policy must be applied to set permissions in the resource group.

1. On the access group page, select the **Access Policies** tab. Click **Assign Access**.
2. Select **IAM Services** and **VPC Infrastructure Services**.
3. Click **Services based on attributes**.
4. UnderAdd attributes, select **Resource group**.
5. From the dropdown, select the resource group you want to use.
6. Select **Resource type** under **Add attributes** and select the resource type **Image Service for VPC**. Optionally you may choose a particular image ID for the policy.
7. Select an RBAC permission role. Roles are displayed in Yable 1.
8. Click **Add**.
9. Click **Assign**.

|RBAC role|Role permissions|
|----------|---------|
|Viewer|Allows the user to view images|
|Operator|Allows the user to utilize images|
|Editor|Allows the user to create and delete images|
{: caption="Table 1.  RBAC roles" caption-side="top"}

You now have an access group permissions in the resource group. To assign users to this access group click the **Users** tab of the access group and **Add Users**.

## Using an image with custom access controls within your VPC
Virtual server instances and images can be in different resource groups. This allows for straight control of each resource type independently. VSIs can be booted from the custom images as long as the user has appropriate authority to both resource groups.

Make sure that any user that needs to deploy a custom image has at least the Operator or Editor role in the above access group. If they only have viewer, then they will not be able to deploy the custom image.
