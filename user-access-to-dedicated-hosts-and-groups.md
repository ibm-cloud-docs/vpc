---

copyright:
  years: 2021
lastupdated: "2021-05-20"

keywords: dedicated host, dedicated hosts, dedicated host group, access, user access,

subcollection: vpc

content-type: tutorial
account-plan: paid
completion-time: 30m

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
{: toc-completion-time="30m"}

This tutorial demonstrates how to use resource groups and access groups to give users access to resources on dedicated hosts without allowing them to see or interact with these hosts directly.
{: shortdesc}

The fictional user profile *Employee Example* is used to show how to give a user access to resources in a dedicated host group without giving them the ability to interact directly with the dedicated hosts in the group. For example, users can provision an instance on the dedicated host group, and the instance is automatically assigned to a dedicated host within the group. However, in this example of resource group configuration, users don't have authority to create or delete a dedicated host.

## Objectives
{: objectives}

- Create two separate resource groups, *Admin resources* and *Users resources*.
- Create a dedicated host in the *Admin resources* resource group. 
- Create the dedicated host's associated dedicated host group in the *Users resources* resource group. 
- Create an access group to assign the Operator role to the resource group, *Users resources*.  
- Invite your user, *Employee Example*, to the access group so that they can provision instances to the dedicated host group.
- Restrict *Employee Example* from interacting directly with the dedicated hosts by excluding *Employee Example* from access to the *Admin resources* resource group.

## Before you begin
{: dedicated-before-you-begin}

This tutorial requires the following prerequisites.
- An IBM Cloud billable account.
- Active IBM Cloud accounts for users receiving access.
-An existing SSH Key

