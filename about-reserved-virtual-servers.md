---

copyright:
  years: 2023, 2024
lastupdated: "2024-01-16"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# About reservations for VPC
{: #about-reserved-virtual-servers-vpc}

[Select availability]{: tag-green}

{{site.data.keyword.cloud}} Reservations are a great option when you want significant cost savings and guaranteed resources for future deployments. You can choose a 1 or 3-year term, server quantity, specific profile, and provision those servers when needed.
{: shortdesc}

{{site.data.keyword.cloud}} Reservations are available in only the Sydney multizone region.
{: note}

Reservations offers many advantages, including the following benefits:

| Benefit | Description |
| ----- | ----- |
| Cost savings | Save up to 60% by choosing a 1 or 3-year term compared to on-demand virtual server billing cycles. |
| Guaranteed capacity | Your servers are there when you need them. {{site.data.keyword.cloud}} Reservations are guaranteed capacity within the availability zone and data center of your choice for the life of your committed term. |
| Predictable management | Accurately plan for the future and define your budgeting roadmap with reserved capacity for 1-year or 3-year terms. Your reserved pricing is a fixed discount rate that is applied to the corresponding virtual server profile on-demand rates that are listed in the {{site.data.keyword.cloud}} portal. |
| Flexibility in deployment | Convert any existing on-demand virtual server to {{site.data.keyword.cloud}} Reservation billing. Attach or detach any compatible virtual server to your reservation. |
{: caption="Table 1. Benefits of IBM Cloud Reservations" caption-side="top"}

## Supported profile families
{: #reserved-virtual-servers-vpc-supported-profiles}

The following x86 Balanced profiles for virtual servers are available when you provision a reservation in the Sydney multizone region.

* bx2-2x8
* bx2d-2x8
* bx2-4x16
* bx2d-4x16

For more information about profiles, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles).

<!-- | Family | Description |
| -------- | ----------- |
| Balanced | Best for midsized databases and common cloud applications with moderate traffic. |
| Compute | Best for workloads with intensive CPU demands, such as high web traffic workloads, production batch processing, and front-end web servers. |
| Memory  | Best for memory intensive workloads, such as large caching workloads, intensive database applications, or in-memory analytics workloads. |
| Very High Memory | Optimized for running small to medium in-memory databases and OLAP workloads, such as SAP BW/4 HANA.  |
| Ultra High Memory | Optimized to run large in-memory databases and OLTP workloads, such as SAP S/4 HANA. |
| GPU | Provide on-demand access to NVIDIA V100 and A100 GPUs to accelerate AI, high-performance computing, data science, and graphics workloads. |
| Optimized storage | Offer temporary SSD instance storage disks at a ratio of 1 vCPU to 300 GB instance storage with a smaller price point per GB. These profiles are designed for storage-dense workloads and offer `virtio` interface type for attached disks. |
| s390x (LinuxONE) | Provides dedicated CPU core, memory, and I/O channel to better manage your high-performance workloads. |
{: caption="Table 2. Supported reserved virtual server profile families" caption-side="top"}-->

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

After you review and decided on your options, you're ready to provision your reservation. For more information, [Provisioning a reservation for VPC](/docs/vpc?topic=vpc-provisioning-reservation-vpc).
