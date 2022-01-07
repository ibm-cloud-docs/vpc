---

copyright:
  years: 2018, 2020
lastupdated: "2020-10-30"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Why does an associated reserved IP show `0.0.0.0` for its address?
{: #troubleshoot-reserved-ip}
{: troubleshoot}
{: support}

Reserved IPs belong to a subnet. You can create these IPs explicitly through the use of subnet APIs (`POST /subnets/{subnet_id}/reserved_ips`), or implicitly as part of an endpoint gateway call by specifying the subnet.
{: shortdesc}

Reserved IP shows an `0.0.0.0` address.
{: tsSymptoms}

The allocation of the reserved IP address is completed in the background after the call is made. The value of the IP address shows `0.0.0.0` until the allocation of the IP address
is complete, which can take up to 30 seconds.
{: tsCauses}

If the IP address is not allocated within this timeframe, try the following:
{: tsResolve}

1. Detach the reserved IP from the endpoint gateway and retry the process.
1. If reattaching does not work, try to create a reserved IP on its own as a subnet operation. If this works, you can attempt to create the endpoint gateway by using the explicitly created reserved IP.
