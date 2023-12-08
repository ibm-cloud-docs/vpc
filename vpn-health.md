---

copyright:
  years: 2023
lastupdated: "2023-12-04"

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

## Diagnosing VPN gateway connection health
{: #vpn-connection-health}

Identifies the health of VPN gateway connections, provides reasons for being down, and suggests solutions to recover.

| Code | Message | Information |
| --------- | -------------- | ---------------- |
| `cannot_authenticate_connection` | `Failed to authenticate a connection because of mismatched IKE ID and PSK (check IKE ID and PSK in peer VPN configuration).` | This error is commonly caused by a mismatched IKE ID and PSK between both sides of the VPN. Check the IKE ID and PSK in the peer VPN configuration to resolve this issue.  \n  \n The IBM VPN gateway uses its public IP address as the IKE local identity and designates the peer's public IP address as the IKE peer identity. In the case where the peer VPN gateway is located behind a NAT firewall, and the peer's public IP address is not associated with the peer VPN gateway interface, you must adjust the configuration of the peer VPN gateway to ensure that the peer's public IP address is used as the IKE identity.|
| `ike_policy_mismatch` | `None of the proposed IKE crypto suites was acceptable (check the IKE policies on both sides of the VPN).` | This error is commonly caused by mismatched IKE policies on both sides of the VPN. Compare the IKE policies on both sides of the VPN and make sure that they match. |
| `ike_v1_id_local_remote_cidr_mismatch` | `Invalid IKE ID or mismatched local CIDRs and remote CIDRs in IKE V1 (check the IKE ID or the local CIDRs and remote CIDRs in IKE V1 configuration).` | For Phase 1, this issue occurs when the IKE ID is mismatched. Check the IKE ID on the on-prem side. The IBM side only supports a public IP address as the IKE ID.  \n  \n For Phase 2, the local/remote subnet might be mismatched in IKE V1. Check the local/remote subnet and make sure they match in the IKE V1 configuration.  \n  \n The IBM VPN gateway uses its public IP address as the IKE local identity and designates the peer's public IP address as the IKE peer identity. In the case where the peer VPN gateway is located behind a NAT firewall, and the peer's public IP address is not associated with the peer VPN gateway interface, you must adjust the configuration of the peer VPN gateway to ensure that the peer's public IP address is used as the IKE identity.|
| `ike_v2_local_remote_cidr_mismatch` | `Mismatched local CIDRs and remote CIDRs in IKE V2 (check the local CIDRs and remote CIDRs in IKE V2 configuration).` |  Check the local/remote subnet and make sure they match in the IKE V2 configuration. |
| `ipsec_policy_mismatch` | `None of the proposed IPsec crypto suites was acceptable (check the IPsec policies on both sides of the VPN).` |  This error is commonly caused by mismatched IPsec policies on both sides of the VPN. Compare the IPsec policies on both sides of the VPN and make sure that they match. |
| `peer_not_responding` | `No response from peer (check network ACL configuration, and on-premise firewall configuration).` |  No response from peer. Check that the VPC network ACL configuration, the on-prem firewall configuration, and the peer IP are correct. |
| `internal_error` | `Internal error. Contact IBM Support.` |  Contact IBM Support to analyze and resolve internal errors. |
{: caption="Table 2. VPN gateway connection status reasons" caption-side="bottom"}
