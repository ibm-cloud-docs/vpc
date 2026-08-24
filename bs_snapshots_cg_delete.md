---

copyright:
  years: 2022, 2026
lastupdated: "2026-08-24"

keywords: consistency group, snapshots, backups, instance snapshot, instance backup, delete consistency group,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deleting snapshot consistency groups
{: #snapshots-vpc-delete-consistency-groups}

Delete a snapshot consistency group while choosing whether to keep or delete its member snapshots.
{: shortdesc}

When you delete a consistency group, you can choose to delete or keep the individual member snapshots. If you update the consistency group to keep the individual snapshots before you delete the group, an activity tracking event is created. However, a backup job is not created because the backup snapshots remain intact.

## Deleting a consistency group in the console
{: #delete-consistencygroup-ui}
{: ui}

1. Go to the list of snapshot consistency groups. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage Snapshots**.
1. On the Consistency groups tab, click the name of a group on the list.
1. Click the Actions > Delete.
1. Type `Delete`.
1. Click **Delete**.

## Deleting a consistency group from the CLI
{: #delete-consistencygroup-cli}
{: cli}

You can delete a consistency group from the CLI by running the following command.

```sh
ibmcloud is snapshot-consistency-group-delete CONSISTENCY_GROUP_ID
```
{: pre}

The following example deletes two consistency groups that are identified by their names.

```sh
ibmcloud is snapshot-consistency-group-delete snapshot-consistency-group-1 snapshot-consistency-group-2
```
{: pre}

```sh
This will delete snapshot consistency group snapshot-consistency-group-1, snapshot-consistency-group-2 and cannot be undone. Continue [y/N] ?> y

Deleting snapshot consistency group snapshot-consistency-group-1, snapshot-consistency-group-2 under account Test account as user test.user@ibm.com...
OK

Deletion request for snapshot consistency groups snapshot-consistency-group-1, snapshot-consistency-group-2 has been accepted.
```
{: screen}

For more information about available command options, see [`ibmcloud is snapshot-consistency-group-delete`](/docs/vpc?topic=vpc-vpc-reference#snapshot-consistency-group-delete).

## Deleting a consistency group with the API
{: #delete-consistencygroup-api}
{: api}

You can programmatically delete a consistency group by calling the `/snapshot_consistency_groups/{id}` method in the [VPC API](/docs/apis/vpc/latest#delete-snapshot-consistency-group){: external} as shown in the following sample request.

```sh
curl -X DELETE\
"$vpc_api_endpoint/v1/snapshot_consistency_groups/$consistency_group_id?version=2023-12-05&generation=2"
-H "Authorization: Bearer $iam_token"
```
{: pre}

## Deleting a consistency group with Terraform
{: #delete-consistencygroup-terraform}
{: terraform}

Use the `terraform destroy` command to conveniently delete a remote object such as a snapshot consistency group. The following example deletes `my-snapshot-consistency-group`.

```terraform
terraform destroy --target ibm_is_snapshot_consistency_group.my-snapshot-consistency-group
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

## Activity Tracking events
{: #consistency-groups-at-events-delete}

All multi-volume snapshot operations generate events in {{site.data.keyword.atracker_full_notm}} regardless if the consistency group was created manually or by the Backup service. For more information, see [Consistency group events](/docs/vpc?topic=vpc-at_events&interface=ui#events-consistency-group).
