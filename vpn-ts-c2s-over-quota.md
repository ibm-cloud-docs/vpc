---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did the advertised route creation fail in my Client VPN for VPC?
{: #troubleshoot-c2s-advertise-routes-over-quota}
{: troubleshoot}
{: support}

You tried to create an advertised route in your Client VPN for VPC, and it didn't work.
{: tsSymptoms}

A VPC can have at most 15 advertised routes by default. If the number of advertised routes is already at 15, the VPN server fails to create another advertised route.
{: tsCauses}

You can resolve the failure with the following steps.
{: tsResolve}

1. Decrease the number of advertised routes in the VPC.

    * Switch **Advertise to** option to **Off** in the routing table
    * In the **Accepts routes from** option of the routing table, switch the **VPN gateway** to **Off**

1. [Open an IBM support case](/unifiedsupport/cases/form){: external} to increase the quota.
1. Continue to use the `translate` action in the VPN server route.
