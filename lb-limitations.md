---

copyright:
  years: 2018, 2020
lastupdated: "2020-08-31"

keywords: application load balancer, limitations, issues, unique combinations, mapping, listener, pool, port

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.alb_full}} limitations
{: #lb-limitations}

Known limitations for {{site.data.keyword.cloud}} {{site.data.keyword.alb_full}} (ALB) are as follows:

* Two members with the same instance X and same port Y cannot exist at the same time for an ALB. This case is not supported and your traffic might not be routed correctly.

* The HTTP/2 protocol is not yet supported for backend pools. However, HTTP and HTTPS protocols are supported.
