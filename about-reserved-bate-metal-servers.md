---

copyright:
  years: 2024, 2024
lastupdated: "2024-08-28"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# WIP About Reservations for Bare Metal Servers VPC (beta)
{: #about-reserved-bare-metal-vpc}

Reserved bare metal servers are a beta feature that is available for evaluation and testing purposes.{: beta}

{{site.data.keyword.cloud}} Reservations are a great option when you want significant cost savings and guaranteed resources for future deployments. You can choose a 1 or 3-year term, server quantity, specific profile, and provision those servers when needed.
{: shortdesc}

Reservations offer many advantages, including the following benefits:

| Benefit | Description |
| ----- | ----- |
| Cost savings | Save up to 60% by choosing a 1 or 3-year term compared to on-demand billing cycles. |
| Guaranteed capacity | Your servers are there when you need them. {{site.data.keyword.cloud}} Reservations are guaranteed capacity within the availability zone and data center of your choice for the life of your committed term. |
| Predictable management | Accurately plan for the future and define your budgeting roadmap with reserved capacity for 1-year or 3-year terms. Your reserved pricing is a fixed discount rate that is applied to the corresponding server profile on-demand rates that are listed in the {{site.data.keyword.cloud}} portal. |
| Flexibility in deployment | Convert any existing on-demand server to {{site.data.keyword.cloud}} Reservation billing. Attach or detach any compatible server to your reservation. |
{: caption="Table 1. Benefits of IBM Cloud Reservations" caption-side="top"}

A reservation cannot be used for a container service discount such as IBM Kubernetes Service or ROKS.
{: note}

## Supported profiles
{: #reserved-bare-metal-vpc-supported-profiles}

The following x86 profiles for bare metal servers are available when you provision a reservation in an available MZR.

* bx2-metal-96x384
* bx2d-metal-96x384
* bx3-metal-48x256
* bx3d-metal-48x256
* bx3-metal-64x256
* bx3d-metal-64x256
* bx3d-metal-192x1024
* mx3d-metal-48x512
* mx3-metal-64x512
* mx3d-metal-64x512
* mx3d-metal-96x1024
* mx3d-metal-128x1024
* vx3-metal-16x256
* vx3d-metal-16x256
* ux3-metal-16x512
* ux3d-metal-16x512

## Limitations
{: #limitations-reserved-bare-metal-vpc}

Consider the following limitations before you provision a reserved bare metal server.

* You can't resize a reserved bate metal server.
* Reservations comply with the VPC quotas and service limits. For more information, see [Quotas and service limits](/docs/vpc?topic=vpc-quotas).
* SGX isn't supported.
* Reservations aren't compatible with container services. For more information, see [Using reservations to reduce classic worker node costs](/docs/containers?topic=containers-reservations).
* A reserved bare metal server can be used only for {{site.data.keyword.cloud}} Bare Metal Servers for VPC resources.

Reservations don't have a resource quota. But, bare metal server reservations share your account's bare metal server quota. So, bare metal server reservations are limited by your [Bare metal server quota](/docs/vpc?topic=vpc-quotas#vsi-quotas). Keep in mind that bare metal servers that are attached to a reservation do not count toward your quota consumption. < REVIEW NOTE > I am not sure how much detail is appropriate here. The SRB has a little more information that might confuse customers.</ REVIEW NOTE >
{: note}

### Bandwidth speed billing for a reservation
{: #reservations-bandwidth-billing-bare-metal}

Bandwidth speed billing occurs when a server is attached to the reservation. This means that billing for premium operating systems and dynamic network bandwidth occurs at the start event for a server that is attached to a reservation. Dynamic network bandwidth is discounted at the same 1 or 3-year rates as the reservation term, but bills only while a server is attached to the reservation.

## Notifications
{: #notifications-reserved-bare-metal-vpc}

When your reservation is complete, a confirmation message is sent.

You receive a [notification](https://cloud.ibm.com/user/notifications){: external} before the end of the term of your reservation. From here, you can decide whether to renew or cancel your reservation.

If **Auto-renew** is selected when you provision a reservation, your reservation automatically renews when it expires.
{: tip}

## Next steps
{: #next-steps-bare-metal-vpc}

After you review and decided on your options, you're ready to provision a reservation. For more information, see...
