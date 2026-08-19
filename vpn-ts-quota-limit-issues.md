---

copyright:
  years: 2024, 2026
lastupdated: "2026-08-19"

keywords: virtual private network, VPN, VPN gateway, troubleshooting, quota, advertised routes

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How to resolve VPN quota limit issues?
{: #troubleshoot-vpn-quota-issues}
{: troubleshoot}
{: support}

VPN operations can fail when resource quotas, service limits, or routing capacity limits are reached. Understanding these limits help you plan your VPN deployment and avoid operational failures.
{: shortdesc}

You experience one or more of the following quota-related or capacity-related issues:
{: tsSymptoms}

* You see quota-related error messages when configuring VPN connection.
* Configuration changes are rejected with quota-related error messages.
* Advertised route creation fails in your VPN gateway.
* Route propagation fails when you turn on **Accept routes from** VPN gateway setting in the routing table.

VPN operations fail when you exceed service quotas, routing limits, or encounter resource limitations:
{: tsCauses}

**VPN gateway or connection limits**
:   Each VPC has limits on the number of VPN gateways and connections that can be created.

**Routing table limitations**
:   Routing tables have limits on the number of routes and route sources that they can accept.

**Advertised route quota exceeded**
:   VPCs have a default advertised routes limit. When this limit is reached, the VPN gateway can't create additional advertised routes.

Follow these steps to diagnose and resolve quota and capacity-related issues:
{: tsResolve}

## Resolving VPN gateway and connection limit issues
{: #resolve-gateway-connection-limits}

If you encounter VPN gateway or connection limits, consider optimizing your existing deployment before you request a quota increase. In many cases, consolidating connections or redesigning the gateway layout can improve scalability and reduce the need for additional resources.

### Optimize gateway usage
{: #optimize-gateway-usage}

1. Review existing VPN connections and identify opportunities to consolidate connections wherever possible.

1. If a VPN gateway is approaching its connection limit, distribute connections across multiple gateways. Organizing gateways by traffic patterns, business units, or geographic regions can improve performance and resiliency.

1. Use route-based VPNs for complex networking environments as they provide greater flexibility and scalability than policy-based VPNs. Route-based VPNs can simplify management by supporting multiple CIDR ranges and dynamic routing protocols such as BGP, reducing the number of required VPN connections. For more information, see [Route-based VPN](/docs/vpc?topic=vpc-vpn-overview#route-based-vpn-gateway).

For site-to-site VPN gateway quota, see [VPN for VPC (site-to-site)](/docs/vpc?topic=vpc-quotas&interface=api#vpn-quotas) and for routing table quota, see [Routing tables and routes](/docs/vpc?topic=vpc-quotas&interface=api#routing-tables-routes-quotas).

## Resolving route quota issues
{: #resolve-route-quota}

VPCs have a default limit on the number of routes. Additionally, the advertise quota includes routes that are advertised from VPN gateways to Transit Gateway, Direct Link, and the combined total across all routing tables in the VPC. All eligible advertised routes are counted together across every routing table in the VPC, rather than per individual routing table.

When the limit is reached, VPN gateways cannot create new or advertised routes. The route propagation fails, and VPN connections that depend on new routes fail to establish.

Follow these steps to resolve route quota issues:

### Reduce the number of advertised routes
{: #reduce-advertised-routes}

When advertised route creation fails due to quota limits, you have several options to resolve the issue. The most immediate solution is to reduce the number of routes that are advertised:

1. **Review current advertised routes**.

   * Go to **Infrastructure > Network > Routing tables**.
   * For each routing table, check the **Advertise to** settings.
   * Identify which routes are being advertised to Transit Gateway or Direct Link.

1. **Disable unnecessary route advertisements**.

   * Edit routing tables that don't need to advertise routes.
   * Switch **Advertise to** to **Off** for Transit Gateway or Direct Link, which reduces the advertised route count.

1. **Aggregate subnets to reduce route count**.

   * Instead of advertising individual subnet routes, use broader address ranges.
   * Consolidate multiple `/24` subnets into a single `/22` or `/20` range, which reduces the number of advertised routes while maintaining connectivity. The following example shows an example of subnet aggregation:

   Before aggregation (4 routes):
   * `10.0.1.0/24`
   * `10.0.2.0/24`
   * `10.0.3.0/24`
   * `10.0.4.0/24`

   After aggregation (1 route):
   * `10.0.0.0/22`

### Use VPC address prefixes.
{: #use-address-prefixes}

Instead of advertising individual subnet routes, advertise VPC address prefixes, which are broader CIDR ranges that include one or more subnets within the VPC.

1. **Review VPC address prefixes**.

   * Go to **Infrastructure > Network > VPCs**.
   * Select your VPC and view the configured **Address prefixes**.
   * Note the configured address prefixes.

1. **Configure routing to use prefixes**.

   * Ensure that the VPC address prefixes cover all subnets that need connectivity.
   * Advertise the broader prefix instead of individual subnet routes, which significantly reduces the advertised route count.

Using address prefixes reduces the number of advertised routes, improves scalability as new subnets are added, and helps minimize route quota consumption. For more information, see [Designing an addressing plan for a VPC](/docs/vpc?topic=vpc-vpc-addressing-plan-design).
{: tip}

## Best practices for quota management
{: #quota-management-best-practices}

Follow these best practices, which can help you prevent quota-related issues and reduce the need for quota increase requests as your network grows.

### Plan your network architecture
{: #plan-network-architecture}

* Design your network with hierarchical, aggregatable address ranges and CIDR blocks that can be easily summarized to reduce route counts and simplify future expansion.

* Use Transit Gateway as a central routing hub and advertise only the routes that are required, allowing Transit Gateway to manage route distribution across connected networks.

* Implement route summarization at network boundaries and advertise supernets instead of individual subnets whenever possible to reduce routing table size and quota consumption.

### Monitor quota usage
{: #monitor-quota-usage}

* Regularly review advertised routes to identify opportunities for consolidation. Remove route advertisements that are no longer required.

* Maintain an inventory of VPN gateways and connections, and monitor usage against known quotas to ensure sufficient capacity for future growth.

* Configure alerts or notifications for quota thresholds, when available, so that potential issues can be addressed before they affect connectivity.

### Design for scalability
{: #design-for-scalability}

* Use route-based VPNs for growing environments because they scale more effectively than policy-based VPNs, support multiple destinations, and integrate with BGP.

* Adopt a modular network design with consistent addressing schemes and clear separation of environments or workloads to simplify management and expansion.

* Maintain up-to-date documentation for VPN configurations, routing decisions, and quota usage to support troubleshooting, planning, and operational continuity.

## Requesting quota increase
{: #request-quota-increase}

If your deployment requires more advertised routes or VPN gateway than the default quota allows, you can request a quota increase through IBM Support. Before you submit a request, gather the information that is needed to help IBM evaluate your requirements.

1. **Prepare your request**.

Document the routes that need to be advertised and explain why route aggregation or address prefixes can't be used to reduce the route count. Include details about your network architecture, connectivity requirements, and any business or technical constraints that necessitate the additional routes. Explain why additional gateways or connections are required.

1. **Submit an IBM support case**.

   * Go to [IBM Support](/unifiedsupport/cases/form){: external}.
   * Select **Infrastructure** as the category.
   * Choose **VPN for VPC** as the offering.
   * Provide your documentation and justification.
   * Include the following information in your request:
      * Account ID and VPC ID
      * Current advertised route count
      * Current VPN gateway and connection counts
      * Requested quota limit
      * Architectural and business justification
      * Required implementation timeline

   IBM Support reviews each request based on technical feasibility and the justification provided. If approved, you will be notified of the outcome and the quota is adjusted accordingly.
   {: note}

   Each routing table supports a maximum of 14 unique route prefixes, which is hard limit. This limit can't be increased through a support request. If your environment exceeds this limit, consider consolidating or redesigning the routing configuration.
   {: note}

## Verifying connection after quota resolution
{: #verify-quota-resolution}

After the quota-related issue is resolved, verify that services are operating normally and take steps to prevent similar issues from occurring in the future.

1. Verify that advertised routes can be created successfully, VPN connections are established, and route propagation works as expected.

1. Monitor the environment for recurring quota-related warnings or errors, and review quota usage trends to identify potential capacity constraints.

1. Document the resolution, including quota usage levels, resource utilization changes, and any relevant operational notes.

1. Review capacity planning and network architecture to reduce the likelihood of future quota-related issues as the environment grows.

## Related links
{: #vpn-quota-related}

* [VPC quotas and service limits](/docs/vpc?topic=vpc-quotas)
* [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes)
* [Configuring route propagation for VPN gateways](/docs/vpc?topic=vpc-advertise-routes-s2s)
* [VPN gateway limitations](/docs/vpc?topic=vpc-vpn-limitations)
* [Getting help and support for VPC](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc)
