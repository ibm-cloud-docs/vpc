---

copyright:
  years: 2019
lastupdated: "2019-07-29"

keywords:  block storage, IBM Cloud, VPC, virtual private cloud, block storage volume, volume, volume attachment, virtual server instance, instance
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

# Attaching a block storage volume using the UI
{: #attaching-block-storage}

When you create a {{site.data.keyword.block_storage_is_short}} volume for a virtual server instance from the UI, the volume is attached to the instance by default. When you detach a volume, it exists as an unattached volume that you can later reattach.  These available volumes are displayed in the list of [all block storage volumes](/docs/vpc?topic=vpc-viewing-block-storage#viewvols). You can attach the volume to another instance from the list of all block storage volumes or when viewing details about a particular instance.
{:shortdesc}

## Volume attachment limits
{: #vol-attach-limits}

Although you can attach only one block storage volume to a virtual server instance at a time, you can attach several different block storage volumes to a single instance, with the following limits:

* Instances with less than 4 virtual CPUs can attach up to 4 block storage secondary volumes, plus the boot volume.
* Instances with 4 or more virtual CPUs can attach up to 12 block storage secondary volumes, plus the boot volume.

## Attach a block storage volume to a virtual server instance
{: #attach}

From the list of all block storage volumes, follow these steps.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc){: external} for the Virtual Private Cloud, navigate to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Storage > Block storage**.
1. In the list of volumes, click the elipsis at the end of a row for an available, unattached volume.  A context-specific action menu displays.
1. Select **Attach to instance**.
1. Select a compute resource (virtual server instance) from the list of available resources, and then click **Attach**.
1. Messages display on the volume details page indicating that the volume is being attached to the image.  When it completes, the image name appears under **Attached instances**.

You can also attach a block storage volume from the virtual server instance details page.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc){: external} for the Virtual Private Cloud, navigate to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Compute > Virtual server instances**.
1. Select an instance from the list of all virtual server instances. If there are any block storage volumes attached, you'll see them listed under **Attached block storage volumes**.
1. Select **Attach volume**.
1. Select a volume from the list of available resources and click **Attach**. Messages display on the instance details page indicating that the volume is being attached.  When it completes, the **Attached block storage volumes** list is updated to include the new volume.

You can also manually attach block storage volumes using the CLI. For information, see [Attaching block storage volumes (CLI)](/docs/vpc?topic=vpc-attaching-block-storage-cli).
{: tip}

## Next steps
{: #next-step-attaching-block-storage}

Create additional volumes and manage existing ones. See the following information.

* [Creating block storage volumes in IBM Cloud console](/docs/vpc?topic=vpc-creating-block-storage)
* [Managing block storage volumes using the UI](/docs/vpc?topic=vpc-managing-block-storage)
