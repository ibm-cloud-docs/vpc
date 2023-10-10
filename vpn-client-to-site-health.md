---

copyright:
  years: 2023
lastupdated: "2023-10-10"

keywords: vpn, health

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Diagnosing VPN server health
{: #vpn-server-health}

Identifies the health of VPN servers, provides reasons for failure, and suggests solutions for recovery.

| Code | Message | Information |
| -------------- | ---------- | ------------ |
| `cannot_access_client_certificate`       | `VPN server's client certificate is inaccessible (verify certificate exists and that IAM policies grant VPN server for VPC access to Secrets Manager).`     |  This issue occurs if you delete the Secrets Manager instance, delete the client certificate, or remove service authorization to Secrets Manager after you create a VPN server with the client certificate. Verify that the certificate exists and its IAM policy grants permission. For more information, see [Creating an IAM service-to-service authorization](/docs/vpc?topic=vpc-client-to-site-authentication#creating-iam-service-to-service).   |
| `cannot_access_server_certificate`       | `VPN server's server certificate is inaccessible (verify certificate exists and that IAM policies grant VPN server for VPC access to Secrets Manager).`       |  This issue occurs if you delete the Secrets Manager instance, delete the server certificate, or remove service authorization to Secrets Manager after you create a VPN server with the server certificate. Verify that the certificate exists and its IAM policy grants permission. For more information, see [Creating an IAM service-to-service authorization](/docs/vpc?topic=vpc-client-to-site-authentication#creating-iam-service-to-service).    |
| `cannot_create_vpc_route`                |  `VPN cannot create route (check for conflict).`  |  This issue is commonly caused by a stale route in the routing table. This error occurs if a route exists with the destination in the subrange of the VPN server's IP pools, but the creator is not the VPN server. For example, you encounter this issue if the VPN server's client IPv4 address pool is `192.168.0.0/16`, a route exists with destination `192.168.0.0/17` in the VPC routing table, and the route creator is not the VPN server. To resolve this issue, delete the conflicting VPC route. |
| `cannot_reserve_ip_address`              | `IP address exhaustion (release addresses on the VPN's subnet).`                                                                             | This issue commonly occurs if an IP address isn't available on the VPN server's subnet. Release associated resources, such as instances, load balancers, or VPN servers on the subnet.       |
| `internal_error` | `Internal error (contact IBM support).` | Contact IBM Support to analyze and resolve internal errors. |
{: caption="Table 1. VPN server health diagnostics" caption-side="bottom"}
