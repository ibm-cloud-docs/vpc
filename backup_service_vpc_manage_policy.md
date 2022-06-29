---

copyright:
  years: 2022
lastupdated: "2022-06-15"

keywords:

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing backup policies
{: #backup-service-manage}

Manage backup policies and their associated plans created for your block storage volumes. Delete policies and plans that you no longer need. Update the policy and plan. Audit your policies by integrating Activity Tracker events. Check the status of your backup policies.
{: shortdesc}

## Backup policy deletion overview
{: #backup-delete}

When you delete a backup policy, you also delete a plan associated with it. You can also delete plans within a policy.

When you delete a backup policy, any backups that were being created based on tags defined in the policy will no longer be made.

## Manage backup policies with the UI
{: #backup-manage-policy-ui}
{: ui}

Delete a backup policy, rename a policy, edit user tags using the UI.

### Delete a backup policy with the UI
{: #backup-delete-policy}
{: ui}

1. Navigate to the [list of backup policies](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-list-all-policies).

2. Locate the policy that you want to delete.

3. From the Actions menu, select **Delete**.

   Confirm that the volumes associated with the backup policy will not be backed up, and that backups created by the policy continue to exist. You must go to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-delete-snapshot-ui) and delete each one manually.

4. Type _delete_ to confirm, the click **Delete policy**.

The policy is deleted. Existing backups will be retained until their expiration date. No further backups for the volumes will be made after you delete the plan.

### Delete a backup plan with the UI
{: #backup-delete-plan}

1. Navigate to the [list of backup policies](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-list-all-policies) and locate the backup policy.

2. On the backup policy details page, select the plan you want to delete. You can have up to four backup plans per policy. 

### Rename a backup policy with the UI
{: #backup-rename-policy-ui}

You can rename a policy from the list of backup policies or from the backup policies details page.

From the list of backup policies, click the overflow menu (elipisis) and select **Rename**.

From the policy details page, click the pencil icon next to the policy name.

### Edit tags for target resources with the UI
{: #backup-edit-tags}

From the Backup policy details page, you can edit the tags for your block storage volumes target resources.

1. Go to the [details page for a backup policy](https://test.cloud.ibm.com/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-policy).

2. For **Tags for target resources**, click the pencil icon.

3. In the popup window, enter a user tag name. For existing block storage volumes with user tags, enter the tag name exactly as it appears in the volume. You can also add a new user tag here and then go back an add it to a volume. You only need one tag for a volume to create a backup.

4. Click **Next**. 

5. Review the information on the next popup. If satisfied, check the disclaimer and click **Save changes**.

### Edit or delete a backup plan with the UI
{: #backup-edit-delete-plan-ui}

When you provision a backup policy and create a backup plan, you can edit the plan details or delete the plan by using the UI. Deleting the plan means you must have at least one remaining for the policy to create backups. 

1. From the [backup plan details](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-policy) page, expand the Actions menu for the plan.

2. Select **Edit** or **Delete**. When you edit, the plan details appear in the side panel. You can modify the same information you specified when creating the plan, such as the name and backup frequency.

3. Confirm your selections when finished.

### Reviewing backup jobs with the UI
{: #backup-jobs-manage}

You can see backup jobs that are running, completed, or failed when a backup is triggered. For more information, see [Viewing backup policy jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## Manage backup policies and plans with the CLI
{: #backup-manage-policy-cli}
{: cli}

Delete a backup policy, rename a policy, edit user tags from the CLI.

### Delete a backup policy with the CLI
{: #backup-delete-policy-cli}

Run the `backup-policy-delete` command and specify the policy ID or policy name for one or more backup policies. Backup plans associated with the policy are also deleted. For information about backup policy deletion, see [Backup policy deletion overview](#backup-delete).

```text
ibmcloud is backup-policy-delete (POLICY1 POLICY2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

This example using the interactive CLI deletes a backup policy by ID:

```text
ibmcloud is backup-policy-delete 7759199b-bc1f-448e-84fa-2aa42bde29af
This will delete backup policy 7759199b-bc1f-448e-84fa-2aa42bde29af and cannot be undone. Continue [y/N] ?> y
Deleting backup policy 7759199b-bc1f-448e-84fa-2aa42bde29af under account VPC1 as user myuser@mycompany.com...
OK
Deletion request for backup policy 7759199b-bc1f-448e-84fa-2aa42bde29af has been accepted.
```
{: screen}

### Rename a backup policy with the CLI
{: #backup-rename-policy-cli}

Run the `ibmcloud is backup-policy-update` command and specify the policy ID or policy name, and a new name for the backup policy. 

```text
ibmcloud is backup-policy-update POLICY [--match-tags MATCH_TAGS] [--name NEW_NAME] [--output JSON] [-q, --quiet]
```
{: pre}

This example renames a backup policy identified by ID:

```text
ibmcloud is backup-policy-update ac2a8be2-aa99-4571-baed-c3ec63a64ce7 --name policy-99
Updating backup policy ac2a8be2-aa99-4571-baed-c3ec63a64ce7 under account VPC1 as user myuser@mycompany.com...
                          
ID                     ac2a8be2-aa99-4571-baed-c3ec63a64ce7   
Name                   policy-99   
CRN                    crn:v1:staging:public:is:us-south:a/2d1bace7b46e4815a81e52c6ffeba5cf::backup-policy:ac2a8be2-aa99-4571-baed-c3ec63a64ce7   
Status                 stable   
Plans                  ID                                     Name         Resource type      
                       361ed7f8-ee19-4c74-86c1-e3aafcac8a0d   bkp-plan-1   backup_policy_plan      
                          
Backup tags            env:dev   
Backup resource type   volume   
Resource group         Default   
Created                2022-06-22T17:56:53+05:30 
```
This example updates a backup policy user tags identified by ID:

```text
ibmcloud is backup-policy-updatee7d5f916-6eff-490a-88d3-e20c029efab7 --match-tags dev:env
Updating backup policy e7d5f916-6eff-490a-88d3-e20c029efab7 under account VPC1 as user myuser@mycompany.com...
                          
ID                     e7d5f916-6eff-490a-88d3-e20c029efab7   
Name                   demo-bkp-policy-new   
CRN                    crn:v1:bluemix:public:is:us-south:a/1431ea2a7958ad20f0fee592ff85f746::backup-policy:r134-e7d5f916-6eff-490a-88e6-e20c029efab9   
Status                 stable   
Plans                  ID                                          Name              Resource type      
                       r134-71ec208e-902b-4153-96e6-f81c2928d2f5   demo-bkp-plan-2   backup_policy_plan      
                       r134-c525d9dc-6d80-49ed-b8d8-3ca3470fbdfd   new-plan1         backup_policy_plan      
                          
Backup tags            dev:env   
Backup resource type   volume   
Resource group         Default   
Created                2022-06-08T19:22:15+05:30   
```
{: screen}

### Update a backup plan with the CLI
{: #backup-update-plan-cli}

Run the `ibmcloud is backup-policy-plan-update` command to update a backup plan. Identify the policy and plan by ID or name in the command line. You can update the plan name, `cron-spec` backup schedule, attach user tags, and update backup retention period.

Syntax:

```zsh
ibmcloud is backup-policy-plan-update POLICY PLAN [--name NAME] [--active] [--attach-tags ATTACH_TAGS] [--copy-tags true | false] [--cron-spec CRON_SPEC] [[--delete-after DELETE_AFTER] [--delete-over-count DELETE_OVER_COUNT]] [--output JSON] [-q, --quiet]
```
{: pre}

This example specifies the backup policy and plan by name, specifies the following parameters to update the plan:

* `--cron-spec` updates the `cron-spec` backup schedule.

* `--name` updates the plan name.

* `--attach-tags` attaches user tags in the backup policy to each backup created by the plan. Updating this value doesn't change the user tags for backups already created by this plan.

* `--copy-tags` specifies whether source volume tags will be copied to the backup snapshot (`true` or `false`).

* `--delete-after` sets the number of days to keep a backup to 20 days.

* `--delete-over-count` sets the maximum number of recent backups to keep.

For example, 

```bash
ibmcloud is backup-policy-plan-update backup-policy-1001 mybackup-plan --cron-spec '42 10 * * *' --name mybackup-plan-1 --attach-tags my-daily-backup-plan --copy-tags false --delete-after 20 --delete-over-count 1
```
{: screen}

### Delete a backup plan with the CLI
{: #backup-delete-plan-cli}

Run the `ibmcloud is backup-policy-plan-delete` command and specify the backup policy and the plan or plans you want to delete. You can specify the backup policy and plans by ID or by name.

Syntax:

```zsh
ibmcloud is backup-policy-plan-delete POLICY (PLAN1 PLAN2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

This example deletes two backup plans for backup policy _backup-policy-1_.

```bash
ibmcloud is backup-policy-plan-delete backup-policy-1 my-plan-2 my-plan-3
```
{: screen}

## Manage backup policies and plans with the API
{: #backup-manage-policy-api}
{: api}

Delete a backup policy, rename a policy, edit user tags using the API.

### Delete a backup policy and plans with the API
{: #backup-delete-policy-api}

Make a `DELETE /backup_policies/{backup_policy_id}` request to delete a backup policy and all associated backup plans.
During the deletion, the status shows `deleting`. When the deletion is complete (status = `deleted`), the backup policy and backup plans cannot be recovered.

Example request:

```curl
curl -X DELETE\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d?version=2022-06-28&generation=2"\
  -H "Authorization: $iam_token"
```
{: codeblock}

The response indicates all backup plans are deleted.

### Delete a backup plan with the API
{: #backup-delete-plan-api}

Make a `DELETE /backup_policies/{backup_policy_id}/plans/{plan_id}` request to delete a specific backup plan from a backup policy. Resources created by the plan remain but will no longer be subject to the plan's deletion trigger. During the deletion, the status shows `deleting`. When the deletion is complete (status = `deleted`), the backup plan cannot be recovered.

Example request:

```curl
curl -X DELETE\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d/plans/4cf9171a-0043-4434-8727-15b53dbc374c?version=2022-06-28&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

The response shows the backup plan was deleted.

```text
{
  "active": true,
  "attach_user_tags": [
    "my-daily-backup-plan"
  ],
  "copy_user_tags": true,
  "created_at": "2022-06-22T01:44:49.070Z",
  "cron_spec": "*/5 1,2,3 * * *",
  "deletion_trigger": {
    "delete_after": 20
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/0fe9e5d8-0a4d-4818-96ec-e99708644a58/plans/4cf9171a-0043-4434-8727-15b53dbc374c",
  "id": "4cf9171a-0043-4434-8727-15b53dbc374c",
  "lifecycle_state": "deleted",
  "name": "my-policy-plan",
  "resource_type": "backup_policy_plan"
}
```
{: codeblock}

### Rename a backup policy with the API
{: #backup-rename-policy-api}

Make a `PATCH /backup_policies/{backup_policies_id}` request and specify a new name for a backup policy. 

For example:

```curl
curl -X PATCH\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d?version=2022-06-28&generation=2"\
  -H "Authorization: $iam_token"\
  -d '{
        "name": "my-backup-policy1"
     }'
```
{: codeblock}

### Update a backup plan with the API
{: #backup-update-plan-api}

Make a `PATCH /backup_policies/{backup_policy_id}/plans/{plan_id}` request to update the backup plan by specifying the following properties:

* `attach_user_tags` specifies the user tags from the backup policy that will be attached to backups created by the plan. Updating this value doesn't change the user tags for backups already created by this plan.

* `copy_user_tags` specifies whether source volume tags will be copied to the backup snapshot. The value is a boolean,`true` or `false`.

* `deletion_trigger` has two properties, `delete_after` sets the number of days to keep a backup and `delete_over_count` sets the maximum number of recent backups to keep, after which they are deleted. Specify `null` to remove the existing maximum.

```curl
curl -X PATCH\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d/plans/78bb42ae-b82e-4560-861b-5bc4bca3fba6?version=2022-06-28&generation=2"\
  -H "Authorization: $iam_token"\
  -d '{
      "active": true,
      "attach_user_tags": [
        "my-daily-backup-plan"
      ],
      "copy_user_tags": true,
      "cron_spec": "*/5 1,2,3 * * *",
      "deletion_trigger": {
         "delete_after": 20,
        "delete_over_count": 1
      },
      "name": "my-policy-plan"
    }'
```
{: codeblock} 

## Backup policy statuses
{: #backup-policy-statuses}

Table 1 defines the statuses for backup policies.

| Status | Description |
|--------|-------------|
| Stable | A policy and plan are actively creating backups or are available for creating new backups. |
| Pending | A policy and plan are being created. |
| Deleting | A policy and plan are being deleted. |
| Deleted | A policy and plan no longer exists. |
{: caption="Table 1. Backup policy statuses" caption-side="top"}

## IAM roles for creating and managing backups
{: #baas-vpc-iam}

Backups require IAM permissions for role-based access control. Table 1 describes these roles as they pertain to backup actions.

| Backup action | IAM role |
|-----------------|----------|
| Create backup policies | Administrator, editor |
| Add tags to a volume resource<sup>1</sup> | Adminstrator, editor |
| Delete backup policies | Administrator, editor |
| Restore a volume from a backup<sup>2</sup> | Administrator, editor, operator |
| List backups | Administrator, editor, operator, viewer |
| View backup policy details | Administrator, editor, operator, viewer |
{: caption="Table 1. IAM roles for snapshots" caption-side="top"}
<sup>1</sup>An administrator on the account must assign the right permissions for tagging resources. For more information, see [Granting users access to tag resources](/docs/account?topic=account-access).
<sup>2</sup>You must have administrator and editor privileges on the volume to perform this action.

## Activity Tracker events
{: #backup-activity-tracker}

When a backup is created, an event is triggered in Activity Tracker for the [Backup service](/docs/vpc?topic=vpc-at-events&interface=ui#events-backup-service) and [Snapshots service](/docs/vpc?topic=vpc-at-events&interface=ui#events-snapshots). For more information, see [Activity Tracker events](/docs/vpc?topic=vpc-at-events).

## Next steps
{: #backup-next-steps-manage}

* [Apply tags to your resources for backups](/docs/vpc?topic=vpc-backup-use-policies)
* [Create additional backup policies](/docs/vpc?topic=vpc-backup-policy-create&interface=ui)
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore)