## Create resource groups
{: #create-resource-group}
{: step}

You can use resource groups to organize dedicated hosts and dedicated host groups. The first step in our example is to create two separate resource groups. The first resource group is for the dedicated host. The second resource group is for the dedicated host group.

### Creating a dedicated host resource group
{: creating-a-dedicated-host-resource-group}

Complete the following steps to create a resource group for the dedicated host.

1. Log in to the [{{site.data.keyword.Bluemix_notm}} console](https://{DomainName}){: external}.
2. Click **Manage** > **Account**. ![**Manage** dropdown menu](/images/click manage.png){: caption="Figure 1. Manage dropdown menu" caption-side="bottom"}
3. From the *Account* page, click **Resource groups** > **Create**.
4. Give the resource group a unique name such as *Admin resources*.
5. Click **Add**.

### Creating a dedicated host group resource group
{: #creating-a-dedicated-host-group-resource-group}

Complete the following steps to create a resource group for the dedicated host group.

1. Click **Manage** > **Account**.
4. From the *Account* page click **Resource groups** > **Create**.
7. Give the resource group a unique name such as *Users resources*.
8. Click **Add**.

## Creating a dedicated host and dedicated host group
{: #create-dedicated-host}
{: step}

When you create a dedicated host, you assign it to a resource group. As part of the dedicated host creation process, you define a dedicated host group for the dedicated host. You can assign the dedicated host group to a separate resource group, allowing you to apply permissions separately for the dedicated host and the dedicated host group.
Complete the following steps to create a dedicated host and dedicated host group in their respective resource groups, *Admin resources* and *Users resources*.

1. Click the **Menu icon** ![Menu icon](../icons/icon_hamburger.svg) > **VPC Infrastructure**.
3. From the *VPC Infrastructure* page, click **Dedicated hosts** > **Create**.
5. Give the dedicated host a unique name such as *Example Dedicated Host*.
6. Change the selected resource group from *Default* to *Admin resources*.
7. Click **New dedicated group** to begin creation of a dedicated host group. ![Add a dedicated group](/images/new dedicated group.png){: caption="Figure 2. New dedicated group" caption-side="bottom"}
8. On the new window, give the dedicated host group a unique name such as *Example Dedicated Host Group*.
9. Change the resource group from *Default* to *Users resources*.
10. Click **Create** > **Create dedicated host** to create the dedicated host.

## Creating a VPC and subnet in the Users resources resource group
{: #creating-a-vpc}

1. Open [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}).
2. Click **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Network > VPCs** and click **Create**.
3. Enter a name for the VPC, such as `my-vpc`.
4. Select *Users resources* as the resource group for the VPC.
6. Create the default access control list for new subnets in this VPC.
7. Select whether the default security group allows inbound SSH and ping traffic to virtual server instances in this VPC.
8. Clear the **Default address prefixes** option so you don't need to assign default address prefixes to each zone in your VPC.
After you create your VPC, you can go to its details page and set your own address prefixes.
9. Enter a name for the new subnet in your VPC, such as `my-subnet`.
10. Select *User resources* as the resource group for the subnet.
11. Select *Dallas 2* as the location for the subnet.
    
    The region that you select is used as the region of the VPC. All additional resources that you create in this VPC are created in the selected region.
    {: tip}
12. Enter an IP range for the subnet in CIDR notation, for example: `10.240.0.0/24`. In most cases, you can use the default IP range. If you want to specify a custom IP range, you can use the IP range calculator to select a different address prefix or change the number of addresses.

    A subnet cannot be resized after it is created.
    {: important}

13. Click **Create virtual private cloud**.


## Creating a virtual server instance and block storage volume
{: creating-block-storage}

Be sure to select VPC infrastructure from the Menu icon.  
{: tip}

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. Click **Create** and enter the information in Table 1.
3. Click **Create virtual server instance** when you are ready to provision.
| Field | Value |
|-------|-------|
| Name  | Required; provide a name for your virtual server instance. |
| Virtual private cloud | Block Storage server instance |
| Resource group | User resources |
| Location | Dallas 2 |
| SSH Key | Required; select an existing SSH key.|
| | **Note:** Alpha-numeric combinations are limited to 100 characters. |
| | For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Data volumes | You can add one or more secondary data volumes to be included when you provision the instance. To add a volume, click **Create** and specify the information in Table 2. When finished, click **Save**. |
| Network interfaces | Assign networking options to connect into the IBM Cloud VPC. You can create and assign up to five network interfaces to each instance. |
{: caption="Table 1. Virtual Server Instance provisioning selections" caption-side="top"}
| Attached block storage volume | You can add one or more secondary data volumes to be included when you provision the instance. To add a volume, click **New block storage volume** and specify the information in Table 2. When finished, click **Create volume**. |
| Virtual Private Cloud | Select 'my-vpc' for the Virtual Private Cloud |
{: caption="Table 1. Virtual Server Instance provisioning selections" caption-side="top"}

| Field | Value |
|-------|-------|
| Name  | Specify a unique, meaningful name for your volume, for example, 'my-data-volume' You can later edit the name if you want. Volume names must be unique the entire VPC infrastructure. |
| Size | Enter a volume size in GBs. Volume sizes can be between 10 GB and 2 TBs. |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also choose **Customer Managed** and use your own encryption key. For a one-time set up procedure, see [Prerequisites for setting up customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-encryption-prereqs). |
{: caption="Table 2. Block storage volume values specified when provisioning an instance" caption-side="top"}

A block storage volume is created and attached to the virtual server instance. On the instance details page, the **Attached block storage volumes** list is updated to show the new volume.


## Creating an access group for the dedicated host group
{: #create-an-access-group
{: step}

To simplify changes in permissions, you can add users to an access group. Access groups apply permissions to all of the users in the group. You can add multiple users to an access group.

Use the following steps to create an access group with an access policy that assigns the *User resources* resource group the operator role. With *Example Dedicated Host Group* existing in the *User resources* resource group, users in the access group can provision instances to *Example Dedicated Host Group*.

1. Click **Manage** > **Access (IAM)**.
2. On the *Access (IAM)* page, click **Access groups** > **Create**.
3. On the open window, enter a unique name for the access group such as *User access*.
4. Enter *Provides access to resources on the dedicated host group.* in the description text box.
5. On the *User access* page, click the **Access policies** tab > **Assign access**.
6. On the *Assign access to User access*, click **IAM services**. ![Access policies](/images/user access.png){: caption="Figure 4. Access policies" caption-side="bottom"}
7. In *Which service do you want to assign access to?*, click **VPC Infrastructure Services**.
8. In *How do you want to scope the access?*, select **Resources based on selected attributes**.
9. Click the **Resource group** box.
10. Select the *User resources* resource group.
11. Click the *Resource type* box and select **Virtual Server for VPC** option.
12.	In *Platform access* select the **Viewer**, **Operator**, and **Editor** options.
13.	Click **Assign** > **Yes**.

Clicking the numbers on the permission options shows what actions these permissions allow. For more information on IAM roles, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started).
{: note}

## Inviting and adding users
{: #invite-users}
{: step}

Users with an active IBM Cloud account must be invited for permissions to be applied. Invite *Employee Example* to the access group with the email address that is associated with their account so that they can provision virtual server instances to the dedicated host group.

Without permissions to the *Example Dedicated Host* in the *Admin resources* resource group, *Employee Example* cannot provision instances directly to the dedicated host or perform actions on the dedicated host, such as delete the host.

1. Go to the *IAM users* page in the IBM Cloud console by clicking **Manage** > **Access (IAM)**.
3. From the *Manage access and users* page, click **Users** > **Invite users**.
5. To invite *Employee Example*, enter *example@ibm.com* in the text box.
6. Click **Add** on *User access* to select the access group.
7. Click **Invite**.

## Related content
{: #related-content}

For more information on dedicated hosts, see.

- [Creating dedicated hosts and groups](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances)
- [Creating instances on dedicated hosts](/docs/vpc?topic=vpc-creating-instance-on-dh)
- [Managing dedicated hosts and groups](/docs/vpc?topic=vpc-manage-dedicated-hosts-groups)
