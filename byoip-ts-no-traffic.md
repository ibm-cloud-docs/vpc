---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: byoip, troubleshooting, authorized CIDR, public address range, traffic, ingress routing, bring your own IP

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is traffic not reaching my resources after creating a public address range from my authorized CIDR?
{: #ts-byoip-no-traffic}
{: troubleshoot}
{: support}

[BYOIP for IPv4 is a beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You created a public address range from your authorized CIDR and bound it to a VPC and zone, but traffic from the internet is not reaching your resources.
{: shortdesc}

Traffic from the internet does not reach your resources after you create and bind a public address range from your authorized CIDR.
{: tsSymptoms}

Public address ranges created from a custom authorized CIDR do not automatically route traffic to resources. You must manually configure a public ingress routing table to define where incoming traffic is delivered.
{: tsCauses}

Configure a public ingress routing table with a route that uses the public address range CIDR as the destination and the next-hop IP of your target resource (such as a firewall, network appliance, or load balancer) as the next hop. For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
{: tsResolve}
