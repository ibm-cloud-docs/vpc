---

copyright:
  years: 2018, 2021
lastupdated: "2021-06-07"

keywords: application load balancer, limitations, issues, unique combinations, mapping, listener, pool, port

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}} limitations
{: #lb-limitations}

Known limitations for {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) are as follows:

* Two members with the same instance X and same port Y cannot exist at the same time for an ALB. This case is not supported and your traffic might not be routed correctly.

* Currently, HTTP/2 and WebSocket protocols are not supported for back-end pools. However, HTTP and HTTPS protocols are supported.

* The default load balancer quota is 50 per region. To increase the number, you must [create a support case](/docs/get-support?topic=get-support-open-case).
