---

copyright:
  years: 2018, 2024
lastupdated: "2024-10-29"

keywords: application load balancer, limitations, issues, unique combinations, mapping, listener, pool, port

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Application load balancer limitations
{: #lb-limitations}

{{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) has the following known limitations.

* Two members with the same instance X and same port Y cannot exist at the same time for an ALB. This case is not supported and your traffic might not be routed correctly.

* The default load balancer quota is 50 per region. To increase the number, you must [create a support case](/docs/account?topic=account-open-case).

* When a member target instance is deleted, an Application load balancer pool member is not automatically deleted.
