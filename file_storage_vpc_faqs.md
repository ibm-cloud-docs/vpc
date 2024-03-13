---

copyright:
  years: 2021, 2024
lastupdated: "2024-03-06"

keywords: file share, file storage, replication, replica, size increase, capacity, encryption, BYOK, security group

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for {{site.data.keyword.filestorage_vpc_short}}
{: #file-storage-vpc-faqs}

The following questions often arise about {{site.data.keyword.filestorage_vpc_short}}. If you have other questions you'd like to see addressed here, provide feedback by using the **Open doc issue** or **Edit topic** links.
{: shortdesc}

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

### Can I mount file shares on a Windows operating system?
{: faq}
{: #faq-fs-os}

No, file shares can be mounted only on supported on Linux operating systems or a z/OS-based {{site.data.keyword.cloud}} Compute Instance that support NFS file shares. For more information, see the mounting information for [Red Hat](/docs/vpc?topic=vpc-file-storage-vpc-mount-RHEL), [CentOS](/docs/vpc?topic=vpc-file-storage-mount-centos), and [Ubuntu](/docs/vpc?topic=vpc-file-storage-vpc-mount-ubuntu) Linux distributions, or [z/OS](/docs/vpc?topic=vpc-file-storage-vpc-mount-zos) systems.

### What is the minimum NFS version supported?
{: faq}
{: #faq-nfs-version}

{{site.data.keyword.filestorage_vpc_short}} requires NFS versions v4.1 or higher.

### Who do I contact to help with any issues? What information do I need to provide?
{: faq}
{: #faq-fs-7}

For more information about who to contact, see [Getting help and support](/docs/vpc?topic=vpc-help-and-support-vpc). Provide as much information as you can, including error messages, screen captures, and API error codes and responses. Include any messages from the VPC and the file storage service.

### How am I charged for usage?
{: faq}
{: #faq-fs-billing}

Cost for {{site.data.keyword.filestorage_vpc_short}} is calculated based on the GiB capacity that is stored per month, unless the duration is less than one month. The share exists on the account until you delete the share or you reach the end of a billing cycle, whichever comes first.

Pricing is also affected when you [expand share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity) or [adjust IOPS](/docs/vpc?topic=vpc-adjusting-share-iops). For example, expanding volume capacity increases costs, and decreasing the IOPS value decreases the monthly and hourly rate. Billing for an updated volume is automatically updated to add the prorated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

You can use the Cost estimator ![Cost estimator icon](../icons/calculator.svg "Cost estimator") in {{site.data.keyword.cloud_notm}} console to see how changes in capacity and IOPS affect the cost. For more information, see [Estimating your costs](/docs/billing-usage?topic=billing-usage-cost).

You also incur charges when you [replicate data](/docs/vpc?topic=vpc-file-storage-replication) to a different region. Charges for data transfer between the two file shares are calculated with a flat rate in GiB increments. The charges are based on the amount of data that was transferred during the entire billing period. You can use the [replication sync information](/docs/vpc?topic=vpc-file-storage-manage-replication#fs-repl-syncinfo) to see the transferred data values, which can help you estimate the global transfer charges at the end of the billing period.

## File share management questions
{: #file-storage-vpc-management-questions}

### Can I mount the same file share in different zones in my region?
{: faq}
{: #faq-fs-mgt-1}
{: support}

Yes, you can mount file shares across different zones in your region. For more information, see [Cross-zone mount targets](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-cross-zone-mount).

### Can I mount file shares for my Kube containers?
{: faq}
{: #faq-fs-mgt-3}

Yes. You can mount file shares by using the NFSv4.1 protocol.

### Can I mount the same file shares between two virtual server instances?
{: faq}
{: #faq-fs-mgt-4}

Yes, when the virtual server instances are in the same region.

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

File shares are not elastic. Currently, you can provision a minimum of 10 GiB to a maximum of 32,000 GiB file shares, depending on the [file share profile](/docs/vpc?topic=vpc-file-storage-profiles).

### Can I change the size of a file share?
{: faq}
{: #faq-fs-mgt-8}

You can increase the size of a file share from its original capacity in GiB increments up to 32,000 GiB capacity, depending on your [file share profile](/docs/vpc?topic=vpc-file-storage-profiles). For more information, see [expanding file share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity).

### Is there a way to replicate my file shares to protect my data from disastrous events?
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

### I want to set up replication. Is there an automatic failover?
{: faq}
{: #faq-fs-mgt-12}

No, choosing to fail over to the replica site is a manual operation, and you must reconcile your data after the failover to the replica share is done. For more information about how failover works for disaster recovery, see [Failover for disaster recovery](/docs/vpc?topic=vpc-file-storage-failover&interface=ui#fs-failover-dr).

### Can I add tags to my file shares?
{: faq}
{: #faq-fs-mgt-13}

Yes. You can specify user and access management tags when you create a file share or update an existing file share. Adding user tags to a file share or replica share can make organizing your resources easier. For more information, see [Add user tags to a file share](/docs/vpc?topic=vpc-file-storage-managing&interface=api#fs-add-user-tags). {{site.data.keyword.filestorage_vpc_short}} also supports access management tags. For more information, see [Access management tags for file shares](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-about-mgt-tags).

### What is the dp2 profile?
{: faq}
{: #faq-fs-mgt-dp2}

The **dp2** profile is the latest file storage profile, offering greater capacity and performance for your file shares. With this profile, you can specify the total IOPS for the file share within the range for a specific file share size. You can provision shares with IOPS performance from 100 IOPS to 96,000 IOPS, based on share size. For more information, see [dp2 file storage profile](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile).

### Can I migrate all my file share profiles to dp2?
{: faq}
{: #faq-fs-mgt-migrate-dp2}

You can migrate file shares that were created by using either the IOPS tier profile or custom IOPS profile to the latest dp2 profile. By migrating to the [dp2 profile](/docs/vpc?topic=vpc-file-storage-profiles#dp2-profile), you can take advantage of the latest {{site.data.keyword.filestorage_vpc_short}} features. Currently, you can use the {{site.data.keyword.filestorage_vpc_short}} UI, CLI, or API to revise a single file share profile. For migrating multiple shares, you need to create your own script that would first list these shares and then go through the list of shares and update each individual share profile.

### Can I restrict access to my file share to a specific virtual server instance?
{: faq}
{: #faq-fs-access-mode}

Yes. When you create a file share, you must specify the access control mode. It can either be based on Security Groups, which restrict the access to the file share to specific resources in the VPC. Or the access mode can allow for VPC-wide file share mounting. For more information, see [Mount target access modes](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode).

## Performance questions
{: #file-storage-vpc-performance-questions}

### Can I adjust the performance of my file shares?
{: faq}
{: #faq-fs-perf-2}

Yes, you can increase or decrease IOPS for file shares based on an **IOPS tier**, **custom**, or **dp2** profile. Adjusting IOPS depends on the file share size. Adjusting the IOPS causes no outage or lack of access to the storage. Pricing is adjusted with your selection. For more information, see [Adjusting file share IOPS](/docs/vpc?topic=vpc-adjusting-share-iops&interface=ui).

### Can I change a file share profile?
{: faq}
{: #faq-fs-update-profile}

Yes, you can use the UI, CLI, or API to update a file share profile. You can change among IOPS tier profiles. When you select a custom profile or dp2 high-performance profile, you specify the maximum IOPS based on the file share size.

You can't use the UI, CLI, or API to update multiple file shares in a single operation. For more on this issue, see [troubleshooting {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-troubleshooting-file-storage).
{: note}

## Data security and encryption questions
{: #file-storage-vpc-security-questions}

### How secure is my data?
{: faq}
{: #faq-fs-sec-1}
{: support}

All data is encrypted at rest by default with IBM-managed encryption. You can also encrypt your file shares with your own root key, which gives your more control over your data security. For example, you can rotate, suspend, delete, and restore your root keys. For more information, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption).

You can also enable secure end-to-end encryption of your file share data by setting up data encryption in transit. When encryption in transit is enabled, you can establish an encrypted mount connection between the virtual server instance and storage system by using the Internet Security Protocol (IPsec) security profile. For more information, see [Enabling file share encryption in transit secure connections](/docs/vpc?topic=vpc-file-storage-vpc-eit).

### Is there support for security groups and network ACLs?
{: faq}
{: #faq-fs-sec-2}

Yes. You can specify the security group access control mode to restrict mounting file shares to specific instances in your VPC. For more information, see [Granular authentication](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-mount-access-mode).

### How is my data protected in a file share? Can I use my own encryption keys?
{: faq}
{: #faq-fs-sec-3}

By default, your file share data is protected at rest with IBM-managed encryption. You can also bring your own keys to the {{site.data.keyword.cloud}} and use them to encrypt your file shares. For more information, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption). By using the API, you can link a primary account that holds a root key to a secondary account, then use that key to encrypt new file shares in the secondary account. For more information, see [Cross-account encryption for multitenant storage resources](/docs/vpc?topic=vpc-getting-started).

### Is my data protected during transit?
{: faq}
{: #faq-fs-sec-4}

You can enable secure end-to-end encryption of your data when you use file shares with security-group-based access control mode and mount targets with virtual network interfaces. When such a mount target is attached and the share is mounted, the virtual network interface performs security group policy check to ensure that only authorized instances can communicate with the share. The traffic between the authorized virtual server instance and the file share can be IPsec encapsulated by the client. For more information, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit).

Encryption in transit is not supported between {{site.data.keyword.filestorage_vpc_short}} and {{site.data.keyword.bm_is_short}}.
{: restriction}
