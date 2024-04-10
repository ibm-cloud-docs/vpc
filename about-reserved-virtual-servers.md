---

copyright:
  years: 2023, 2024
lastupdated: "2024-04-10"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About Reservations for VPC
{: #about-reserved-virtual-servers-vpc}

{{site.data.keyword.cloud}} Reservations are a great option when you want significant cost savings and guaranteed resources for future deployments. You can choose a 1 or 3-year term, server quantity, specific profile, and provision those servers when needed.
{: shortdesc}

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

The following x86 profiles for virtual servers are available when you provision a reservation in all [ten available MZRs](/docs/overview?topic=overview-locations).

* bx2 profiles
* bx2d profiles
* cx2 profiles
* cx2d profiles
* mx2 profiles
* mx2d profiles
* ux2d profiles
* vx2d profiles

The following profiles are available in specific MZRs.

* bx2a profiles are available in Toronto only.
* bx3d, cx3d, and mx3d profiles are available in Dallas, Frankfurt, London, Madrid, Toronto, and Washington DC.
* gx2 profiles are available in all regions except Madrid.
* ox2 profiles are available in Dallas, Frankfurt, London, Osaka, Tokyo, and Washington DC.

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
