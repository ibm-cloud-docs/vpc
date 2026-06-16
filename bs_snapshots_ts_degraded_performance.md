---

copyright:
  years: 2021, 2026
lastupdated: "2026-06-16"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my new volume that I restored from a snapshot not performing as well as my older volumes?
{: #snapshots-vpc-troubleshooting}
{: troubleshoot}

Volumes restored from snapshots show degraded performance during initialization while data downloads from regional storage. Performance returns to normal after download completes.
{: shortdesc}

When you restore a volume from a snapshot, while the data downloads, the API `health_state` and `health_reasons` properties indicate that the volume is being initialized. Performance is degraded during this process. The performance impact applies to snapshots of unattached volumes and snapshots of volumes that are attached to an instance.
{: tsSymptoms}

The data for the snapshot is stored in a regional storage repository. After the volume is created, the data is downloaded from the regional storage repository to the availability zone where the volume is being created. Depending on the snapshot size, the download can take several minutes, at which time some of the data blocks are read or written directly from the regional storage repository, but not all data. You might experience less IOPS and bandwidth than what you'd expect when you create a regular volume. The health state show as `degraded` with health reason `initializing_from_snapshot`.
{: tsCauses}

Wait until the data is fully downloaded to the volume. When the data is fully downloaded, you can expect the normal provisioned IOPS and bandwidth for the volume.
{: tsResolve}
