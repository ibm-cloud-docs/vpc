---

copyright:
  years: 2023
lastupdated: "2023-12-18"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why isn't the service route for my VPN server correct?
{: #troubleshoot-service-route-incorrect}
{: troubleshoot}
{: support}

For a VPN server, service routes are propagated to routing tables that have `VPN server` selected for the `Accepts routes from` attribute. These routes have names prefixed with `ibm-vpn-server-`.
{: shortdesc}

In rare cases, you might find that these service routes are incorrect. For example, the `Next hop` is not same as the private IP of your VPN server. In these cases, traffic is broken even if you connected to the VPN server successfully.
{: tsSymptoms}

The VPN server service keeps monitoring the health of each VPN server. When a fault is detected, the service tries to recover the VPN server automatically. The recovery process might fail and cause inaccurate routes to remain.
{: tsCauses}

Follow these steps to fix the service routes:
{: tsResolve}

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation Menu** icon![menu icon](../../icons/icon_hamburger.svg), then click **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **>Routing tables** in the Network section.
1. Select your VPC from the VPC drop-down menu.
1. Click the routing table to open its details page, then click **Edit**.
1. Clear the **VPN server** checkbox in the Accepts routes from (optional) section and click **Save**. Service routes propagated by the VPN server are removed.
1. Click **Edit** again.
1. Select **VPN server** in the Accepts routes from (optional) section and click **Save**. Service routes propagated by the VPN server are generated.
