---

copyright:
  years: 2025, 2026
lastupdated: "2026-09-10"

keywords: block storage, planning, Block Storage for VPC, volume profile, IOPS, encryption, capacity, boot volume, data volume, customer-managed encryption, snapshot, backup

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Planning {{site.data.keyword.block_storage_is_short}} volumes
{: #block-storage-vpc-planning}

Plan your {{site.data.keyword.block_storage_is_short}} strategy by considering volume type, profile, capacity, performance, encryption, and data protection requirements before you create volumes.
{: shortdesc}

## Planning for creating block storage volumes
{: #block-storage-planning-checklist}

Consider the following topics and prerequisites before you create {{site.data.keyword.block_storage_is_short}} volumes.

| Item | Considerations |
|------|----------------|
| - [ ] **{{site.data.keyword.iamshort}} (IAM) permissions** | Confirm that you have the necessary role to create and manage volumes. For more information, see [IAM roles and actions for {{site.data.keyword.block_storage_is_short}}](/docs/iam?topic=iam-iam-service-roles-actions#is.volume-roles). |
| - [ ] **Service-to-service authorizations** | If you plan to use customer-managed encryption, create a service-to-service authorization between {{site.data.keyword.block_storage_is_short}} and your key management service. If you plan to use the Backup service, more authorizations are required. For more information, see [Establishing service-to-service authorizations for {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-s2s-auth). |
| - [ ] **Volume type** | Decide whether you need a boot volume, a data volume, or both. Boot volumes are automatically created when you provision a virtual server instance. Data volumes can be created during instance provisioning or as stand-alone volumes and attached later. For more information, see [{{site.data.keyword.block_storage_is_short}} volume types](/docs/vpc?topic=vpc-block-storage-about#block-storage-vpc-volumes). |
| - [ ] **Volume profile** | Select a volume profile that matches your performance requirements. The `sdp` profile offers flexible capacity up to 32,000 GB with custom IOPS and throughput. [Select availability]{: tag-green} Access to the `sdp` profile is restricted to allowlisted accounts. The tiered profiles (`general-purpose`, `5iops-tier`, `10iops-tier`) provide predefined IOPS/GB ratios for volumes up to 16,000 GB. With the `custom` profile, you can define your IOPS within a range that depends on capacity. For more information, see [{{site.data.keyword.block_storage_is_short}} profiles](/docs/vpc?topic=vpc-block-storage-profiles). |
| - [ ] **Capacity** | Determine the capacity that you need. Data volumes that are created with the `sdp` profile can range from 1 - 32,000 GB. Data volumes that are created with first-generation profiles range from 10 - 16,000 GB. Boot volumes are 100 GB by default. You can specify a different size when you provision an instance. You can increase capacity later, but not decrease it. For more information, see [Block Storage capacity and performance](/docs/vpc?topic=vpc-capacity-performance). |
| - [ ] **IOPS and throughput** | Evaluate the performance requirements for your workload. If you use the `sdp` profile, specify IOPS in the range of 100 - 64,000 and a throughput limit of 125 - 1024 MBps. For tiered or custom profiles, IOPS scales with capacity. For more information, see [Block Storage capacity and performance](/docs/vpc?topic=vpc-capacity-performance). |
| - [ ] **Naming and resource group** | Choose a unique, meaningful name for each volume. Volume names must begin with a lowercase letter, contain only lowercase alphanumeric characters and hyphens, and be unique across the VPC. Names can be up to 63 characters. Associate volumes with a resource group to organize access control and track costs. |
| - [ ] **Encryption** | All volumes are encrypted at rest with IBM-managed encryption by default. For greater control, use customer-managed encryption with root keys that are stored in {{site.data.keyword.keymanagementserviceshort}}. After the encryption type is set for a volume, it cannot be changed. For more information, see [Creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption). |
| - [ ] **Auto-delete** | Decide whether data volumes are deleted when the attached virtual server instance is deleted. The auto-delete feature is disabled by default. Volumes persist after the instance is deleted. You can change this setting at volume creation or at any time afterward. |
| - [ ] **Tags** | Plan user tags and access management tags for your volumes. User tags are used by [backup policies](/docs/vpc?topic=vpc-backup-service-about) to identify volumes for automatic backup. Access management tags help organize IAM access control. For more information, see [Tags for {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-block-storage-about#storage-about-tags). |
| - [ ] **Storage limits** | You can configure up to 300 {{site.data.keyword.block_storage_is_short}} volumes per account in a region by default. If you need more, you can request a quota increase. For more information, see [Managing {{site.data.keyword.block_storage_is_short}} volume count limits](/docs/vpc?topic=vpc-manage-storage-limit). |
| - [ ] **Snapshots** | Consider whether you need point-in-time snapshots of your volumes for data protection or disaster recovery. You can use snapshots to create new volumes, share them with other accounts, or cache them for fast restore. For more information, see [Planning {{site.data.keyword.block_storage_is_short}} snapshots](/docs/vpc?topic=vpc-snapshots-vpc-planning). |
| - [ ] **Backups** | Consider whether to use the Backup service to automate snapshot creation based on a schedule and retention policy. For more information, see [Planning backups](/docs/vpc?topic=vpc-backups-vpc-planning). |
| - [ ] **Interface** | Choose the UI, CLI, API, or Terraform for creating and managing your volumes. |
| - [ ] **Billing** | Review pricing for the volume profile and capacity that you choose. More charges apply for fast-restore snapshot clones and cross-regional backup copies. For more information, see the [FAQs](/docs/vpc?topic=vpc-block-storage-vpc-faq). |
{: caption="Checklist for planning block storage volumes" caption-side="bottom"}

## Next steps
{: #block-storage-vpc-planning-next-steps}

After you plan your volumes, you can [create {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-creating-block-storage).
