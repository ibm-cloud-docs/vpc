---

copyright:
  years: 2019, 2021
lastupdated: "2021-02-01"

keywords: block storage, virtual private cloud, boot volume, data volume, volume, data storage, virtual server instance, instance

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
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Managing block storage volumes by using the UI
{: #managing-block-storage}

Manage {{site.data.keyword.block_storage_is_short}} from the UI. Detach a volume from a virtual server instance or transfer a volume from one instance to another. Attach a previously attached volume or rename a volume. Set automatic volume deletion or manually delete a volume. Assign access to a volume.
{:shortdesc}

## Detach a block storage volume from a virtual server instance
{: #detach}
{: help}
{: support}

You can detach a block storage volume that is attached to a virtual server instance. Detaching frees the volume for use by another instance.

To detach a volume:

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Locate the volume and then click overflow icon (...) to open a list of options.
1. From the options menu, click **Detach from instance**.
1. Confirm by clicking **Detach instance** in the pop-up.

Alternatively, you can click an individual volume in the list of all block storage volumes and navigate to the  **Volume Details** page for that volume. Under **Attached instances**, click the minus sign next to the virtual server instance to detach the volume from that instance.

## Transfer a block storage volume from one virtual server instance to another
{: #transfer}

To transfer a block storage volume to another virtual server instance:

1. [Detach the volume from its virtual server instance](#detach).
1. Navigate to the virtual server instance to which you want to transfer the volume. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
1. Select a virtual server instance from the list.
1. Under Attached Storage volumes, click the plus sign to add a volume. All block storage volumes are displayed.
1. From the list of volumes, select the volume that you previously detached.

## Attach a previously attached block storage data volume
{: #reattach}

A block storage data volume is attached by default when you create the volume during virtual server instance creation. When you detach a volume from an instance, it exists as an unattached volume and is displayed in the list of [all block storage volumes](/docs/vpc?topic=vpc-viewing-block-storage#viewvols). You can attach it to another instance from the list of block storage volumes.

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Locate the volume and then click overflow icon (...) to open a list of options.
1. From the options menu, click **Attach to instance**.
1. Select an available virtual server instance.
1. Confirm your selection.

## Rename a block storage volume
{: #rename}

You can change the name of an existing volume to make it more meaningful.

1. Go to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Locate the volume and then click the name of the volume to go to the Volume Details page.
1. Click the pencil icon after the name of the volume to edit the name. Provide a [valid volume name](/docs/vpc?topic=vpc-managing-block-storage#volume-name-conventions).
1. Confirm your edit.

## Guidelines for naming volumes
{: #volume-name-conventions}

Valid volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter.

Volume names must be unique across the entire VPC infrastructure. For example, if you create two volumes from Gen 1 and Gen 2 compute resources that are in the same account, the same region, and have the same name, a "volume name duplicate" error is triggered.

## Delete a block storage data volume
{: #block-storage-delete}
{: help}
{: support}

Deleting a block storage volume completely removes its data. The volume cannot be restored.

You cannot delete an active block storage volume. To delete a volume, first [detach it](#detach) from the virtual server instance, then follow these steps:

1. Navigate to the list of all block storage volumes. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
1. Locate the volume that you want to delete and then click the overflow icon (...) to open a list of options.
1. From the options menu, click **Delete**.
1. Confirm the deletion.

## Automatic deletion of block storage data volumes
{: #auto-delete}

Using the Auto Delete feature, you can specify that a block storage data volume is automatically deleted when you delete an instance to which it is attached.

**Note**: You don't have to set automatic deletion for boot volumes. Boot volumes are created during instance creation and automatic deletion is enabled by default. When you delete the instance, the boot volume is also deleted.

To enable Auto Delete for an existing block storage data volume that is attached to an instance, follow these steps:

1. Locate the virtual server instance to which the data volume is attached. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
1. Under **Attached block storage volumes**, select a volume.
1. On the next page, click the Auto Delete button to enable.
1. Confirm your selection.

Alternatively, begin by selecting a data volume from the list of block storage volumes (**Storage > Block storage volumes**).

To enable Auto Delete on a new data volume when you create an instance, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi).

## Block storage data eradication
{: #block-storage-data-eradication}

When you delete a block storage volume, that data immediately becomes inaccessible. All pointers to the data on the physical disk are removed. If you later create a new volume in the same or another account, a new set of pointers is assigned. The account can't access any data that was on the physical storage because those pointers are deleted. When new data is written to the disk, any inaccessible data from the deleted volume is overwritten.

IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten and eradicated. Further, when you delete a block storage volume, those blocks must be overwritten before that block storage is made available again, either to you or to another customer.

## Assign access to a block storage volume
{: #assign-volume-access}

With Administrator privileges, you can assign volume-level user access to the {{site.data.keyword.block_storage_is_short}} service. The following table shows the information that you must provide in the [Identity and Access Management (IAM) UI](/docs/account?topic=account-account_setup).

| IAM field | Action |
|--------|-------------|
| Services | Select **Infrastructure Services** |
| Resource Type | Select **Block Storage for VPC** |
| Volume ID | Enter the [volume ID](/docs/vpc?topic=vpc-viewing-block-storage#view-vol-details) to assign access to a specific volume |
| Select Roles | Assign platform access roles, typically, Operator |
{: caption="Table 1. Information for IAM" caption-side="top"}

For more information, see the [best practices for assigning access](/docs/account?topic=account-account_setup#account_setup). For the complete IAM process, which includes inviting users to your account and assigning Cloud IAM access, see the [IAM getting started tutorial](/docs/account?topic=account-iamoverview).

## Block storage volume status
{: #status}

The following table shows statuses that you might see when you create, view, or manage your block storage volumes.

| Status | Meaning |
|--------|-------------|
| Available | A volume is available and can be attached to an instance |
| | An attached data volume is available |
| | A boot volume is available |
| Failed  | A volume failed to create |
| Pending | A volume is being created |
| Pending deletion | A volume is being deleted. If you're unsure the volume is deleted, verify this state. Attempting to delete a volume again while in this state results in a conflict error. |
| Unusable | A volume is unusable because the customer root key (CRK) has been [deleted](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys) or [disabled](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys). |
{: caption="Table 2. Block storage statuses" caption-side="top"}

Do you prefer to manage block storage volumes by using the CLI? For information, see [Managing block storage volumes (CLI)](/docs/vpc?topic=vpc-managing-block-storage-cli).
{: tip}

## Next steps
{: #next-step-managing-block-storage}

You can [create more volumes](/docs/vpc?topic=vpc-creating-block-storage).

For issues with existing block storage volumes, you might be able to troubleshoot and fix the problems yourself. For more information, see [Troubleshooting {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-troubleshooting-block-storage).
