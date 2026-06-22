---

copyright:
  years: 2019, 2026
lastupdated: "2026-06-22"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I delete the volume by its name or ID with the API or the CLI?
{: #troubleshoot-topic-3}
{: troubleshoot}
{: support}

Resolve issues when unable to delete {{site.data.keyword.block_storage_is_short}} volumes by name or ID, including verification of volume state and attachment status.
{: shortdesc}

You can't delete a {{site.data.keyword.block_storage_is_short}} volume by name or ID.
{: tsSymptoms}

The volume name and ID are not accepted.
{: tsCauses}

Verify that the volume name or identifier is correct and that the volume is not attached to a virtual server instance. Also, verify that the volume is not in a _pending_ state.
{: tsResolve}
