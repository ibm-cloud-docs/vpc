---

copyright:
  years: 2020, 2023
lastupdated: "2023-11-06"

keywords: route, route table, direct link, transit gateway, ip

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my route not being advertised to my direct link or transit gateway?
{: #rt-ts-valid-ip}
{: troubleshoot}
{: support}

Your route may not be advertised to your direct link of transit gateway.
{: shortdesc}

Routes are not being advertised to your direct link or transit gateway.
{: tsSymptoms}

If the next hop in the route is not a valid IP address in the VPC, the route will not be advertised to your direct link or transit gateway. In addition, if the next hop is in the same zone as the route, the route will not be advertised to your direct link or transit gateway.
{: tsCauses}

Make sure the next hop in the route is a valid IP address in VPC, and that the next hop is in same zone as the route.
{: tsResolve}
