---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-20"

keywords: confidential computing, secure execution, security claims, disks encryption

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Verifying disk encryption status
{: #hpvs-disks-encryption-validate}

The {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for VPC is deprecated. As of 28 February 2026, you can't create new instances. Existing instances are supported until 20 February 2027. Any instances that still exist on that date will be deleted. You can redeploy your workloads by using [IBM Confidential Computing Container Runtime (formerly known as Hyper Protect Virtual Servers)](https://www.ibm.com/docs/en/hpvs) or [IBM Confidential Computing Container Runtime for Red Hat Virtualization Solutions (formerly known as Hyper Protect Container Runtime for Red Hat Virtualization Solutions)](https://www.ibm.com/docs/en/hpcr/1.1.x). For information about data migration, see the [Migration guide](/docs/vpc?topic=vpc-migration_guide). For more information, see the [Service deprecation announcement](/docs/vpc?topic=vpc-ichpcs_deprecated_anmt).
{: deprecated}

Both the root disk and data disks in the {{site.data.keyword.hpvs}} for VPC instance are encrypted with Linux Unified Key Setup (LUKS) Encryption. You can verify the encryption status by checking the messages in the log.

- The root disk is re-created and encrypted on every boot with the original content. A passphrase for the root disk is not stored.

- The data disk encryption is configured by using the “seeds” provided in the `workload` and `env` sections within the contract and, if you enable the [integration](/docs/vpc?topic=vpc-hyper-protect-virtual-server-mng-data), a third seed from Hyper Protect Crypto Services. During the instance initiation, the disks are attached and encrypted by using the seeds to create a LUKS passphrase. If the seed information or the data disk is not configured, the instance fails to initiate. For more information, see [The `workload` - `volumes` subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_volumes) of the contract.

The disk encryption status check daemon checks the crypto headers of the root disk and data disks that are attached to the instance each hour. Then, it writes the information messages about the disk encryption status into the log.

The following example shows the disk encryption check related messages within the log.

```text
...
Nov 29 10:24:07 hpvs211vsi verify-disk-encryption info HPL13000I: Verify LUKS Encryption...
Nov 29 10:24:07 hpvs211vsi systemd info verify-disk-encryption-invoker.service: Succeeded.
...
Nov 29 10:24:08 hpvs211vsi verify-disk-encryption info Checked for mount point /mnt/data, LUKS encryption with 1 key slot found
Nov 29 10:24:08 hpvs211vsi verify-disk-encryption info HPL13001I: Root disk and all the data disks are encrypted
...
```
{: codeblock}

**Note:** 

1. You can't configure the interval of the disk encryption status checks.

2. To address Denial-of-Service attacks, requests are throttled at three per five minutes.

3. To verify that the mounted data volume is encrypted, you can trigger explicitly `echo 'HPL13000I' | systemd-cat` on the VSI, and the results are captured in the logs.
