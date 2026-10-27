---

copyright:
  years: 2024, 2026
lastupdated: "2026-10-27"

subcollection: vpc

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for public address ranges
{: #faq-public-address-ranges}

The following questions are commonly asked about public address ranges. If you have additional questions you'd like to see addressed here, open an issue by using the **Open doc issue** or **Edit topic** links at the end of this page.
{: shortdesc}


## Can I bring my own public IP address range?
{: #assign-public-ips}
{: faq}
{: support}

[BYOIP (IPv4) Beta]{: tag-cyan}
Yes. You can bring your own publicly routable IPv4 address range to {{site.data.keyword.vpc_short}} through custom authorized CIDRs. To get started, see [Creating a custom authorized CIDR (BYOIP)](/docs/vpc?topic=vpc-provision-custom-authorized-cidr) and open a support case. IBM verifies ownership and configures the range as an authorized CIDR in your account. After the range is available, you can allocate public address ranges and floating IPs from it.


No. Only IBM-provided public IPs are supported. BYOIP (Bring Your Own IP) is not available for this service.

## What public IP address range sizes can I reserve?
{: #prefix-sizes}
{: faq}
{: support}

You can reserve ranges with the following prefix sizes:

* `/28` = 16 IPs
* `/29` = 8 IPs
* `/30` = 4 IPs
* `/31` = 2 IPs
* `/32` = 1 IP

After reserving a range, you can't change its size. Make sure to reserve a range that's large enough to meet your current and future needs.

If you require a prefix size larger than `/28`, review [Public address range quotas](/docs/vpc?topic=vpc-quotas#par-quotas) and [contact support](/unifiedsupport/cases/form){: external} to request a quota increase.

IPs in different public address ranges aren't guaranteed to be contiguous.
{: note}

[IPv6 Beta]{: tag-cyan}IPv6 public address ranges support prefix sizes from `/64` to `/128`, allocated from a `/64` IPv6 authorized CIDR.

## Can I divide a public address range across multiple VPCs or zones?
{: #divide-par}
{: faq}
{: support}

No. A public address range is indivisible. It cannot be split or shared across multiple VPCs or availability zones.

## Can I reserve and use multiple public address ranges in a single VPC?
{: #reserve-use-multiple-public-address-ranges}
{: faq}
{: support}

Yes. You can reserve and bind multiple public address ranges to a single VPC to meet scaling or isolation requirements.

## Are public address ranges automatically redundant across zones?
{: #redundant-by-default}
{: faq}
{: support}

Not by default. However, you can create separate public address ranges in different zones and configure routing to ensure high availability across zones.

[IPv6 Beta]{: tag-cyan}

## What is the difference between an IPv4 and IPv6 public address range?
{: #ipv4-ipv6-par-difference}
{: faq}
{: support}

IPv4 public address ranges bind to a VPC and zone, and require ingress routing configuration to direct traffic to a target resource. IPv6 public address ranges bind directly to a VNI, and traffic routes to that VNI automatically without requiring ingress routing. IPv6 public address ranges also require an IPv6 authorized CIDR to be created first, whereas IPv4 ranges can be created from IBM-managed pools directly.

## What is an IPv6 authorized CIDR and do I need one?
{: #what-is-ipv6-authorized-cidr}
{: faq}
{: support}

An IPv6 authorized CIDR is a `/64` block of IBM-provided public IPv6 addresses allocated to your account for a specific zone. You must create an authorized CIDR before you can create any IPv6 public address ranges in that zone. See [Creating IPv6 authorized CIDRs](/docs/vpc?topic=vpc-ipv6-par-authorized-cidr).

## Can I bring my own IPv6 address range?
{: #byoipv6}
{: faq}
{: support}

Only IBM-provided IPv6 authorized CIDRs are available. Support for bringing your own IPv6 address range is planned for a future release.

## Can I bind an IPv6 public address range to a VPC?
{: #ipv6-par-vpc-binding}
{: faq}
{: support}

IPv6 public address ranges bind to a VNI, not a VPC.

## Does IPv6 public address range support DHCP for address configuration?
{: #ipv6-par-dhcp}
{: faq}
{: support}

IPv6 addresses from a public address range must be configured statically in the guest OS. DHCP is not supported for IPv6 public address range addresses.




## Related links
{: #faq-related-links-par}

- [Getting started with public address ranges](/docs/vpc?topic=vpc-about-par)
- [Planning considerations for public address ranges](/docs/vpc?topic=vpc-par-planning)
- [IPv6 Beta]{: tag-cyan} [About public IPv6 address ranges](/docs/vpc?topic=vpc-ipv6-par-about)
- [IAM roles and actions](/docs/iam?topic=iam-iam-service-roles-actions#is.public-address-range-roles)
- [Quotas](/docs/vpc?topic=vpc-quotas#par-quotas) and [service limits](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services)
- [Known issues](/docs/vpc?topic=vpc-par-known-issues)
- [Troubleshooting](/docs/vpc?group=tbs-par)
