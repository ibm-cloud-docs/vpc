---

copyright:
  years: 2022
lastupdated: "2022-04-29"

keywords:

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:preview: .preview}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}


# Establishing service to service authorizations
{: #backup-s2s-auth}

Before you can create backup policies, you need to establish service-to-service authorizations and specify user roles. This will enable the Backup for VPC service to detect volume tags and create backups.
{: shortdesc}

This service is available only to accounts with special approval to preview this feature. Contact IBM Support if you're interested in getting access.
{: preview}

## Overview
{: #backup-s2s-auth-overview}

Every user that accesses VPC infrastructure services resources must be assigned one or more access policies that define their IAM roles. These policies determine what actions a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the IBM Cloud service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

To use Backup for VPC to create backups of block storage volumes, you must create service-to-service authorizations and user roles:

* IBM Cloud Backup for VPC (source) to Block Storage for VPC (target) as _Operator_
* BM Cloud Backup for VPC (source)  to Block Storage Snapshots for VPC (target) as _Editor_
* IBM Cloud Backup for VPC (source) to  Virtual Server for VPC (target) as _Operator_

For more information about managing access to Virtual Private Cloud (VPC) resources, see [Getting Started with IAM](/docs/vpc?topic=vpc-iam-getting-started&interface=ui).

## Procedure
{: #backup-s2s-auth-procedure}

From the UI, follow this procedure to create three authorization policies:

1. In the IBM Cloud console, go to **Manage > Access (IAM)**. The **Manage access and users** page displays.

2. From the side panel, select **Authorizations**.

3. On the **Manage authorizations** page, click **Create**. You will create three separate service authorizations by repeating steps 4 - 6.

4. On the **Grant a service authorization** page, select **VPC infrastructure service** from the dropdown menu for both the source and target service. Identify the source and target by checking **Resources based on selected attributes**
and **Resource type**. Table 1 lists the resource types for the source and target services, and user roles.

5. Under **Authorize dependent services**, select the role based on Table 1.

6. Click **Authorize**.

| Source service - resource type | Target service - resource type | Dependent service user role |
|----------------|----------------|-----------|
| IBM Cloud Backup for VPC | Block Storage for VPC | operator |
| IBM Cloud Backup for VPC | Block Storage Snapshots for VPC | editor |
| IBM Cloud Backup for VPC | Virtual Server for VPC | operator |
{: caption="Table 1. Service-to-service authorizations" caption-side="bottom"}

## Next Steps
{: #backup-s2s-next-steps}

[Create backup policies](/docs/vpc?topic=vpc-backup-policy-create).
