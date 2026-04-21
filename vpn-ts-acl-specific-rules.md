---

copyright:
  years: 2018, 2026
lastupdated: "2026-04-21"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does VPN traffic stop working after I update Network ACL rules?
{: #troubleshoot-vpn-acl-specific-rules}
{: troubleshoot}
{: support}

VPN connectivity from your on‑premises network to a virtual server instance through an IBM Cloud VPN gateway fails when allow _any‑to‑any_ Network ACL rules are removed and replaced with restrictive, protocol‑specific rules.
{: shortdesc}

VPN connectivity appears healthy, and the VPN tunnel is established successfully. However, traffic from the on‑premises source doesn't reach the virtual server instance. Application connectivity, backup access, or service port (for example, TCP port `3200`) fails, even though corresponding network ACL rules seem to allow the traffic.
{: tsSymptoms}

This issue occurs because Network ACLs are stateless and VPN gateways require additional bidirectional traffic beyond the application port to function correctly. When broad allow _any‑to‑any_ rules are removed and replaced with restrictive TCP‑only rules, required VPN‑related flows can be unintentionally blocked.
{: tsCauses}

Follow these steps to resolve the issue:
{: tsResolve}

1. Identify the Network ACL that is associated with the subnet, which is hosting the virtual server instance.
1. Review all inbound and outbound Network ACL rules that are applied to that subnet.
1. Verify that the ACL rules allow not only the application traffic but also all required VPN‑related traffic, including bidirectional flows between the VPN gateway and subnets.
1. Make sure that the rules allow UDP traffic on ports `500` and `4500`.
1. Temporarily re‑enable an `allow‑all` rule during testing to confirm that the ACL restrictions are the cause of the connectivity issue.
1. After validation, replace the `allow‑all rules` with a complete set of rules that permit all required VPN gateway and the virtual server traffic. For more information, see [Configuring network ACLs for use with VPN](/docs/vpc?topic=vpc-troubleshoot-vpn-acl-specific-rules&interface=ui).
