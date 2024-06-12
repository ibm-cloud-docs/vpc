---

copyright:
  years: 2020, [{CURRENT_YEAR}]
lastupdated: "[{LAST_UPDATED_DATE}]"

keywords: pricing, billing, data, instance, VSI, suspend 

subcollection: vpc


---


{{site.data.keyword.attribute-definition-list}}

# Suspend billing for VPC
{: #suspend-billing}

When you stop a virtual server instance by using the {{site.data.keyword.cloud_notm}} console, CLI, or API, you don't accrue costs for certain compute resources. Billing stops automatically when you stop the instance. The suspend billing feature helps you reduce cost and prevents you from having to re-create an instance when you need its resources again.

In situations where you want to scale your infrastructure up and down in response to workload needs, you can stop an instance (and stop billing) as a faster alternative to creating and deleting instances.
{: tip}

## Billing details
{: #billing-details}

It's important to understand what costs stop accruing and what costs persist when your virtual server instance is powered off.

Review the following table for details on how suspended billing impacts various resource charges.

| Resource                      | Billing stopped   | Billing persists |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| Operating system licenses     |          X        |                  |
| Floating IPs, Load balancers, or other attached networking offerings |                   |         X        |
| Storage                       |                   |         X        |
{: caption="Table 7. Resource billing details" caption-side="bottom"}

Usage times are calculated per second, for both the in use time and suspended time of your virtual server instance. Even if you never initiate the suspend billing feature by powering off your instance, the billing is calculated per second of the instance's lifecycle. No minimum usage requirement exists for an instance. 
{: note}

### Suspend billing and sustained usage discounts
{: #suspend-billing-and-sustained-usage-discounts}

For sustained usage discounts, a suspended image picks up from where it was on the discount tier. In other words, a usage discount applies only to the time the image is in use, not to the time the image was suspended.

### Billing invoice
{: #billing-invoice}

When you suspend billing on a virtual server, you see a few changes in your billing invoice. The relevant charges now appear as usage-based details. For example, you might see the following additions that reflect hours available, hours that instances were used, and total number of hours charged:

```sh
Computing instance usage...
RAM usage...
Operating system usage...
```
{: screen}

## Resource details
{: #resource-details}

### Storage
{: #suspend-billing-storage}

When you suspend billing on a virtual server instance, the associated storage persists, but you can't access data while the virtual server instance is powered off. When you resume billing on the instance, you can then access your data, and normal billing begins again.

### IP addresses
{: #suspend-billing-ip-addresses}

All network configurations and IPs (private IPs from the subnet range) remain unchanged while the instance is suspended.

You can use the [Activity Tracker](/docs/vpc?topic=vpc-at-events) to check the status of your instance.

### Limitations
{: #suspend-billing-limitations}

Suspended virtual servers continue to count toward your account-wide device quota.

