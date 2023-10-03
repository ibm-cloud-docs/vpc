---

copyright:
  years: 2018, 2023
lastupdated: "2023-01-27"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does an associated reserved IP address show `0.0.0.0`?
{: #troubleshoot-reserved-ip}
{: troubleshoot}
{: support}

Reserved IP addresses belong to a subnet. You can create these IP addresses explicitly by using subnet APIs (`POST /subnets/{subnet_id}/reserved_ips`), or implicitly as part of an endpoint gateway call by specifying the subnet.
{: shortdesc}

Reserved IP address shows an `0.0.0.0` address.
{: tsSymptoms}

The allocation of the reserved IP address is completed in the background after the call is made. The value of the IP address shows `0.0.0.0` until the allocation of the IP address is complete, which can take up to 30 seconds.
{: tsCauses}

If the IP address is not allocated within this timeframe, try the following steps:
{: tsResolve}

1. Detach the reserved IP address from the endpoint gateway and retry the process.
1. If reattaching does not work, try to create a reserved IP address on its own as a subnet operation. If the reserved IP address is created, you can attempt to create the endpoint gateway by using the explicitly created reserved IP address.
