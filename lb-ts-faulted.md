---

copyright:
  years: 2019, 2025
lastupdated: "2025-12-19"

keywords: load balancer, network, faqs

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is back-end member health under my pool `faulted`?
{: #troubleshoot-lb-faulted}
{: troubleshoot}
{: support}

Use the following information to troubleshoot a back-end member in your pool with a status of `faulted`.
{: shortdesc}

The back-end member under your pool displays a status of `faulted`.
{: tsSymptoms}

The `faulted` status usually results from a misconfigured back-end pool.
{: tsCauses}

Verify the following configurations:
{: tsResolve}

* Does the port of the configured back-end protocol match the port that the application is listening on?
* Does the configured health-check have the correct protocol (TCP or HTTP), port, and URL (for HTTP) information? For HTTP, make sure that your application responds with `200 OK` for the configured health check URL.
* Is the back-end server a virtual server with an enabled security group? If so, make sure that the security group rules allow traffic between the load balancer and the virtual server.

For more information, refer to [Health Checks](/docs/vpc?topic=vpc-nlb-health-checks&interface=ui).
