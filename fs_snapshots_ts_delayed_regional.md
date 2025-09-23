---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-23"

keywords: File storage for VPC, file share snapshots, file share restore

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my snapshot not showing up in the `.snap` folder?
{: #fs-snapshots-delayed-regional-snapshot}
{: troubleshoot}
{: support}

You created a snapshot of a regional file share in the console, from the CLI, or the API. The operation succeeded without errors. However, the new share snapshot does not appear to be in the `.snap` folder.
{: tsSymptoms}

A possible reason for the missing snapshot is a delay due to client-side caching.
{: tsCauses}

Run the following command to refresh the cache.
{: tsResolve}

```sh
sudo sh -c "echo 3 > /proc/sys/vm/drop_caches"
```
{: pre}
