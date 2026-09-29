---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: byoip, troubleshooting, authorized CIDR, floating IP, not reachable, bring your own IP

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my floating IP from a custom authorized CIDR not reachable from the internet?
{: #ts-byoip-fip-not-reachable}
{: troubleshoot}
{: support}

[BYOIP for IPv4 is a beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You reserved a floating IP by specifying an address from your custom authorized CIDR and attached it to a virtual network interface (VNI) or public gateway, but the IP is not reachable from the internet.
{: shortdesc}

The floating IP is reserved and attached, but traffic from the internet does not reach the associated resource.
{: tsSymptoms}

One or more of the following may be the cause:
{: tsCauses}

- The floating IP is reserved but not yet attached to a VNI or public gateway.
- The VNI or public gateway it is attached to has not been associated with an active instance or subnet.
- Security group rules are blocking inbound traffic to the instance.

Check the following:
{: tsResolve}

- Confirm that the floating IP is attached to a VNI or public gateway, and that the associated instance or subnet is active.
- Review the security group rules for the instance to ensure inbound traffic on the required ports is permitted.
- If attaching to a public gateway, confirm the public gateway is attached to the correct subnet.
- For more information, see [Creating floating IPs from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create).
