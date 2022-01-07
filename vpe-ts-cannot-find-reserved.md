---

copyright:
  years: 2018, 2020
lastupdated: "2020-10-30"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I find the reserved IPs associated with an endpoint gateway?
{: #troubleshoot-cannot-find-reserved}
{: troubleshoot}
{: support}

A reserved IP is part of a subnet. If the subnet gets deleted, all reserved IPs belonging to the subnet are deleted.
{: shortdesc}

Cannot find reserved IP addresses associated with an endpoint gateway.
{: tsSymptoms}

It is possible that the reserved IP associated with the endpoint gateway was automatically deleted with the subnet.  
{: tsCauses}

You can re-create a reserved IP, but cannot recover reserved IPs associated with a deleted subnet.
{: tsResolve}
