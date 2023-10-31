---

copyright:
  years:  2023
lastupdated: "2023-10-30"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About secondary IP addresses
{: #vni-about-secondary-ip}

This VPC feature is available only to accounts with special approval to preview this feature.
{: preview}

You can configure multiple private IPv4 addresses for your virtual server instances or bare metal servers.
{: shortdesc}

Secondary IP addresses are only configurable through virtual network interfaces. Child network interfaces don't support secondary IP address configurations. You must therefore create your instance or bare metal server with network attachments and virtual network interfaces to be able to configure secondary IP addresses.

You are responsible for configuring the same secondary IP addresses on the interfaces in the guest operating system of your instances or bare metal servers to ensure the addresses take effect.
{: tip}

## Configuring secondary IP addresses for a virtual network interface
{: #configuring-secondary-ips-for-vni}

You must configure the primary IP address when creating a virtual network interface. You can configure additional (secondary) reserved IP addresses when creating the virtual network interface, or you can later add secondary IP addresses individually. All secondary IP addresses must be in the same subnet as the primary IP address. The total number of IP addresses on a virtual network interface is limited by [quota](/docs/vpc?topic=vpc-quotas#virtual-network-interfaces-quotas).
