---

copyright:
  years: 2022
lastupdated: "2022-04-29"

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
{:preview: .preview}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Viewing backup policies
{: #backup-view-policies}

Use the UI, CLI, or API to a list and view all backup policies you created for your block storage volumes. Look at details of individual policies.
{: shortdesc}

This service is available only to accounts with special approval to preview this feature. Contact IBM Support if you're interested in getting access.
{: preview}

## View backup policies with the UI
{: #backup-view-ui}
{: ui}

List all backup policies and view details of a specific policy.

### List all backup policies with the UI
{: #backup-list-all-policies}

List all backup policies you created for volumes in your account for the selected region.

In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Backup policies**. By default, the Overview tab is selected.

Table 1 describes the information on the Backup policy list page. The default region for the account is selected by default. To select a different region, use the dropdown menu. Policies listed on the page are newest to oldest.

| Field | Value |
|-------|-------|
| Name | Click on the name of a policy to see it details. |
| Status | Current status of the policy, such as _Stable_. For a list of policy statuses, see [Backup policy statuses](/docs/vpc?topic=vpc-backup-service-manage#backup-policy-statuses) |
| Applied resources | Number of block storage volumes being backed up by the policy. The number of volumes is a link that takes you to a [list of volumes](#backup-view-vol-backup-policies) applied to the policy. |
| Tags for target resources | Tags for target block storage volumes you are backing up. |
| Last run time | The most recent time a job ran for the backup policy. If blank, there are no applied resources for a job to run. |
| Created at | Date and time the backup created. |
| Actions menu | Click the icon (...) for additional actions. These include restore and delete. |
{: caption="Table 1. Backup policy list view" caption-side="top"}

### View details of a backup policy with the UI
{: #backup-view-policy}

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Backup policies**. By default, the Overview tab is selected.

2. Click on a policy name. Table 2 describes the information about the selected backup policy.

| Field | Value |
|-------|-------|
| Actions | Actions on this policy: Add auditing and Delete. |
| **Policy details** | |
| Name | Name of the backup policy. Click the pencil icon to edit. |
| Resource group | [Resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups) for the block storage volume. |
| Location | Policies for the selected region. |
| Created | Date the policy was created. |
| CRN | Cloud resource name of the policy. |
| ID | Backup policy ID. |
| Tags for target resources | User tags that when applied to any block storage volumes will create a backup.  Click the pencil icon to add more tags. For more information, see  [Edit tags for target resources](docs/vpc?topic=vpc-backup-service-manage&interface=ui#backup-edit-tags).  See also [Applying backup policies to resources using tags](/docs/vpc?topic=vpc-backup-use-policies). |
| **Plan** | Show the plan name, retention policy, and plan status. Optionally, click **Create** to add additional plans for an existing policy. |
| Plan name | Unique name for the plan. Expand to see plan details, status, and tags associated with this plan. |
| Retention | Period you set for the plan to be in effect, for example, 30 days. |
| Status | Plan status. Shows `enabled` for an active plan. |
| Actions menu | Edit or delete a plan. Let's you change the name and plan details such as retention, or delete a plan. For more information, see [Edit or delete a backup plan with the UI](/docs/vpc?topic=vpc-backup-service-manage&interface=ui). |
{: caption="Table 2. Backup policy details" caption-side="top"}

### View a list of volumes that have a backup policy
{: #backup-view-vol-backup-policies}

View a list of block storage volumes being backed up by the policy. For a volume to be backed up by a policy, the volume must be tagged with at least one of the policy’s tags for the target resources.

Look at which block storage volumes have policies and verify that the policies are correctly applied.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > storage > Backup policies**. 

2. Click on a policy name.

3. Click the **Applied resources** tab. 

A list of block storage volumes being backed up by this policy are shown.
Information about the volumes include the volume name, status, volume size, and encryption type (see Table 3).

| Field | Description |
|-------|-------------|
| Name | Name of the volume. Click the pencil icon to edit. |
| Status | [Status of the volume](/docs/vpc?topic=vpc-managing-block-storage#status). |
| Size | Size of the volume in GBs.|
| Encryption | [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption). |
| Add volume | Add a volume to this policy. The informational side panel provides a list of tags for target resources you can apply to the volume, and a link to the go to a [list of block storage volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui). You must apply at least one of the policy's tags for target resources to the volume. |
{: caption="Table 3. List of block storage volumes for the backup policy" caption-side="top"}

You can also view a more detailed list of all block storage volumes in your account region, including user tags applied to the volume. For more information, see [View information about all block storage volumes with the UI](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

When you click on a volume name to see the details, you need administrator privileges to access the information.
{: note}


## View backup policies from the CLI
{: #backup-view-cli}
{: cli}

### List all backup policies from the CLI
{: @backup-view-all-cli}

Run the `backup-policies` command to list all backup policies you created in your account and region. 

```text
ibmcloud is backup-policies [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups] [--output JSON] [-q, --quiet]
```
{: pre}

For example:

```text
$ ibmcloud is backup-policies

Listing backup policies in all resource groups and region us-south under account vpcdemo as user myuser.mycompany.com...
ID                                     Name                          Status    Resource group  
d8a0e8d9-c592-4175-80bc-3056f6fd1da5   demopolicy20                  pending   Default  
688816c5-788d-4c2c-ba95-b9c588195dfb   demopolicy21                  stable    Default
```
{: screen}

### View backup policy details from the CLI
{: #backup-view-details-cli}

Run the `backup-policy` command and specify either the backup policy ID or policy name. This example uses the policy ID. 

```text
ibmcloud is backup-policy POLICY_ID|POLICY_NAME [--output JSON] [-q, --quiet]
```
{: pre}

For example:

```text
ibmcloud is backup-policy ac2a8be2-aa99-4571-baed-c3ec63a64ce7
Getting backup policy ac2a8be2-aa99-4571-baed-c3ec63a64ce7 under account VPC1 as user myuser@mycompany.com...
                          
ID                     ac2a8be2-aa99-4571-baed-c3ec63a64ce7   
Name                   bkp-policy-1
CRN                    crn:v1:staging:public:is:us-south:a/2d1bace7b46e4815a81e52c6ffeba5cf::backup-policy:r134-ac2a8be2-aa99-4571-baed-c3ec63a64ce7   
Status                 stable   
Plans                  ID                                     Name              Resource type      
                       361ed7f8-ee19-4c74-86c1-e3aafcac8a0d   bkp-plan-1        backup_policy_plan      
                          
Backup tags            env:dev   
Backup resource type   volume   
Resource group         Default   
Created                2022-04-28T17:56:53+05:30 
```
{: screen}

### List all plans for a backup policy from the CLI
{: #backup-view-plans-cli}

Run the `ibmcloud is backup-policy-plans` and specify the either the policy ID or policy name to see all plans created for this policy. This example specifies the policy ID.  The backup plan uses a CRON expression to schedule backups.

```text
ibmcloud is backup-policy-plans POLICY_ID|POLICY_NAME [--output JSON] [-q, --quiet]
```
{: pre}

For example, 

```text
ibmcloud is backup-policy-plans ac2a8be2-aa99-4571-baed-c3ec63a64ce7                 
Listing plans of backup policy ac2a8be2-aa99-4571-baed-c3ec63a64ce7 under account VPC1 as user myuser@mycompany.com...
ID                                     Name         Active   Lifecycle State   Cron specification   Delete after days   
361ed7f8-ee19-4c74-86c1-e3aafcac8a0d   bkp-plan-1   true     stable            0 1,2,3 * * *        30     
ea79b2ce-5b7a-463e-8356-fb2558531945   bkp-plan-2   true     stable            0 1,2,3 * * *        20   
```
{: screen}

### View backup plan details from the CLI
{: backup-view-plan-details-cli}

Run the `ibmcloud is backup-policy-plans` command and specify the policy ID or policy name, and plan ID or plan name. This example specifies the policy ID and plan ID.

```text
ibmcloud is backup-policy-plan POLICY_ID|POLICY_NAME PLAN_ID|PLAN_NAME [--output JSON] [-q, --quiet]
```
{: pre}

For example:

```text
ibmcloud is backup-policy-plan ac2a8be2-aa99-4571-baed-c3ec63a64ce7 361ed7f8-ee19-4c74-86c1-e3aafcac8a0d 
Getting plan r134-361ed7f8-ee19-4c74-86c1-e3aafcac8a0d under account VPC1 as user myuser@mycompany.com...
                        
ID                   361ed7f8-ee19-4c74-86c1-e3aafcac8a0d   
Name                 bkp-plan-1   
Active               true   
Lifecycle State      stable   
Tag                  -   
Copy tags            true   
Cron specification   0 1,2,3 * * *   
Delete after days    30   
Created              2022-04-28T17:56:53+05:30 
```
{: screen}

## View backup policies and plans with the API
{: #backup-view-api}
{: api}

### Show details of a backup policy with the API
{: #backup-view-policy-details-api}

Make a `GET /backup_policies/{backup_policy_id}` request to show details of a backup policy identified by ID.

```curl
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f?version=2022-05-03&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

A successful response looks like this:

```text
{
  "created_at": "2022-05-04T23:29:55.833Z",
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

Make a `GET /backup_policies/{backup_policy_id}/plans` request to list plans associated for a backup policy. YOU can have up to four plans per policy.

```curl
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/{backup_policy_id}/plans?version=2022-05-03&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

A successful response looks like this:

```text
{
  "plans": [
    {
      "active": true,
      "attach_user_tags": [
        "my-daily-backup-plan"
      ],
      "copy_user_tags": true,
      "created_at": "2022-05-04T00:45:28.421Z",
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
curl -X GET "$vpc_api_endpoint/v1/backup_policies/{backup_policy_id}/plans/{plan_id}?version=2022-05-03&generation=2"
```
{: codeblock}


## Next steps
{: #backup-view-next-steps}

* [Apply tags to your resources for backups](/docs/vpc?topic=vpc-backup-use-policies).
* [Manage your backup policies and plans](/docs/vpc?topic=vpc-backup-service-manage).
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).
