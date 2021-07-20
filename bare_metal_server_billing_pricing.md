---

copyright:
  years: 2021
lastupdated: "2021-06-01"

keywords: bare metal servers, billing, pricing, vpc

subcollection: vpc

---

{:beta: .beta}
{:codeblock: .codeblock}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:preview: .preview}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Billing information of Bare Metal Servers for VPC (beta)
{: #bare-metal-servers-billing}

This page documents information related to billing and pricing for Bare Metal Servers for VPC.

This is a Beta feature that requires special approval. Contact your IBM Sales representative if you are interested in getting access.
{:beta}

Beta of Bare Metal Servers for VPC is provided free of charge.
{:important}

## Invoices
{: #invoices}

To view your account invoices, in the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Manage > Billing and Usage**.

Each account receives a single bill. If you need separate billing for different sets of resources, then you need to create multiple accounts.

For more information, see [Viewing your invoices](/docs/billing-usage?topic=billing-usage-managing-invoices).

## How will I be billed?
{: #price-info}

You are billed for Bare Metal Servers for VPC based on the server profile that you selected. Charging stops only when you delete the bare metal server. Powering it off has no impact on billing. 

See the following table for the price plan:

| Profile | Price per hour |
|---------|---------|
| bx2-metal-192x768 | USD 9.219 |
| bx2d-metal-192x768 | USD 12.726 |
{: caption="Table 1. Price plan" caption-side="top"}

<!--For full information on VPC pricing, see **(Link to the pricing information page on Marketplace)**-->

You are also billed for other VPC services and resources that are attached to the bare metal servers. For more information, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).
{: note}

## Billing difference compared to virtual server instances
{: #vs-vsi-billing}

The key difference between virtual server instances and the bare metal servers is that powering off a bare metal server has no effect on the billing cycle. Meaning that hourly metering still accrues in power off state at the normal rate. The billing stops only when the bare metal server is deleted. 
