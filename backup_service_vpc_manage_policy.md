---

copyright:
  years: 2022, 2023
lastupdated: "2023-02-07"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing backup policies
{: #backup-service-manage}

Manage backup policies and their associated plans that were created for your {{site.data.keyword.block_storage_is_short}} volumes. Delete policies and plans that you no longer need. Update the policy and plan. Audit your policies by integrating Activity Tracker events. Check the status of your backup policies.
{: shortdesc}

## Backup policy deletion overview
{: #backup-delete}

When you delete a backup policy, you also delete the plan that is associated with it. You can also delete plans within a policy.

When you delete a backup policy, any backups that were scheduled to be created based on the tags that were defined in the policy are no longer to be taken.

## Manage backup policies in the UI
{: #backup-manage-policy-ui}
{: ui}

Delete a backup policy, rename a policy, and edit user tags in the UI.

### Delete a backup policy in the UI
{: #backup-delete-policy}
{: ui}

1. Go to the [list of backup policies](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-list-all-policies).

2. Locate the policy that you want to delete.

3. From the Actions menu, select **Delete**.

   Confirm that the volumes that are associated with the backup policy are no longer to be backed up, and that backups that were created by the policy continue to exist. You must go to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-delete-snapshot-ui) and delete each one manually.

4. Type _delete_ to confirm, the click **Delete policy**.

The policy is deleted. Existing backups are retained until their expiration date. No further backups for the volumes are made after you delete the plan.

### Delete a backup plan in the UI
{: #backup-delete-plan}

1. Go to the [list of backup policies](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-list-all-policies) and locate the backup policy.

2. On the backup policy details page, select the plan that you want to delete. You can have up to four backup plans per policy.

### Rename a backup policy in the UI
{: #backup-rename-policy-ui}

You can rename a policy from the list of backup policies or from the backup policies details page.

* From the list of backup policies, click the overflow menu (ellipsis) and select **Rename**.

* From the policy details page, click the pencil icon next to the policy name.

### Edit tags for target resources in the UI
{: #backup-edit-tags}

From the Backup policy details page, you can edit the tags for your {{site.data.keyword.block_storage_is_short}} volumes target resources.

1. Go to the [details page for a backup policy](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-policy).

2. For **Tags for target resources**, click the pencil icon.

3. In the new window, enter a user tag name. For existing {{site.data.keyword.block_storage_is_short}} volumes with user tags, enter the tag name exactly as it appears in the volume. You can also add a brand new user tag here and then go back and add it to a volume. You need only one tag for a volume to create a backup.

4. Click **Next**.

5. Review the information on the next window. If you're satisfied, check the disclaimer and click **Save changes**.

### Edit or delete a backup plan in the UI
{: #backup-edit-delete-plan-ui}

After you provisioned a backup policy and created a backup plan, you can edit the plan details or delete the plan. You must have at least one remaining plan for the policy to create backups.

1. From the [backup plan details](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-policy) page, expand the Actions menu for the plan.

2. Select **Edit** or **Delete**. When you edit, the plan details appear in the side panel. You can modify the same information that you specified when you created the plan, such as the name and backup frequency. You can enable or disable fast restore, or change which regions fast restore is available in.

3. Confirm your selections when you're finished.

### Review backup jobs in the UI
{: #backup-jobs-manage}

You can see backup jobs that are running, completed, or failed when a backup is triggered. For more information, see [Viewing backup policy jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## Manage backup policies and plan from the CLI
{: #backup-manage-policy-cli}
{: cli}

Delete a backup policy, rename a policy, and edit user tags from the CLI.

### Delete a backup policy from the CLI
{: #backup-delete-policy-cli}

Run the `backup-policy-delete` command and specify the policy ID or policy name for one or more backup policies. Backup plans that are associated with the policy are also deleted. For more information about backup policy deletion, see [Backup policy deletion overview](#backup-delete).

```sh
ibmcloud is backup-policy-delete (POLICY1 POLICY2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

The following example uses the interactive CLI to delete a backup policy by ID:

```sh
ibmcloud is backup-policy-delete 7759199b-bc1f-448e-84fa-2aa42bde29af
This will delete backup policy 7759199b-bc1f-448e-84fa-2aa42bde29af and cannot be undone. Continue [y/N] ?> y
Deleting backup policy 7759199b-bc1f-448e-84fa-2aa42bde29af under account VPC1 as user myuser@mycompany.com...
OK
Deletion request for backup policy 7759199b-bc1f-448e-84fa-2aa42bde29af has been accepted.
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-delete`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-delete).

### Rename a backup policy from the CLI
{: #backup-rename-policy-cli}

Run the `ibmcloud is backup-policy-update` command and specify the policy ID or policy name, and a new name for the backup policy.

```sh
ibmcloud is backup-policy-update POLICY [--match-tags MATCH_TAGS] [--name NEW_NAME] [--output JSON] [-q, --quiet]
```
{: pre}

The following example renames a backup policy that is identified by ID:

```sh
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
{: screen}

The following example updates backup policy user tags that are identified by ID.

```sh
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

For more information about available command options, see [`ibmcloud is backup-policy-update`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-update).

### Update a backup plan from the CLI
{: #backup-update-plan-cli}

Run the `ibmcloud is backup-policy-plan-update` command to update a backup plan. Identify the policy and plan by ID or name in the command line. You can update the plan name, `cron-spec` backup schedule, attach user tags, update backup retention period, add, or remove fast restore zones.

The following example shows the command syntax.

```zsh
ibmcloud is backup-policy-plan-update POLICY PLAN --cron-spec CRON_SPEC [--name NAME] [--active] [--attach-tags ATTACH_TAGS] [--copy-tags true | false] [[--delete-after DELETE_AFTER] [--delete-over-count DELETE_OVER_COUNT]] [[--clone-policy-zones ZONES [--clone-policy-max-snapshots MAX_SNAPSHOTS]]] [--output JSON] [-q, --quiet]
```
{: pre}

The following example specifies the backup policy and plan by name, and changes the name of the plan from `my-plan-2` to `my-plan-1`.

```sh
ibmcloud is backup-policy-plan-update my-backup-policy-2 my-plan-2 --name my-plan-1
Updating plan my-plan-2 of the backup policy r134-88ed58e0-d5ee-4b97-abe0-a26c07585b35 under account IBM Cloud Account as user IBM Cloud User...

ID                   r134-f488a7cd-e7f3-4f74-97ea-334ea0e16d1c
Name                 my-plan-1
Active               true
Lifecycle state      updating
Deletion trigger     Delete after   Delete over count
                     60             2

Attached tags        dev:test
Copy tags            false
Cron specification   45 09 * * *
Created at           2022-11-18T16:18:38-06:00
Resource type        backup_policy_plan
```
{: screen}

The following example specifies the backup policy and plan by their IDs and activates fast restore backup snapshots in two zones in the `us-south` region. The update also specifies that only one snapshot clone is to be stored in the availability zone after the backup is created and stored in {{site.data.keyword.cos_short}} regionally.

```sh
ibmcloud is backup-policy-plan-update r134-48b18c4b-f079-4576-802e-fe0b4186ae0b r134-e3abfc9c-327e-42a9-87d7-3cb793684c71  --clone-policy-max-snapshots 1 --clone-policy-zones us-south-2,us-south-1  --active
Updating plan r134-e3abfc9c-327e-42a9-87d7-3cb793684c71 of the backup policy r134-48b18c4b-f079-4576-802e-fe0b4186ae0b under account VPC as user myuser@mycompany.com...
                        
ID                   r134-e3abfc9c-327e-42a9-87d7-3cb793684c71   
Name                 my-policy-plan-1   
Active               true   
Lifecycle state      updating   
Clone policy         Max snapshots   Zones      
                     1               us-south-2,us-south-1      
                        
Deletion trigger     Delete after   Delete over count      
                     20             1      
                        
Attached tags        my-daily-backup-plan   
Copy tags            true   
Cron specification   20 19 * * *   
Created at           2022-11-18T00:40:51+05:30   
Resource type        backup_policy_plan    

```

For more information about available command options, see [`ibmcloud is backup-policy-plan-update`](/docs/cli?topic=cli-vpc-reference#backup-policy-plan-update).

### Delete a backup plan from the CLI
{: #backup-delete-plan-cli}

Run the `ibmcloud is backup-policy-plan-delete` command and specify the backup policy and the plan or plans that you want to delete. You can specify the backup policy and plans by ID or by name.

The following example shows the command syntax.

```zsh
ibmcloud is backup-policy-plan-delete POLICY (PLAN1 PLAN2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

The following example deletes two backup plans for backup policy _backup-policy-1_.

```bash
ibmcloud is backup-policy-plan-delete backup-policy-1 my-plan-2 my-plan-3
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-plan-delete`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-plan-delete).

## Manage backup policies and plans with the API
{: #backup-manage-policy-api}
{: api}

You can delete a backup policy, rename a policy, and edit user tags with the API.

### Delete a backup policy and plans with the API
{: #backup-delete-policy-api}

Make a `DELETE /backup_policies/{backup_policy_id}` request to delete a backup policy and all associated backup plans. During the deletion, the status shows `deleting`. When the deletion is complete (status = `deleted`), the backup policy and backup plans cannot be recovered.

See the following example.

```curl
curl -X DELETE\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d?version=2022-06-28&generation=2"\
  -H "Authorization: $iam_token"
```
{: codeblock}

The response indicates that all backup plans are deleted.

### Delete a backup plan with the API
{: #backup-delete-plan-api}

Make a `DELETE /backup_policies/{backup_policy_id}/plans/{plan_id}` request to delete a specific backup plan from a backup policy. Resources that were created by the plan remain but they are no longer subject to the plan's deletion trigger. During the deletion, the status shows `deleting`. When the deletion is complete (status = `deleted`), the backup plan cannot be recovered.

See the following example.

```curl
curl -X DELETE\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d/plans/4cf9171a-0043-4434-8727-15b53dbc374c?version=2022-06-28&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

The response shows that the backup plan was deleted.

```json
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
{: screen}

### Rename a backup policy with the API
{: #backup-rename-policy-api}

Make a `PATCH /backup_policies/{backup_policies_id}` request and specify a new name for a backup policy. The following example shows how to rename policy `5063bfe5-c16f-4606-ba26-fba0f099b97d` to `my-backup-policy1`.

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

* `attach_user_tags` specifies the user tags from the backup policy that are to be attached to backups created by the plan. Updating this value doesn't change the user tags for backups that were already created by this plan.

* `copy_user_tags` specifies whether source volume tags are to be copied to the backup snapshot. The value is a Boolean, it's either `true` or `false`.

* `deletion_trigger` has two properties. The `delete_after` property sets the number of days to keep a backup. The `delete_over_count` property sets the maximum number of recent backups to keep. When the maximum number is reached, the oldest backup is deleted to make space for a new one. Specify `null` to remove the existing maximum.

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

### Update a backup policy plan for the fast restore option with the API
{: #baas-update-plan-fast-restore-api}

Make a `PATCH /backup_policies/{backup_policy_id}/plans/{plan_id}` request to update a backup plan. Specify the `clone_policy` property and `zones` subproperty to indicate which zone in your region you want to create the backup snapshot clone. The zone must be different than your source zone. You can specify multiple zones. The plan includes keeping a maximum of two cached backup snapshots in each zone.

This example updates a backup policy to create a backup plan. The new plan defines a clone policy to create snapshot clones in zone _us-south-3_. The plan specifies keeping a maximum of three cached backup snapshots.

```json
curl -X POST "$vpc_api_endpoint/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans?version=2022-12-09&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
        "active": true,
        "attach_user_tags": [
          "daily-backups"
        ],
        "clone_policy": {
          "max_snapshots": 3,
          "zones": [
            {
              "name": "us-south-3"
            },
            {
              "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3"
            }
          ]
        },
        "copy_user_tags": true,
        "cron_spec": "30 */2 * * 1-5",
        "deletion_trigger": {
          "delete_after": 20,
          "delete_over_count": 20
        },
        "name": "my-daily-plan-1"
      }'
```
{: codeblock}

A successful response shows that the clone policy is created.

```json
{
  "active": true,
  "attach_user_tags": [
    "daily-backups"
  ],
  "clone_policy": {
    "max_snapshots": 3,
    "zones": [
      {
        "name": "us-south-2"
      },
      {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3"
      }
    ]
  },
  "copy_user_tags": false,
  "created_at": "2022-12-09T15:16:37Z",
  "cron_spec": "30 */2 * * 1-5",
  "deletion_trigger": {
    "delete_after": 5
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans/6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "id": "6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "lifecycle_state": "stable",
  "name": "my-daily-plan-1",
  "resource_type": "backup_policy_plan"
}
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
| Deleted | A policy and plan no longer exist. |
{: caption="Table 1. Backup policy statuses." caption-side="bottom"}

## IAM roles for creating and managing backups
{: #baas-vpc-iam}

Backups require IAM permissions for role-based access control. Table 2 describes these roles as they pertain to the backup actions.

| Backup action | IAM role |
|-----------------|----------|
| Create backup policies. | Administrator, editor |
| Add tags to a volume resource [^addtags]. | Administrator, editor |
| Delete backup policies. | Administrator, editor |
| Restore a volume from a backup [^restore]. | Administrator, editor, operator |
| List backups. | Administrator, editor, operator, viewer |
| View backup policy details. | Administrator, editor, operator, viewer |
{: caption="Table 2. IAM roles for snapshots" caption-side="bottom"}

[^addtags]: An administrator on the account must assign the right permissions for tagging resources. For more information, see [Granting users access to tag resources](/docs/account?topic=account-access).

[^restore]: You must have administrator and editor privileges on the volume to perform this action.

## Activity Tracker events
{: #backup-activity-tracker}

When a backup is created, an event is triggered in Activity Tracker for the [Backup service](/docs/vpc?topic=vpc-at-events&interface=ui#events-backup-service) and [Snapshots service](/docs/vpc?topic=vpc-at-events&interface=ui#events-snapshots). For more information, see [Activity Tracker events](/docs/vpc?topic=vpc-at-events).

## Next steps
{: #backup-next-steps-manage}

* [Apply tags to your resources for backups.](/docs/vpc?topic=vpc-backup-use-policies)
* [Create more backup policies.](/docs/vpc?topic=vpc-backup-policy-create&interface=ui)
* [Restore a volume from a backup snapshot.](/docs/vpc?topic=vpc-baas-vpc-restore)
