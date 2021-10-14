---

copyright:
  years: 2021
lastupdated: "2021-09-14"

keywords: bare metal server billing, billing, pricing

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

# Billing information for Bare Metal Servers for VPC (Beta)
{: #bare-metal-servers-billing}

Bare Metal Servers on VPC is a Beta feature that requires special approval. Contact your IBM Sales representative if you're interested in getting access.
{: beta}

The following information is an overview of billing and pricing for Bare Metal Servers for VPC. Billing and pricing are subject to change. For more information about billing and pricing, contact your IBM Sales representative.
{: shortdesc}

The Beta version of Bare Metal Servers for VPC is provided free of charge.
{: important}

## Invoices
{: #invoices}

To view your account invoices, in the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Manage > Billing and Usage**.

Each account receives a single bill. If you need separate billing for different sets of resources, then you need to create multiple accounts.

For more information about invoices, see [Viewing your invoices](/docs/billing-usage?topic=billing-usage-managing-invoices).

## Billing
{: #price-info}

You are billed for Bare Metal Servers for VPC based on the server profile that you selected. Charging stops only when you delete the bare metal server. Powering off the server doesn't change billing. 

See the following table for the available price plans.

| Profile | Price per hour |
|---------|---------|
| bx2-metal-192x768 | USD 9.219 |
| bx2d-metal-192x768 | USD 12.726 |
{: caption="Table 1. Price plans" caption-side="top"}

You are also billed for other VPC services and resources that are attached to any bare metal servers. For more information about pricing for Bare Metal Servers on VPC, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).
{: note}

## Billing compared to virtual server instances
{: #vs-vsi-billing}

The main difference between virtual server instances and the bare metal servers is that powering off a bare metal server has no effect on the billing cycle. Meaning that hourly metering still accrues at the normal rate whether the server is powered off or on. The billing stops only when the bare metal server is deleted. 
