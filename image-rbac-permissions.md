---

copyright:
  years: 2021, 2024
lastupdated: "2024-10-24"

keywords: custom image, creating a custom image, migrating a custom image, rbac, permissions, granular, granular permissions, rbac role-based access control

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using granular RBAC permissions for custom images
{: #using-granular-RBAC-permissions-for-custom-images}

The VPC image service allows the [creation of custom images](/docs/vpc?topic=vpc-planning-custom-images). These images are only visible and usable within a single account. Custom images are assigned to a [Resource group](/docs/account?topic=account-rgs) when they are created. Users can have multiple resource groups to host custom images with different access control privileges or assign permissions directly to an image, restricting their access to specific users. These access privileges are referred to as role-based access control, or RBAC.

The custom resource group that hosts the images can contain a set of images. The resource group can control either a single image or multiple images. It's common for a family of images to be hosted within a resource group. This hosting can make management of the control of the images easier because the access rules are attached to the resource group.

The process that is described here is for the console. However, this function can also be carried out with the [API](/apidocs/iam-identity-token-api) and from the [CLI](/docs/account?topic=account-rgs&interface=cli#rgs_cli).
{: shortdesc}

## Creating a custom resource group for image access controls
{: #createcustomimageACL-UI}

You can use an existing Resource group or create a new one.

To create a new Resource group, follow these steps:
1.	Go to **Manage> Account> Account Resources> Resource groups**.
2.	Enter a name for your resource group and click **Create**.

## Adding role based access control to the Resource group
{: #addRBAC-UI}

[Create an access group](/docs/account?topic=account-account-getting-started#account-gs-accessgroups) within the resource group that gives privileges to create images in your resource group.

1.	Go to **Manage > Access (IAM)> Access Groups** and click **Create**.
2.	Enter the name and description for your access group and click **Create**.

### Assigning permissions
{: #assignRBCperm}

When an access group is created, a policy must be applied to set permissions in the resource group.

1. On the access group page, select the **Access policies** tab. Click **Assign access**.
2. Select **IAM Services** and **VPC Infrastructure Services**.
3. Click **Services based on attributes**.
4. Under Add attributes, select **Resource group**.
5. From the list, select the resource group that you want to use.
6. Select **Resource type** under **Add attributes** and select the resource type **Image Service for VPC**. Optionally, you can choose a particular image ID for the policy.
7. Select an RBAC permission role. Roles are displayed in Table 1.
8. Click **Add**.
9. Click **Assign**.

|RBAC role |Role permissions|
|----------|----------------|
|Viewer|A user with this role can view images.|
|Operator|A user with this role can use images.|
|Editor|A user with this role can create and delete images|
{: caption="RBAC roles" caption-side="bottom"}

You now have access group permissions in the resource group. To assign users to this access group, click the **Users** tab of the access group and **Add Users**.

## Using an image with custom access controls within your VPC
{: #RVACusage}

Virtual server instances and images can be in different resource groups. This difference allows for straight control of each resource type independently. You can start virtual server instances from the custom images if the user has appropriate authority to both resource groups.

Make sure that any user that needs to deploy a custom image has at least the *Operator* or *Editor* role in the previous access group. If they have only the *Viewer* role, then they can't deploy the custom image.
