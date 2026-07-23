---

copyright:
  years: 2026, 2026
lastupdated: "2026-07-23"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot, snapshot, snapshots_source_volume_busy

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my snapshot creation operation fail with a 'snapshots_source_volume_busy' message?
{: #troubleshooting-block-storage}
{: troubleshoot}
{: support}

Resolve snapshot creation failures that occur when you attempt to create a {{site.data.keyword.block_storage_is_short}} snapshot while another snapshot is already being created from the same source volume.
{: shortdesc}

You receive an error message when you attempt to create a snapshot of a volume while another snapshot creation is in progress for the same volume.
{: tsSymptoms}

A {{site.data.keyword.block_storage_is_short}} volume can process only one snapshot creation operation at a time. When a snapshot creation request is pending, additional snapshot creation requests for the same volume fail with the following errors:
{: tsCauses}

- `snapshots_source_volume_busy`
- `volume is locked`

Wait until the current snapshot request completes, and then retry creating the new snapshot. To check the status of the snapshot, see [Viewing snapshots](/docs/vpc?topic=vpc-snapshots-vpc-view).
{: tsResolve}
