---

copyright:
  years: 2019
lastupdated: "2019-07-29"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, boot volume, data volume, volume, data storage, VSI, virtual server instance, instance, IOPS

subcollection: vpc


---

{:shortdesc: .Shortdesc}
{: codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:important: .important}
{:table: .Aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Creating block storage volumes in {{site.data.keyword.cloud_notm}} console
{: #creating-block-storage}
[comment]: # (linked help topic)

You can create a {{site.data.keyword.block_storage_is_short}} volume when you create a virtual server instance, or create a standalone volume to later attach to an instance.
{:shortdesc}

Before you get started, make sure you have [created a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{:important}

## Create and attach a block storage volume when you create a new instance
{: #create-from-vsi}

1. Create an instance. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc){: external} for the Virtual Private Cloud, navigate to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Compute > Virtual server instances > New instance**.
1. [Configure your virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers). Under **Attached block storage volumes**, select **New block storage volume**.
1. Enter the information described in the following table.  When finished, click **Create volume**.

| Field | Value |
|-------|-------|
| Name  | Specify a meaningful name for your volume, for example, a name that describes your compute or workload function. You can enter a combination of alpha and numeric characters, and later edit the name if you want. |
| Profile | Select [Tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) and select the performance level you require from the IOPs list. If your performance requirements don't fall within a predefined IOPS tier, select [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) and  select an IOPS value within the range for that volume size. Click the **storage size** link to see a table of size and IOPS ranges. |
| Auto Delete | Enable this feature to automatically delete this volume when the attached virtual server instance is deleted. You can change this setting later on the virtual server details page. |
| IOPS | Select 3, 5, or 10 IOPS/GB for a Tiered profile |
| Size | Enter a volume size in GBs.  Volume sizes can be between 10 GB and 2 TBs. |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. |
{: caption="Table 1. Block storage volume values specified when provisioning an instance" caption-side="top"}

A block storage volume is created and attached to the virtual server instance. On the instance details page, the **Attached block storage volumes** list is updated to show the new volume.

A block storage volume can be attached to only one virtual server at a time. On the block storage volume summary page, you can view details about the virtual server instance by selecting the instance name under **Attached instances**.

## Create a standalone block storage volume
{: #create-standalone-vol}

You can create a block storage volume independent of virtual server provisioning and attach the volume to an instance later.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc){: external} for the Virtual Private Cloud, navigate to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Storage > Block storage volumes** and select **New volume**.
1. Enter the information in the table below to define your new block storage volume.
1. When finished, click **Create volume**. You're returned to the Block storage volumes page, where a message indicates that the volume is being created. A second message displays when the volume is created.
1. To see details of the new volume, select the **View resource** link in the second message to navigate to the Volume details page.

| Field | Value |
|-------|-------|
| Name  | Specify a meaningful name for your volume. For example, provide a name that describes your compute or workload function. You can enter up to 40 alpha-numeric characters and later edit the name. |
| Resource Group | Specify a resource group. Resource groups help organize your account resources for access control and billing purposes. For information about setting up a resource group, see [Best practices for organizing resources in a resource group](/docs/resources?topic=resources-bp_resourcegroups#setuprgs). |
| Tags | Specify a tag to organize your resources. A tag is a label that you assign to a resource for easy filtering of resources in your resource list. For information about tags, see [Working with tags](/docs/resources?topic=resources-tag). |
| Location | The availability zone in your region, inherited from the VPC (for example, US South 1). |
| Size | Enter a volume size in GBs.  Volume sizes can be between 10 GB - 2 TBs. |
| IOPS | Select [IOPS Tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) and select the tile with performance level you require. |
| Custom | Depending on the size of the volume you specified, select an IOPS value within the range for that volume size.  Click the **storage size** link to see a table of size and IOPS ranges. For more information, see [Custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom). |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. |
{: caption="Table 2. Values for defining a block storage volume" caption-side="top"}

Do you prefer to create block storage volumes using the CLI? For information, see [Creating block storage volumes using the CLI](/docs/vpc?topic=vpc-creating-block-storage-cli).
{: tip}

## Next steps
{: #next-step-creating-block-storage}

When you refresh the Block storage volumes page, the new volume appears at the top of the list of volumes. If the volume was created successfully, it shows a status of Available. For standalone volumes, the Attachment Type column is blank (-). The Action menu (...) at the end of a table row provides a link for [attaching a block storage volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).

You can continue creating more block storage volumes or manage existing volumes.

* [View details about a block storage volume](/docs/vpc?topic=vpc-viewing-block-storage)
* [Detach a volume from a virtual server instance](/docs/vpc?topic=vpc-managing-block-storage#detach)
* [Delete a block storage volume](/docs/vpc?topic=vpc-managing-block-storage#delete)
