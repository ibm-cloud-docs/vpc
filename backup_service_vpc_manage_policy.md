---

copyright:
  years: 2022
lastupdated: "2022-02-10"

keywords: storage, backup, virtual private cloud

subcollection: vpc


---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:beta: .beta}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing backup policies (Beta)
{: #backup-service-manage}

Manage backup policies your created for your block storage volumes. Delete policies that you no longer need. Update the policy name. Audit your policies by integrating Activity Tracker events. Check the status of your backup policies.
{: shortdesc}

This service is available only to accounts with special approval to preview this beta feature. Contact your IBM Support if you are interested in getting access.
{: beta}

## Manage backup policies using the UI
{: #backup-manage-policy-ui}
{: ui}

Delete a backup policy, rename a policy, edit user tags using the ui.

### Backup policy deletion overview
{: #backup-delete}

When you delete a backup policy, you also delete a plan associated with it. Any backups that were being created based on tags defined in the policy will no longer be made. Existing backups are defined by their retention policy and exist until their expiration date. 

Be careful before deleting a backup policy. If you delete a policy to create a new plan, create the new policy with the same user tags and new plan first. Then, delete the old policy.
{: note}

### Delete a backup policy using the UI
{: #backup-delete-policy}
{: ui}

1. Navigate to the [list of backup policies](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-list-all-policies).

2. Locate the policy that you want to delete.

3. From the Actions menu, select **Delete**.

   Confirm that the volumes associated with the backup policy will not be backed up, and that backups created by the policy continue to exist. You must go to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-delete-snapshot-ui) and delete each one manually.

4. Type _delete_ to confirm, the click **Delete policy**.

The policy is deleted. Existing backups will be retained until their expiration date. No further backups for the volumes will be made after you delete the plan.

### Rename a backup policy using the UI
{: #backup-rename-policy-ui}

You can rename a policy from the list of backup policies or from the backup policies details page.

From the list of backup policies, click the overflow menu (elipisis) and select **Rename**.

From the policy details page, click the pencil icon next to the policy name.

### Edit tags for target resources
{: #backup-edit-tags}

From the Backup policy details page, you can edit the tags for your block storage volumes target resources.

1. Go to the [details page for a backup policy](https://test.cloud.ibm.com/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-policy).

2. For **Tags for target resources**, click the pencil icon.

3. In the popup window, enter the tag name. The name should be entered exactly as it appears in the block storage volume. Enter only one tag from the volume.

### Edit or delete a backup plan using the UI
{: #backup-edit-delete-plan-ui}

When you provision a backup policy and create a backup plan, you can edit the plan settings or delete the plan by using the UI. Deleting the plan means you need to create a new one for the policy. After the policy is created, you can't edit or delete the backup plan. For Beta, you can have one plan per policy.

## Manage backup policies from the CLI
{: #backup-manage-policy-cli}
{: cli}

Delete a backup policy, rename a policy, edit user tags from the CLI.

### Delete a backup policy from the CLI
{: #backup-delete-policy-cli}

Run the `backup-policy-delete` command and specify the policy ID or policy name for one or more backup policies. Backup plans associated with the policy are also deleted. For information about backup policy deletion, see [Backup policy deletion overview](#backup-delete).

```text
ibmcloud is backup-policy-delete (POLICY_ID1 POLICY_ID2 ...)|(POLICY_NAME1 POLICY_NAME2 ...) [--output JSON] [-f, --force] [-q, --quiet]
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

### Rename a backup policy using the CLI
{: #backup-rename-policy-cli}

Run the `ibmcloud is backup-policy-plan-update` command and specify the policy ID or policy name, and a new name for the backup policy. 

```text
ibmcloud is backup-policy-update POLICY_ID|POLICY_NAME [--name NEW_NAME] [--output JSON] [-q, --quiet]
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
Created                2021-11-19T17:56:53+05:30 
```
{: screen}

### Rename a backup plan from the CLI
{: #backup-rename-plan-cli}

For the Beta release, you cannot modify a backup plan after it's created.

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
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d?version=2021-08-31&generation=2"\
  -H "Authorization: $iam_token"
```
{: codeblock}

The response indicates all backup plans are deleted.

### Delete a backup plan
{: #backup-delete-plan-api}

Make a `DELETE /backup_policies/{backup_policy_id}/plans/{plan_id}` request to delete a specific backup plan from a backup policy. Resources created by the plan remain but will no longer be subject to the plan's deletion trigger. During the deletion, the status shows `deleting`. When the deletion is complete (status = `deleted`), the backup plan cannot be recovered.

Example request:

```curl
curl -X DELETE\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d/plans/4cf9171a-0043-4434-8727-15b53dbc374c?version=2021-08-31&generation=2"\
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
  "created_at": "2021-09-03T01:44:49.070Z",
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
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d?version=2021-11-10&generation=2"\
  -H "Authorization: $iam_token"\
  -d '{
     "name": "my-backup-policy1"
     }'
```
{: codeblock}

### Rename a backup plan with the API
{: #backup-rename-plan-api}

For the Beta release, you cannot modify a backup plan after it's created.
{: note}

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

When a backup is created, an event is triggered in Activity Tracker. For more information, see [Activity Tracker Storage resources](/docs/vpc?topic=vpc-at-events#events-storage]).

## Next steps
{: #backup-next-steps-manage}

* [Apply tags to your resources for backups](/docs/vpc?topic=vpc-backup-use-policies)
* [Create additional backup policies](/docs/vpc?topic=vpc-backup-policy-create&interface=ui)
* [Restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore)
