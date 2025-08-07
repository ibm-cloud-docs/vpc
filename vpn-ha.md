---

copyright:
  years:  2022, 2025
lastupdated: "2025-08-07"

keywords: VPN, vpn gateways, HA, High availability, Redundancy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability for VPN for VPC
{: #vpn-ha}

[High availability](#x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures.

VPN for VPC is a regional service and you can find the available region and data center locations in the [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region) documentation. As a regional service, VPN for VPC fulfills the defined [Service Level Objectives (SLO)](/docs/resiliency?topic=resiliency-slo) with the Standard plan. The SLO is not a warranty and IBM doesn't issue credits for failure to meet an objective.

To enable HA in a VPN gateway, you must deploy a VPN gateway in each zone that is associated with the subnet that you select. This action helps ensure that the VPN gateway can connect only to virtual server instances within that specific zone. For fault tolerance across multiple zones, you must create a separate VPN gateway in each zone. For example, in a VPC with three zones (us-south1, us-south2, and us-south3), each zone requires its own VPN gateway (`gateway1` for us-south1, `gateway2` for us-south2, and `gateway3` for us-south3).

When you provision a VPN gateway, two appliances are created automatically within the zone, functioning in an *active-active* or *active-standby* configuration. For a policy-based VPN, you must explicitly choose HA, while for route-based VPNs, HA is the default behavior. If one appliance fails, the other automatically takes over to ensure uninterrupted service. However, VPN gateways don't support multiple zone redundancies (MZR) and are confined to a single zone, which means that a VPN gateway cannot connect across different zones.

## High availability architecture
{: #ha-architecture-vpn}

This section provides an overview of the high availability architecture for a VPN gateway, starting with a depiction of a single-zone configuration followed by a discussion of a multi-zone region setup for enhanced redundancy and fault tolerance.

### VPN gateway high availability in a single-zone
{: #vpn-ha-in-single-zone}

A VPN gateway is composed of two back-end instances within the same zone to ensure high availability. The VPN service continuously monitors these instances and automatically fails over to the other instance during failure. Routine maintenance of the VPN is conducted through rolling upgrades of the two back-end instances. During maintenance, the VPN’s private IP address might change, but the public IP addresses remain unaffected, and your VPN connections automatically switches to the available instance.

![VPN gateway HA in single zone](images/vpn-gateway-ha.svg "VPN gateway HA in single zone"){: caption="VPN gateway HA in single zone" caption-side="bottom"}

### VPN gateway high availability in a multizone region
{: #vpn-gateway-ha-in-multiple-zones}

During a network outage in a specific [zone](#x2070723){: term}, you might lose network access to all resources within that zone, including the VPN gateway. To ensure high availability, it is recommended to distribute your workload across multiple zones, with a VPN gateway in each zone, and implement load balancing between zones.

The following diagram illustrates how to distribute your workload across [multizone regions](#x9774820){: term}. Each VPN gateway provides access to the VPC network within the same zone that helps ensure resilience and minimizes the impact of localized outages.

![VPN gateway HA in multiple zones](images/vpn-gateway-ha-in-multiple-zones.svg "VPN gateway HA in multiple zones"){: caption="VPN gateway HA in multiple zones" caption-side="bottom"}
