---

copyright:
  years: 2024
lastupdated: "2024-04-15"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why isn't route advertisement to ingress sources working?
{: #troubleshoot-advertise-route-not-work-c2s}
{: troubleshoot}
{: support}

After enabling route advertisement to Transit Gateway, Direct Link, or both for an ingress routing table, the traffic isn't working as expected even though the VPN server is up and running.
{: tsSymptoms}

A route conflict might exist.
{: tsCauses}

Follow the steps to resolve this issue:
{: tsResolve}

1. Navigate to **Interconnectivity > Transit Gateway** and locate the transit gateway that connects to the VPC where the VPN server exists.
1. In the Routes section on its Details page, click **Generate report** to see if there is a conflict (for example, overlapping routes).
1. Resolve the conflicted routes by adjusting address prefixes in VPC.
