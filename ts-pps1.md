---

copyright:
  years: 2024
lastupdated: "2024-11-06"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't consumers connect to my Private Path service?
{: #troubleshoot-pps-1}
{: troubleshoot}
{: support}

As a service provider, you created a Private Path service so that consumers can privately connect to a service or application. However, even though your consumers were given the correct cloud resource name (CRN), they cannot access your service. The problem can be found in either the VPE gateway, or on the Private Path network load balancer side of the service.
{: shortdesc}

The provider gives a service CRN to the consumer. The consumer creates a Virtual Private Endpoint (VPE) gateway, but it fails to connect to the target service. A consumer might get a timeout on their request. The consumer VPE and the provider's service cannot communicate with each other.
{: tsSymptoms}

Possible causes include:
{: tsCauses}

* The provider's target service is down.
* The Private Path service was not set up correctly.
* You, as the service provider, created your service, but didn't attach virtual servers to the Private Path network load balancer.

Follow these steps to troubleshoot this Private Path issue:
{: tsResolve}

1. Verify that you correctly configured your load balancer. To do so, use the VPC API to ensure that your load balancer has at least one pool, one front-end listener, and one member. The listener must have the default pool defined, and the pool must have at least one member virtual server instance attached.

1. Verify that the `Health` field for at least one Private Path network load balancer member shows as `ok`. Otherwise, check whether there is an application running on the load balancer member virtual server instance that can respond to the health checks.

If this issue still persists, [open an IBM Support case](/unifiedsupport/cases/form){: external}.
