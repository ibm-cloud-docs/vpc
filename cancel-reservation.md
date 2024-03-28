---
copyright:
  years: 2023, 2024
lastupdated: "2024-03-25"

keywords: 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting an inactive or expired reservation
{: #reserved-capacity-cancel-reservation}

After you purchase a reservation, you can't delete it before the end of the term. However, you can delete an inactive or expired reservation.
{: shortdesc}

You can't delete an active reservation. You can delete only an _inactive_ or _expired_ reservation. This action can't be reversed.
{: note}

## Deleting an inactive or expired reservation with the UI
{: #request-reservation-delete-ui}
{: ui}

Use the following steps to delete an inactive or expired reservation.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click **Navigation Menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Reservations**.
2. In the reservation list, select the vertical action button to delete the reservation. You can also select the reservation first, then delete the reservation by using the **Actions** menu.

Virtual servers that are attached to a reservation are charged the on-demand rates when the reservation is deleted.
{: important}

Reservations can be deleted for cases such as degraded capacity. All deletion requests are reviewed on a case-by-case basis. To delete an active reservation, [contact support](/docs/get-support?topic=get-support-using-avatar).

## Deleting an inactive or expired reservation with the CLI
{: #reservation-delete-cli}
{: cli}

Use the following command to delete a reservation.

```sh
ibmcloud is reservation-delete (RESERVATION1 RESERVATION2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

For this example, you see a response similar to the following output.

```sh
ibmcloud is reservation-delete r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2
ibmcloud is reservation-delete r006-81222eee-b3e0-4dc3-b429-aee9e5c0abf2 r106-81222eee-b3e0-4dc3-b429-aee9e5c0abf3
```
{: screen}

Where the following argument and option values are used

* RESERVATION1: ID or name of the reservation.
* RESERVATION2: ID or name of the reservation.
* --force, -f: Force the operation without confirmation
* --output: Specify output format, only JSON is supported. One of: JSON.
* -q, --quiet: Suppress verbose output.
