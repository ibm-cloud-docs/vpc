---

copyright:
  years: 2019, 2021
lastupdated: "2023-02-23"

keywords: load balancer, network, maintence pending

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my load balancer in `maintenance_pending` state?
{: #troubleshoot-lb-maintenance-pending}
{: troubleshoot}
{: support}

Use the information in this topic to troubleshoot your load balancer if it is in a `maintenance_pending` state.
{: shortdesc}

Your load balancer displays it is in a `maintenance_pending` state.
{: tsSymptoms}

An NLB enters a `maintenance_pending` state during various maintenance activities, such as:
{: tsCauses}

* Recovery activities
* Load balancer failover
* Upgrades to address vulnerabilities and appllication of security patches

The `maintenance_pending` state will resolve once the associated maintenance activity completes.
{: tsResolve}
