
---

copyright:
  years: 2023, 2024
lastupdated: "2024-10-25"

keywords: asymmetric routing, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why didn't my routing work?
{: #troubleshoot-asymmetric-routing}
{: troubleshoot}
{: support}

You sent a packet, but it didn't go through.
{: shortdesc}

Sometimes packets go through with no issues, but other times the packets fail to arrive as expected.
{: tsSymptoms}

It is possible that you have experienced an asymmetric routing issue, where packets take one path from the source to the destination, but replies follow a different return path.
{: tsCauses}

Asymmetric routing isn't always a problem, but it can cause issues when something in your packet is stateful. For example:

* The initial TCP connection (SYN) establishes a path through the Software Defined Network (SDN).
* If the initial SYN passes through a firewall-router, a stateful route is established for the connection through the firewall-router.
* The TCP acknowledgement (SYN, ACK) and all future packets for the connection must follow the path of the stateful connection.

Packets are dropped when the return path takes a route that is different then the initial SYN.

Force traffic in both directions through the same firewall-router.
{: tsResolve}

For more information about asymemtric routing solutions, see [Centralize communication through a VPC Transit Hub and Spoke architecture - Part one](/docs/solution-tutorials?topic=solution-tutorials-vpc-transit1) and [Centralize communication through a VPC Transit Hub and Spoke architecture - Part two](/docs/solution-tutorials?topic=solution-tutorials-vpc-transit2).
