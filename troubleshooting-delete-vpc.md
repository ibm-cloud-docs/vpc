---

copyright:
  years: 2018, 2026
lastupdated: "2026-11-11"

keywords: vpc, troubleshoot, delete vpc, delete resources, delete resource

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I delete a VPC or VPC resources
{: #troubleshoot-cannot-delete-1}
{: troubleshoot}
{: support}

You can't delete your VPC or VPC resources.
{: shortdesc}

You can't delete your VPC or VPC resources.
{: tsSymptoms}

You can't delete a VPC until it is empty and you can't delete a resource while it is in a transient state, such as `pending`, or while other resources depend on it. Most delete requests are completed asynchronously through the API. After you request a deletion, the resource shows a `deleting` status in list queries until it is removed. Deleting an instance, load balancer, or VPN gateway can take up to 30 minutes. Because of this behavior, you must poll the resources that you delete and confirm that they don't appear in the list query before you delete the parent resource.
{: tsCauses}
 
To facilitate deletion, the best practice is to delete resources in the following order. Wait for each resource to disappear from the list query before you continue.
{: tsResolve}

1. Delete your VPN gateways.
2. Delete your load balancers.
3. Stop and delete your instances.
4. Release your floating IP addresses. Deleting an instance detaches its floating IP addresses but does not release them.
5. Delete your subnets. Any public gateway that is attached to a subnet is automatically detached.
6. Delete your public gateways.
7. Delete your VPC.

For more information, see [Deleting a VPC](/docs/vpc?topic=vpc-deleting) and [Deleted Resources](/docs/apis/vpc/latest#deleted-resources).

If a resource stays in `deleting` status for more than 30 minutes, or if you can't delete a resource that is in `failed` status, create a support case. For more information, see Creating support cases](/docs/support?topic=support-open-case).
