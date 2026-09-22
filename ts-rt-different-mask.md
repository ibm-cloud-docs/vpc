---

copyright:
  years: 2020, 2026
lastupdated: "2026-09-22"

keywords: route, right table, mask

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my longer route mask not working?
{: #rt-ts-different-mask}
{: troubleshoot}
{: support}

If you have two routes with different masks from the same destination network advertised to your direct link or transit gateway, you may have issues with the longer mask.
{: shortdesc}

A route from a longer mask is not working.
{: tsSymptoms}

You cannot have two routes with different masks from the same destination network in different zones.
{: tsCauses}

Create the two routes with different masks from the same destination network in the same zone.
{: tsResolve}
