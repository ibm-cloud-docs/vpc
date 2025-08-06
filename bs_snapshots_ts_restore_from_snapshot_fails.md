---

copyright:
  years: 2021, 2025
lastupdated: "2025-08-06"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my new volume that I restored from a snapshot not moving to 'stable' status?
{: #snapshot_ts_restore_fail}
{: troubleshoot}

A fully restored volume was not created from the snapshot. The API health status shows `degraded`, with the health reason `initializing_from_snapshot` and a message indicates that initializing from the snapshot failed.
{: tsSymptoms}

Some or all of the data wasn't downloaded from {{site.data.keyword.cos_short}} to the volume that's being created. Data blocks were not written.
{: tsCauses}

Contact IBM support to determine the root cause of the failure and resolve the problem.
{: tsResolve}
