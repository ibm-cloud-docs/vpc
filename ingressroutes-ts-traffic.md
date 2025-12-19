---
copyright:
  years: 2022, 2025
lastupdated: "2025-12-19"

keywords:

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my public ingress traffic not route to the next hop?
{: #troubleshoot-pi-traffic}
{: troubleshoot}
{: support}

You can resolve public ingress traffic routing errors by correcting your route configuration.
{: shortdesc}

You tried to route public ingress traffic to the next hop, but the traffic didn't route.
{: tsSymptoms}

Your traffic routing error was most likely caused by a public ingress route configuration error.
{: tsCauses}

Follow these steps to fix public ingress traffic routing issues:
{: tsResolve}

1. Verify that the route destination includes the floating-IP external IP, that the routing table has an **INTERNET-FIP** traffic source, and that the routing table is defined correctly. For more information, see [How do I fix my public ingress route configuration problem?](/docs/vpc?topic=vpc-troubleshoot-pi-configuration-problem&interface=cli)

1. Verify that the floating IP is defined correctly.

For more information, see [Why am I not getting a response from the destination floating-IP?](/docs/vpc?topic=vpc-troubleshoot-pi-floatingip-response&interface=cli)
