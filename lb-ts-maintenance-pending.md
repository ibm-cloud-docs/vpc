---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-19"

keywords: load balancer, network, maintence pending

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my load balancer in `maintenance_pending` state?
{: #troubleshoot-lb-maintenance-pending}
{: troubleshoot}
{: support}

Use the following information to troubleshoot your load balancer if it is in a `maintenance_pending` state.
{: shortdesc}

Your load balancer displays it is in a `maintenance_pending` state.
{: tsSymptoms}

An NLB enters a `maintenance_pending` state during various maintenance activities, such as:
{: tsCauses}

* Recovery activities
* Load balancer failover
* Upgrades to address vulnerabilities and application of security patches

The `maintenance_pending` state resolves when the associated maintenance activity completes.
{: tsResolve}
