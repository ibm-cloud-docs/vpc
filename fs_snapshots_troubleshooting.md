---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-06"

keywords: File storage for VPC, file share snapshots, file share restore

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my file share creation fail when I used a file share snapshot with a customer-managed encryption key?
{: #fs-snapshots-troubleshooting}
{: troubleshoot}
{: support}

You initiated the creation of a share from a snapshot. However, the new share did not move to the `stable` state because the initialization failed. 
{: tsSymptoms}

A possible reason for such a failure is a problem with the encryption key of either the source share or the new share.
{: tsCauses}

Try to create the share from the snapshot again with a different encryption key.
{: tsResolve}
