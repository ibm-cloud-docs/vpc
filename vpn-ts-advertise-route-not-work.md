---

copyright:
  years: 2024
lastupdated: "2024-04-11"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why isn't route advertisement to ingress sources working?
{: #troubleshoot-advertise-route-does-not-work-s2s}
{: troubleshoot}
{: support}

After enabling route advertisement to Transit Gateway, Direct Link, or both for an ingress routing table, the traffic isn't working as expected even though the VPN gateway connection is up and running.
{: tsSymptoms}

A route conflict might exist.
{: tsCauses}

Follow the steps to resolve this issue:
{: tsResolve}

1. Navigate to **Interconnectivity > Transit Gateway** and locate the transit gateway that connects to the VPC where the VPN gateway exists.
1. In the Routes section on its Details page, click **Generate report** to see if there is a conflict (for example, overlapping routes).
1. Resolve the conflicted routes by adjusting address prefixes in VPC.
