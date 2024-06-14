---

copyright:
  years: 2021, 2024
lastupdated: "2024-06-11"

keywords: dedicated host, dedicated hosts, dedicated host group, access, user access,

subcollection: vpc

content-type: tutorial
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Assigning separate user access to dedicated hosts and dedicated groups
{: #user-access-to-dedicated-hosts-and-groups}
{: toc-content-type="tutorial"}
{: toc-completion-time="30m"}

This tutorial demonstrates how to use resource groups and access groups to give users access to resources on dedicated hosts without allowing them to see or interact with these hosts directly.
{: shortdesc}

The fictional user profile *Employee Example* is used to show how to give a user access to resources in a dedicated host group without giving them the ability to interact directly with the dedicated hosts in the group. For example, users can provision an instance on the dedicated host group, and the instance is automatically assigned to a dedicated host within the group. However, in this example of resource group configuration, users don't have authority to create or delete a dedicated host.

## Objectives
{: #dedicated-objectives}

- Create two separate resource groups, *Admin resources* and *Users resources*.
- Create a dedicated host in the *Admin resources* resource group.
- Create the dedicated host that is associated dedicated host group in the *Users resources* resource group.
- Create an access group to assign the Operator role to the resource group, *Users resources*.
- Invite your user, *Employee Example*, to the access group so that they can provision instances to the dedicated host group.
- Restrict *Employee Example* from interacting directly with the dedicated hosts by excluding *Employee Example* from access to the *Admin resources* resource group.

## Before you begin
{: #dedicated-before-you-begin}

This tutorial requires the following prerequisites.

- An IBM Cloud billable account.
- Active IBM Cloud accounts for users who are to receive access.
- An existing SSH Key

## Create resource groups
{: #create-resource-group}
{: step}

You can use resource groups to organize dedicated hosts and dedicated host groups. The first step in our example is to create two separate resource groups. The first resource group is for the dedicated host. The second resource group is for the dedicated host group.

### Creating a dedicated host resource group
{: #creating-a-dedicated-host-resource-group}

Complete the following steps to create a resource group for the dedicated host.

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/login){: external}.
2. Click **Manage** > **Account**.
   ![Manage list](/images/click_manage.png){: caption="Figure 1. Manage menu" caption-side="bottom"}
3. From the *Account* page, click **Resource groups** > **Create**.
4. Give the resource group a unique name such as *Admin resources*.
5. Click **Add**.

### Creating a dedicated host group resource group
{: #creating-a-dedicated-host-group-resource-group}

Complete the following steps to create a resource group for the dedicated host group.

1. Click **Manage** > **Account**.
1. From the Account page, click **Resource groups** > **Create**.
1. Give the resource group a unique name such as *Users resources*.
1. Click **Add**.

## Creating a dedicated host and dedicated host group
{: #create-dedicated-host}
{: step}

When you create a dedicated host, you assign it to a resource group. As part of the dedicated host creation process, you define a dedicated host group for the dedicated host. You can assign the dedicated host group to a separate resource group, so you can apply permissions separately for the dedicated host and the dedicated host group.

Complete the following steps to create a dedicated host and dedicated host group in their respective resource groups: *Admin resources* and *Users resources*.

1. Click the **menu icon** ![menu icon](../icons/icon_hamburger.svg) > **VPC Infrastructure**.
1. From the *VPC Infrastructure* page, click **Dedicated hosts** > **Create**.
1. Give the dedicated host a unique name such as *Example Dedicated Host*.
1. Change the selected resource group from *Default* to *Admin resources*.
1. Click **New dedicated group** to begin creation of a dedicated host group.
   ![Add a dedicated group](/images/new_dedicated_group.png){: caption="Figure 2. New dedicated group" caption-side="bottom"}
1. On the new window, give the dedicated host group a unique name such as *Example Dedicated Host Group*.
1. Change the resource group from *Default* to *Users resources*.
1. Click **Create** > **Create dedicated host** to create the dedicated host.

## Creating a VPC and subnet in the Users resources resource group
{: #creating-a-vpc}

1. Open [{{site.data.keyword.cloud_notm}} console](/login){: external}.
2. Click **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Network > VPCs** and click **Create**.
3. Enter a name for the VPC, such as `my-vpc`.
4. Select *Users resources* as the resource group for the VPC.
5. Create the default access control list for new subnets in this VPC.
6. Select whether the default security group allows inbound SSH and ping traffic to virtual server instances in this VPC.
7. Clear the **Default address prefixes** option so you don't need to assign default address prefixes to each zone in your VPC. After you create your VPC, you can go to its details page and set your own address prefixes.
8. Enter a name for the new subnet in your VPC, such as `my-subnet`.
9. Select *User resources* as the resource group for the subnet.
10. Select *Dallas 2* as the location for the subnet.

   The region that you select is used as the region of the VPC. All additional resources that you create in this VPC are created in the selected region.
   {: tip}

11. Enter an IP range for the subnet in CIDR notation, for example: `10.240.0.0/24`. In most cases, you can use the default IP range. If you want to specify a custom IP range, you can use the IP range calculator to select a different address prefix or change the number of addresses.

   A subnet cannot be resized after it is created.
   {: important}

12. Click **Create virtual private cloud**.

## Creating a virtual server instance and Block Storage volume
{: #creating-block-storage-dh}

Be sure to select VPC infrastructure from the menu icon.
{: tip}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![Menu icon](../../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.
2. Click **Create** and enter the information in Table 1.
3. Click **Create virtual server instance** when you are ready to provision.

| Field | Value |
|-------|-------|
| Name  | Required; provide a name for your virtual server instance. |
| Virtual private cloud | Block Storage server instance |
| Resource group | User resources |
| Location | Dallas 2 |
| SSH Key | You must select an existing public SSH key or click **Create an SSH key** to create a new one. For more information about creating an SSH key, see [Creating your SSH key using the UI](/docs/vpc?topic=vpc-ssh-keys&interface=ui#generate-ssh-keys-ui). SSH keys are used to securely connect to the instance after it's running. |
| | **Note:**  SSH keys can either be RSA or Ed25519. You can generate new RSA key pairs using the UI. Pre-existing RSA and Ed25519 SSH keys can be uploaded. Ed25519 can be used only if the operating system supports this key type. Ed25519 can't be used with Windows or VMware images. |
| | For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Data volumes | You can add one or more secondary data volumes to be included when you provision the instance. To add a volume, click **Create** and specify the information in Table 2. When finished, click **Save**. |
| Network interfaces | Assign networking options to connect into the IBM Cloud VPC. You can create and assign up to five network interfaces to each instance. |
| Attached Block Storage volume | You can add one or more secondary data volumes to be included when you provision the instance. To add a volume, click **New Block Storage volume** and specify the information in Table 2. When finished, click **Create volume**. |
| Virtual Private Cloud | Select 'my-vpc' for the Virtual Private Cloud |
{: caption="Table 1. Virtual Server Instance provisioning selections" caption-side="bottom"}

| Field | Value |
|-------|-------|
| Name  | Specify a unique, meaningful name for your volume, for example, 'my-data-volume' You can later edit the name if you want. Volume names must be unique the entire VPC infrastructure. |
| Size | Enter a volume size in GBs. Volume sizes can be between 10 GB and 2 TBs. |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also choose **Customer Managed** and use your own encryption key. For more information about one-time setup procedure, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs). |
{: caption="Table 2. Block storage volume values specified when provisioning an instance" caption-side="bottom"}

A Block Storage volume is created and attached to the virtual server instance. On the instance details page, the **Attached Block Storage volumes** list is updated to show the new volume.

## Creating an access group for the dedicated host group
{: #create-an-access-group}
{: step}

To simplify changes in permissions, you can add users to an access group. Access groups apply permissions to all of the users in the group. You can add multiple users to an access group.

Use the following steps to create an access group with an access policy that assigns the *User resources* resource group the operator role. With *Example Dedicated Host Group* existing in the *User resources* resource group, users in the access group can provision instances to *Example Dedicated Host Group*.

1. Click **Manage** > **Access (IAM)**.
2. On the *Access (IAM)* page, click **Access groups** > **Create**.
3. On the open window, enter a unique name for the access group such as *User access*.
4. Enter *Provides access to resources on the dedicated host group.* in the description text box.
5. On the *User access* page, click the **Access policies** tab > **Assign access**.
6. On the *Assign access to User access*, click **IAM services**. ![Access policies](/images/user_access.png){: caption="Figure 4. Access policies" caption-side="bottom"}
7. In *Which service do you want to assign access to?*, click **VPC Infrastructure Services**.
8. In *How do you want to scope the access?*, select **Resources based on selected attributes**.
9. Click the **Resource group** box.
10. Select the *User resources* as the resource group.
11. Click the *Resource type* box and select **Virtual Server for VPC** option.
12. In *Platform access* select the **Viewer**, **Operator**, and **Editor** options.
13. Click **Assign** > **Yes**.

Clicking the numbers on the permission options shows the list of actions that these permissions allow. For more information about IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).
{: note}

## Inviting and adding users
{: #invite-users}
{: step}

Users with an active IBM Cloud account must be invited for permissions to be applied. Invite *Employee Example* to the access group with the email address that is associated with their account so that they can provision virtual server instances to the dedicated host group.

Without permissions to the *Example Dedicated Host* in the *Admin resources* resource group, *Employee Example* cannot provision instances directly to the dedicated host or perform actions on the dedicated host, such as delete the host.

1. Go to the *IAM users* page in the IBM Cloud console by clicking **Manage** > **Access (IAM)**.
1. From the *Manage access and users* page, click **Users** > **Invite users**.
1. To invite *Employee Example*, enter *example@ibm.com* in the text box.
1. Click **Add** on *User access* to select the access group.
1. Click **Invite**.

## Related content
{: #dh-related-content}

For more information about dedicated hosts, see the following topics.

- [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)
- [Creating instances on dedicated hosts](/docs/vpc?topic=vpc-creating-instance-on-dh)
- [Managing dedicated hosts and groups](/docs/vpc?topic=vpc-manage-dedicated-hosts-groups)
