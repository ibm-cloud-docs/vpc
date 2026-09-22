---

copyright:
  years: 2020, 2026
lastupdated: "2026-09-22"

keywords: route, route table, advertise

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are my routes not being advertised to my direct link as a spoke transit gateway?
{: #rt-ts-spoke-tgw}
{: troubleshoot}
{: support}

Routes cannot be advertised to your direct link as a spoke transit gateway.
{: shortdesc}

Routes are not advertised to your direct link as a spoke transit gateway.
{: tsSymptoms}

Advertising routes to your direct link as spoke transit gateways is not supported.
{: tsCauses}

Enable route advertising in the route table for both your direct link and transit gateway.
{: tsResolve}
