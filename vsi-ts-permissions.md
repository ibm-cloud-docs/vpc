---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-09"

keywords: troubleshoot virtual server instance, permissions, vpc permissions, failed status, pending status

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my virtual server instance showing a `Failed` status after creation?
{: #troubleshooting-permissions-in-ibm-cloud}
{: troubleshoot}
{: support}

You create a virtual server instance, but it shows `Pending` status and quickly changes to `Failed` status.
{: shortdesc}

Before you can create virtual server instances for {{site.data.keyword.vpc_short}}, you need to set up the correct permissions in {{site.data.keyword.cloud_notm}} console. If your permissions are not correct, then you can create a {{site.data.keyword.vsi_is_short}} instance and the status shows `Pending`, which quickly changes to `Failed`.
{: tsSymptoms}

Your account permissions might not be configured correctly to create virtual server instances.
{: tsCauses}

Ask the administrator of your account to assign you the correct permissions in the {{site.data.keyword.cloud_notm}} console. For more information, see [Granting user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources#managing-user-permissions-for-vpc-resources).
{: tsResolve}
