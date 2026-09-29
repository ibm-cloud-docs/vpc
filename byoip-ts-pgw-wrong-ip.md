---

copyright:
  years: 2026
lastupdated: "2026-09-29"

keywords: byoip, troubleshooting, authorized CIDR, public gateway, floating IP, bring your own IP

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my public gateway not using an IP from my custom authorized CIDR?
{: #ts-byoip-pgw-wrong-ip}
{: troubleshoot}
{: support}

[IPv4 BYOIP is an allowlisted beta feature](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

You reserved a floating IP from your custom authorized CIDR and attached it to a public gateway, but outbound traffic from the subnet is not using the expected IP address.
{: shortdesc}

Outbound traffic from the subnet uses a different source IP address rather than the floating IP from your custom authorized CIDR.
{: tsSymptoms}

The floating IP may not be correctly associated with the public gateway, or the public gateway may not be attached to the intended subnet.
{: tsCauses}

Check the following:
{: tsResolve}

- Confirm that the floating IP is attached to the correct public gateway and that the public gateway is attached to the intended subnet.
- Verify the floating IP status is `available` and not in a `pending` or `failed` state.
- For more information, see [Creating floating IPs from a custom authorized CIDR](/docs/vpc?topic=vpc-byoip-fip-create).
