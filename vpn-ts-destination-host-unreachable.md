---

copyright:
  years: 2018, 2026
lastupdated: "2026-04-21"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I get "Destination host unreachable" even though the VPN is active?
{: #troubleshoot-vpn-destination-unreachable}
{: troubleshoot}
{: support}

The __Destination host unreachable__ error occurs when a routing misconfiguration exists between the VPC and on-premises network. Even if the VPN tunnel is active, traffic fails when routes are missing or incorrect, on either side of the VPN connection.
{: shortdesc}

When you attempt to connect to a resource over the VPN, the connection fails even though the VPN shows as active. You might observe the following conditions:
{: tsSymptoms}

* The VPN connection status is visible and shows as active.
* Security groups and network ACLs allow the required traffic.
* Traffic appears to stop before reaching the VPN gateway on either side.
* No response traffic is received from the destination on either side.
* Ping or traceroute results in the following error message:

   ```sh
   Destination host unreachable..
   ```

This issue occurs when traffic isn't properly routed through the VPN tunnel in one or both directions. Routing misconfigurations can prevent traffic from reaching the VPN gateway, or allow traffic to reach the gateway but fail to reach the destination network. Similarly, return traffic might not have a valid route back through the VPN tunnel.
{: tsCauses}

Follow these steps to resolve the issue:
{: tsResolve}

1. __Validate the VPN gateway__ - Review the IPsec logs on the VPN gateway and confirm that the tunnel is __Active__ and that the IBM Cloud VPN gateway sends and receives traffic.
1. __Use traceroute to check traffic path__ - Run traceroute from the virtual server and verify that one of the hops is the VPN gateway’s private IP.
1. __Verify VPC route tables__
   * Check the VPC route table associated with the subnet where the workload resides. Ensure that a route for the on-premises CIDR (for example, `170.226.3.64/27`) exists and points to the correct VPN connection.
   * If the VPN gateway and workload are in the same subnet, re‑create or move the workload to a separate, nonoverlapping subnet (for example, `10.249.195.0/24`).
   * When you configure the VPN gateway, ensure that __Accept routes from__ is enabled for the ingress routing table. This option allows the VPN gateway to learn and add routes for the peer devices. For more information see, [Creating a routing table](/docs/vpc?topic=vpc-create-vpc-routing-table&interface=ui#cr-using-the-ui).
1. __Review NAT-T__ - Enable NAT Traversal (NAT-T) on the on-premises device if required, and verify that connectivity to the VPN gateway private IP address (for example, `10.249.194.6`) is successful.
1. __Validate on-premises routing__
   * Validate routing on the on-premises network. Confirm that routes for VPC CIDRs (for example, `10.249.194.0/24`) exist and that return traffic is directed back through the VPN tunnel.
