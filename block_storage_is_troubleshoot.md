---

copyright:
  years: 2019, 2021
lastupdated: "2021-05-21"

keywords: block storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot

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

When you create or manage {{site.data.keyword.block_storage_is_short}}, you might encounter issues. Often, you can recover by following a few steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

## Cannot retrieve a volume in a specified region
{: #troubleshoot-topic-1}
{: troubleshoot}
{: support}

Information about a block storage volume or volumes can't be retrieved for a region.
{: tsSymptoms}

Any of the following causes might apply:
{: tsCauses}

* The volume name and information is missing in either the UI or CLI.
* You might also be attempting to access a volume in a region other than your region.
* You might be trying to access an obsolete volume, created on a deprecated VPC infrastructure.

Verify that the volume wasn't detached from a virtual server instance and deleted. Search for the instance to which you last attached the volume from the list of all virtual server instances:
{: tsResolve}

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}/vpc-ext), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

1. Select a virtual server instance from the list of all virtual servers.

If the volume is not attached as expected and does not appear in the list of volumes, it was probably deleted. Because deleting a volume completely removes its data, it cannot be restored.  

If you use the CLI, verify that you entered the correct command syntax for viewing volumes. See [View all block storage volumes from the CLI](/docs/vpc?topic=vpc-viewing-block-storage-cli). Verify that you specified the correct resource group or zone.

## Cannot update a volume name by using the API or CLI
{: #troubleshoot-topic-2}
{: troubleshoot}
{: support}

You receive an error message when you attempt to rename an existing volume.
{: tsSymptoms}

This condition might appear in the API or CLI:

* In the API, when you send a PATCH request to rename the volume
* In the CLI, when you specify the `ibmcloud is volume-update` command

You might be renaming the volume with an invalid volume name. In this case, you see a 400 "validation_invalid_name" error.
You might also be specifying a valid volume name, but one that exists in the VPC. For example, if you create two volumes from compute resources that are in the same account, the same region, and have the same name, you see a 400 "volume_name_duplicate" error.
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

## Expandable volume remains in an updating state when an attempt is made to delete an instance
{: #troubleshoot-topic-4}
{: troubleshoot}

When you attempt to delete a virtual server instance with an attached volume that is being resized, the volume remains in an _updating_ state and can't be deleted.
{: tsSymptoms}

A volume is being resized and you tried to delete the instance that the volume is attached to, either manually or by auto-delete. The status of the volume remains _updating_ and the volume isn't deleted with the instance.
{: tsCauses}

A volume must be in an _available_ state for operations such as attach, detach, delete. When you are expanding a volume, wait for the volume resize to complete before you perform any operations. If you try to delete a volume that's resizing, the volume remains in an _updating_ state and not be deleted with the instance. To delete the volume, reattach the volume to a different instance, and make sure that the resizing is complete (volume status becomes _available_, and then delete the volume.
{: tsResolve}

## Removing IAM authorization from the storage service to the KMS causes root key deregistration failure
{: #troubleshoot-topic-5}
{: troubleshoot}

The root keys in the Key management service (KMS) instance remain registered to the deleted block storage volume or image resources.
{: tsSymptoms}

If you remove IAM authorization from Cloud Block Storage to the KMS before you delete all BYOK volumes or images, the root key fails to unregister from the resource.
{: tsCauses}

As best practice, delete all storage or image resources before you remove IAM authorization. If you already removed authorization, you must restore the IAM authorization between Cloud Block Storage Cloud Block Storage (source service) and your KMS (target service). For more information, see [Using authorizations to grant access between services](/docs/account?topic=account-serviceauth) to establish IAM service-to-service authorizations by using the UI, CLI, or API.
{: tsResolve}

## Resolving volume resize issues while a snapshot is taken
{: #troubleshoot-topic-6}

If you take a snapshot of a volume and resize the source volume while the snapshot is being created, you get an error.
{: tsSymptoms}

While the snapshot is in a _pending_ state, a volume resize error displays with the message "The resize validation failed." The correct message says, "volume is locked."
{: tsCauses}

Wait until the snapshot is created and in an _available_ state before you resize the source volume.
{: tsResolve}
