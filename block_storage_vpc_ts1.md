---

copyright:
  years: 2019, 2026
lastupdated: "2026-06-22"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I retrieve a volume in a specified region?
{: #troubleshoot-topic-1}
{: troubleshoot}
{: support}

Resolve issues when {{site.data.keyword.block_storage_is_short}} volume information cannot be retrieved in a specific region, including missing volume names in the UI or CLI.
{: shortdesc}

Information about a {{site.data.keyword.block_storage_is_short}} volume or volumes cannot be retrieved for a region.
{: tsSymptoms}

Any of the following causes might apply:
{: tsCauses}

* The volume name and information is missing in either the UI or CLI.
* You might also be attempting to access a volume in a region other than your region.
* You might be trying to access an obsolete volume that was created on a deprecated VPC infrastructure.

Verify that the volume wasn't detached from a virtual server instance and deleted. Search for the instance to which you last attached the volume from the list of all virtual server instances:
{: tsResolve}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances**.

1. Select a virtual server instance from the list of all virtual servers.

If the volume is not attached as expected and does not appear in the list of volumes, it was probably deleted. Because deleting a volume completely removes its data, it cannot be restored.

If you use the CLI, verify that you entered the correct command syntax for viewing volumes. See [Viewing all Block Storage volumes from the CLI](/docs/vpc?topic=vpc-viewing-block-storage&interface=cli#viewall-vol-cli). Verify that you specified the correct resource group or zone.
