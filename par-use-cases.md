---

copyright:
  years: 2026
lastupdated: "2026-10-28"

keywords: vpc, public address ranges, use cases, secure workloads, high availability, BYOIP

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

[BYOIP for IPv4](/docs/vpc?topic=vpc-release-notes#vpc-sep2926) and [Public IPv6 address ranges](/docs/vpc?topic=vpc-release-notes&interface=ui#vpc-oct2726) are beta features that are available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

# Use cases for public address ranges
{: #par-use-cases}

Public address ranges simplify the integration of network and security appliances into VPC environments by providing greater control over routing, security, and traffic management.

## Securing your workloads in VPC
{: #secure-workloads-vpc}

To secure workloads in a VPC, you can assign public address ranges to provide secure access to selected services or applications, enabling controlled ingress into the environment. Next, configure routing rules for those public endpoints to redirect incoming traffic to third-party appliances for inspection before reaching the final destination. Firewalls then enforce defined security policies, inspecting traffic and mitigating threats during the process. This approach simplifies the deployment of production-grade applications with the networking and security services that are required in a VPC.

The following diagram illustrates how to secure your workloads in a VPC using public address ranges. First, traffic from the internet enters the VPC through a reserved public address range that is bound to the VPC. The traffic is then routed by the ingress routing table to a security appliance (for example, a firewall or third-party appliance), where it is inspected. After inspection, the traffic is forwarded to the protected applications.

![Secure your workloads in VPC](images/par_use_case_1.svg "Secure your workloads in VPC"){: caption="Secure your workloads in VPC" caption-side="bottom"}

## Deploying highly-available and resilient workloads in VPC
{: #deploy-ha-resilient-workloads-vpc}

To ensure workload resilience, you can use public address ranges to maintain consistent access to services during zonal failures. When a zonal failure occurs, internet traffic is rerouted to a virtual network function (VNF) appliance that is deployed in another zone for monitoring and inspection, maintaining high availability within the VPC.

The following diagram illustrates how routes and firewalls can be configured using public address ranges to enable cross-zone failover and support highly available, resilient workloads in your VPC. Set up two routes: Route 1 for the internet traffic reaching the Active Firewall in Zone 1 (`us-south-1`), and Route 2 for the Passive Firewall in Zone 2 (`us-south-2`). The routes use the public address range as the destination, with the next hop set to the firewall in each zone.

The public address range is attached to the zone with the Active Firewall, `us-south-1`. Internet traffic incoming through the public address range is routed to the Active Firewall per Route 1 in `us-south-1` for inspection and filtering. When the Active Firewall is down, you must manually (or through automation) update the zone attachment of the public address range to `us-south-2`, so that incoming traffic is routed through Route 2 to the Passive Firewall. This setup makes the firewall highly available for internet traffic inspection, securing workloads in the VPC even during zonal failures.

![Deploying highly-available and resilient workloads in VPC](images/par_use_case_2.svg "Deploy highly-available and resilient workloads in VPC"){: caption="Deploy highly-available and resilient workloads in VPC" caption-side="bottom"}

## Retaining your own public IP addresses in VPC (beta)
{: #byoip-public-address-range}

If your organization has existing public IP address ranges that are already known to your customers, partners, or DNS infrastructure, you can bring those IPs into IBM Cloud VPC instead of replacing them with IBM-provided addresses. For a full walkthrough of this use case, see [About custom authorized CIDRs for VPC](/docs/vpc?topic=vpc-byoip#byoip-use-case-retain-ips).


## IPv6 connectivity use cases (beta)
{: #ipv6-par-use-cases-ref}

### Direct IPv6 connectivity to a VNI appliance
{: #ipv6-par-use-case-vni}

You can bind an IPv6 public address range directly to a VNI attached to a virtual server instance or bare metal server. Inbound internet traffic for any IP in the range is routed to the VNI. Response traffic egresses with a source IP from the range. This pattern provides direct public IPv6 connectivity without requiring ingress routing or a public gateway.

### IPv6-to-IPv4 translation with an NFV appliance
{: #ipv6-par-use-case-nfv}

In this scenario, an IPv6 public address range is bound to a VNI attached to a network function virtualization (NFV) appliance. The NFV appliance receives inbound IPv6 traffic, translates it to IPv4, and forwards it to IPv4 back-end members. Response traffic is translated back to IPv6 and returned to the internet using source IPs from the range.



## Next steps
{: #par-use-cases-related-links}

* [Create a public address range](/docs/vpc?topic=vpc-par-creating).
* [Learn about custom authorized CIDRs](/docs/vpc?topic=vpc-byoip). [BYOIP (IPv4) Beta]{: tag-cyan}
* [Learn about public IPv6 address ranges](/docs/vpc?topic=vpc-ipv6-par-about). [IPv6 Beta]{: tag-cyan}
