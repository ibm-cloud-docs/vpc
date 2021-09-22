---

copyright:
  years: 2021
lastupdated: "2021-05-26"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance, instances, virtual servers, creating virtual servers, virtual server instances, virtual machines, Virtual Servers for VPC, compute, vsi, vpc, creating, UI, console, generation 2, gen 2

subcollection: vpc

content-type: tutorial
account-plan: paid
completion-time: 15m

---

{:shortdesc: .shortdesc}
{:screen: .screen}  
{:codeblock: .codeblock}  
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}
{:step: data-tutorial-type='step'}

# Creating a custom image from volume and provisioning an instance from the UI
{: #creating-and-using-an-image-from-volume}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

This tutorial demonstrates how to create a custom image from the boot volume of a virtual server instance, and then use that image to create a new instance.

## Before you begin
{: before-you-begin}

This tutorial requires the following prerequisites.

- An IBM Cloud billable account
- An IBM Cloud [VPC](/docs/vpc?topic=vpc-getting-started)
- An IBM Cloud [SSH key](/docs/vpc?topic=vpc-ssh-keys)
- An IBM Cloud [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers)

## Objectives
{: objectives}
- Create a resource group, *Image resources*.
- Create a custom image from an instances' boot volume.
- Create a new virtual server instance by using a custom image.

## Creating a resource group
{: creating-a-resource-group}
{: step}

You can use [resource groups](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups) to organize images. The first step in our example is to create a resource group. Complete the following actions to create a resource group for the custom image.

1. Log in to the [{{site.data.keyword.Bluemix_notm}} console](https://{DomainName}){: external}.
2. Click **Manage** > **Account**.
3. From the *Account* page, click **Resource groups** > **Create**.
4. Give the resource group a unique name such as *Image resources*.
5. Click **Add**.

## Creating an image from a volume by using the UI
{: #image-from-volume-vpc-api}
{: step}

You can use the UI to create an image from a volume that's attached to an available virtual server instance as the primary boot volume. Complete the following actions to create a custom image from a boot volume by using the UI.

### Choose the source for your custom image
{: #import-custom-image-source-UI}

Complete the following actions to import your custom image by using the UI.

1. In [{{site.data.keyword.cloud_notm}} console](https://console.cloud.ibm.com/vpc-ext){: external}, 
navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.

2. Click **Create**. The Import custom image page displays.

3. Complete the following required fields on the Import custom images page (see Table 1).

| Field | Value |
|-------|-------|
| Name | Custom OS |
| Resource group | Image resources |
| Tags | Custom image |
| Region | Dallas 2 |
| Source | [Virtual server instance boot volume](#import-custom-image-vsi) (default) |
{: caption="Table 1. Import custom image user interface fields" caption-side="top"}

### Create an image from a virtual server instance boot volume
{: #import-custom-image-vsi}

When you select **Virtual server instance boot volume** as the source of your custom image, a list of instances displays. Complete the following actions to create an image from the instance's boot volume.

1. On the **Import custom image** page, select **Virtual server instance boot volume** (default).

2. Select an instance from the list. You're actually selecting the boot volume of the instance.

3. Make sure that the instance has stopped running. Stop a running instance by clicking the overflow menu (ellipsis) and click **Stop**.

4. Select IBM-managed encryption as the encryption type.

5. On the side panel, click **Import custom image**.


## Creating virtual server instances by using the UI
{: #creating-virtual-servers}
{: step}

You can create virtual server instances in your {{site.data.keyword.vpc_short}} by using the {{site.data.keyword.cloud_notm}} console. Complete the following actions to create a virtual server instance.
{: shortdesc}

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

   Be sure to select VPC infrastructure from the Menu icon.
   {: tip}

2. Click **Create** and enter the information in Table 2.

3. Click **Create virtual server instance** when you are ready to provision.

| Field | Value |
|-------|-------|
| Name  | Server Instance |
| Resource group | Image resources |
| Tags | OS |
| Location | Dallas 2 |
| Type of virtual server | Public |
| Operating system | Select **Custom image** > (custom image) |
| SSH key | An existing SSH key |
| | For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
| Data volumes | You can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **Create** in the Data volumes section of the page. For information about provisioning the volume, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi). |
| Networking | Select your pre-existing VPC |
{: caption="Table 2. Instance provisioning selections" caption-side="top"}

## Related content
{: related-content}

* [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc)
* [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv)
* [View and use your image from a volume](/docs/vpc?topic=vpc-create-ifv#ifv-image-creation-completed)
* [Resource group](/docs/account?topic=account-account_setup)
* [Best practices for organizing resources and assigning access](/docs/account?topic=account-account_setup)

