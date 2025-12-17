---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-17"

keywords: VPE, virtual private endpoint, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are my VPN gateways or virtual server instances not communicating?
{: #troubleshoot-routing-issues}
{: troubleshoot}
{: support}

When a connection is created successfully, the VPN service adds a `0.0.0.0/0 via <VPN gateway private IP>` route into the default routing table of the VPC. However, this new route can cause routing issues.
{: shortdesc}

* Virtual server instances in different subnets cannot communicate with each other.
* If the VPN gateway is in a subnet that uses the default routing table, it creates a route loop and the VPN gateway cannot communicate with the on-premises VPN gateway.
{: tsSymptoms}

This issue occurs only when the peer CIDR is `0.0.0.0/0`.
{: tsCauses}

To address these routing issues:
{: tsResolve}

* If the virtual server instances in different subnets cannot communicate with each other, create a route with **Delegate** type for each subnet.
* If the VPN gateway is in a subnet that uses the default routing table, complete these steps:

   * Create a dedicated subnet for the VPN gateway.
   * Create a dedicated custom routing table.
   * Associate the dedicated subnet with the dedicated custom routing table.
