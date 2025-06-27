---
copyright:
  years: 2018, 2025
lastupdated: "2025-06-27"

keywords: port, range, nlb, configuring, udp

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Configuring UDP for network load balancers
{: #nlb-udp}

You can configure the User Datagram Protocol (UDP) for the {{site.data.keyword.nlb_full}} (NLB) listener and pool. UDP is a communications protocol that establishes low-latency and loss-tolerating connections between applications on the internet. It speeds up transmissions by enabling the transfer of data before an agreement is provided by the receiving party.
{: shortdesc}

The following information does not apply to Private Path network load balancers.
{: note}

UDP is only supported on network load balancers. When configuring UDP and attaching a pool to your listener, you must configure the pool with the same protocol as the listener.
{: important}

For more information about limitations, see [Known issues for network load balancers](/docs/vpc?topic=vpc-nlb-limitations) and [Port range limitations](/docs/vpc?topic=vpc-nlb-port-ranges&interface=ui#port-range-limitations).

## Configuring UDP using the UI
{: #udp-ui}
{: ui}

You can configure UDP when [creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer), or afterwards using the following procedure:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](/login){: external} and log in to your account.

2. Select the **Navigation menu** ![Menu icon](../icons/icon_hamburger.svg), then click **Infrastructure** ![VPC icon](../../icons/vpc.svg) > **Network** > **Load balancers**.

3. Click the NLB that you want to change.

4. From the NLB details page, select the Front-end listeners tab. Then, select the listener that you want to edit.

   You can also click the **Create listener** button to create a listener.
   {: tip}

   Select the UDP that receives your inbound customer traffic from the Protocol drop-down menu.

5. From the NLB details page, select the Back-end pools tab. Then, select the pool that you want to edit.

   You can also click the **Create pool** button to create a pool.
   {: tip}

   Select the UDP that sends your outbound customer traffic from the Protocol drop-down menu.

## Configuring UDP using the CLI
{: #udp-cli}
{: cli}

You can configure the UDP using the CLI when [creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer&interface=cli) using the `udp` option.

## Configuring UDP using the API
{: #udp-api}
{: api}

For listeners and pools, you can configure UDP using the API when [creating a network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer&interface=api) by specifying `udp` for the `protocol` attribute.
