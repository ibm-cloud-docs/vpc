---

copyright:
  years: 2025, 2026
lastupdated: "2026-07-31"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Action required: Changes to VPN gateways in VPC API
{: #vpn-gateway-subnet-hcr}

## What are we changing?
{: #vpn-gw-subnet-what-changed}

Currently, when creating a VPN gateway, only one subnet can be specified and both VPN gateway members are placed in this subnet. In an upcoming release, the Virtual Private Cloud (VPC) service will support VPN gateways with members in multiple subnets in different zones in a region. This will introduce a breaking behavior change to the VPN gateway API and will affect automation that provisions or manages Regional VPN gateways.

The `subnet` property for a VPN gateway will be absent if the VPN gateway has members in more than one zone in the region.

## Why are we making this change?
{: #vpn-gw-subnet-why-change}

The upcoming regional VPN gateways will support members that span multiple subnets across different zones in a region.

## Who will be affected by this change?
{: #vpn-gw-subnet-those-affected-by-change}

Accounts where client code or automation retrieves, validates, or updates Regional VPN gateways will be affected, particularly:

* Code that directly accesses the `subnet` property for Regional VPN gateways
* Monitoring and reporting tools that track subnet associations for VPN gateways

All existing VPN gateways will have members in only one subnet, and the `subnet` property for these VPN gateways will continue to be included in the API responses.
{: note}

## What actions can you take to avoid a disruption?
{: #vpn-gw-subnet-actions-to-avoid-disruption}

If you or other users in your account are currently using VPN gateways, perform the following tasks before this feature becomes generally available:

* Review any validation logic that accesses the `subnet` property in VPN gateway API responses. If needed, update the validation logic to check for the absence of the property. You can choose to emit a log, halt processing and surface an error, or bypass the resource on which the property is absent.
* Update validation logic to follow [API best practices](/docs/apis/vpc/latest#api-best-practices).
