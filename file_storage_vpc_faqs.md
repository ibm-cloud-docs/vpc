---

copyright:
  years: 2021
lastupdated: "2021-09-17"

keywords: file storage, virtual private cloud, file share, troubleshooting

subcollection: vpc

---

{:faq: data-hd-content-type='faq'}
{:new_window: target="_blank"}
{:shortdesc: .Shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:table: .aria-labeledby="caption"}
{:pre: .pre}
{:tip: .tip}
{:preview: .preview}
{:important: .important}
{:screen: .screen}
{:support: data-reuse='support'}


# FAQs for file storage
{: #file-storage-vpc-faqs}

The following questions often arise about File Storage for VPC. If you have other questions you'd like to see addressed here, provide feedback by using the **Open Issue** or **Edit Topic** links.
{: shortdesc}

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, and London regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Offering questions
{: #file-storage-vpc-offering-questions}

### Do I need to create a VPC before I can create a file share?
{: faq}
{: #faq-fs-2}

No. You can create a file share independent of a VPC. However, to create a mount target, you must have a VPC available. To mount a share, you must provision a virtual server instance within that VPC.

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
*	Delete all mount targets or delete a single mount target. When you delete one or several mount targets, the instances that are mounting the share for the VPCs where the mount target is deleted can't access the share.
*	List shares and mount targets.
*	Update share and mount target name.

### Who do I contact to help with any issues? What information do I need to provide?
{: faq}
{: #faq-fs-7}

For information about who to contact, see [Getting help and support](/docs/vpc?topic=vpc-getting-help). Provide as much information as you can, including error messages, screen captures, and API error codes and responses. Include any messages from the VPC and the file storage service.


## File share management questions
{: #file-storage-vpc-management-questions}

### Can I mount the same file share in different zones in my region?
{: faq}
{: #faq-fs-mgt-1}
{: support}

No. As a zonal shared file service, file shares that are created for a zone are accessible to instances only within that zone. For this release, File Storage for VPC is available in single zones in the Dallas (us-south), Frankfurt (eu-de), and Washington DC (us-east) regions for allow-listed IBM Cloud customer accounts. 

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

No. As a best practice, independently back up your data. When your share data is deleted, it can't be restored.

### Are file shares elastic?
{: faq}
{: #faq-fs-mgt-7}

File shares are not elastic. Currently, you can provision minimum of 10 GB to maximum of 16 TB file shares.

### Can I change the size of a file share?
{: faq}
{: #faq-fs-mgt-8}

You can increase the size of a file share from its original capacity in GB increments up to 32,000 GB capacity, depending on your share profile. For more information, see [Expanding file share capacity](/docs/vpc?topic=vpc-file-storage-expand-capacity).


## Performance questions
{: #file-storage-vpc-performance-questions}

### What read-write latency can I expect?
{: faq}
{: #faq-fs-perf-1}
{: support}

You can expect an average latency less than 100 ms for writes and less than 50 ms for reads for block sizes less than one MB.

## Data security and encryption questions
{: #file-storage-vpc-security-questions}

### How secure is my data?
{: faq}
{: #faq-fs-sec-1}
{: support}

All data-at-rest is encrypted by default using IBM-managed encryption. You can also encrypt your file shares using your own root keys, which gives your more control over your data security. For information about his option, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption).

Data-in-transit is not supported in this release.

### Is there support for security groups and network ACLs?
{: faq}
{: #faq-fs-sec-2}

No.

### How is my data protected in a file share? Can I use my own encryption keys?
{: faq}
{: #faq-fs-sec-3}

Your data is protected at rest by using IBM-managed encryption. You can't use your own keys for encryption. This feature is planned in a later release.
