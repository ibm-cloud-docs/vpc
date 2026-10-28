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

Support for VPN gateways with members in multiple subnets across different zones in a region is now generally available in the Virtual Private Cloud (VPC) service. Previously, both VPN gateway members were placed in the same subnet. This enhancement introduces a breaking behavior change to the VPN gateway API and CLI and affects automation that provisions or manages regional VPN gateways.

The `subnet` property for a VPN gateway will be absent if the VPN gateway has members in more than one zone in the region.

## Why did we make this change?
{: #vpn-gw-subnet-why-change}

Regional VPN gateways support members that span multiple subnets across different zones in a region.

## Who is affected by this change?
{: #vpn-gw-subnet-those-affected-by-change}

Clients using code or automation to retrieve, validate, or update regional VPN gateways are affected, particularly:

* Code that directly accesses the `subnet` property for regional VPN gateways
* Monitoring and reporting tools that track subnet associations for VPN gateways

All existing VPN gateways have members in only one subnet, and the `subnet` property continues to be included in the CLI and API responses for these gateways.
{: note}

## What actions can you take to avoid a disruption?
{: #vpn-gw-subnet-actions-to-avoid-disruption}

If you or other users in your account use VPN gateways, perform the following tasks to avoid a disruption:

* Review any validation logic that accesses the `subnet` property in VPN gateway responses. If needed, update the validation logic to check for the absence of the property. You can choose to emit a log, halt processing and surface an error, or bypass the resource on which the property is absent.
* Update validation logic to follow [API best practices](/docs/apis/vpc/latest#api-best-practices).
* Clients using API integrations, the CLI, SDKs, or Terraform to manage VPN gateways should review [Migration considerations for updating to Regional VPNs](/docs/vpc?topic=vpc-vpn-gateway-regional-migration-update&interface=cli) for migration requirements, API schema changes, and example CLI and API responses.
