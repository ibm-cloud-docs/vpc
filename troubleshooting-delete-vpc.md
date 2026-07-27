---

copyright:
  years: 2018, 2026
lastupdated: "2026-07-27"

keywords: vpc, troubleshoot, delete

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I delete a VPC or other VPC resources
{: #troubleshoot-cannot-delete-1}
{: troubleshoot}
{: support}

You are unable to delete your VPC or VPC resources.
{: shortdesc}

You are unable to delete your VPC or VPC resources.
{: tsSymptoms}

Certain operations--creating and deleting virtual server instances, and creating and deleting subnets, for example--are completed asynchronously through the API. Because of this fact, it is recommended to poll the resources you're deleting, to check for deletion before proceeding.
{: tsResolve}

It can take several minutes for resources to be deleted from the system, due to these asynchronous operations. To facilitate deletion, the best practice is to do things in this order:

1. Delete your instances
2. Delete your public gateways
3. Delete your subnets
4. Delete your VPCs

For more information, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting) and [Deleted Resources](/docs/apis/vpc/latest#deleted-resources).

If you need more help after trying these steps, create a support case. For more information, see [Creating support cases](/docs/support?topic=support-open-case).
