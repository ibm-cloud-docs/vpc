---

copyright:
  years: 2025
lastupdated: "2025-04-28"

keywords: troubleshooting floating ip address, floating ip

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I seeing an incomplete list of floating IP addresses on my bare metal server network interface?
{: #fip-incomplete-ips-on-bare-metal-server}
{: troubleshoot}
{: support}

Users see an incomplete list of floating IPs on a bare metal server interface when the interface is not yet available.
{: shortdesc}

When you use the VPC API to [list floating IP addresses on a bare metal server network interface](/apidocs/vpc#list-bare-metal-server-network-interface-floating-), you might get an incomplete list of the floating IP addresses.
{: tsSymptoms}

The floating IP associated with a bare metal network interface is not available before the network interface `status` is `available`.
{: tsCauses}

- Wait for the bare metal server network interfaces to be `available` before you list the floating IP addresses on the interfaces.

- [List all floating IPs](/apidocs/vpc#list-floating-ips) to view those addresses associated with bare metal server interfaces that are not yet `available`.
{: tsResolve}
