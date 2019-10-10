---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-09-30"

keywords: IPv4, ranges, subnets, CIDR, 1918

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}


# Choosing IP ranges for your VPC
{: #choosing-ip-ranges-for-your-vpc}

Use Classless Inter-Domain Routing (CIDR) notation in the format `<IPv4 address>/number`, such as 10.10.0.0/16. Reserve the last 16 bits (65,536 addresses) of the IPv4 as 0s so that you can use them for various subnet IP addresses within the same {{site.data.keyword.cloud}} VPC, such as 10.10.1.0/24.
{:shortdesc}

CIDR notation is defined in [RFC 1518](https://tools.ietf.org/html/rfc1518) and [RFC 1519](https://tools.ietf.org/html/rfc1519).
{: note}

If you use an IP range outside of those ranges that are defined by [RFC 1918](https://tools.ietf.org/html/rfc1918) (`10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`) for a subnet, the instances that are attached to that subnet might be unable to reach parts of the public internet.

The smaller the number after the slash, the more IP addresses you are allocating. The number after the slash represents the number of leading 1 bits in the subnet's prefix mask.
{: tip}

Table 1 lists the number of available addresses in a subnet, based on its specified CIDR block size:

| CIDR block size | Available Addresses |
| --------------- | ------------------- |
|      /22        |        1019         |
|      /23        |         507         |
|      /24        |         251         |
|      /25        |         123         |
|      /26        |          59         |
|      /27        |          27         |
|      /28        |          11         |
{: caption="Table 1. Available addresses in a subnet" caption-side="top"}
