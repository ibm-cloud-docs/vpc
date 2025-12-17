---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN server, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why isn't the service route for my VPN server correct?
{: #troubleshoot-service-route-incorrect}
{: troubleshoot}
{: support}

For a VPN server, service routes are propagated to routing tables that contain `VPN server` value for the `Accepts routes from` attribute. These routes have names that are prefixed with `ibm-vpn-server-`.
{: shortdesc}

In rare cases, you might find that these service routes are incorrect. For example, the `Next hop` is not the same as the private IP of your VPN server. In these cases, traffic is broken even if you connected to the VPN server successfully.
{: tsSymptoms}

The VPN server service keeps monitoring the health of each VPN server. When a fault is detected, the service tries to recover the VPN server automatically. The recovery process might fail and cause inaccurate routes to remain.
{: tsCauses}

Follow these steps to fix the service routes:
{: tsResolve}

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Routing tables**.
1. Select your VPC from the VPC drop-down menu.
1. Click the routing table to open its details page, then click **Edit**.
1. Clear the **VPN server** checkbox in the Accepts routes from (optional) section and click **Save**. Service routes propagated by the VPN server are removed.
1. Click **Edit** again.
1. Select the **VPN server** option in the Accepts routes from (optional) section and click **Save**. Service routes propagated by the VPN server are generated.
