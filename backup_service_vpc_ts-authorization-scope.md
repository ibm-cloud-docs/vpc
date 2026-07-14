---

copyright:
  years: 2024, 2026
lastupdated: "2026-07-14"

keywords: Backup for VPC, backup service, backup plan, backup policy, authorization scope, granular authorization, VSI authorization

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are backups failing for some volumes but not others?
{: #baas-ts-authorization-scope}
{: troubleshoot}
{: support}

Backup jobs succeed for volumes on some virtual server instances but fail for volumes on other instances, even though all instances have matching tags and the backup policy was created successfully.
{: shortdesc}

You created a backup policy and it was accepted without errors. Backups work correctly for volumes that are attached to one or more virtual server instances. However, backup jobs fail for volumes that are attached to other instances with the event type `service-to-service-policy-missing`, even though:
- All target volumes have the correct user tags
- All instances are running
- The backup policy was created successfully without authorization errors
{: tsSymptoms}

This issue occurs when service-to-service authorization was configured with overly granular scope. Specifically, if you granted the Backup service authorization to access only a specific virtual server instance, the authorization applies only to that one instance. Backup jobs fail for all other instances because the Backup service lacks permission to access them.

The backup policy creation process checks only that *some* authorization exists, it can't detect that the authorization scope is too narrow to cover all target resources.
{: tsCauses}

Review and update your service-to-service authorization policies:
{: tsResolve}

1. Go to **Manage > Access (IAM) > Authorizations** in the {{site.data.keyword.cloud_notm}} console.

2. Find authorization policies where:
   - Source service: **VPC Infrastructure Services** with resource type **IBM Cloud Backup for VPC**
   - Target service: **VPC Infrastructure Services** with resource type **Virtual Server for VPC**
 
 Check the **Target** column for each policy. If you see specific resource attributes (such as a single instance ID), the authorization is scoped too narrowly.

4. To broaden the existing authorization scope:
   - Delete the existing granular authorization policy.
   - Create a new authorization with **All resources** scope for the Virtual Server for VPC target service.
   - This approach ensures all current and future instances can be backed up.

5. After updating authorizations, monitor your next scheduled backup job to verify it succeeds for all target volumes.

6. To check backup job status:
   - Go to **Infrastructure > Storage > Backup policies**.
   - Select your backup policy.
   - Click the **Backup jobs** tab.
   - Review the status and any error messages for recent jobs.

For more information about setting up service-to-service authorizations with the correct scope, see [Establishing service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth).
