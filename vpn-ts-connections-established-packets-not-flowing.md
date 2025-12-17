---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my dynamic VPN connection established but packets not flowing?
{: #troubleshoot-packets-not-flowing}
{: troubleshoot}
{: support}

Even when all connections are established between IBM Cloud and an on-premises network, packets might not flow due to various underlying issues. This issue can occur despite successful IPsec and BGP connections that might indicate potential configuration or routing problems.
{: shortdesc}

Connections are established between IBM Cloud and an on-premises network but packets are not flowing between them.
{: tsSymptoms}

The following factors are some of the common causes for packet not flowing:

* Absence of a Transit Gateway connection for dynamic routing.
* Missing routes in the Transit Gateway for the source and destination IP addresses.
* Misconfigured ACLs and security groups that block the necessary traffic.
{: tsCauses}

Follow these steps to fix the issue of packets not flowing:
{: tsResolve}

1. **Check Transit Gateway connection**: Verify that your VPN is connected to a Transit Gateway and that the service connection status is "up." Dynamic routing connections require this connection to function properly, even if the IPsec connection is "up" and the BGP session is established.
1. **Review route report**: Check the route report on Transit Gateway to make sure that the source and destination IP addresses are present in the Transit Gateway. The on-premises IP address must be advertised from the VPN connection, while the IBM Cloud IP address must be advertised from a VPC, Classic, PowerVS, or other supported connection types.
1. **Inspect ACLs and security groups**: Check the ACLs and security groups on the VPN and the destination VPCs to confirm that the appropriate allow rules are configured to allow traffic flow.
