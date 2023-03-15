---

copyright:
  years: 2022, 2023
lastupdated: "2023-03-15"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Establishing service to service authorizations
{: #backup-s2s-auth}

Before you can create backup policies, you need to establish service-to-service authorizations and specify user roles. This authorization enables the Backup for VPC service to detect volume tags and create backups.
{: shortdesc}

## Overview
{: #backup-s2s-auth-overview}

Every user that accesses VPC infrastructure resources must be assigned one or more access policies that define their IAM roles. These policies determine the actions that a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

To use Backup for VPC to create backups of block storage volumes, you must create service-to-service authorizations and user roles:

* IBM Cloud Backup for VPC (source) to {{site.data.keyword.block_storage_is_short}} (target) as _Operator_.
* IBM Cloud Backup for VPC (source) to Block Storage Snapshots for VPC (target) as _Editor_.
* IBM Cloud Backup for VPC (source) to Virtual Server for VPC (target) as _Operator_.

For more information about managing access to {{site.data.keyword.vpc_full}} (VPC) resources, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started&interface=ui).

If you set up service authorizations incorrectly, the backup service is designed not to create backup policies. For more information, see the troubleshooting topic [Backup policy not created due to incorrect authorizations](/docs/vpc?topic=vpc-baas-troubleshoot&interface=ui#baas-ts-3).
{: note}

## Enabling authorization
{: #backup-s2s-auth-procedure}

When you create a backup policy in the UI, the UI checks that the correct service-to-service authorizations and user access roles are enabled. If any of them is incorrect or missing, you can assign the required authorizations without leaving the backup policy creation page. Enabling the source service to delegate its access to other dependent services automatically creates authorization policies for those services.

For more information about setting authorizations when you create a backup policy, see [Create a backup policy in the UI](https://test.cloud.ibm.com/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=ui#backup-provisioning-page-ui).

You can also directly manage IAM access to create the service-to-service authorization policies by following this procedure.

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM)**. The **Manage access and users** page is displayed.

2. From the side panel, select **Authorizations**.

3. On the **Manage authorizations** page, click **Create**. Then, create three separate service authorizations by repeating steps 4 - 6.

4. On the **Grant a service authorization** page, select **VPC infrastructure service** from the menu for both the source and target service. Identify the source and target by checking **Resources based on selected attributes** and **Resource type**. Table 1 lists the resource types for the source and target services, and user roles.

5. Under **Authorize dependent services**, select the role based on Table 1.

   | Source service - resource type | Target service - resource type | Dependent service user role |
   |----------------|----------------|-----------|
   | IBM Cloud Backup for VPC | Block Storage for VPC | Operator |
   | IBM Cloud Backup for VPC | Block Storage Snapshots for VPC | Editor |
   | IBM Cloud Backup for VPC | Virtual Server for VPC | Operator |
   {: caption="Table 1. Service-to-service authorizations" caption-side="bottom"}

6. Click **Authorize**.

## Next Steps
{: #backup-s2s-next-steps}

[Create backup policies](/docs/vpc?topic=vpc-backup-policy-create).
