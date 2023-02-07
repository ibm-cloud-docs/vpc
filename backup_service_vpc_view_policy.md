---

copyright:
  years: 2022, 2023
lastupdated: "2023-02-07"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing backup policies
{: #backup-view-policies}

You can list and view all backup policies that you created for your {{site.data.keyword.block_storage_is_short}} volumes in the console, from the CLI, or with the API. You can review the details of individual policies.
{: shortdesc}

## View backup policies in the UI
{: #backup-view-ui}
{: ui}

List all backup policies and view details of a specific policy.

### List all backup policies in the UI
{: #backup-list-all-policies}

List all backup policies that you created for volumes in your account for the selected region.

In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Backup policies**. By default, the Overview tab is selected.

Table 1 describes the information on the Backup policy list page. The default region for the account is selected. You can select a different region from the menu. Policies are listed on the page from newest to oldest.

| Field | Value |
|-------|-------|
| Name | Click the name of a policy to see its details. |
| Status | The status of the policy, such as _Stable_. For more information about policy statuses, see [Backup policy statuses](/docs/vpc?topic=vpc-backup-service-manage#backup-policy-statuses). |
| Applied resources | Number of {{site.data.keyword.block_storage_is_short}} volumes that are backed up by the policy. The number of volumes is a link that takes you to a [list of volumes](#backup-view-vol-backup-policies) that apply for the policy. |
| Tags for target resources | Tags for target {{site.data.keyword.block_storage_is_short}} volumes that you are backing up. |
| Last run time (local) | The most recent time a job ran for the backup policy. If the field is blank, the volumes don't have matching tags for a job to run. |
| Created date (local) | Date and time of when the backup was created. |
| Actions menu | Click the Actions icon (...) for available actions, such as restore and delete. |
{: caption="Table 1. Backup policy list view" caption-side="bottom"}

### View details of a backup policy with the UI
{: #backup-view-policy}

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Backup policies**. By default, the Overview tab is selected.

2. Click a policy name. Table 2 describes the information about the selected backup policy.

| Field | Value |
|-------|-------|
| Actions | Actions on this policy: Add auditing and Delete. |
| **Policy details** | |
| - Name | Name of the backup policy. Click the pencil icon to edit. |
| - Resource group | [Resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups) for the {{site.data.keyword.block_storage_is_short}} volume. |
| - Location | Policies for the selected region. |
| - Created date | Date the policy was created. |
| - CRN | Cloud resource name of the policy. |
| - Policy ID | Backup policy ID. |
| - Tags for target resources | - User tags that can trigger the creation of a backup when they are applied to any {{site.data.keyword.block_storage_is_short}} volumes. \n - Click the pencil icon to add more tags. For more information, see [Edit tags for target resources](/docs/vpc?topic=vpc-backup-service-manage&interface=ui#backup-edit-tags). \n - See also [Applying backup policies to resources by using tags](/docs/vpc?topic=vpc-backup-use-policies). |
| **Plan** | Show the plan name, retention policy, and plan status. Optionally, click **Create** to add more plans for an existing policy. |
| - Plan name | Unique name for the plan. Expand to see plan details, such as frequency, tags that are associated with this plan, and whether fast restore is enabled. |
| - Retention | Period that you set for the plan to be in effect, for example, 30 days. |
| - Status | Plan status. Shows `enabled` for an active plan. |
|-  Actions menu | Edit or delete a plan. You can change the name and other plan details such as retention, tags, and fast restore. For more information, see [Edit or delete a backup plan in the UI](/docs/vpc?topic=vpc-backup-service-manage&interface=ui). |
{: caption="Table 2. Backup policy details" caption-side="bottom"}

### View a list of volumes that have a backup policy
{: #backup-view-vol-backup-policies}

View a list of {{site.data.keyword.block_storage_is_short}} volumes that are backed up by the policy. For a volume to be backed up by a policy, the volume must be tagged with at least one of the policy’s tags for the target resources.

Look at which {{site.data.keyword.block_storage_is_short}} volumes have policies and verify that the policies are correctly applied.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Backup policies**.

2. Click a policy name.

3. Click the **Applied resources** tab.

A list of {{site.data.keyword.block_storage_is_short}} volumes that are backed up by this policy is shown. Information about the volumes includes the volume name, status, volume size, and encryption type (see Table 3).

| Field | Description |
|-------|-------------|
| Name | Name of the volume. Click the pencil icon to edit. |
| Status | [Status of the volume](/docs/vpc?topic=vpc-managing-block-storage#status). |
| Size | Size of the volume in GBs.|
| Encryption | [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption). |
| Add volume | Add a volume to this policy. The informational side panel provides a list of tags for target resources that you can apply to the volume, and a link to the [list of {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui). You must apply at least one of the policy's tags for target resources to the volume. |
{: caption="Table 3. List of {{site.data.keyword.block_storage_is_short}} volumes for the backup policy" caption-side="top"}

You can also view a more detailed list of all {{site.data.keyword.block_storage_is_short}} volumes in your account region, including user tags that are applied to the volume. For more information, see [View information about all {{site.data.keyword.block_storage_is_short}} volumes in the UI](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

When you click a volume name to see the details, you need administrator privileges to access the information.
{: requirement}


## View backup policies from the CLI
{: #backup-view-cli}
{: cli}

### List all backup policies from the CLI
{: #backup-view-all-cli}

Run the `backup-policies` command to list all backup policies that you created in your account and region.

```sh
ibmcloud is backup-policies [--tag TAG_NAME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups] [--output JSON] [-q, --quiet]
```
{: pre}

The following example shows the output that you can expect.

```sh
$ ibmcloud is backup-policies

Listing backup policies in all resource groups and region us-south under account vpcdemo as user myuser.mycompany.com...
ID                                     Name                          Status    Resource group
d8a0e8d9-c592-4175-80bc-3056f6fd1da5   demopolicy20                  pending   Default
688816c5-788d-4c2c-ba95-b9c588195dfb   demopolicy21                  stable    Default
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policies`](/docs/vpc?topic=vpc-vpc-reference#backup-policies).

### List all backup policies filter by user tags from the CLI
{: #backup-view-all-filter-by-tags-cli}

Run the `backup-policies` command to list all backup policies that you created in your account and region. The `--tag` parameter filters the collection of backup policies with the exact user tag value.

```sh
ibmcloud is backup-policies --tag
```
{: pre}

The following example produces a list of backup policies that have the `dev:test` tag.

```text
$ ibmcloud is backup-policies --tag dev:test
Listing backup policies in all resource groups and region us-south under account vpcdemo as user myuser.mycompany.com...
ID                                          Name                 Status   Resource group
16a6e20b-a99b-4275-9662-832bc6af23f6        demo-bkp-policy-x    stable   Default
ab94137e-7794-454e-9b4b-6274082172d9        demo-bkp-policy-x2   stable   Default
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policies`](/docs/vpc?topic=vpc-vpc-reference#backup-policies).

### View backup policy details from the CLI
{: #backup-view-details-cli}

Run the `backup-policy` command and specify either the backup policy ID or the policy name. 

```text
ibmcloud is backup-policy POLICY [--output JSON] [-q, --quiet]
```
{: pre}

The following example uses the policy ID.

```sh
ibmcloud is backup-policy ac2a8be2-aa99-4571-baed-c3ec63a64ce7
Getting backup policy ac2a8be2-aa99-4571-baed-c3ec63a64ce7 under account VPC as user myuser@mycompany.com...

ID                     ac2a8be2-aa99-4571-baed-c3ec63a64ce7
Name                   bkp-policy-1
CRN                    crn:v1:staging:public:is:us-south:a/2d1bace7b46e4815a81e52c6ffeba5cf::backup-policy:r134-ac2a8be2-aa99-4571-baed-c3ec63a64ce7
Status                 stable
Plans                  ID                                     Name              Resource type
                       361ed7f8-ee19-4c74-86c1-e3aafcac8a0d   bkp-plan-1        backup_policy_plan

Backup tags            env:dev
Backup resource type   volume
Resource group         Default
Created                2022-06-28T17:56:53+05:30
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy`](/docs/vpc?topic=vpc-vpc-reference#backup-policy).

### List all plans for a backup policy from the CLI
{: #backup-view-plans-cli}

Run the `ibmcloud is backup-policy-plans` and specify either the policy ID or the policy name to see all plans created for this policy. 

```sh
ibmcloud is backup-policy-plans POLICY [--output JSON] [-q, --quiet]
```
{: pre}

The following example specifies the policy ID. The backup plan uses a CRON expression to schedule backups.

```sh
ibmcloud is backup-policy-plans ac2a8be2-aa99-4571-baed-c3ec63a64ce7
Listing plans of backup policy ac2a8be2-aa99-4571-baed-c3ec63a64ce7 under account VPC as user myuser@mycompany.com...
ID                                          Name               Active   Lifecycle state   Cron specification
2dc6ecf9-8f09-4c34-86c6-c10ea6526563        my-policy-plan-1   true     stable            42 10 * * *
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-plans`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-plans).

### View backup plan details from the CLI
{: #backup-view-plan-details-cli}

Run the `ibmcloud is backup-policy-plan` command and specify the policy ID or policy name, and plan ID or plan name. 

```sh
ibmcloud is backup-policy-plan POLICY PLAN [--output JSON] [-q, --quiet]
```
{: pre}

The following example specifies the policy ID and the plan ID.

```sh
ibmcloud is backup-policy-plan r134-48b18c4b-f079-4576-802e-fe0b4186ae0b r134-b19c5178-3b58-4aca-8a17-95607585dcff
Getting plan r134-b19c5178-3b58-4aca-8a17-95607585dcff under account VPC as user myuser@mycompany.com...
                        
ID                   r134-b19c5178-3b58-4aca-8a17-95607585dcff   
Name                 demo-bkp-plan-2   
Active               true   
Lifecycle state      stable   
Clone policy         Max snapshots   Zones      
                     4               us-south-1,us-south-2      
                        
Deletion trigger     Delete after   Delete over count      
                     60             2      
                        
Attached tags        dev:test   
Copy tags            false   
Cron specification   40 17 * * *   
Created at           2022-11-17T23:05:18+05:30   
Resource type        backup_policy_plan
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-plan`](/docs/cli?topic=cli-vpc-reference#backup-policy-plan).

## View backup policies and plans with the API
{: #backup-view-api}
{: api}

### Show details of a backup policy with the API
{: #backup-view-policy-details-api}

Make a `GET /backup_policies/{backup_policy_id}` request to show details of a backup policy that is identified by ID.

```curl
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f?version=2022-06-03&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "created_at": "2022-06-06T23:29:55.833Z",
  "crn": "crn:v1:bluemix:public:is:us-south:a/123456::backup-policy:fb721535-2cc6-45d6-ade7-3ceb95b7f26f",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f",
  "id": "fb721535-2cc6-45d6-ade7-3ceb95b7f26f",
  "lifecycle_state": "stable",
  "match_resource_types": [
    [
      "volume"
    ]
  ],
  "match_user_tags": [
    "my-daily-backup-policy"
  ],
  "name": "my-backup-policy",
  "plans": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f/plans/4cf9171a-0043-4434-8727-15b53dbc374c",
      "id": "4cf9171a-0043-4434-8727-15b53dbc374c",
      "name": "my-policy-plan",
      "resource_type": "backup_policy_plan"
    }
  ],
  "resource_group": {
    "crn": "crn:v1:bluemix:public:resource-controller::a/123456::resource-group:20b0c459-3cc6-411b-ba4b-2daa89c8c1b7",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/20b0c459-3cc6-411b-ba4b-2daa89c8c1b7",
    "id": "20b0c459-3cc6-411b-ba4b-2daa89c8c1b7",
    "name": "my-resource-group"
  },
  "resource_type": "backup_policy"
}
```
{: codeblock}

### List all plans for a backup policy with the API
{: #backup-view-plans-api}

Make a `GET /backup_policies/{backup_policy_id}/plans` request to list the plans that are associated with a backup policy. You can have up to four plans per policy. See the following example.

```curl
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f/plans?version=2022-12-16&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "plans": [
    {
      "active": true,
      "attach_user_tags": [
        "my-daily-backup-plan"
      ],
      "copy_user_tags": true,
      "created_at": "2022-12-16T00:45:28.421Z",
      "cron_spec": "*/5 1,2,3 * * *",
      "deletion_trigger": {
        "delete_after": 20
      },
      "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f/plans/4cf9171a-0043-4434-8727-15b53dbc374c",
      "id": "4cf9171a-0043-4434-8727-15b53dbc374c",
      "lifecycle_state": "stable",
      "name": "my-policy-plan",
      "resource_type": "backup_policy_plan"
    }
  ]
}
```
{: codeblock}

To retrieve information about a single plan, specify the plan ID in the request:

```curl
curl -X GET "$vpc_api_endpoint/v1/backup_policies/{backup_policy_id}/plans/{plan_id}?version=2022-12-16&generation=2"
```
{: codeblock}


## Retrieve a backup plan with the fast restore option
{: #baas-view-plan-fast-restore}

You can see information about the zone in which fast restore backup snapshots are being created and the number of snapshots in each zone.

Make a `GET /backup_policy/{backup_policy_id}/plans/{plan_id}` call. In the response, the `clone_policy` property shows the zone in which the snapshot clone for fast restore is created and the maximum number of recent snapshots per volume that keeps clones.

You can also retrieve all plans for a backup policy and view plans you set up for backup snapshot clones. For more information, see the [List all plans for a backup policy](/apidocs/vpc/latest#list-backup-policy-plans) in the API reference.
{: note}

Example call:

```curl
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f/plan/4d58eb08-d950-498b-8175-b4d617b6ba6a?version=2022-12-16&generation=2"\
?version=2022-06-03&generation=2"
   -H "Authorization: $iam_token"
```
{: codeblock}

The response shows the clone policy information. In this example, clones are created for snapshots in us-south-2 with a maximum of two clones per snapshot to be kept. 

```json
{
  "active": true,
  "attach_user_tags": [
    "my-plan-2"
  ],
  "clone_policy": {
  "max_snapshots": 2,
  "zones": [
    {
      "name": "us-south-2"
    },
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1"
    },
  "copy_user_tags": true,
  "created_at": "2022-12-16T15:16:37Z",
  "cron_spec": "45 * * * *",
  "deletion_trigger": {
    "delete_after": 5
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f/plans/4d58eb08-d950-498b-8175-b4d617b6ba6a",
  "id": "r134-6da51cfe-6f7b-4638-a6ba-00e9c327b178",
  "lifecycle_state": "stable",
  "name": "my-backup-plan-2",
  "resource_type": "backup_policy_plan"
}
```
{: codeblock}

## Next steps
{: #backup-view-next-steps}

* [Apply tags to your resources for backups](/docs/vpc?topic=vpc-backup-use-policies).
* [Manage your backup policies and plans](/docs/vpc?topic=vpc-backup-service-manage).
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).
