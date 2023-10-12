---

copyright:
  years: 2023
lastupdated: "2023-10-10"

keywords: vpn, health

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Diagnosing VPN gateway health
{: #vpn-health}

Identifies the health of VPN gateways, provides reasons for failure, and suggests solutions for recovery.

| Code | Message | Information
|------------- | -------- | ---------------- |
| `cannot_create_vpc_route` | `VPN gateway cannot create the route (check for conflict).` | This issue is commonly caused by a stale route in the routing table. This error occurs if a route exists with the destination in the remote CIDR of the VPN gateway connection, but the creator is not the VPN gateway. To resolve this issue, delete the conflicting VPC route. |
| `cannot_reserve_ip_address` | `IP address exhaustion (release addresses on the VPN's subnet).` | No IP addresses are available in the subnet to allocate. Release addresses on the VPN gateway's subnet by deleting associated resources, such as servers, load balancers, or endpoint gateways. |
| `internal_error` | `Internal error (contact IBM support).` | Contact IBM Support to analyze and resolve internal errors. |
{: caption="Table 1. VPN gateway health reasons" caption-side="bottom"}
