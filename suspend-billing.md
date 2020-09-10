---

copyright:
  years: 2017, 2020

lastupdated: "2020-09-10"

keywords: pricing, billing, data, instance, suspend 

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}


# Suspend billing for VPC
{: #suspend-billing}

When you power off an instance, you don't accrue costs for certain compute resources. Billing stops automatically when you stop the instance. The suspend billing feature helps you reduce cost and prevents you from having to re-create an instance when you need its resources again.

In situations where you want to scale your infrastructure up and down in response to workload needs, you can use the suspend billing feature as a faster alternative to creating and deleting instances.
{:tip}

## Billing details
{: #billing-details}

It's important to understand what costs stop accruing and what costs persist when your virtual server instance is powered off.

Billing is suspended only when you power off your virtual server instance through the {{site.data.keyword.cloud_notm}} console, CLI, or API. If you power off your virtual server instance directly through the OS, billing isn't suspended for that instance.
{:note}

Review the following table for details on how suspend billing impacts various resource charges.

| Resource                      | Billing stopped   | Billing persists |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| Operating system licenses     |          X        |                  |
| Floating IPs, Load balancers, or other attached networking offerings |                   |         X        |
| Storage                       |                   |         X        |
{: caption="Table 7. Resource billing details" caption-side="top"}

Usage times are calculated per second, for both the in use time and suspended time of your virtual server instance. Even if you never initiate the suspend billing feature by powering off your instance, the billing is calculated per second of the instance's lifecycle.
{:note}

### Suspend billing and sustained usage discounts
{: #suspend-billing-and-sustained-usage-discounts}

For sustained usage discounts, a suspended image picks up from where it was on the discount tier. In other words, a usage discount applies only to the time the image is in use, not to the time the image was suspended.

### Billing invoice
{: #billing-invoice}

When you suspend billing on a virtual server, you'll see a few changes in your billing invoice. The relevant charges now appear as usage-based details. For example, you might see the following additions that reflect hours available, hours that instances were used, and total number of hours charged:

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{:screen}

## Resource details
{: #resource-details}

### Storage
{: #suspend-billing-storage}

When you suspend billing on a virtual server instance, the associated storage persists, but you can't access data while the virtual server instance is powered off. When you resume billing on the instance, you can then access your data and normal billing begins again.

### IP addresses
{: #suspend-billing-ip-addresses}

All network configurations and IPs (private IPs from the subnet range) remain unchanged while the instance is suspended.

You can use the [Activity Tracker](/docs/vpc?topic=vpc-at-events) to check the status of your instance.

### Limitations
{: #suspend-billing-limitations}

Suspended virtual servers continue to count towards your account-wide device quota.

