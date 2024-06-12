---

copyright:
  years: 2017, 2024
lastupdated: "2024-02-06"

keywords: IPv4, ranges, subnets, CIDR, rfc 1918

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Choosing IP ranges for your VPC
{: #choosing-ip-ranges-for-your-vpc}

Use Classless Inter-Domain Routing (CIDR) notation in the format `<IPv4_address>/number`, such as `10.10.0.0/16`. Reserve the last 16 bits (65,536 addresses) of the IPv4 as 0s so that you can use them for various subnet IP addresses within the same {{site.data.keyword.cloud}} VPC, such as `10.10.1.0/24`.
{: shortdesc}

For a definition on CIDR notation, see [RFC 1518](https://datatracker.ietf.org/doc/html/rfc1518) and [RFC 1519](https://datatracker.ietf.org/doc/html/rfc1519).
{: note}

If you use an IP range outside of the ranges [RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918) defines (`10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`) for a subnet, the instances that are attached to that subnet might be unable to reach parts of the public internet. If you plan to configure VPCs that use both non-RFC-1918 addresses and also have public connectivity (floating IP addresses or public gateways), make sure to use a custom route that contains the `Delegate-VPC` action.

The smaller the number after the slash, the more IP addresses that you are allocating. The number after the slash represents the number of leading bits in the subnet's prefix mask.
{: tip}

Table 1 lists the number of available addresses in a subnet, based on its specified CIDR block size:

| CIDR block size | Available addresses |
| --------------- | ------------------- |
|      /22        |        1019         |
|      /23        |         507         |
|      /24        |         251         |
|      /25        |         123         |
|      /26        |          59         |
|      /27        |          27         |
|      /28        |          11         |
{: caption="Table 1. Available addresses in a subnet" caption-side="bottom"}
