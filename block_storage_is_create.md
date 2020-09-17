---

copyright:
  years: 2019, 2020
lastupdated: "2020-6-19-20"

keywords: block storage, boot volume, data volume, volume, data storage, IOPS

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
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Creating block storage volumes by using the UI
{: #creating-block-storage}
[comment]: # (linked help topic)

In the {{site.data.keyword.cloud_notm}} console, you can create a {{site.data.keyword.block_storage_is_short}} volume when you create a virtual server instance, or create a stand-alone volume to later attach to an instance.
{:shortdesc}

Before you get started, make sure that you [created a VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
{:important}

## Create and attach a block storage volume when you create a new instance
{: #create-from-vsi}
{: help}
{: support}

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
1. Click **New instance**.
1. Follow these instructions to [create your virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers).
1. To create a block storage volume, under **Attached block storage volumes**, select **New block storage volume**.
1. Enter the information described in the Table 1. When finished, click **Create volume**.

| Field | Value |
|-------|-------|
| Name  | Specify a unique, meaningful name for your volume, for example, a name that describes your compute or workload function. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want. <br> **Note:** Volume names must be unique the entire VPC infrastructure. For example, if you create two volumes from Gen 1 and Gen 2 compute resources that are in the same account, the same region, and have the same name, a  "volume name duplicate" error is triggered.</br> |
| Profile | Select [Tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) and select the performance level that you require from the IOPS list. If your performance requirements don't fall within a predefined IOPS tier, select [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) and select an IOPS value within the range for that volume size. Click the **storage size** link to see a table of size and IOPS ranges. |
| Auto Delete | Enable this feature to automatically delete this volume when the attached virtual server instance is deleted. You can change this setting later on the virtual server details page. |
| IOPS | Select 3, 5, or 10 IOPS/GB for a Tiered profile |
| Size | Enter a volume size in GBs. Volume sizes can be between 10 GB and 2 TBs. |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also choose **Customer Managed** and use your own encryption key. For prerequistes and steps to set up customer-managed encryption, see [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption). |
{: caption="Table 1. Block storage volume values specified when provisioning an instance" caption-side="top"}

A block storage volume is created and attached to the virtual server instance. On the instance details page, the **Attached block storage volumes** list is updated to show the new volume.

A block storage volume can be attached to only one virtual server at a time. On the block storage volume summary page, you can view details about the virtual server instance by selecting the instance name under **Attached instances**.

## Create a stand-alone block storage volume
{: #create-standalone-vol}
{: help}
{: support}

You can create a block storage volume independent of virtual server provisioning and attach the volume to an instance later.

1.  [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Select **New volume**.
1. Enter the information in Table 2 to define your new block storage volume.
1. When finished, click **Create volume**. You're returned to the Block storage volumes page, where a message indicates that the volume is being created. A second message displays when the volume is created.
1. To see details of the new volume, select the **View resource** link in the second message to go to the Volume details page.

| Field | Value |
|-------|-------|
| Name  | Specify a meaningful name for your volume. For example, provide a name that describes your compute or workload function. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. Volume names must be unique across the entire VPC infrastructure. You can later edit the name. |
| Resource Group | Specify a resource group. Resource groups help organize your account resources for access control and billing purposes. For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-bp_resourcegroups#setuprgs). |
| Tags | Specify a tag to organize your resources. A tag is a label that you assign to a resource for easy filtering of resources in your resource list. For more information about tags, see [Working with tags](/docs/account?topic=account-tag). |
| Location | The availability zone in your region, inherited from the VPC (for example, us-south-1). You can select a different availability zone in your region from the dropdown menu. |
| Size | Enter a volume size in GBs. Volume sizes can be between 10 GB - 2 TBs. |
| IOPS | Select [IOPS Tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) and then select the tile with performance level you require. <br>Select [Custom](/docs/vpc?topic=vpc-block-storage-profiles#custom) to specify a custom IOPS value based on the size of the volume you're creating. Click the **storage size** link to see a table of size and IOPS ranges. For more information, see [Custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom).</br> |
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. You can also choose **Customer Managed** and use [your own encryption key](/docs/vpc?topic=vpc-block-storage-vpc-encryption). |
{: caption="Table 2. Values for defining a block storage volume" caption-side="top"}

Do you prefer to create block storage volumes by using the CLI? For more information, see [Creating block storage volumes by using the CLI](/docs/vpc?topic=vpc-creating-block-storage-cli).
{: tip}

## Next steps
{: #next-step-creating-block-storage-vpc}

When you refresh the Block storage volumes page, the new volume appears at the beginning of the list of volumes. If the volume was created successfully, it shows a status of Available. For stand-alone volumes, the Attachment Type column is blank (-). The Action menu (...) at the end of a table row provides a link for [attaching a block storage volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).

You can continue creating more block storage volumes or manage existing volumes.

* [View details about a block storage volume](/docs/vpc?topic=vpc-viewing-block-storage)
* [Detach a volume from a virtual server instance](/docs/vpc?topic=vpc-managing-block-storage#detach)
* [Delete a block storage volume](/docs/vpc?topic=vpc-managing-block-storage#delete)
