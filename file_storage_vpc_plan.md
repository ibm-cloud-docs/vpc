---

copyright:
  years: 2021, 2022
lastupdated: "2022-10-24"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning your file shares
{: #file-storage-planning}

When you're planning to create file shares on your VPC, you might find this checklist helpful to set up and use the file service.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, Sydney, Sao Paulo, Tokyo, and Toronto regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Planning for creating file shares
{: #planning-for-file-shares}

Consider the following prerequisites before you set up {{site.data.keyword.filestorage_vpc_short}}.

|        Considerations|
|-------------------|
|__ Evaluate your [IAM access permissions](/docs/vpc?topic=vpc-file-storage-managing#file-storage-vpc-iam) to create file shares. |
|__ Evaluate the capacity and performance that you need by selecting a [File Storage profile](/docs/vpc?topic=vpc-file-storage-profiles). You can select an IOPS tier profile or use Custom IOPS to tailor capacity and IOPS performance to meet your needs. |
|__ Choose the UI, CLI, or API for creating and managing your file shares. |
|__ Decide whether to [set up replication](/docs/vpc?topic=vpc-file-storage-replication) when you create the share. The replica file share is in a different zone from the primary file share. Replication is a good way to recover from events because it provides a read-only copy of the share in a different zone of your region. You can fail over to the replica site if the primary site becomes unavailable or compromised. |
|__ Evaluate how you plan to use your file shares. |
|__ Make sure that you have a unique name for your file shares that easily identify the file share as your list of shares grows. Associate it with a resource group in your IBM Cloud customer account. |
|__ When you create a file share, choose the encryption type that best suits your needs. By default, your file shares are encrypted with IBM-managed encryption. For greater control, consider the use of customer-managed encryption. Your data is protected at rest with your own root keys. For more information about his option, see [Creating file shares with customer-managed encryption](/docs/vpc?topic=vpc-file-storage-vpc-encryption). For more information about using the same root keys across accounts, see [Cross-account encryption for multitenant storage resources](/docs/vpc?topic=vpc-vpc-byok-cross-acct-key). |
|__ When you create a file share, decide when to create a mount target. You can create extra mount targets separate from the file share creation. Also, make sure that you created a [VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).|
|__ Estimate the size of the file share you require for your current needs. You can later [increase the size of the file share](/docs/vpc?topic=vpc-file-storage-expand-capacity), depending on what the file share profile allows. |
|__ To mount a file share on a target, you must create at least one mount target. |
|__ To create a mount target, you must have a VPC. Identify an existing VPC or create a new one. |
{: caption="Table 1. Checklist for planning file shares" caption-side="bottom"}

## Next Steps
{: #file-storage-vpc-planning-next-steps}

[Create file shares and mount points](/docs/vpc?topic=vpc-file-storage-create).
