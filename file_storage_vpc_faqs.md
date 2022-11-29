---

copyright:
  years: 2021, 2022
lastupdated: "2022-11-08"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for {{site.data.keyword.filestorage_vpc_short}}
{: #file-storage-vpc-faqs}

The following questions often arise about {{site.data.keyword.filestorage_vpc_short}}. If you have other questions you'd like to see addressed here, provide feedback by using the **Open doc issue** or **Edit topic** links.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Offering questions
{: #file-storage-vpc-offering-questions}

### Do I need to create a VPC before I can create a file share?
{: faq}
{: #faq-fs-2}

No. You can create a file share independent of a VPC. However, to create a mount target, you must have a VPC available. To mount a file share, you must provision a virtual server instance within that VPC.

### I have existing VPCs. Can I create a file share within that VPC?
{: faq}
{: #faq-fs-3}

Yes.

### What interfaces are supported for this release to create file shares?
{: faq}
{: #faq-fs-4}

You can use the UI, CLI, or API to create and manage your file shares.

### What functionality is supported in this release?
{: faq}
{: #faq-fs-5}
{: support}

In this release, you can:

*	Create a file share.
*	Mount a file share to one or multiple virtual server instances across multiple VPCs within the same zone.
*	Delete a file share.
*	Delete all mount targets or delete a single mount target. When you delete one or several mount targets, the instances that are mounting the file share for the VPCs where the mount target is deleted can't access the file share.
*	List file shares and mount targets.
*	Update file share and mount target name.
* Create replication between the source file share and a replica file share.
* Fail over to a replica file share when the source share is compromised. Fall back to the original share when the issue is resolved.
* Add, manage, and delete file share user tags. Create and manage access management tags.

### Who do I contact to help with any issues? What information do I need to provide?
{: faq}
{: #faq-fs-7}

For more information about who to contact, see [Getting help and support](/docs/vpc?topic=vpc-getting-help). Provide as much information as you can, including error messages, screen captures, and API error codes and responses. Include any messages from the VPC and the file storage service.


## File share management questions
{: #file-storage-vpc-management-questions}

### Can I mount the same file share in different zones in my region?
{: faq}
{: #faq-fs-mgt-1}
{: support}

No. As a zonal service, file shares that are created for a zone are accessible to instances only within that zone. For this release, {{site.data.keyword.filestorage_vpc_short}} is available in single zones in the Dallas (us-south), Washington DC (us-east), Frankfurt (eu-de), and London (eu-gb) regions for allow-listed {{site.data.keyword.cloud}} customer accounts.

### Can I mount file shares for my Kube containers?
{: faq}
{: #faq-fs-mgt-3}

Yes. You can mount file shares by using the NFSv4.1 protocol.

### Can I mount same file shares between two virtual server instances?
{: faq}
{: #faq-fs-mgt-4}

Yes, when the virtual server instances are in the same zone. A file share can't be mounted to resources in two separate zones.

### I use a load balancer across zones. Is there a way to copy the file share?
{: faq}
{: #faq-fs-mgt-5}

No.

### Are there any options for backup for data retention?
{: faq}
{: #faq-fs-mgt-6}

No. As a best practice, independently back up your data. When your file share data is deleted, it can't be restored.

### Are file shares elastic?
{: faq}
{: #faq-fs-mgt-7}

File shares are not elastic. Currently, you can provision minimum of 10 GB to maximum of 16 TB file shares.

### Can I change the size of a file share?
{: faq}
{: #faq-fs-mgt-8}

You can increase the size of a file share from its original capacity in GB increments up to 32,000 GB capacity, depending on your file share profile. For more information, see [Expanding file share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity).

### Is there a way to replicate my file shares in case of events?
{: faq}
{: #faq-fs-mgt-9}

Yes. You can create replicas of your file shares by setting up a replication relationship between primary file shares in one zone to replica file share in another zone. Using replication is a good way to recover from incidents at the primary site, when data becomes inaccessible or applications fail. For more information, see [About file share replication](/docs/vpc?topic=vpc-file-storage-replication&interface=ui).

### How does file share replication work?
{: faq}
{: #faq-fs-mgt-10}

when you create a file share, you can set up a replication relationship between a primary source file share to a replica file share in a different zone. When the file share is created, so is the replica share in the other zone. When the replication relationship is established, the replica file share begins pulling data from the source file share. The replica file share is read-only until you break the replication relationship, creating two independent file shares, or fail over to the replica file share. For more information about setting up replication, see [Creating replica file shares](/docs/vpc?topic=vpc-file-storage-create-replication&interface=ui).

### How do I schedule replication?
{: faq}
{: #faq-fs-mgt-11}

You can choose the frequency of replication by creating a schedule with a `cronspec` and can replicate as frequently as every hour. Set up replication from the UI, CLI, or by calling the API.

### I want to set up replication. Is there automatic failover?
{: faq}
{: #faq-fs-mgt-12}

No, choosing to fail over to the replica site is a manual operation, and you have to reconcile your data after failing over the the replica share. For more information about how failover works for disaster recovery, see [Failover for disaster recovery](/docs/vpc?topic=vpc-file-storage-failover&interface=ui#fs-failover-dr).

### Can I add tags to my file shares?
{: faq}
{: #faq-fs-mgt-13}

Yes. You can specify user and access management tags when you create a file share or update an existing file share. Adding user tags to a file share or replica share can make organizing your resources easier. For more information, see [Add user tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=api#fs-add-user-tags). {{site.data.keyword.filestorage_vpc_short}} also supports access management tags. For more information, see [Access management tags for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-about-mgt-tags).

## Performance questions
{: #file-storage-vpc-performance-questions}

### What read/write latency can I expect?
{: faq}
{: #faq-fs-perf-1}
{: support}

You can expect an average latency less than 100 ms for writes and less than 50 ms for reads for block sizes less than one MB.

### Can I adjust the performance of my file shares?
{: faq}
{: #faq-fs-perf-2}

Yes, you can increase or decrease IOPS for file shares based on an IOPS tier profile or custom profile. Adjusting IOPS is dependent on the file share size. There's no outage or lack of access to the storage while adjusting IOPS. For more information, see [Adjusting file share IOPS](/docs/vpc?topic=vpc-adjusting-share-iops&interface=ui).

## Data security and encryption questions
{: #file-storage-vpc-security-questions}

### How secure is my data?
{: faq}
{: #faq-fs-sec-1}
{: support}

All data-at-rest is encrypted by default with IBM-managed encryption. You can also encrypt your file shares with your own root keys, which gives your more control over your data security. For example, you can rotate, suspend, delete, and restore your root keys. For more information about his option, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption).

Data-in-transit is not supported in this release.

### Is there support for security groups and network ACLs?
{: faq}
{: #faq-fs-sec-2}

No.

### How is my data protected in a file share? Can I use my own encryption keys?
{: faq}
{: #faq-fs-sec-3}

By default, your file share data is protected at rest with IBM-managed encryption. You can also bring your own keys to the {{site.data.keyword.cloud}} and use them to encrypt your file shares. For more information, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption). By using the API, you can link a primary account containing a root key to a secondary account, then use that key to encrypt new file shares in the secondary account. For more information, see [Cross-account encryption for multitenant storage resources](/docs/vpc?topic=vpc-getting-started).
