---

copyright:
  years: 2023, 2024
lastupdated: "2024-10-14"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About Reservations for VPC
{: #about-reserved-virtual-servers-vpc}

{{site.data.keyword.cloud}} Reservations are a great option when you want significant cost savings and dedicated resources for future deployments. You can choose a 1 or 3-year term, server quantity, specific profile, and provision those servers when needed.
{: shortdesc}

Reservations offer many advantages, including the following benefits:

| Benefit | Description |
| ----- | ----- |
| Cost savings | Save up to 60% by choosing a 1 or 3-year term compared to on-demand virtual server billing cycles. |
|  |  Save up to 55% by choosing a 1 or 3-year term compared to on-demand bare metal server billing cycles. |
| Dedicated capacity | Your servers are there when you need them. {{site.data.keyword.cloud}} Reservations provide dedicated capacity within the zone of your choice for the life of your committed term. |
| Predictable management | Accurately plan for the future and define your budgeting roadmap with reserved capacity for 1-year or 3-year terms. Your reserved pricing is a fixed discount rate that is applied to the corresponding virtual server profile on-demand rates that are listed in the {{site.data.keyword.cloud}} portal. |
| Flexibility in deployment | Convert any existing on-demand virtual server to {{site.data.keyword.cloud}} Reservation billing. Attach or detach any compatible virtual server to your reservation. |
{: caption="Benefits of IBM Cloud Reservations" caption-side="top"}

A reservation cannot be used for a container service discount such as IBM Kubernetes Service or ROKS.
{: note}

## Supported virtual server profiles
{: #reserved-virtual-servers-vpc-supported-profiles}

The following x86 profiles for virtual servers are available when you provision a reservation in all [available MZRs](/docs/overview?topic=overview-locations).

* bx2 profiles
* bx2d profiles
* cx2 profiles
* cx2d profiles
* mx2 profiles
* mx2d profiles
* ux2d profiles
* vx2d profiles

The following virtual server profiles are available in specific MZRs.

* bx2a profiles are available in Toronto only.
* bx3d, cx3d, and mx3d profiles are available in Dallas, Frankfurt, London, Madrid, Osaka, Sydney, Toronto, and Washington DC.
* gx2 profiles are available in all regions except Madrid.
* gx3d profiles are available in Frankfurt, London, Madrid, Sao Paulo, Sydney, Tokyo, Toronto, and Washington DC.
* ox2 profiles are available in Dallas, Frankfurt, London, Osaka, Tokyo, and Washington DC.

For more information about profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles).

## Supported bare metal server profiles (beta)
{: #reserved-bare-metal-servers-vpc-supported-profiles}

Reserved bare metal servers are a beta feature that is available for evaluation and testing purposes.
{: beta}

All [Gen 2 and Gen 3 bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui#bare-metal-servers-profile-list) are available when you provision a reservation in an available MZR.


### Bandwidth speed billing for a reservation
{: #reservations-bandwidth-billing-bare-metal}

Bandwidth speed billing occurs when a server is attached to the reservation. This means that billing for premium operating systems and dynamic network bandwidth occurs at the start event for a server that is attached to a reservation. Dynamic network bandwidth is discounted at the same 1 or 3-year rates as the reservation term, but bills only while a server is attached to the reservation.

## Notifications
{: #notifications-reserved-virtual-servers-vpc}

When your reservation is complete, a confirmation message is sent.

You receive a [notification](https://cloud.ibm.com/user/notifications){: external} before the end of the term of your reservation. From here, you can decide whether to renew your reservation or cancel.

If **Auto-renew** is selected when you provision a reservation, your reservation automatically renews when it expires.
{: tip}

## Next steps
{: #next-steps-reserved-virtual-servers-vpc}

After you review and decided on your options, you're ready to provision a reservation. For more information, [Provisioning a reservation for VPC](/docs/vpc?topic=vpc-provisioning-reserved-capacity-vpc).
