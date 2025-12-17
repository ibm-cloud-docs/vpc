---

copyright:
  years: 2023, 2025
lastupdated: "2025-12-17"

keywords: virtual private network, VPN, VPN gateway, troubleshooting

subcollection: vpc

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is the service route for my VPN gateway incorrect?
{: #troubleshoot-gateway-unable-to-establish-vpn-connection}
{: troubleshoot}
{: support}

For a policy-based VPN gateway, service routes are propagated to routing tables that contain the `VPN gateway` value as the `Accepts routes from` attribute. These routes have names that are prefixed with `ibm-vpn-gateway-`.
{: shortdesc}

You might find that these service routes are not correct. For example, the `Next hop` is not the same as the private IP of your active gateway member. In this case, traffic is broken even if the VPN connection is `Active`.
{: tsSymptoms}

The VPN gateway service keeps monitoring the health of each VPN gateway. When a fault is detected, the service tries to recover the VPN gateway automatically. The recovery process might fail and cause inaccurate routes to remain.
{: tsCauses}

Follow these steps to fix the service routes:
{: tsResolve}

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.
1. Select the **Navigation menu** ![Navigation menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Routing tables**.
1. Select your VPC from the VPC drop-down menu.
1. Click the routing table to open its details page, then click **Edit**.
1. Clear the **VPN gateway** checkbox in the Accepts routes from (optional) section and click **Save**. Service routes propagated by the VPN gateway are removed.
1. Click **Edit** again.
1. Select the **VPN gateway** option in the Accepts routes from (optional) section and click **Save**. Service routes propagated by the VPN gateway are generated.
