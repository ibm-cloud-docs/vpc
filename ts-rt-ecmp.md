---

copyright:
  years: 2020, 2023
lastupdated: "2023-11-06"

keywords: zone, advertise, route, route table, ecmp

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are my ECMP routes not working?
{: #rt-ts-ecmp}
{: troubleshoot}
{: support}

ECMP routes may not work when advertised to your direct link or transit gateway.
{: shortdesc}

Two ECMP routes in different zones that are advertised to your direct link or transit gateway are not working as ECMP routes.
{: tsSymptoms}

It is not possible to have two ECMP routes in different zones.
{: tsCauses}

Create the two ECMP routes in the same zone.
{: tsResolve}
