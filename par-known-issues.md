---

copyright:
  years: 2026
lastupdated: "2026-10-28"

keywords: public address ranges, limitations

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Known issues for public address ranges
{: #par-known-issues}  

As the rollout of public address ranges for VPC continues, a few known issues and limitations have been identified. These are temporary and are scheduled to be addressed in upcoming updates.

[IPv6 Beta]{: tag-cyan}

## IPv6 public address range known issues
{: #par-known-issues-ipv6}

* **IPv6 traffic is denied by default after binding**

   Description: Binding an IPv6 public address range to a VNI does not automatically permit traffic to or from that range. The default policy for IPv6 traffic on both security groups and network ACLs is `deny`. If you do not add explicit allow rules for your IPv6 addresses before or immediately after binding, all inbound and outbound IPv6 traffic is dropped, even though the range is successfully bound.

   Resolution: After binding, add security group rules with `--ip-version ipv6` (or `"ip_version": "ipv6"` in the API) to allow the required protocols and sources or destinations. Also add matching network ACL rules on the subnet attached to the VNI's VPC. For guidance, see [Adding IPv6 rules to a security group](/docs/vpc?topic=vpc-configuring-the-security-group#sg-ipv6-rules-cli) and [Adding IPv6 ACL rules](/docs/vpc?topic=vpc-acl-create-ui#acl-ipv6-rules-cli).

* **IPv6 Neighbor Discovery Protocol (NDP) is handled by IBM infrastructure, so do not create rules for it**

   Description: IPv6 Neighbor Discovery messages (ICMPv6 types 133–136: Router Solicitation, Router Advertisement, Neighbor Solicitation, and Neighbor Advertisement) are intercepted and handled by the SDN hypervisor layer. These messages are never evaluated against security group or network ACL rules. Creating allow or deny rules for ICMPv6 types 133–136 has no effect.

   Resolution: Do not create security group or ACL rules for NDP traffic. NDP functions automatically as long as the VNI has an IPv6 public address range bound to it.

* **Protocol 46 (IPv6 Encapsulation) and Protocol 59 (IPv6 No Next Header)**

   Description: These two IPv6-specific protocol numbers (protocol 46, IPv6 Encapsulation, and protocol 59, No Next Header) are not valid in security group or network ACL rules. Attempts to create rules with `protocol: ipv6_encapsulation` (46) or `protocol: ipv6_no_next` (59) are rejected by the API. IPv6 extension headers that use these protocols are also dropped in the data plane.

   Resolution: Do not use protocols 46 or 59 in security group or ACL rules. Use the supported protocol values: `ipv6_icmp` (58), `ipv6_hop_opt` (0), `ipv6_route` (43), `ipv6_frag` (44), `ipv6_dest_opts` (60), `ipv6_mobility` (135), `tcp`, or `udp`.

* **Flow logs do not capture IPv6 public address range traffic**

   Description: Flow log collectors do not record traffic that flows through IPv6 public address ranges in this release. If you rely on flow logs for visibility, auditing, or troubleshooting IPv6 traffic, those records are not present.

   Resolution: This is a known limitation in the current release. IPv6 flow log support is planned for a future update. For troubleshooting, use security group and network ACL rule testing as an alternative.



## IPv4 public address range known issues
{: #par-known-issues-ipv4}

* Intermittent 500 error when listing or deleting public address ranges

   Description: You might occasionally encounter a `500 Internal Server Error` when listing or deleting public address ranges. This is caused by back-end delays while confirming the deletion across all zonal nodes. As a result, the operation might exceed the 30-second timeout.

   Workaround: Wait a few minutes and retry the operation. In most cases, the operation completes successfully on subsequent attempts.

* CIDR allocation conflict error

   Description: This issue can occur when multiple requests attempt to allocate the same CIDR block at the same time, resulting in a failure.

   Workaround: Retry the operation after a short delay. The request typically completes successfully on retry.

* 500 Error from Cloudflare when creating public address ranges

   Description: You might see a `500 Worker Threw Exception` error during the creation of a public address range. This error originates from Cloudflare's edge layer before the request reaches IBM servers.

   Workaround: Retry the request. This issue is transient, and most retries are successful.

* 502 Bad Gateway errors when creating or updating public address ranges

   Description: A `502 Bad Gateway` error might occur when creating or updating a public address range. This happens when a connection to the backend is closed before the server finishes processing the request, such as in scenarios involving client-side timeouts, transient network issues, or high system load.

   Workaround: Retry the operation. The backend is healthy, and most retries complete successfully.

## Next steps
{: #known-issues-par-related-link}

* [Plan for public address ranges](/docs/vpc?topic=vpc-par-planning).
* [Create a public address range](/docs/vpc?topic=vpc-par-creating).
* [Learn about public IPv6 address ranges](/docs/vpc?topic=vpc-ipv6-par-about). [IPv6 Beta]{: tag-cyan}
