---

copyright:
  years: 2026
lastupdated: "2026-10-29"

keywords: vpc, public address ranges, ipv6, about

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About public IPv6 address ranges (beta)
{: #ipv6-par-about}

[Public IPv6 address ranges](/docs/vpc?topic=vpc-release-notes&interface=ui#vpc-oct2726) is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

A public IPv6 address range is a contiguous block of IBM-provided public IPv6 addresses that you can bind directly to a virtual network interface (VNI) in an availability zone.
{: shortdesc}

Unlike IPv4 public address ranges, which bind to a VPC and zone, IPv6 public address ranges bind to a VNI. After binding, traffic destined for addresses in the range is routed automatically to that VNI. No ingress routing configuration is required.

## How public IPv6 address ranges work
{: #ipv6-par-how-it-works}

Public IPv6 address ranges use a two-level allocation model:

1. **IPv6 authorized CIDR**: You request a `/64` block of IBM-provided public IPv6 addresses for a specific zone. IBM provides the block, and it becomes the authorized CIDR for that zone, acting as your address pool.
2. **Public address range**: You carve a subblock (prefix length `/64`–`/128`) from the authorized CIDR and bind it to a VNI. The VNI must be in the same zone as the authorized CIDR.

After a public address range is bound to a VNI, traffic for any IP in that range is routed to the VNI.

## Key differences from IPv4 public address ranges
{: #ipv6-par-vs-ipv4}

| Behavior | IPv4 public address range | IPv6 public address range |
| --- | --- | --- |
| Bound to | VPC + zone | VNI |
| Ingress routing required after binding? | Yes | No |
| Address source | IBM-managed pool or custom authorized CIDR (BYOIP) | IBM-managed pool only |
| Prefix sizes | `/32`–`/28` (1–16 addresses) | `/64`–`/128` |
| Multiple ranges per target | Yes: Multiple public address ranges can bind to one VPC | Yes: Multiple public address ranges can bind to one VNI |
| One range per target at a time? | Yes: One public address range is bound to one VPC at a time | Yes: One public address range is bound to one VNI at a time |
| Cross-account binding | No | Yes: A public address range in one account can bind to a VNI in a service account |
| Billing | Billed per IP per hour (egress charged at the standard rate) | Not billed (egress charged at the standard rate) |
{: caption="IPv4 and IPv6 public address range behavior" caption-side="bottom"}

## Planning considerations
{: #ipv6-par-planning}

Review the following before you create an IPv6 public address range.

* Create an IPv6 authorized CIDR for the target zone before you create a public address range. See [Creating and managing IPv6 authorized CIDRs](/docs/vpc?topic=vpc-ipv6-par-authorized-cidr).
* The public address range must be in the same zone as its authorized CIDR. The target VNI must also be in that zone.
* Multiple public address ranges can bind to the same VNI. Each public address range can bind to only one VNI at a time.
* IPv6 addresses must be configured statically in the guest OS.
* After binding, add security group rules and network ACL rules with `ip_version: ipv6` to allow traffic. IPv6 traffic is denied by default.
* IPv6 Neighbor Discovery Protocol (NDP) is handled automatically. No security group or network ACL rules are required for NDP.
* Security group inbound rules accept an IPv6 CIDR as the remote source. A remote security group is not a valid source for IPv6 inbound rules.
* IPv6 public address ranges route traffic to a VNI as a block. Individual addresses from the range cannot be reserved as floating IPs.
* Protocol 46 (IPv6 Encapsulation) and protocol 59 (No Next Header for IPv6) are not valid values in security group or network ACL rules.
* Use custom IAM roles on the Public Address Range resource to control which users in an account can manage IPv6 public address ranges.

## Limitations
{: #ipv6-par-limitations}

IPv6 public address ranges are available for VNIs that are attached to virtual server instances and bare metal servers. The following scope applies to this release:

* IPv6 authorized CIDRs are IBM-provided. Bringing your own IPv6 address range is planned for a future release.
* IPv6 public address ranges bind to a VNI, not a VPC, and route the entire address block to that VNI. Individual addresses from the range cannot be reserved as floating IPs.
* Flow log collectors do not record IPv6 public address range traffic. This capability is planned for a future release.

* IPv6 public address ranges work with security groups and network ACLs only. They do not interoperate with VPN gateways, VPN servers, {{site.data.keyword.tg_full_notm}}, {{site.data.keyword.dl_full_notm}}, endpoint gateways, Private Path service gateways, {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.openshiftlong_notm}}, or other platform services in this release.

## Getting started
{: #ipv6-par-getting-started}

1. Ensure that you have created a VNI in the zone where you want public IPv6 connectivity.
1. [Create an IPv6 authorized CIDR](/docs/vpc?topic=vpc-ipv6-par-authorized-cidr) for that zone.
1. [Create an IPv6 public address range and bind it to your VNI](/docs/vpc?topic=vpc-ipv6-par-creating-binding).

## Related links
{: #ipv6-par-related-links}

- [Getting started with public address ranges](/docs/vpc?topic=vpc-about-par)
- [Planning considerations for public address ranges](/docs/vpc?topic=vpc-par-planning)
- [About virtual network interfaces](/docs/vpc?topic=vpc-vni-about)
- 
- [IAM roles and actions](/docs/iam?topic=iam-iam-service-roles-actions#is.public-address-range-roles)
- [Quotas](/docs/vpc?topic=vpc-quotas#par-quotas) and [service limits](/docs/vpc?topic=vpc-quotas#service-limits-for-vpc-services)
- [FAQ for public address ranges](/docs/vpc?topic=vpc-faq-public-address-ranges)
- [Known issues for public address ranges](/docs/vpc?topic=vpc-par-known-issues)
