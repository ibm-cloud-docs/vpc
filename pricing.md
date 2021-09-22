---

copyright:
  years: 2017, 2021

lastupdated: "2020-03-17"

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


# Pricing for VPC
{: #pricing-for-vpc}

Pricing of {{site.data.keyword.vpc_full}} is applied separately for [internet data transfer](#pricing-for-data-transfer), [load balancers](#lb-for-vpc-pricing), [VPNs](#vpn-for-vpc-pricing), [virtual server instances](#pricing-for-virtual-servers-for-vpc), and [block storage](#pricing-for-block-storage-for-vpc) used within your {{site.data.keyword.vpc_short}}. The following charges apply to your use of {{site.data.keyword.vpc_short}}. For services billed on a consumption basis, the service tiers are bound to your account, not to any specific VPC instance. Amounts are shown in US dollars.

## Pricing for internet data transfer with IBM Cloud™ Virtual Private Cloud
{: #pricing-for-data-transfer}

The table summarizes the pricing for internet data transfer with {{site.data.keyword.vpc_short}}. There is no charge for private traffic between instances within a VPC and other IBM Cloud services (within the IBM data centers). Traffic between instances that egresses to the public internet is charged.

### Free allowances for internet data transfer
{: #free-allowances-for-internet-data-transfer}

There are no charges for private traffic within your VPC and the use of public gateways.

| Data transfer |  Cost for all {{site.data.keyword.vpc_short}} Customers |
|---------------|------------------|
| Private traffic within the zone | Free |
| Private traffic between zones in the same region | Free |
| Use of public gateway (PGW) | Use of the public gateway is free<br>Traffic exiting the public gateway is billed |
{: caption="Table 1. Free allowances for internet data transfer" caption-side="top"}

### Basic pricing for internet data transfer
{: #basic-paygo-pricing-for-internet-transfer}

For traffic that leaves the VPC, the following pricing applies.

| Data transfer | Amount of data | Pricing (US)|
|-----------|-----------|------------------|
| Egress to internet |  0 - 5 GB | Free |
|  | 6 - 10,000 GB | $0.087 per GB |
|  | 10,001 - 50,000 GB | $0.083 per GB |
|  | 50,001 - 150,000 GB | $0.07 per GB |
|  | 150,001 GB and over | $0.05 per GB |
{: caption="Table 2. Pricing for internet data transfer" caption-side="top"}

*Notes:*
* When you create a new VPC, it can take up to an hour for initial billing charges to appear in the {{site.data.keyword.cloud_notm}} console or API.
* 5 Gb of free data is applied at the account level not for each VPC.


If you have a public gateway or floating IP, you might still see some minimal egress charges even if you didn't send any egress traffic during that time. These charges are for ARP traffic, which is necessary to operate your account.
{: important}

## Pricing for floating IPs
{: #floating-ip-pricing}

A floating IP is charged at the rate of $1 (US) per month, starting when it is reserved. The fee is charged even if the floating IP is not associated to a virtual server instance or not in use. The $1 for the monthly fee is charged even if the floating IP is reserved for only a few days.

## Pricing for load balancers
{: #lb-prices}

Load balancers (LB) for VPC pricing is based on the following metrics, calculated monthly:

* Service usage hours
* Data processed

Load balancer pricing by region is shown in the following table.

| Region | LB service usage / hour | LB data processed / GB |
|---|---|---|
| Dallas | $0.025 | $0.008 |
| Frankfurt | $0.027 | $0.009 |
| Tokyo | $0.028 | $0.009 |

Prices vary by multi-zone regions.
{: note}

Example: The following chart shows an example for a customer using 4500 GB per month for public load
balancing, based in the Dallas region.

| Item | Usage | Rate | Cost |
|---|---|---|---|
| LB service usage | 720 hours | $0.025 / hour | $18 / month |
| Data processed | 4500 GB | $0.008 / GB | $36 / month |

The total charge for this scenario is $54 per month.

## Pricing for VPN for VPC
{: #vpn-vpc-prices}

Virtual Private Network (VPN) for VPC pricing by region is shown in the following table.

| Region | Connection (Peer) per hour | Instance (gateway) per hour |
|---|---|---|
| Dallas | $0.04 | $0.045 |
| Frankfurt |	$0.044 |	$0.0495 |
| Tokyo |	$0.0452 |	$0.0585 |

Data transfer to the internet as a result of using VPNaaS is charged as regular data transfer; outbound to the internet, billed at the VPC level.
{: note}

## Pricing for Virtual Servers for VPC
{: #pricing-for-virtual-servers-for-vpc}
[comment]: # (linked help topic)


For {{site.data.keyword.vsi_is_full}} you're billed at an hourly rate based on instance configurations. Discounts are applied the longer your instance runs. Usage times are calculated per second while the virtual server instance is running. You are not be billed for the time your instance is powered off. For example, if the instance runs for 45 minutes and 32 seconds, you're billed for 45 minutes and 32 seconds. Another example is if you start your instance and run for 20 minutes, then stop your instance for 24 hours, then start the instance and run for 30 minutes; you're billed for the 50 minutes (20 minutes + 30 minutes) the instance was running.
{: shortdesc}

### x86-based instance prices
{: #base-instance-prices}

Base instance prices start at $0.087 per hour. When you create a virtual server, you are prompted to select a virtual server family and select a profile configuration. When you make your selection, the associated hourly rate is displayed in the table. <!-- You can also use the Pricing Calculator to estimate your costs. -->

| Profile | vCPU | RAM | Network Performance Cap (Gbps) | Pricing / Hour |
|---------|---------|---------|---------|---------|
| bx2-2x8 | 2 | 8 | 4 | $0.099 |
| bx2-4x16 | 4 | 16 | 8 | $0.199 |
| bx2-8x32 | 8 | 32 | 16 | $0.397 |
| bx2-16x64 | 16 | 64 | 32 | $0.795 |
| bx2-32x128 | 32  | 128 | 64 | $1.590 |
| bx2-48x192 | 48 | 192 | 80 | $2.385 |
| cx2-2x4 | 2 | 4 | 4 | $0.087 |
| cx2-4x8 | 4 | 8 | 8 | $0.174 |
| cx2-8x16 | 8 | 16 | 16 | $0.348 |
| cx2-16x32 | 16 | 32 | 32 | $0.695 |
| cx2-32x64 | 32  | 64 | 64 | $1.391 |
| mx2-2x16 | 2 | 16 | 4 | $0.124 |
| mx2-4x32 | 4 | 32 | 8 | $0.248 |
| mx2-8x64 | 8 | 64 | 16 | $0.497 |
| mx2-16x128 | 16 | 128 | 32 | $0.994 |
| mx2-32x256 | 32 | 256 | 64 | $1.987 |
{: caption="Table 3. x86-based virtual server pricing by profile option" caption-side="top"}

### POWER-based instance prices
{: #base-instance-prices-power}

Base instance prices start at $0.080 per hour. When you create a virtual server, you are prompted to select a virtual server family and select a profile configuration. When you make your selection, the associated hourly rate is displayed in the table.

| Profile | vCPU | RAM | Network Performance Cap (Gbps) | Pricing / Hour |
|---------|---------|---------|---------|---------|
| bp2-2x8 | 2 | 8 | 6 | $0.089 |
| bp2-4x16 | 4 | 16 | 12 | $0.177 |
| bp2-8x32 | 8 | 32 | 24 | $0.355 |
| bp2-16x64 | 16 | 64 | 48 | $0.710 |
| bp2-32x128 | 32  | 128 | 96 | $1.419 |
| cp2-2x4 | 2 | 4 | 6 | $0.080 |
| cp2-4x8 | 4 | 8 | 12 | $0.155 |
| cp2-8x16 | 8 | 16 | 24 |$0.310 |
| cp2-16x32 | 16 | 32 | 48 | $0.619 |
| cp2-32x64 | 32  | 64 | 96 | $1.239 |
| mp2-2x16 | 2 | 16 | 6 | $0.111 |
| mp2-4x32 | 4 | 32 | 12 | $0.223 |
| mp2-8x64 | 8 | 64 | 24 | $0.445 |
| mp2-16x128 | 16 | 128 | 48 | $0.890 |
| mp2-32x256 | 32 | 256 | 96 | $1.781 |
{: caption="Table 3. POWER-based virtual server pricing by profile option" caption-side="top"}

### Accelerated computing (GPU) instance prices
{: #base-instance-prices-gpu}

Accelerated computing (GPU) instance prices start at $6.502 per hour. When you create a virtual server, you are prompted to select a virtual server family and select a profile configuration. When you make your selection, the associated hourly rate is displayed in the table.

| Profile | vCPU | RAM | GPU | Network Performance Cap (Gbps) | Pricing / Hour |
|---------|---------|---------|---------|---------|---------|
| gp2-24x224x2 | 24 | 224 | 2 | 72 | $6.502 |
| gp2-32x256x4 | 32 | 256 | 4 |  96 | $11.781 |
| gp2-56x448x4 | 56  | 448 | 4 | 100 | $13.116 |
| gp2-56x896x4 | 56 | 896 | 4 | 100 | $15.446 |
{: caption="Table 3. GPU instance pricing by profile option" caption-side="top"}

 Accelerated computing (GPU) instances aren't eligible for the sustained usage tier discount model. They are charged at a flat hourly rate.
 {: note}

### Included operating systems
{: #included-operating-systems}

The following operating systems are included free of charge:

* CentOS 7.latest
* Debian 9.latest (minimal)
* Ubuntu 16.04 LTS, 18.04 (minimal)

### Premium operating systems and add-ons
{: #premium-os-add-ons}

Premium operating systems and other add-ons are available. You'll see the pricing in your Cost Summary.

| Premium operating system       | Amount of data    |  Pricing     |
| ------------------------------ | ----------------- | ---------------------- |
| Red Hat Enterprise Linux 7.x   | 1-4 vCPU      | $.08 per instance-hour   |     
| Red Hat Enterprise Linux 7.x   | 5-64 vCPU     | $.13 per instance-hour   |   
| Windows 2012, 2012 R2, 2016    |                   | $.04 per core per hour  |            
{: caption="Table 4. Premium operating system pricing" caption-side="top"}

### Sustained usage
{: #sustained-usage}

While the instances are charged at an hourly rate, the longer your instance is running, the less expensive the rate is. As the billing month progresses, you receive the following hourly discounts.

| Time elapsed in a month       | Billing discount  |
| ----------------------------- | ----------------- |
| 0-20%                         | Regular retail rate |                 
| 21-40%                        | 5%        |                  
| 41-60%                        | 10%       |                  
| 61-80%                        | 15%        |                  
| 81-100%                       | 20% |
{: caption="Table 5. Tiered discounts" caption-side="top"}  

These discounted tiers provide you with a 10% savings for keeping instances running monthly versus hourly. This discount applies only to base hourly rates; it doesn't apply to software, storage, network, or other charges.

Accelerated computing (GPU) instances aren't eligible for the sustained usage tier discount model. They are charged at a flat hourly rate.
 {: note}

#### Sustained usage example
{: #sustained-usage-example}

Say you purchase an instance from the Balanced virtual server family, with 16 CPUs and 64 GB RAM, at a base price of $0.795 per hour. As the month progresses, your rate decreases as follows:

| Time elapsed in a month       | Billing discount  |  Example rate     |
| ----------------------------- | ----------------- | -------- |
| 0-20%                         | Regular retail rate ($.795) | $116.07    |                
| 21-40%                        | 5%        |   $110.27   |                 
| 41-60%                        | 10%       |    $104.46  |            
| 61-80%                        | 15%        |    $98.66    |                
| 81-100%                       | 20% |       $92.86      |
{: caption="Table 6. Tiered discounts example" caption-side="top"}  

Your total bill, if left running for the entire month, with this model is $522.32. The discounts result in an overall monthly savings of 10%, compared to the hourly rate.

### Suspend billing
{: #suspend-billing}

When you power off an instance, you don't accrue costs for certain compute resources. Billing stops automatically when you stop the instance. The suspend billing feature helps you reduce cost and prevents you from having to re-create an instance when you need its resources again.

In situations where you want to scale your infrastructure up and down in response to workload needs, you can use the suspend billing feature as a faster alternative to creating and deleting instances.
{: tip}

### Billing details
{: #billing-details}

It's important to understand what costs stop accruing and what costs persist when your virtual server instance is powered off.

Billing is suspended only when you power off your virtual server instance through the {{site.data.keyword.cloud_notm}} console, CLI, or API. If you power off your virtual server instance directly through the OS, billing isn't suspended for that instance.
{: note}

Review the following table for details on how suspend billing impacts various resource charges.

| Resource                      | Billing stopped   | Billing persists |
| ----------------------------- | ----------------- | ---------------- |
| vCPU                          |          X        |                  |
| RAM                           |          X        |                  |
| Bandwidth upgrades            |          X        |                  |
| Operating system licenses     |          X        |                  |
| Floating IPs, Load balancers, or other attached networking offerings |                   |         X        |
| Storage                       |                   |         X        |
{: caption="Table 7. Resource billing details" caption-side="top"}

Usage times are calculated per second, for both the in use time and suspended time of your virtual server instance. Even if you never initiate the suspend billing feature by powering off your instance, the billing is calculated per second of the instance's lifecycle.
{: note}


### Suspend billing and sustained usage discounts
{: #suspend-billing-and-sustained-usage-discounts}

For sustained usage discounts, a suspended image picks up from where it was on the discount tier. In other words, a usage discount applies only to the time the image is in use, not to the time the image was suspended.

### Minimum usage charge
{: #minimum-usage-charge}

Virtual server instances have a minimum usage charge per month. If usage is greater than 25% in the billing cycle, you're billed for the actual usage. If usage is less than 25% of the time it existed in the billing cycle, then the minimum charge of 25% applies.

For example, let's say you have an instance running for an entire billing cycle (720 hours). Of that time, the instance was suspended for 577 hours and running for 143 hours. The instance is charged for 180 hours (the minimum of the available hours in that billing period).  

However, let's say you had an instance that was both created and stopped within a billing cycle that lasted for 400 hours total. Of that time, the instance was suspended for 120 hours and ran for 280 hours. The instance would be charged for its usage of 280 hours.

### Billing invoice
{: #billing-invoice}

When you suspend billing on a virtual server, you'll see a few changes in your billing invoice. The relevant charges now appear as usage-based details. For example, you might see the following additions that reflect hours available, hours that instances were used, and total number of hours charged:

```
Computing instance usage...
RAM usage...
Operating system usage...
```
{: screen}

### Resource details
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

## Pricing for Block Storage for VPC
{: #pricing-for-block-storage-for-vpc}

Pricing for {{site.data.keyword.block_storage_is_short}} is based on the capacity or the IOPS that was provisioned, depending on which storage profile you chose. The published monthly rate is [calculated on an hourly basis](#how-are-charges-calculated).

| IOPS Tier  | Published Monthly Rate |
|------------|--------------|
|  3 IOPS/GB |  0.13 USD/GB |
|  5 IOPS/GB |  0.25 USD/GB |
| 10 IOPS/GB |  0.58 USD/GB |
| Custom IOPS| 0.10 USD/GB + 0.07 USD/IOPS |
{: caption="Table 8. Block Storage for VPC pricing" caption-side="top"}

### How are charges calculated?
{: #how-are-charges-calculated}

{{site.data.keyword.block_storage_is_short}} is calculated hourly, based on the total number of hours that the block storage volume exists on the account, until the volume is deleted or the end of a billing cycle, whichever comes first. Hourly billing is calculated differently for a predefined IOPS tier than for custom IOPS. See the following examples.

### Billing calculation for a volume with the general-purpose IOPS tier
{: #billing-calculation-for-a-volume-with-the-general-purpose-IOPS-tier}

You provision a 1000-GB volume with the general-purpose 3 IOPS/GB tier, then use the volume for 72 hours before you delete it. The total price for the volume is billed by the hour, as follows:

((1000 GB x 0.13 USD/GB)/730 hrs) x 72 hrs = $12.82

### Billing calculation for a volume with custom IOPS
{: #billing-calculation-for-a-volume-with-custom-IOPS}

You provision a 1000-GB volume with 2500 IOPS, then use the volume for 72 hours before you delete it. The total price for the volume is billed by the hour, as follows:

(((1000 x 0.10 USD/GB) + (2500 x 0.07 USD)) / 730 hrs) x 72 hrs = $27.12
