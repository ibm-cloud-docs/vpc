---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Activity tracking event changes for routing tables
{: #at-changes-announcement-routing-table}

As of 30 September 2024, VPC routing tables support tagging, which requires routing tables to be identified by a Cloud Resource Name (CRN). As part of this change, the field values of the following {{site.data.keyword.atracker_full_notm}} events were updated:
{: shortdesc}

* `is.vpc.routing-table.create`
* `is.vpc.routing-table.read`
* `is.vpc.routing-table.attach`
* `is.vpc.routing-table.detach`
* `is.vpc.routing-table.update`
* `is.vpc.routing-table.delete`

The following fields' contain changed values:

* Field: `logSourceCRN`
   * Old value: VPC's CRN
   * New value: VPC routing table's CRN

* Field: `message`
   * Old value: contains the VPC's name
   * New value: contains the VPC routing table's name

* Field: `target.id`
   * Old value: VPC's CRN
   * New value: VPC routing table's CRN

* Field: `target.name`
   * Old value: VPC's name
   * New value: VPC routing table's name

If the `responseData` field contains a `routingTable` object, the `routingTable` includes a CRN field.
{: note}
