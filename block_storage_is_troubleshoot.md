---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-07"

keywords: block storage, IBM Cloud, VPC, virtual private cloud, volume, data storage, troubleshooting, troubleshoot

subcollection: vpc


---

{:support: data-reuse='support'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Troubleshooting {{site.data.keyword.block_storage_is_short}}
{: #troubleshooting-block-storage}

When you create or manage {{site.data.keyword.block_storage_is_short}}, you might encounter issues. Often, you can recover by following a few easy steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{:shortdesc}

## Cannot retrieve a volume in a specified region
{: #troubleshoot-topic-1}
{: troubleshoot}
{: support}

Information about a block storage volume or volumes could not be retrieved for a region.
{: tsSymptoms}

Any of the following causes might apply:
{: tsCauses}

* The volume name and information is missing in either the UI or CLI.
* You might also be attempting to access a volume in a region other than your region.
* You might be trying to access a generation 1 volume.

Verify that the volume wasn't detached from a virtual server instance and deleted. Search for the instance to which you last attached the volume from the list of all virtual server instances:
{: tsResolve}

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

1. Select a virtual server instance from the list of all virtual servers.

If the volume is not attached as expected and does not appear in the list of volumes, it was probably deleted. Because deleting a volume completely removes its data, it cannot be restored.  

If you use the CLI, verify that you entered the correct command syntax for viewing volumes. See [View all block storage volumes from the CLI](/docs/vpc?topic=vpc-viewing-block-storage-cli). Verify that you specified the correct resource group or zone.

## Cannot update a volume name using the API or CLI
{: #troubleshoot-topic-2}
{: troubleshoot}
{: support}

You receive an error message when you attempt to rename an existing volume.
{: tsSymptoms}

This condition might appear in the API or CLI:

* In the API, when you send a PATCH request to rename the volume
* In the CLI, when you specify the `ibmcloud is volume-update` command

You might be renaming the volume with an invalid volume name. In this case, you'll see a 400 "validation_invalid_name" error.
You might also be specifying a valid volume name, but one that already exists in the VPC. For example, if you create two volumes from Gen 1 and Gen 2 compute resources that are in the same account, the same region, and have the same name, you'll see a 400 "volume_name_duplicate" error.
{: tsCauses}

**Note:** The UI prevents you are entering an invalid volume name.

Follow these guidelines for valid volume names:
{: tsResolve}

* Volume names can include a combination of lowercase alpha-numeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters
* Volume names must begin with a lowercase letter
* Volume names must be unique across the VPC infrastructure

## Cannot delete a volume by name or ID
{: #troubleshoot-topic-3}
{: troubleshoot}
{: support}

You can't delete a block storage volume by name or ID.
{: tsSymptoms}

The volume name and ID are not accepted.
{: tsCauses}

Verify that the volume name or identifier is correct and that the volume is not attached to a virtual server instance. Also, verify that the volume is not in a _pending_ state.
{: tsResolve}

## Expandable volume remains in an updating state when deleting an instance
{: #troubleshoot-topic-4}
{: troubleshoot}

When you attempt to delete a virtual server instance with an attached volume that is being resized, the volume remains in an _updating_ state and won't be deleted.
{: tsSymptoms}

A volume is in the process of being resized and you tried to delete the instance to which it's attached, either manually or by auto-delete. The status of the volume remains _updating_ and the volume won't be deleted with the instance.
{: tsCauses}

A volume must in an _available_ state for operations such as attach, detach, delete, and so on. When expanding a volume, wait for the volume resizing to complete before performing any operations. If you try to delete a volume that's resizing, the volume will remain in an _updating_ state and not be deleted with the instance.  To delete the volume, reattach the volume to a different instance, let the resizing complete (volume status becomes _available_, and then delete the volume.
{: tsResolve}
