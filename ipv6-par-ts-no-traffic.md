---

copyright:
  years: 2026
lastupdated: "2026-10-28"

keywords: troubleshoot, public address range, ipv6, no traffic, security group, network ACL

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is IPv6 traffic not flowing after I bound a public address range to a VNI?
{: #troubleshoot-ipv6-par-no-traffic}
{: troubleshoot}
{: support}

[Public IPv6 address ranges](/docs/vpc?topic=vpc-release-notes&interface=ui#vpc-oct2726) is a beta feature that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

After binding an IPv6 public address range to a VNI, inbound or outbound IPv6 traffic does not reach the workload or is dropped.
{: shortdesc}

After binding an IPv6 public address range to a VNI, inbound or outbound IPv6 traffic does not reach the workload or is dropped.
{: tsSymptoms}

The default security policy for IPv6 traffic is `deny`. Binding an IPv6 public address range to a VNI does not automatically create security group rules or network ACL rules to permit IPv6 traffic. Even when the public address range lifecycle state shows `stable`, all IPv6 traffic is dropped until explicit allow rules are added.
{: tsCauses}

All IPv6 traffic is denied by default. You must explicitly add security group rules and network ACL rules to permit IPv6 traffic before any inbound or outbound communication is possible.
{: important}

Complete the following steps:
{: tsResolve}

1. Verify that the public address range is in a `stable` state:

   ```sh
   ibmcloud is public-address-range PAR_ID
   ```
   {: pre}

   If the lifecycle state is `failed`, `pending`, or `updating`, the range is not yet active. Retry the bind operation or check for IAM permission errors before proceeding.

1. Add inbound security group rules to permit IPv6 traffic on the VNI's security group.

   - **Console**: Follow the steps in [Adding IPv6 rules to a security group in the console](/docs/vpc?topic=vpc-configuring-the-security-group#sg-ipv6-rules-ui). Set **IP version** to **IPv6** and select **ICMPv6 (58)** or the appropriate protocol.
   - **CLI**: The following example allows all ICMPv6 inbound traffic:

     ```sh
     ibmcloud is security-group-rule-add SG_ID inbound ipv6_icmp \
       --ip-version ipv6 \
       --remote ::/0
     ```
     {: pre}

   - **API**: Use `POST /security_groups/{security_group_id}/rules` with `"ip_version": "ipv6"` and `"direction": "inbound"`. For more information, see [Create a security group rule](/docs/apis/vpc/latest#create-security-group-rule){: external}.

   Add more rules as required for TCP or UDP traffic.

1. Verify that the subnet's network ACL has inbound and outbound rules that permit IPv6 traffic.

   - **Console**: Follow the steps in [Adding IPv6 ACL rules in the console](/docs/vpc?topic=vpc-acl-create-ui#acl-ipv6-rules-ui). Set **IP version** to **IPv6** and allow the required traffic in both directions.
   - **CLI**: The following example adds an inbound allow rule for all IPv6 traffic:

     ```sh
     ibmcloud is network-acl-rule-add ACL_ID allow inbound all \
       --ip-version ipv6 \
       --source ::/0 \
       --destination ::/0
     ```
     {: pre}

   - **API**: Use `POST /network_acls/{network_acl_id}/rules` with `"ip_version": "ipv6"`. For more information, see [Create a network ACL rule](/docs/apis/vpc/latest#create-network-acl-rule){: external}.

   Network ACLs require both an inbound and an outbound rule. Add matching rules for both directions.

1. Confirm that the IPv6 address from the public address range is configured statically in the guest OS. IPv6 public address ranges require static address configuration. Use `ip -6 addr show` on Linux to verify.

1. If traffic still does not flow, verify that the VNI and the authorized CIDR are in the same zone. A cross-zone bind is not permitted, and an apparent success in the API might indicate a stale state.

For more information, see [Setting up a security group for your resource](/docs/vpc?topic=vpc-configuring-the-security-group) and [Adding IPv6 ACL rules](/docs/vpc?topic=vpc-acl-create-ui#acl-ipv6-rules-cli).

## Related links
{: #ipv6-par-ts-no-traffic-related}

- [About public IPv6 address ranges](/docs/vpc?topic=vpc-ipv6-par-about)
- [Creating and managing IPv6 authorized CIDRs](/docs/vpc?topic=vpc-ipv6-par-authorized-cidr)
- [Creating IPv6 public address ranges and binding to a VNI](/docs/vpc?topic=vpc-ipv6-par-creating-binding)
- [Known issues for public address ranges](/docs/vpc?topic=vpc-par-known-issues)
