---

copyright:
  years: 2026
lastupdated: "2026-10-27"

keywords: troubleshoot, public address range, ipv6, guest OS, link-local, default route, router advertisement

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my guest OS not have IPv6 connectivity after I configure the address for an IPv6 public address range?
{: #troubleshoot-ipv6-par-guest-os}
{: troubleshoot}
{: support}

[Public IPv6 address ranges is an allowlisted beta feature](/docs/vpc?topic=vpc-release-notes&interface=ui#vpc-oct2726) that is available for evaluation and testing purposes to select customers. Access is restricted to allowlisted accounts.
{: beta}

After binding an IPv6 public address range and configuring a static IPv6 address in the guest OS, the guest cannot reach IPv6 destinations or receive inbound connections.
{: shortdesc}

After binding an IPv6 public address range and configuring a static IPv6 address in the guest OS, the guest cannot reach IPv6 destinations or receive inbound connections.
{: tsSymptoms}

One or more of the following conditions might apply:
{: tsCauses}

- The IPv6 link-local address is not configured or was derived using a method other than Modified EUI-64.
- The static IPv6 address configured in the OS is outside the bound range's CIDR block.
- The default IPv6 route is missing from the guest OS routing table.
- The address is configured correctly but security group or network ACL rules are blocking traffic.

Complete the following steps:
{: tsResolve}

1. Verify that the link-local address in the guest OS is derived from the VNI's MAC address using the Modified EUI-64 method (the address should begin with `fe80::`).

   - **Linux**: Run the following command:

     ```sh
     ip -6 addr show
     ```
     {: pre}

   - **Windows**: Run the following command in PowerShell or at the command prompt:

     ```sh
     netsh interface ipv6 show address
     ```
     {: pre}

   If the link-local address is missing, ensure that the guest OS has IPv6 networking enabled and that the network interface is up.

1. Verify that the static IPv6 address that you configured falls within the CIDR block of the bound public address range. Run the following command to retrieve the range's CIDR:

   ```sh
   ibmcloud is public-address-range PAR_ID --output JSON | jq .cidr
   ```
   {: pre}

   The address that you configure in the OS must be an address within that CIDR block.

1. Verify that the guest OS has a default IPv6 route. IBM infrastructure sends periodic Router Advertisement (RA) messages every 200 seconds. If the guest OS starts before the next RA, it might temporarily lack a default route.

   - **Linux**: Run the following command:

     ```sh
     ip -6 route show
     ```
     {: pre}

     If no default route (`default via fe80::...`) is present, wait for the next Router Advertisement (up to 200 seconds) or restart the network interface to trigger a Router Solicitation.

   - **Windows**: Run the following command in PowerShell or at the command prompt:

     ```sh
     netsh interface ipv6 show route
     ```
     {: pre}

     If no default route is present, run `ipconfig /release6` followed by `ipconfig /renew6` to trigger a Router Solicitation, or disable and re-enable the network adapter in **Device Manager**.

1. If routes and addresses look correct, verify your security group and network ACL rules as described in [Why is IPv6 traffic not flowing after I bound a public address range to a VNI?](/docs/vpc?topic=vpc-troubleshoot-ipv6-par-no-traffic).

## Related links
{: #ipv6-par-ts-guest-os-related}

- [About public IPv6 address ranges](/docs/vpc?topic=vpc-ipv6-par-about)
- [Creating and managing IPv6 authorized CIDRs](/docs/vpc?topic=vpc-ipv6-par-authorized-cidr)
- [Creating IPv6 public address ranges and binding to a VNI](/docs/vpc?topic=vpc-ipv6-par-creating-binding)
- [Known issues for public address ranges](/docs/vpc?topic=vpc-par-known-issues)
