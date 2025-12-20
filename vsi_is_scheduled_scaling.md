---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-20"

keywords: scheduled scaling, instance scaling, virtual server instance scaling, scheduled actions, scaling actions

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Scheduled scaling
{: #scheduled-scaling-vpc}

Use scheduled scaling for VPC to schedule actions that automatically add or remove instance group capacity, based on daily, intermittent, or seasonal demand. You can create multiple scheduled actions that scale capacity monthly, weekly, daily, hourly, or even every set number of minutes.
{: shortdesc}

Scheduled scaling offers the following benefits:

* Available in all MZRs
* Supports image templates
* Set one-time or recurring scheduled actions
* Set scheduled actions for static or dynamic instance groups

## Example scenario for creating a scheduled action
{: #scheduled-action-scenario}

As an example, imagine that the fictitious company, Acme Web Retailer, experiences higher than normal levels of website traffic during Cyber Monday. To compensate for the increased traffic, they create a recurring scheduled action to scale up instance group capacity to 10 between the hours of 8:00 AM and 11:00 PM on Cyber Monday.

## Creating a scheduled action with the UI
{: #set-up-scheduled-scaling-with-UI}
{: ui}

A scheduled action that targets `membership_count` is incompatible with an instance group that uses auto scale because when enabled, the auto scale manager controls `membership_count`.
{: important}

For more information about auto scale, see [Auto scale for VPC](/docs/vpc?topic=vpc-creating-auto-scale-instance-group#auto-scale-vpc)

### Creating a one-time scheduled action
{: #set-up-one-time-scheduled-scaling}

To create a one-time scheduled action, use the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Instance groups**.
1. Select the instance group that you want to create a scheduled action for by clicking its name.
1. Click **Scheduled actions**.
1. In the new screen, click **Create**
1. Enter an appropriate name for the scheduled action.
1. Select **One time**, then select the start date and time for your scheduled action.
1. If your instance group is _dynamic_, enter a minimum and or maximum instance group size to apply when this action runs. If your instance group is _static_, enter the instance group size to apply when this action runs.
1. Click **Create**.

### Creating a recurring scheduled action
{: #set-up-recurring-scheduled-scaling}

To create a recurring scheduled action, use the following steps.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **Navigation menu** ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../../icons/vpc.svg) **> Compute > Instance groups**.
1. Select the instance group that you want to create a scheduled action for by clicking its name.
1. Click **Scheduled actions**.
1. In the new screen, click **Create**
1. Enter an appropriate name for the scheduled action.
1. Click **Recurring**.
1. Select a date and time to run the scheduled action by using the predefined options for each pattern section (Monthly, Days, Hours, Minutes). Or choose a custom schedule by clicking what months, days, hours, and minutes that you want the schedule to run. \n Alternatively, you can select a date and time schedule by entering a cron expression. Click **Cron** and enter an appropriate cron expression.
8. If your instance group is _dynamic_, enter a minimum and or maximum instance group size to apply when this action runs. If your instance group is _static_, enter the instance group size to apply when this action runs.
9. Click **Create**.

24 hours after a scheduled action reaches a final status, such as _Completed_, _Omitted_, or _Failed_, the action is marked for deletion.
{: note}

## Creating a scheduled action with the CLI
{: #set-up-scheduled-scaling-with-CLI}
{: cli}

You can create a scheduled scaling action for your auto-scaled instances by using the {{site.data.keyword.cloud_notm}} CLI.

### Before you begin
{: #before-scheduled-scaling-CLI}

Make sure that you set up your {{site.data.keyword.cloud}} CLI environment and your {{site.data.keyword.vpc_short}}.
{: important}

To create a scheduled action by using the CLI, you must complete the following steps:

1. Make sure that you have the required IBM {{site.data.keyword.iamshort}} (IAM) permissions to create instance group resources. For more information, see * [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started&interface=ui).
2. Create scaling policies to dynamically add or remove instances from your group based on the target utilization metrics that you define.

### Create a scheduled action
{: #create-scheduled-action-cli}

Use the following command to create a scheduled action.

```sh
ibmcloud is instance-group-manager-action-create INSTANCE_GROUP MANAGER [--run-at RUN_AT | --cron CRON] [--membership-count MEMBERSHIP_COUNT | (--max-members MAX_MEMBERS --min-members MIN_MEMBERS)] [--name NAME] [--output JSON] [-q, --quiet]
```
{: pre}

Where:
- `INSTANCE_GROUP` is the ID of the instance group.
- `MANAGER`is the ID of the manager.
- `--run-at` is the date and time that is specified for the scheduled action. The format is in ISO 8601 format. Example: `2024-03-05T15:31:50.701Z` or `2024-03-05T15:31:50.701+8:00`.
- `--cron` is the cron specification for a recurring scheduled action.
- `--membership-count` is the number of members in the instance group at the scheduled time.
- `--max-members` is the maximum number of members in a managed instance group. Range 1-100.
- `--min-members`is the minimum number of members in a managed instance group. Range 1-100.
- `--name` the name of the action.
- `--output`: Specify output format, only JSON is supported. One of: **JSON**.
- `-q, --quiet`: Suppress verbose output.





### Update a scheduled action
{: #update-scheduled-action-cli}

Use the following command to update a scheduled action.

```sh
ibmcloud is instance-group-manager-action-update INSTANCE_GROUP MANAGER ACTION (--run-at RUN_AT | --cron CRON) [--membership-count MEMBERSHIP_COUNT | (--min-members MIN_MEMBERS --max-members MAX_MEMBERS)] [--name NEW_NAME] [--output JSON] [-q, --quiet]
```
{: pre}

Where:

- `INSTANCE_GROUP`: is the ID of the instance group.
- `MANAGER` is the ID of the manager.
- `ACTION`is the ID of the action.
- `--run-at`is the date and time that is specified for the scheduled action. The format is in ISO 8601 format. Example: `2024-03-05T15:31:50.701Z` or `2024-03-05T15:31:50.701+8:00`.
- `--cron` is the cron specification for a recurring scheduled action.
- `--membership-count` is the number of members in the instance group that you want at the scheduled time.
- `--min-members` is the minimum number of members in the instance group that you want at the scheduled time. Range 1-100.
- `--max-members`: The maximum number of members in the instance group that you want at the scheduled time. Range 1-100.
- `--name` is the new name of the instance group manager action.
- `--output` specifies output format. Only JSON is supported. One of: JSON.
- `-q, --quiet`suppresses verbose output.





### Delete a scheduled action
{: #delete-scheduled-action-cli}

Use the following command to delete a scheduled action.

```sh
ibmcloud is instance-group-manager-action-delete INSTANCE_GROUP MANAGER ACTION [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

Where:

- `INSTANCE_GROUP`: is the ID of the instance group.
- `MANAGER` is the ID of the manager.
- `ACTION`is the ID of the action.
- `--output` specifies output format. Only JSON is supported. One of: JSON.
- `--force, -f` forces the operation without confirmation.
- `-q, --quiet`suppresses verbose output.





## Creating scheduled actions with the API
{: #creating-scheduled-action-api}
{: api}

You can create a scheduled scaling action for your auto-scaled instances by calling the VPC API.

### Create a scheduled action
{: #create-scheduled-action-api}

The following example creates a scheduled action

   ```curl
   curl -X POST "$vpc_api_endpoint/v1/instance_groups/$instance_group_id/managers?   version=2021-04-20&generation=2" -H "Authorization: $iam_token" -d '{
         "manager_type": "autoscale",
         "max_membership_count": 50
       }'
   ```
   {: codeblock}

A successful response looks like the following example:

   ```json
   {
     "aggregation_window": 90,
     "cooldown": 300,
     "href": "https://eu-gb.iaas.cloud.ibm.com/v1/instance_groups/   r018-7b3ac170-01f3-43d6-87ec-f0ed11ed3f60/managers/r018-4f2f7036-86b0-4d1b-a729-12357d45b00f",
     "id": "r018-4f2f7036-86b0-4d1b-a729-12357d45b00f",
     "management_enabled": true,
     "manager_type": "autoscale",
     "max_membership_count": 50,
     "min_membership_count": 1,
     "name": "my-manager",
     "policies": []
   } created a scheduled action
   ```
   {: codeblock}

### Update a scheduled action
{: #update-scheduled-action-api}

The following example updates a scheduled action.

   ```curl
   curl -X PATCH "$vpc_api_endpoint/v1/instance_groups/$instance_group_id/managers/   $instance_group_manager_id/policies?version=2021-04-20&generation=2" -H "Authorization:    $iam_token" -d '{
         "metric_type": "cpu",
         "metric_value": 50
       }'
   ```
   {: codeblock}

A successful response looks like the following example:

   ```json
   {
     "href": "https://eu-gb.iaas.cloud.ibm.com/v1/instance_groups/   r018-7b3ac170-01f3-43d6-87ec-f0ed11ed3f60/managers/r018-4f2f7036-86b0-4d1b-a729-12357d45b00f/   policies/r018-02d7b6c3-e3c8-4569-ba6a-caa5d4d6146c",
     "id": "r018-02d7b6c3-e3c8-4569-ba6a-caa5d4d6146c",
     "metric_type": "cpu",
     "metric_value": 50,
     "name": "my-policy",
     "policy_type": "target"
   ```
   {: codeblock}

### Delete a scheduled action
{: #delete-scheduled-action-api}

The following example deletes a scheduled action.

   ```bash

   curl -X DELETE "$vpc_api_endpoint/v1/instance_groups/$instance_group_id/memberships?   version=2021-04-20&generation=2" -H "Authorization: $iam_token"
   ```
   {: codeblock}

No sample response available.
