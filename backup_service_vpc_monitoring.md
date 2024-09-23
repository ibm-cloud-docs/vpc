---

copyright:
  years: 2022, 2024
lastupdated: "2024-09-23"

keywords: Backup, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring backup policy health states, lifecycle status, and events
{: #backup-vpc-monitoring}

By using the UI, CLI, or API, you can check on the status and health states of the backup service.
{: shortdesc}

## Backup policy statuses
{: #backup-policy-statuses}

When you [view the list of your backup policies in the console](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-ui), you can see the status of each backup policy in the second column.
{: ui}

When you [view backup policy details from the CLI](/docs/vpc?topic=vpc-backup-view-policies&interface=cli#backup-view-details-cli) by running either `ibmcloud is backup-policies` or the `ibmcloud is backup-policy` command, the response contains the `Status` of the displayed backup policy.
{: cli}

When you make a `GET /backup_policies/` request, the API returns a `lifecycle_state` value as part of the information about the backup policy.
{: api}

Table 1 shows the lifecycle statuses that the backup policy can have.

| Status    | Description |
|-----------|-------------|
| `Stable`  | A policy and a plan are actively creating backups or are available for creating new backups. |
| `Pending` | A policy and a plan are being created. |
| `Deleting`| A policy and a plan are being deleted. |
| `Deleted` | A policy and a plan no longer exist. |
{: caption="Table 1. Backup policy statuses." caption-side="bottom"}

## Backup policy health states
{: #backup-policy-health-states}

When you [view your backup policy in the console](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-ui), you can see its **Health** status in the Policy details section.
{: ui}

When you [view the details of a policy from the CLI](/docs/vpc?topic=vpc-backup-view-policies&interface=cli#backup-view-details-cli) by running the `ibmcloud is backup-policy` command, the response contains the `Health State` of the specified backup policy.
{: cli}

When you make a `GET /backup_policies/{id}` request, the API returns a `health_state` value as part of the information about the backup policy.
{: api}

Table 2 shows all the health states that the backup policies can have.

| Health state | Meaning |
|--------------|---------|
|`ok`          | No abnormal behavior was detected. |
|`degraded`    | Experiencing compromised performance, capacity, or connectivity. |
|`faulted`     | Unreachable, inoperative, or otherwise entirely incapacitated. |
|`inapplicable`| The health state does not apply because of the current lifecycle state. A resource with a lifecycle state of `failed` or `deleting` also has a health state of `inapplicable`. A `pending` resource can also have this state.|
{: caption="Table 2. Backup policy health states." caption-side="bottom"}

## Reviewing backup jobs
{: #backup-jobs-manage}

Backup jobs create or delete backup snapshots based on the backup plan's frequency and retention settings. You can use the UI, CLI, API, or Terraform to check on the status of the backup jobs. You can see backup jobs that are running, completed, or failed when a backup is triggered. For more information, see [Viewing backup policy jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## Activity tracking events
{: #backup-activity-tracker}

When a backup is created, an event is triggered in the Activity Tracker for the [Backup service](/docs/vpc?topic=vpc-at_events&interface=ui#events-backup-service) and [Snapshots service](/docs/vpc?topic=vpc-at_events&interface=ui#events-snapshots). Similarly, when the service fails to create a backup due to missing authorization, an event is triggered to notify you. For more information, see [Activity tracking events for IBM Cloud VPC](/docs/vpc?topic=vpc-at_events).

## Event notifications
{: #backup-event-notifications}

[New]{: tag-new}[Backup Event Notification]{: tag-teal}

By setting up {{site.data.keyword.en_full}}, you can send notifications of events in IBM Cloud Backup for VPC to other users by using email, SMS, or other supported delivery channels. Additionally, you might want to send these notifications of events to other applications to build logic by using event-driven programming with webhooks, for example. For more information, see [Enabling event notifications for Backup for VPC](/docs/vpc?topic=vpc-event-notifications-events).
