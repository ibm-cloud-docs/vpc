---

copyright:
  years: 2019, 2026
lastupdated: "2026-06-22"

keywords: Block Storage, virtual private cloud, volume, data storage, troubleshooting, troubleshoot

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my volume resize operation fail with a 'volume is locked' message?
{: #troubleshooting-block-storage}
{: troubleshoot}
{: support}

Resolve volume resize failures when attempting to expand {{site.data.keyword.block_storage_is_short}} volume capacity while a snapshot is being created from the same volume.
{: shortdesc}

If you take a snapshot of a volume and increase the capacity of the source volume while the snapshot is being created, you get an error.
{: tsSymptoms}

While the snapshot is in the _pending_ state, a volume resize operation fails with the error messages `The resize validation failed` and `volume is locked`.
{: tsCauses}

Wait until the snapshot is created and it is in an _available_ state before you resize the source volume.
{: tsResolve}
