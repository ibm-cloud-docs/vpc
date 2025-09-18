---

copyright:
  years: 2021, 2025
lastupdated: "2025-09-18"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my new volume that I restored from a snapshot not performing as well as my older volumes?
{: #snapshots-vpc-troubleshooting}
{: troubleshoot}

When you restore a volume from a snapshot, while the data downloads, the API `health_state` and `health_reasons` properties indicate that the snapshot is being initialized. Performance is degraded while the volume is being initialized from the snapshot. This performance impact applies to snapshots of unattached volumes and snapshots of volumes that are attached to an instance.
{: tsSymptoms}

The data for the snapshot is stored in {{site.data.keyword.cos_full}}. After the volume is created, the data is downloaded from {{site.data.keyword.cos_short}} bucket to the availability zone where the volume is being created. Depending on the snapshot size, the download can take several minutes, at which time some of the data blocks are read or written directly from {{site.data.keyword.cos_short}}, but not all data. You might experience less IOPS and bandwidth than what you'd expect when you create a regular volume. The health state show as `degraded` with health reason `initializing_from_snapshot`.
{: tsCauses}

Wait until the data is fully downloaded from {{site.data.keyword.cos_short}} to the volume. When the data is fully downloaded, you can expect the normal provisioned IOPS and bandwidth for the volume.
{: tsResolve}
