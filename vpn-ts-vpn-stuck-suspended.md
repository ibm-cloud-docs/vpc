---

copyright:
  years: 2021, 2026
lastupdated: "2026-05-25"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does my site‑to‑site VPN remain in a Suspended state even after my account is reactivated?
{: #troubleshoot-s2s-vpn-stuck-suspended}
{: troubleshoot}
{: support}

A site‑to‑site VPN gateway can remain in a _Suspended_ state even after the account is reactivated. In such cases, the VPN suspension isn't automatically cleared during account recovery and requires intervention from the IBM side to return to an active state.
{: shortdesc}

After your account is reactivated, the VPN gateway still shows a _Suspended_ status in the console. The VPN traffic doesn't pass, which indicates that the resource is suspended and requires support assistance.
{: tsSymptoms}

You might observe the following conditions:

* The console shows that the VPN gateway is suspended.
* VPN traffic is down

This issue occurs when an account‑level suspension places dependent resources, such as VPN gateways, into a suspended state. When the account is later reactivated, the VPN gateway doesn't always automatically recover.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1. Confirm that your account status is **Active**.
1. Identify the exact VPN gateway ID that is showing as suspended in the console.
1. Verify that the VPN gateway ID belongs to the correct account.
1. [Open an IBM support case](/unifiedsupport/cases/form){: external} and provide the following information:

   * VPN gateway ID
   * Account ID
   * Region where the VPN gateway exists

1. Request recovery of the VPN gateway and wait for confirmation for the VPN gateway status to change to **Active**.
1. Refresh the console and verify that the VPN traffic flows.
