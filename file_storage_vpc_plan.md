---

copyright:
  years: 2021, 2025
lastupdated: "2025-09-23"

keywords: file share, file storage, requirements, planning, encryption, share size, capacity, performance profile, IOPS, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning your file shares
{: #file-storage-planning}

When you're planning to create {{site.data.keyword.filestorage_vpc_short}}, you might find this checklist helpful to set up and use the file service.
{: shortdesc}

## Planning for creating file shares
{: #planning-for-file-shares}

Consider the following prerequisites before you set up {{site.data.keyword.filestorage_vpc_short}}.

| Considerations|
|-------------------|
|__ Check your [IAM access permissions](/docs/account?topic=account-iam-service-roles-actions#is.share-roles) to verify that you can create file shares, accessor shares, bindings, and mount targets as needed.  |
|__ Think about the location and resources where you're going to use the file share. The file share needs to be mounted to a Compute resource within a VPC. Identify an existing VPC or create a [VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console). |
|__ To mount a file share on a server, you must create at least one mount target. You can create a mount target along with the file share and add more mount targets separately later.|
|__ Decide whether you want to [restrict access](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-mount-access-mode) to the file share from specific virtual server instances or allow mounting on all virtual server instances within a VPC. Using [security groups](/docs/vpc?topic=vpc-using-security-groups) to control traffic between the file share and the Compute resources is recommended. For more information, see [Granular authorization](/docs/vpc?topic=vpc-file-storage-vpc-about#fs-mount-granular-auth). |
|__ Evaluate the capacity and performance that you need. The `dp2` profile provides ample flexibility in this regard. The range of total IOPS that you can specify is based on the size of the file share. You can provision shares with IOPS performance from 100 IOPS (the default minimum) to 96,000 IOPS. [Select availability]{: tag-green} Customers with special approval can create file shares with the `rfs` profile that offers regional data availability and adjustable bandwidth values. For more information, see [File Storage profiles](/docs/vpc?topic=vpc-file-storage-profiles).|
|__ Estimate the size of the file share that you require for your current needs. File share sizes can range between 10 - 32,000 GB. You can later [increase the size of the file share](/docs/vpc?topic=vpc-file-storage-expand-capacity), depending on what the file share profile allows. |
|__ Decide whether to [set up replication](/docs/vpc?topic=vpc-file-storage-replication). The replica file share is in a different zone from the primary file share. Replication is a good way to recover from events and protect against prolonged outages. Replication provides a read-only copy of the share in a different zone of your geography (Americas, Europe, Asia Pacific). You can fail over to the replica site if the primary site becomes unavailable or compromised. |
|__ Make sure that you have a unique name for your file share that easily identifies the file share as your list of shares grows. Associate it with a resource group in your IBM Cloud customer account. |
|__ Think about which encryption type suits your needs the best. By default, your file shares are encrypted with IBM-managed encryption. For greater control, consider the use of customer-managed encryption. Your data can be protected at rest with your own root keys. For more information about this option, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-byok-encryption). |
|__ You can also choose to [encrypt data in transit](/docs/vpc?topic=vpc-file-storage-vpc-eit). This feature can decrease performance for increased security. The impact depends on the workload characteristics. Workloads that perform synchronous writes or bypass VSI caching, such as databases, might have a substantial performance impact when EIT is enabled.|
|__ Choose between the UI, CLI, API, or Terraform for creating and managing your file shares. |
|__ Review pricing information in the console. For more information, see the [FAQs](/docs/vpc?topic=vpc-file-storage-vpc-faqs#faq-fs-pricing). |
{: caption="Checklist for planning file shares" caption-side="bottom"}

## Next Steps
{: #file-storage-vpc-planning-next-steps}

* [Create file shares and mount points](/docs/vpc?topic=vpc-file-storage-create).
* Learn [about file share replication](/docs/vpc?topic=vpc-file-storage-replication).
