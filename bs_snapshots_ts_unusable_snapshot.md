---

copyright:
  years: 2021, 2025
lastupdated: "2025-09-23"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my snapshot in an 'unusable' state?
{: #snapshot_ts_unusable}
{: troubleshoot}

The snapshot is in an unusable state and the data can't be restored from the snapshot. In this case, the health state is faulted. You can see the API health reason `initializing_from_snapshot` and a message that the snapshot is in an unusable state.
{: tsSymptoms}

The data was not downloaded from the regional storage repository when you tried to restore a volume because the snapshot is not available. A possible cause is that the customer root key for the snapshot was disabled or deleted.
{: tsCauses}

You can [enable a root key](/docs/key-protect?topic=key-protect-disable-keys&interface=ui#enable-ui) or [restore a root key](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-restore-root-key) within 30 days of deletion. Restoring the root key makes the snapshot available. For other issues, contact IBM support to determine the root cause of the failure and fix the problem.
{: tsResolve}
