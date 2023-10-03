---

copyright:
  years: 2018, 2023
lastupdated: "2023-01-27"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I find the reserved IP addresses that are associated with an endpoint gateway?
{: #troubleshoot-cannot-find-reserved}
{: troubleshoot}
{: support}

A reserved IP address is part of a subnet. If the subnet gets deleted, all reserved IP addresses that belong to the subnet are deleted.
{: shortdesc}

Cannot find reserved IP addresses that are associated with an endpoint gateway.
{: tsSymptoms}

It is possible that the reserved IP address that is associated with the endpoint gateway was automatically deleted with the subnet.
{: tsCauses}

You can re-create a reserved IP address, but cannot recover reserved IP addresses that were associated with a deleted subnet.
{: tsResolve}
