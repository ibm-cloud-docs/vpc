---

copyright:
  years: 2023, 2024
lastupdated: "2024-03-18"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About Reservations for VPC
{: #about-reserved-virtual-servers-vpc}

[Select availability]{: tag-green}

{{site.data.keyword.cloud}} Reservations are a great option when you want significant cost savings and guaranteed resources for future deployments. You can choose a 1 or 3-year term, server quantity, specific profile, and provision those servers when needed.
{: shortdesc}

{{site.data.keyword.cloud}} Reservations are available in only the US South (Dallas), US East (Washington DC), Brazil (São Paulo), Canada (Toronto), United Kingdom (London), EU Germany (Frankfurt), Spain (Madrid), Japan (Osaka), Japan (Tokyo), and Australia (Sydney) multizone regions (MZRs).
{: note}

Reservations offer many advantages, including the following benefits:

| Benefit | Description |
| ----- | ----- |
| Cost savings | Save up to 60% by choosing a 1 or 3-year term compared to on-demand virtual server billing cycles. |
| Guaranteed capacity | Your servers are there when you need them. {{site.data.keyword.cloud}} Reservations are guaranteed capacity within the availability zone and data center of your choice for the life of your committed term. |
| Predictable management | Accurately plan for the future and define your budgeting roadmap with reserved capacity for 1-year or 3-year terms. Your reserved pricing is a fixed discount rate that is applied to the corresponding virtual server profile on-demand rates that are listed in the {{site.data.keyword.cloud}} portal. |
| Flexibility in deployment | Convert any existing on-demand virtual server to {{site.data.keyword.cloud}} Reservation billing. Attach or detach any compatible virtual server to your reservation. |
{: caption="Table 1. Benefits of IBM Cloud Reservations" caption-side="top"}

## Supported profiles
{: #reserved-virtual-servers-vpc-supported-profiles}

The following x86 Balanced profiles for virtual servers are available when you provision a reservation in an available MZR.

* bx2-2x8
* bx2d-2x8
* bx2-4x16
* bx2d-4x16

The following profiles are available in only the Dallas (_us-south_), Frankfurt (_eu-de_), and London (_eu-gb_) MZRs.

| Balanced profiles | Compute profiles | Memory profiles |
| --- | --- | --- |
| bx3d-2x10 | cx3d-2x5 | mx3d-128x1280  |
| bx3d-4x20 |  cx3d-4x10 | mx3d-16x160  |
| bx3d-8x40 |  cx3d-8x20 |  mx3d-176x1760 |
| b3d-16x80 |  cx3d-16x40 |  mx3d-24x240 |
| bx3d-24x120 |  cx3d-24x60 |  mx3d-2x20 |
| bx3d-32x160 | cx3d-32x80 |  mx3d-32x320 |
| bx3d-48x240 | cx3d-48x120  |  mx3d-48x480 |
| bx3d-64x320 | cx3d-64x160  | mx3d-4x40 |
| bx3d-96x480 | cx3d-96x240 | mx3d-64x640  |
| bx3d-128x640 | cx3d-128x320 | mx3d-8x80 |
| bx3d-176x880 | cx3d-176x440  |  mx3d-96x960 |
{: caption="Table 2. Supported Gen 3 profiles for reservations" caption-side="top"}

For more information about profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles).

## Limitations
{: #limitations-reserved-virtual-servers-vpc}

Consider the following limitations before you provision a reservation and provision a virtual server within that reservation.

* You can't resize a virtual server that's in a reservation.
* Reservations comply with the VPC quotas and service limits. For more information, see [Quotas and service limits](/docs/vpc?topic=vpc-quotas).
* SGX isn't supported.

## Notifications
{: #notifications-reserved-virtual-servers-vpc}

When your reservation is complete, a confirmation message is sent.

You receive a [notification](https://cloud.ibm.com/user/notifications){: external} before the end of the term of your reservation. From here, you can decide whether to renew your reservation or cancel.

If **Auto-renew** is selected when you provision a reservation, your reservation automatically renews when it expires.
{: tip}

## Next steps
{: #next-steps-reserved-virtual-servers-vpc}

After you review and decided on your options, you're ready to provision your reservation. For more information, [Provisioning a reservation for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).
