---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-19"

keywords: load balancer, network, faqs

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I receiving a 409 conflict error code when I update or delete a load balancer?
{: #troubleshoot-lb-409}
{: troubleshoot}
{: support}

Use the following information to resolve 409 conflict error codes when updating or deleting a load balancer.
{: shortdesc}

You receive a 409 error code when updating or deleting a load balancer.
{: tsSymptoms}

The HTTP 409 status code indicates that the request could not be processed because of conflict in the request. If you are receiving this status code, it might be for one of the following reasons:
{: tsCauses}

* You cannot `update` the load balancer configuration if the load balancer status is not `ACTIVE`.
* You cannot `delete` a load balancer if the load balancer status is not `ACTIVE` or `ERROR`.
* You cannot `delete` a load balancer that is already in `DELETE_PENDING` state.
* You cannot `delete` a load balancer if it has one or more pools that are attached to instance groups.
* You cannot `delete` a load balancer pool if it is attached to an instance group.
* You cannot `add`/`delete`/`update` members to a pool that is managed by an instance group.

Check and resolve any of the possible existing causes listed previously.
{: tsResolve}
