---

copyright:
  years: 2025, 2026
lastupdated: "2026-10-28"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Action required: Changes to API and CLI for VPN for VPC (site-to-site gateways)
{: #vpn-gateway-subnet-hcr}

## What did we change?
{: #vpn-gw-subnet-what-changed}

Support for VPN gateways with members in multiple subnets across different zones in a region is now generally available in the Virtual Private Cloud (VPC) service. Previously, both VPN gateway members were placed in the same subnet. This enhancement introduces a breaking behavior change to the VPN gateway API and CLI and might affect automation that provisions or manages regional VPN gateways.

The `subnet` property for a VPN gateway is omitted from VPN gateway responses when the gateway has members more than one zone in the region.

## Why did we make this change?
{: #vpn-gw-subnet-why-change}

This enhancement allows regional VPN gateway members to reside in separate subnets across different zones in a region.

## Who is affected by this change?
{: #vpn-gw-subnet-those-affected-by-change}

You might be affected if you use code or automation to retrieve, validate, or update regional VPN gateways, particularly if you have:

* Code that directly accesses the `subnet` property for regional VPN gateways
* Monitoring and reporting tools that track subnet associations for VPN gateways

Existing VPN gateways have members in only one subnet. The `subnet` property continues to be included in the CLI and API responses for these gateways.
{: note}

## What actions can you take to avoid a disruption?
{: #vpn-gw-subnet-actions-to-avoid-disruption}

If you or other users in your account use VPN gateways, take the following actions to avoid disruptions:

* Review code and validation logic that accesses the `subnet` property in VPN gateway responses. Update the logic to handle cases where the property is absent. Depending on your requirements, you can log a message, halt processing and report an error, or skip processing the affected resource.
* Ensure that your integrations follow [API best practices](/apidocs/vpc/latest#api-best-practices).
* If you use API integrations, the CLI, SDKs, or Terraform to manage VPN gateways, review [Migration considerations for updating to regional VPNs](/docs/vpc?topic=vpc-vpn-gateway-regional-migration-update&interface=cli) for migration requirements, API schema changes, and example CLI and API responses.
