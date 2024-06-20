---

copyright:
  years: 2022, 2024
lastupdated: "2024-06-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring custom routes
{: #config-custom-routes}

You must configure custom routes to ensure that ingress data traffic is routed through the Network Load Balancer (NLB) on its way to the Virtual Network Function (VNF) and target destination. In some cases, you might also require custom routes to ensure egress traffic is returned to the original client source.

After the NLB with routing mode is enabled and is in active state, you must create the necessary custom routes to ensure data traffic is routed through the NLB and VNFs.

1. Gather the following information:

   * The subnet CIDR for the target (destination) resources. If the VNF is transparent, this value is typically the subnet where the workload virtual servers reside. If the VNF is nontransparent and acting as a proxy, this value might be the subnet CIDR for the VNF.
   * The subnet CIDR for the client (source) resources.

1. Create ingress routing table routes that route data traffic destined for your target resources through the NLB's primary IP address.
1. If you require an egress route, create an egress routing table route that routes data traffic that is destined for your source resources through the NLB.

   In some cases, such as a nontransparent VNF setup to use Source NAT, you might not require an egress route since the VNF bypasses the NLB on the return trip to the source IP.

If the NLB fails over to the other node, the custom routes are automatically updated to hop to the new NLB IP address. For more information about custom routes, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).
{: note}
