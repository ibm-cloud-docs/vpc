---

copyright:
  years: 2021, 2026
lastupdated: "2026-07-08"

keywords: snapshot, restore, volume, degraded, initializing_from_snapshot, block storage

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my new volume that I restored from a snapshot not moving to 'stable' status?
{: #snapshot_ts_restore_fail}
{: troubleshoot}

Resolve issues where a volume that is restored from a snapshot remains in a degraded state and does not reach stable status due to a failed initialization.
{: shortdesc}

The volume was not successfully restored from the snapshot. The API health status shows `degraded`, with the health reason `initializing_from_snapshot`, indicating that initializing from the snapshot failed.
{: tsSymptoms}

Some or all of the data wasn't downloaded from the regional storage repository to the volume that's being created. Data blocks were not written.
{: tsCauses}

Contact IBM support to determine the root cause of the failure and resolve the problem.
{: tsResolve}
