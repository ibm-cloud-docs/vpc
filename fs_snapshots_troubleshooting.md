---

copyright:
  years: 2024, 2025
lastupdated: "2025-01-17"

keywords: File storage for VPC, file share snapshots, file share restore

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting {{site.data.keyword.filestorage_vpc_short}} snapshots
{: #fs-snapshots-troubleshooting}



When you create or manage Snapshots for VPC, you might encounter issues. Often, you can recover by following a few steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

## Failure to create a share from a snapshot
{: #fs_snapshot_ts_restore_fail}
{: troubleshoot}

You initiated the creation of a share from a snapshot. However, the new share did not move to the `stable` state because the initialization failed. 
{: tsSymptoms}

A possible reason for such a failure is a problem with the encryption key of either the source share or the new share.
{: tsCauses}

Try to create the share from the snapshot again with a different encryption key.
{: tsResolve}
