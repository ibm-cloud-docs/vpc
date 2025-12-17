---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN gateway, troubleshooting, classic

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I access my classic virtual server instance privately through a client-to-site VPN server after I integrate with Transit Gateway?
{: #troubleshoot-c2s-cannot-access-classic-vsi}
{: troubleshoot}
{: support}

I tried to access the classic network through a client-to-site VPN server integrated with Transit Gateway. I made sure that and the allocated client IP pool is not in the `10.x.x.x` range. However, I still cannot access my virtual server instances on classic through its private IP address even though the client-to-site VPN connection is up and running.
{: tsSymptoms}

By default, your classic virtual server instance is configured to route through the public interface and doesn't know how to route traffic to the private network on-premises or remote.
{: tsCauses}

Follow the steps to resolve this issue:
{: tsResolve}

1. Go to **Classic Infrastructure > Devices** and locate the virtual server instance.
1. Use your preferred way of virtual server management to access your classic virtual server instance through its public IP address.
1. In the Network details table, find the gateway of the private interface by hovering over the information icon of the IP address.
1. Add a route to specify the destination CIDR and the gateway IP. As an example, in the following command for Linux, `10.240.5.0/24` is the CIDR of your network on-premises and `10.188.170.65` is the gateway of the private IP address.

   ```sh
   ip route add 10.240.5.0/24 via 10.188.170.65
   ```
For more details about adding routes on different operating systems, see [How do I add the new routing for an operating system?](/docs/virtual-servers?topic=virtual-servers-faqs-servers-general-#how-to-add-the-new-routing-for-various-oses).
