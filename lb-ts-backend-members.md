---

copyright:
  years: 2019, 2021
lastupdated: "2023-02-23"

keywords: load balancer, network, traffic, members

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is traffic not reaching my back-end members?
{: #troubleshoot-lb-traffic-member}
{: troubleshoot}
{: support}

Use the information in this topic to help troubleshoot issue where traffic is not reaching your back-end member.
{: shortdesc}

Incoming traffic fails to reach your back end members.
{: tsSymptoms}

This could be caused by any of the following issues:
{: tsCauses}

* Your back-end pool health checks are failing.
* Your back-end member application is not up and running.
* Your load balancer security group rules do not allow outgoing traffic from load balancers on your back-end member port.
* Your back-end member's security group rules, if any, do not allow all incoming traffic from the load balancer.

Check and resolve any of the previously listed issues that exist.
{: tsResolve}
