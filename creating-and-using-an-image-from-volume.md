---

copyright:
  years: 2021, 2024
lastupdated: "2024-10-10"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance, instances, virtual servers, creating virtual servers, virtual server instances, virtual machines, Virtual Servers for VPC, compute, vsi, vpc, creating, UI, console

subcollection: vpc

content-type: tutorial
account-plan: paid
completion-time: 15m

---

{{site.data.keyword.attribute-definition-list}}

# Creating a custom image from volume and provisioning an instance in the UI
{: #creating-and-using-an-image-from-volume}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

This tutorial demonstrates how to create a custom image from the boot volume of a virtual server instance, and then use that image to create an instance.
{: shortdesc}

## Before you begin
{: #before-you-begin-create-image}

This tutorial requires the following prerequisites.

- An IBM Cloud billable account.
- An IBM Cloud [VPC](/docs/vpc?topic=vpc-getting-started).
- An IBM Cloud [SSH key](/docs/vpc?topic=vpc-ssh-keys).
- An IBM Cloud [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers).

## Objectives
{: #objectives}

- Create a resource group, *Image resources*.
- Create a custom image from an instances' boot volume.
- Create a new virtual server instance by using the custom image.

## Creating a resource group
{: #creating-a-resource-group}
{: step}

You can use [resource groups](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups) to organize images.

1. Log in to the [{{site.data.keyword.Bluemix_notm}} console](/login){: external}.
2. Click **Manage** > **Account**.
3. From the *Account* page, click **Resource groups** > **Create**.
4. Give the resource group a unique name such as *Image resources*.
5. Click **Add**.

## Creating an image from a volume in the UI
{: #image-from-volume-vpc-ui}
{: step}

You can use the UI to create an image from a volume that is attached to an available virtual server instance as the primary boot volume.

### Choose the source for your custom image
{: #import-custom-image-source-UI}

Complete the following actions to import your custom image in the UI.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Images**.
2. On the **Custom images** tab, click **Create**. The Custom image provision page is displayed.
3. Complete the following required fields on the Custom image provision page.

| Field | Value |
|-------|-------|
| Name | Custom OS |
| Resource group | Image resources |
| Tags | Custom image |
| Region | Dallas 2 |
| Source | Virtual server instance boot volume (default) |
{: caption="Custom image provision user interface fields" caption-side="bottom"}

### Create an image from a virtual server instance boot volume
{: #import-custom-boot-image-vsi}

When you select **Virtual server instance boot volume** as the source of your custom image, a list of instances is displayed. Complete the following actions to create an image from the instance's boot volume.

1. On the **Custom image provision** page, select **Virtual server instance boot volume** (default).
2. Select an instance from the list. You're selecting the boot volume of the instance.
3. Make sure that the instance is not running. You can stop a running instance by clicking the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and selecting **Stop**.
4. Select IBM-managed encryption as the encryption type.
5. On the side panel, click **Create custom image**.

## Creating virtual server instances in the UI
{: #creating-virtual-servers-ifv}
{: step}

You can create virtual server instances in your {{site.data.keyword.vpc_short}} in the {{site.data.keyword.cloud_notm}} console. Complete the following actions to create a virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Virtual server instances**.

2. Click **Create** and enter the required information.

   | Field | Value |
   |-------|-------|
   | Name  | Server Instance |
   | Resource group | Image resources |
   | Tags | OS |
   | Location | Dallas 2 |
   | Type of virtual server | Public |
   | Operating system | Select **Custom image** > (custom image). |
   | SSH key | An existing SSH key \n For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
   | Data volumes | You can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **Create** in the Data volumes section of the page. \n For more information about provisioning the volume, see [Create and attach a Block Storage volume when you create an instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi). |
   | Networking | Select your pre-existing VPC |
   {: caption="Instance provisioning selections" caption-side="bottom"}

3. Click **Create virtual server instance** when you are ready to provision.

## Related content
{: #related-content}

* [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc)
* [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv)
* [Viewing and using your custom image](/docs/vpc?topic=vpc-create-ifv#ifv-image-creation-completed)
* [resource group](/docs/account?topic=account-account_setup)
* [Best practices for organizing resources and assigning access](/docs/account?topic=account-account_setup)
