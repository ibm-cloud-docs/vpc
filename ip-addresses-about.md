---

copyright:
  years: 2023
lastupdated: "2024-06-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About IP addresses
{: #managing-ip-addresses}

IP addresses in {{site.data.keyword.vpc_full}} are IPv4 addresses that are created based on the RFC 1918 specification. VPC IPv4 addresses allow VPC resources to communicate within the IBM backbone as well as within the internet.
{: shortdesc}

Subnets created within a VPC represent address prefixes, which these IP addresses can be created from. These address prefixes are CIDRs with an address range that dictates the number of individual IP addresses that you can create. For instance, a `/26` CIDR block allows for up to 59 IP addresses.

## Hierarchy of IP addresses
{: #hierarchy-of-ip-addresses}

VPC IPv4 addresses are created from address prefixes, and those address prefixes are created within VPCs. Subnets can then be created with a subset range or max range of addresses that fit within the address prefix range using CIDRs. For example, a subnet created with a CIDR of `172.16.0.0/12` has a range of IP addresses from `172.16.0.0` - `172.31.255.255` where there are 1048576 IP addresses that can be created. When you create a floating IP, it receives an IP address from within the subnet CIDR. You can now assign this floating IP to a public gateway or network interface of an instance to give an internet IP address that enables connectivity and communication across the internet.

## Understanding the differences between public and private IP addresses
{: #public-v-private}

Public IPv4 addresses allow communication with VPC resources on the internet. One example is virtual instances. After you create a floating IP with an IP address, then attach the floating IP to the virtual instance's network interface, the floating IP can now receive and send information through the internet. Another example is a public gateway. When you create a floating IP with an IP address then attach that floating IP to a subnet's public gateway, you enable all instances within the subnet to send information through the internet, but the instances cannot receive information.

Private IPv4 addresses allow intra-communication within VPCs. When subnets are created, they receive a `10.xxx.xxx.xxx` IP address from the VPC's default address prefix, which can be customized. This internal address prefix is assigned to subnet resources for communication within the subnet and also across the zones of the VPC that the subnet resides in.

## Related links
{: #ips-related-links}

- [About floating IPs](/docs/vpc?topic=vpc-fip-about)
- [About public gateways](/docs/vpc?topic=vpc-about-public-gateways)
- [About reserved IPs](/docs/vpc?topic=vpc-about-reserved-ips)
- [About subnets](/docs/vpc?topic=vpc-about-subnets-vpc)
