---

copyright:
  years: 2024
lastupdated: "2024-09-20"

keywords: custom routes

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Activity Tracker event changes for routing tables
{: #at-changes-announcement-routing-table}

Starting 30 Sep 2024, VPC routing tables support tagging, which requires routing tables to be identified by a Cloud Resource Name (CRN). 
As a result of this new feature, the field values of the following Activity Tracker events were updated:
{: shortdesc}

* `is.vpc.routing-table.create`
* `is.vpc.routing-table.read`
* `is.vpc.routing-table.attach`
* `is.vpc.routing-table.detach`
* `is.vpc.routing-table.update`
* `is.vpc.routing-table.delete`

Changed values:

* Field: `logSourceCRN`

   Old value: VPC's CRN \n New value: VPC routing table's CRN

* Field: `message`
   Old value: contains VPC's name \n New value: contains VPC routing table's name

* Field: `target.id`
   Old value: VPC's CRN \n New value: VPC routing table's CRN

* Field: `target.name`
   Old value: VPC's name \n New value: VPC routing table's name

If the responseData field contains a `routingTable` object, the `routingTable` includes a CRN field.
{: note}
