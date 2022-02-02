---

copyright:
  years: 2022
lastupdated: "2022-01-26"

keywords: VPN, VPN gateways, HA, High availability, Redundancy

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPN gateway high availability
{: #vpn-ha}

## VPN gateway high availability in a single-zone
{: #vpn-ha-in-single-zone}

A VPN gateway consists of two back-end instances for high availability in the same zone. The VPN service monitors the back-end instances and failover when the back-end instance is down. VPN routine maintenance is performed with rolling upgrade of the two back-end instances. During the maintenance, you might see a VPN private IP address change. The VPN public IP addresses are not impacted and your VPN connections will be switched over to the available instance automatically.

![VPN gateway HA in single zone](images/vpn-gateway-ha.png "VPN gateway HA in single zone"){: caption="Figure 1. VPN gateway HA in single zone" caption-side="bottom"}

## VPN gateway high availability in multiple zones
{: #vpn-gateway-ha-in-multiple-zones}

An availability zone (AZ) is the fault domain defined by the data center within a region. AZs provide low latency connectivity between them. Each VPC region supports 3 availability zones.

When there is a network outage in an availability zone, you might lose network access to all resources, including the VPN gateway in that zone. It is recommended to spread your workload across fault domains (AZs) with a VPN gateway in each AZ, and load-balance between AZs to achieve high availability.

Figure 2 describes how to spread your workload over multiple zones. Each VPN gateway in the zone should only be used to access the VPC network in the same zone.

![VPN gateway HA in multiple zones](images/vpn-gateway-ha-in-multiple-zones.png "VPN gateway HA in multiple zones"){: caption="Figure 2. VPN gateway HA in multiple zones" caption-side="bottom"}
