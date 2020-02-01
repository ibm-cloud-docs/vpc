---

copyright:
  years: 2019, 2020
lastupdated: "2019-11-06"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Viewing block storage volume details
{: #viewing-block-storage}

View details about a {{site.data.keyword.block_storage_is_short}} volume or summary information about all volumes.
{:shortdesc}

## View information about all block storage volumes
{: #viewvols}

Navigate to the list of block storage volumes. In [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.

By default, block storage volumes display for all resource groups in your region. In the list of all **Block storage for VPC volumes**, you'll see the following information.

| Field | Description |
|-------|-------------|
| Status | Status of the volume, which functions as the default filter for all rows. For information about volume statuses, see [Block Storage volume statuses](/docs/vpc?topic=vpc-managing-block-storage#status). |
| Name | Click the name of the volume to see individual volume details. |
| Resource Group | Resource groups are defined when you set up your VPC. Resource groups help organize your account resources for access control and billing purposes. For information, see [Best practices for organizing resources in a resource group](/docs/resources?topic=resources-bp_resourcegroups). |
| Location | Availability zone in your region, inherited from the VPC (for example, US South 1).|
| Size | Size of the volume you specified, in GBs.|
| Max IOPS | Maximum IOPS available on the volume, defined by the general-purpose IOPS tier or custom IOPS value you specified. |
| Attachment type | Data, for a secondary volume attached to an instance, boot when attached as a boot volume, or blank for an unattached volume.|
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes |
| Actions (...) | Click the ellipsis to display a menu of context-specific actions you can take. For example, an available, unattached volume would have menu options for attaching to an instance or deleting the volume.|
{: caption="Table 1. Details about all volumes" caption-side="top"}

By default, 10 volumes are shown in the list of all block storage volumes.  Change this default by clicking the Page Control down arrow and increase the list to 20 or 50 volumes.  Use the Page Control arrows on the lower-right to navigate to the following page or return to the previous page.

## View details about a block storage volume
{: #view-vol-details}

To view details about a block storage volume, navigate to the list of all block storage volumes and select a volume. For a single volume, you'll see the following information.

| Field | Description |
|-------|-------------|
| Name  | Name of the volume you specified when you created the volume. Click the pencil icon to edit the volume name. The volume name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. |
| Resource group | Resource group defined when you set up your VPC. Resource groups manage access to resources but do not affect billing or monitoring.|
| Attachment type | Data, for a secondary volume attached to an instance, boot when attached as a boot volume, or blank for an unattached volume.|
| ID | System-generated volume ID. |
| Created | System-generated date when the volume was created.|
| Location | Availability zone in your region.|
| Size | Size of the volume you specified.|
| Profile | IOPS tier or custom IOPS profile.|
| Max IOPS | Maximum IOPS value for a predefined IOPS tier or the value you specified for custom IOPS. |
| Throughput | The performance a disk can deliver, measured in Gigabits/second (Gbps). Based on your volume profile, throughput is calculated as the amount of IOPS * 16K block size.|
| Encryption | Encryption with IBM-managed keys is enabled by default on all volumes. |
{: caption="Table 2. Volume details" caption-side="top"}

Volumes attached to a virtual server instance are displayed under **Attached instances** on the **Volume details** page. You can also [attach a volume to an instance](/docs/vpc?topic=vpc-attaching-block-storage).

For a volume attached to an instance, you can also navigate to information about the instance and then return to a list of all Block Storage volumes.

## View attached block storage volume details in instance details

You can view information about an attached block storage volume from the **Virtual server instance details** page:

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances** and select an instance.
1. Under **Attached block storage volumes**, click the name of a volume to go to the volume details page.

Do you prefer to viewing block storage volumes by using the CLI? For information, see [Viewing block storage volumes (CLI)](/docs/vpc?topic=vpc-viewing-block-storage-cli).
{: tip}

## Next steps
{: #next-step-viewing-block-storage}

Create more volumes or manage your existing block storage volumes.

* [Creating block storage volumes in IBM Cloud console](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing block storage volumes by using the UI](/docs/vpc?topic=vpc-managing-block-storage)
