---

copyright:
  years: 2022, 2025
lastupdated: "2025-09-02"

keywords: Backup, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing backup policies
{: #backup-view-policies}

You can list and view all backup policies that you created for your storage resources in the console, from the CLI, with the API, or Terraform. You can review the details of individual policies, and view the number of resources that have tags that match a backup policy.
{: shortdesc}

For the backup operation to be successful, the tagged volumes must be attached to running virtual server instances. Unattached volumes are not backed up even if they have the right tags.
{: note}

## Viewing backup policies in the console
{: #backup-view-ui}
{: ui}

You can list all backup policies and view details of a specific policy by using the UI.

### Listing all backup policies in the console
{: #backup-list-all-policies}

List all backup policies that you created for volumes in your account for the selected region by using the UI.

In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Backup policies**.

Table 1 describes the information on the Backup policy list page. The default region for the account is selected. You can select a different region from the menu. Policies are listed on the page from newest to oldest.

| Field | Value |
|-------|-------|
| Name | Click the name of a policy to see its details. |
| Status | The status of the policy, such as _Stable_. For more information about policy statuses, see [Backup policy statuses](/docs/vpc?topic=vpc-backup-vpc-monitoring). |
| Applied resources | Number of resources ({{site.data.keyword.block_storage_is_short}} volumes, {{site.data.keyword.filestorage_vpc_short}} shares, or virtual server instances) that are tagged to be backed up by the policy [^Note1]. The number is a link that takes you to a list of resources that apply for the policy. |
| Tags for target resources | Tags for target volumes or shares that you are backing up. |
| Last run time (local) | The most recent time a job ran for the backup policy. If the field is blank, the volumes don't have matching tags for a job to run. |
| Created date (local) | Date and time of when the backup was created. |
{: caption="Backup policy list view" caption-side="bottom"}

[^Note1]: For the backup operation to be successful, the tagged volumes must be attached to running virtual server instances. Unattached volumes are not backed up even if they have the right tags.

By clicking the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), you can display a menu of context-specific actions.
   - Restore
   - Delete

### Viewing details of a backup policy in the console
{: #backup-view-policy}

You can view details of a backup policy by using the UI.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Backup policies**.

2. Click a policy name. Tables 2 and 3 describe the information about the selected backup policy and its associated plans. You can add activity tracking, event notifications or delete the policy from the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions").

   | Field | Value |
   |-------|-------|
   | Name | Name of the backup policy. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit. |
   | Policy ID | Backup policy ID. |
   | Resource group | [Resource group](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups) for the {{site.data.keyword.block_storage_is_short}} volume. |
   | Location | Policies for the selected region. |
   | Created date | The date the policy was created. |
   | CRN | Cloud resource name of the policy. |
   | Applied resources | The number of volumes that are covered by the policy. This list includes volumes and shares that were created by users for the account. If the policy is an enterprise-wide policy, the list shows volumes of the enterprise account, and not the volumes of its child accounts. |
   | Tags for target resources | This field shows the user tags that can trigger the creation of a backup when they are applied to a resource. \n - You can click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to add more tags. For more information, see [Edit tags for target resources](/docs/vpc?topic=vpc-backup-service-manage&interface=ui#backup-edit-tags). \n - See also [Applying backup policies to resources by using tags](/docs/vpc?topic=vpc-backup-use-policies). |
   | Target resource type  | The backup policy can apply to individual block **volumes** or a consistency group of block volumes of **instances**. |
   | Last backup job | It shows the date and time when the last backup job ran. Backup jobs create or delete backup snapshots based on the plan frequency and retention settings. To see which type of job ran last, click **view**. |
   | Enterprise account CRN | This value is only shown when the backup policy is an enterprise-wide policy. It is the Cloud Resource Name of the Enterprise that created the policy.|
   | Health | The current health state of the policy. For more information, see the [FAQs](/docs/vpc?topic=vpc-backup-service-enterprise-faq&interface=terraform#faq-baas-ee-5).|
   {: caption="Backup policy details" caption-side="bottom"}

   The Plans card shows the plan name, retention policy, and plan status. Optionally, you can click **Create** to add more plans for an existing policy.

   | Field | Value |
   |-------|-------|
   | Plan name | Unique name for the plan. Expand to see plan details, such as frequency, tags that are associated with this plan, and whether fast restore and remote copies are enabled. |
   | Retention | Period that you set for the plan to be in effect, for example, 30 days. |
   | Status | Plan status. Shows `enabled` for an active plan. |
   {: caption="Backup policy details" caption-side="bottom"}

   By clicking the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions"), you can display a menu of context-specific actions. You can change the name and other plan details such as retention, tags, fast restore, and remote copies. For more information, see [Edit or delete a backup plan in the console](/docs/vpc?topic=vpc-backup-service-manage&interface=ui).

### Viewing the list of resources that are associated to a backup policy
{: #backup-view-vol-backup-policies}

View the list of resources that are backed up by the policy. For a resource to be backed up by a policy, the resource must be tagged with at least one of the policy’s tags for the target resources.

You can use this list of {{site.data.keyword.block_storage_is_short}} volumes, {{site.data.keyword.filestorage_vpc_short}} shares, or virtual server instances to verify that the backup policies are correctly applied.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Backup policies**.

2. Click a policy name.

3. Click the **Applied resources** tab.

   - If the policy is for individual {{site.data.keyword.block_storage_is_short}} volumes, a list of the volumes that are backed up by this policy is shown. Information about the volumes includes the volume name, status, volume size, and encryption type.

     | Field | Description |
     |-------|-------------|
     | Name | Name of the volume. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit. |
     | Status | Status of the volume. |
     | Size | Size of the volume in GBs.|
     | Encryption | [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption). |
     {: caption="List of {{site.data.keyword.block_storage_is_short}} volumes for the backup policy" caption-side="top"}

     You can add other volumes to this policy by clicking **Add volumes**. The informational side panel provides a list of tags for target resources that you can apply to the volume, and a link to the [list of {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui). You must apply at least one of the policy's tags for target resources to the volume. |

   - If the policy is for multi-volume backups of a consistency group of {{site.data.keyword.block_storage_is_short}} volumes, a list of virtual server instances is shown. The list contains the virtual server instances whose Block Storage volumes are backed up this policy. Information about the virtual server instances includes the name, status, VPC, and profile.

      | Field  | Description |
      |--------|-------------|
      | Name   | Name of the volume. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit. |
      | Status | Status of the volume. |
      | Virtual private cloud | The name of the VPC that the virtual server instance is in. Click it to go to the details page of the VPC. |
      | Profile| The Compute profile of the virtual server instance. |
      {: caption="List of virtual server instances." caption-side="top"}

       Click **Add virtual server instance** to add the attached volumes of a virtual server instance to this policy. The informational side panel provides a list of tags for target resources that you can apply to the virtual server instance. It also contains a link to the list of virtual server instances of your account. You must apply at least one of the policy's tags for target resources to the volume.


   - If the policy is for file shares, a list of the shares that are backed up by this policy is shown. Information about the shares includes their names, status, volume sizes, and encryption types.

     | Field | Description |
     |-------|-------------|
     | Name | Name of the share. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to edit. |
     | Status | Status of the share. |
     | Size | Size of the share in GBs.|
     | Encryption | [IBM-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption). |
     {: caption="List of {{site.data.keyword.filestorage_vpc_short}} shares for the backup policy" caption-side="top"}

## Viewing backup policies from the CLI
{: #backup-view-cli}
{: cli}

You can view backup policies by using the CLI.

### Before you begin
{: #backup-view-cli-prereq}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

Log in to {{site.data.keyword.cloud}}.

```sh
ibmcloud login --sso -a cloud.ibm.com
```
{: pre}

This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

### Listing all backup policies from the CLI
{: #backup-view-all-cli}

Run the `backup-policies` command to list all backup policies that you created in your account and region.

```sh
ibmcloud is backup-policies [--tag TAG_NAME] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME | --all-resource-groups] [--output JSON] [-q, --quiet]
```
{: pre}

The following example shows the output that you can expect.

```sh
$ ibmcloud is backup-policies
Listing backup policies in all resource groups and region eu-de under account Test Account as user test.user@ibm.com...
ID                                          Name                  Status   Resource group
r138-0521986d-963c-4c18-992d-d6a7a99d115f   backup-policy-v1      stable   defaults
r138-a84c2063-e95f-46dc-98a8-9799b0e2fd2f   demo2                 stable   defaults
r138-8c494618-9e4f-4b67-9a08-ee3491404f3b   my-backup-policy-v1   stable   defaults
r138-5c719085-cf26-456e-9216-984866659e29   my-backup-policy-v2   stable   defaults
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policies`](/docs/vpc?topic=vpc-vpc-reference#backup-policies-list). For more information about the values in the `Status` column, see [Backup policy statuses](/docs/vpc?topic=vpc-backup-vpc-monitoring)

### Listing all backup policies filtered by user tags from the CLI
{: #backup-view-all-filter-by-tags-cli}

Run the `backup-policies` command to list all backup policies that you created in your account and region. The `--tag` option filters the collection of backup policies with the exact user tag value.

```sh
ibmcloud is backup-policies --tag
```
{: pre}

The following example produces a list of backup policies that have the `dev:test` tag.

```sh
$ ibmcloud is backup-policies --tag dev:test
Listing backup policies in all resource groups and region eu-de under account Test Account as user test.user@ibm.com...
ID                                          Name                  Status   Resource group
r138-0521986d-963c-4c18-992d-d6a7a99d115f   backup-policy-v1      stable   defaults
r138-8c494618-9e4f-4b67-9a08-ee3491404f3b   my-backup-policy-v1   stable   defaults
r138-5c719085-cf26-456e-9216-984866659e29   my-backup-policy-v2   stable   defaults
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policies`](/docs/vpc?topic=vpc-vpc-reference#backup-policies-list).

### Viewing backup policy details from the CLI
{: #backup-view-details-cli}

Run the `backup-policy` command and specify either the backup policy ID or the policy name.

```text
ibmcloud is backup-policy POLICY [--output JSON] [-q, --quiet]
```
{: pre}

The following example lists the properties of a backup policy with the plan that it contains.

```sh
$ ibmcloud is backup-policy my-backup-policy-v2
Getting backup policy my-backup-policy-v2 under account Test Account as user test.user@ibm.com...

ID                      r006-0723c648-9a47-4d51-b1ba-349e21e715b6
Name                    my-backup-policy-v2
CRN                     crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-0723c648-9a47-4d51-b1ba-349e21e715b6
Status                  stable
Last job completed at   2023-09-26T10:13:18.000Z
Plans                   ID                                          Name        Resource type
                        r006-e888bb31-7bf2-4885-a9f3-d448c1c37326   my-plan-b   backup_policy_plan

Backup tags             dev:test
Match resource type    volume
Resource group          ID                                 Name
                        6edefe513d934fdd872e78ee6a8e73ef   defaults

Created at              2023-09-05T16:30:09+00:00
```
{: screen}

The following example uses the policy ID and the option to receive the response in JSON format. This policy creates consistency group backups in the child accounts of an Enterprise.

```sh
$ ibmcloud is backup-policy r006-0723c648-9a47-4d51-b1ba-349e21e715b6 --output JSON
{
    "created_at": "2023-09-05T16:30:09.000Z",
    "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-0723c648-9a47-4d51-b1ba-349e21e715b6",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-0723c648-9a47-4d51-b1ba-349e21e715b6",
    "id": "r006-0723c648-9a47-4d51-b1ba-349e21e715b6",
    "lifecycle_state": "stable",
    "match_resource_type": "instance",
    "included_content": "data_volumes",
    "match_user_tags": "dev:test",
    "name": "my-backup-policy-v2",
    "plans": [
        {
            "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-0723c648-9a47-4d51-b1ba-349e21e715b6/plans/r006-e888bb31-7bf2-4885-a9f3-d448c1c37326",
            "id": "r006-e888bb31-7bf2-4885-a9f3-d448c1c37326",
            "name": "my-plan-b",
            "resource_type": "backup_policy_plan"
        }
    ],
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/6edefe513d934fdd872e78ee6a8e73ef",
        "id": "6edefe513d934fdd872e78ee6a8e73ef",
        "name": "defaults"
    },
    "resource_type": "backup_policy",
    "scope": {
        "id": "7e44cb4667ba4b88b1b1f8dcc15e33b3",
        "crn": "crn:v1:bluemix:public:enterprise::a/a1234567::enterprise:7e44cb4667ba4b88b1b1f8dcc15e33b3",
        "resource_type": ""
    }
}
```
{: screen}

The following example uses the policy name and no other options. The policy applies to individual Block Storage volumes.

```sh
$ ibmcloud is backup-policy my-backup-policy-v1
Getting backup policy my-backup-policy-v1 under account Test Account as user test.user@ibm.com...

ID                      r138-8c494618-9e4f-4b67-9a08-ee3491404f3b
Name                    my-backup-policy-v1
CRN                     crn:v1:bluemix:public:is:eu-de:a/a1234567::backup-policy:r138-8c494618-9e4f-4b67-9a08-ee3491404f3b
Status                  stable
Last job completed at   2023-02-22T20:12:44.000Z
Plans                   ID                                          Name                    Resource type
                        r138-4d77d84c-929c-49e9-9f05-952be9486406   not-just-another-plan   backup_policy_plan
                        r138-7734be40-e2a5-4ee6-b4bd-75763639092b   my-policy-plan          backup_policy_plan

Backup tags             dev:test
Match resource type    volume
Resource group          defaults
Created at              2023-02-21T18:37:17+00:00
```
{: screen}

The following example lists the properties of an Enterprise backup policy. The scope shows the enterprise account's CRN.

```sh
$ ibmcloud is backup-policy r006-0bc533ed-4796-407a-982e-693b418f3de3
Getting backup policy r006-0bc533ed-4796-407a-982e-693b418f3de3 under account Enterprise Test as user test.user@ibm.com...

ID                     r006-0bc533ed-4796-407a-982e-693b418f3de3
Name                   backup-scope-2
CRN                    crn:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-0bc533ed-4796-407a-982e-693b418f3de3
Status                 stable
Plans                  ID                                          Name           Resource type
                       r006-0741b600-e8d5-41b4-88a7-c19b6fbf89ca   scope-plan-2   backup_policy_plan

Backup tags            dev:test
included-content       data_volumes
Match resource type   volume
Resource group         ID                                 Name
                       e579217258f74f42974e6ec4da287fc5   Default

Scope                  ID                                 CRN                                                                                         Resource type
                       7e44cb4667ba4b88b1b1f8dcc15e33b3   crn:v1:bluemix:public:enterprise::a/a1234567::enterprise:7e44cb4667ba4b88b1b1f8dcc15e33b3   -

Health State           ok
Created at             2023-08-30T13:39:10+05:30

```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-view).

### Listing all plans for a backup policy from the CLI
{: #backup-view-plans-cli}

Run the `ibmcloud is backup-policy-plans` and specify either the policy ID or the policy name to see all plans created for this policy.

```sh
ibmcloud is backup-policy-plans POLICY [--output JSON] [-q, --quiet]
```
{: pre}

The following example specifies the policy name. The output provides basic information of the backup plans, such as ID, name, status, and the CRON expressions that define the schedules for the backups.

```sh
$ ibmcloud is backup-policy-plans backup-policy-v1
Listing plans of backup policy backup-policy-v1 under account Test Account as user test.user@ibm.com...
ID                                          Name               Active   Lifecycle state   Cron specification
r138-2129a79a-5629-4069-bf79-7bb0af3b0bd3   my-policy-plan-a   true     stable            05 15 * * *
r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9   my-policy-plan-c   true     stable            10 20 * * *
```
{: screen}

The following example specifies the policy ID of the policy from the previous example. The output provides more detailed information about the plans (`my-policy-plan-a` and `my-policy-plan-c`) that includes information about fast restore clones and the retention policy as well.

```json
$ ibmcloud is backup-policy-plans r138-0521986d-963c-4c18-992d-d6a7a99d115f  --output JSON
[
    {
        "active": true,
        "attach_user_tags": [
            "daily-backup-plan"
        ],
        "clone_policy": {
            "max_snapshots": 4,
            "zones": [
                {
                    "href": "https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-1",
                    "name": "eu-de-1"
                },
                {
                    "href": "https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-2",
                    "name": "eu-de-2"
                }
            ]
        },
        "copy_user_tags": true,
        "created_at": "2023-02-21T22:42:31.000Z",
        "cron_spec": "05 15 * * *",
        "deletion_trigger": {
            "delete_after": 20,
            "delete_over_count": 20
        },
        "href": "https://eu-de.iaas.cloud.ibm.com/v1/backup_policies/r138-0521986d-963c-4c18-992d-d6a7a99d115f/plans/r138-2129a79a-5629-4069-bf79-7bb0af3b0bd3",
        "id": "r138-2129a79a-5629-4069-bf79-7bb0af3b0bd3",
        "lifecycle_state": "stable",
        "name": "my-policy-plan-a",
        "resource_type": "backup_policy_plan"
    },
    {
        "active": true,
        "attach_user_tags": [
            "daily-backup-plan"
        ],
        "clone_policy": {
            "max_snapshots": 3,
            "zones": [
                {
                    "href": "https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-1",
                    "name": "eu-de-1"
                },
                {
                    "href": "https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-2",
                    "name": "eu-de-2"
                }
            ]
        },
        "copy_user_tags": true,
        "created_at": "2023-02-21T22:42:32.000Z",
        "cron_spec": "10 20 * * *",
        "deletion_trigger": {
            "delete_after": 20,
            "delete_over_count": 20
        },
        "href": "https://eu-de.iaas.cloud.ibm.com/v1/backup_policies/r138-0521986d-963c-4c18-992d-d6a7a99d115f/plans/r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9",
        "id": "r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9",
        "lifecycle_state": "stable",
        "name": "my-policy-plan-c",
        "resource_type": "backup_policy_plan"
    }
]
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-plans`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-plans-list).

### Viewing backup plan details from the CLI
{: #backup-view-plan-details-cli}

Run the `ibmcloud is backup-policy-plan` command and specify the policy ID or policy name, and plan ID or plan name.

```sh
ibmcloud is backup-policy-plan POLICY PLAN [--output JSON] [-q, --quiet]
```
{: pre}

The following example specifies the policy and the plan name.

```sh
$ ibmcloud is backup-policy-plan backup-policy-v1 my-policy-plan-a
Getting plan my-policy-plan-a under account Test Account as user test.user@ibm.com...

ID                   r138-2129a79a-5629-4069-bf79-7bb0af3b0bd3
Name                 my-policy-plan-a
Active               true
Lifecycle state      stable
Clone policy         Max snapshots   Zones
                     4               eu-de-1,eu-de-2

Deletion trigger     Delete after   Delete over count
                     20             20

Remote Region Policies   Region    Encryption Key   Delete over count
                         us-east   -                99

Attached tags        daily-backup-plan
Copy tags            true
Cron specification   05 15 * * *
Created at           2023-02-21T22:42:31+00:00
Resource type        backup_policy_plan
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-plan`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-plan-view).

## Viewing backup policies and plans with the API
{: #backup-view-api}
{: api}

You can view backup policies and plans by using the API.

### Showing details of a backup policy with the API
{: #backup-view-policy-details-api}

You can programmatically retrieve the details of a backup policy by calling the `/backup_policies/{backup_policy_id}` method in the [VPC API](/apidocs/vpc/latest#get-backup-policy){: external} as shown in the following sample request.

```sh
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/r006-076191ba-49c2-4763-94fd-c70de73ee2e6?version=2023-09-26&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "created_at": "2023-09-26T15:06:03.000Z",
  "crn": "crn:v1:bluemix:public:is:us-south:a/123456::backup-policy:r006-076191ba-49c2-4763-94fd-c70de73ee2e6",
  "included_content": [
        "data_volumes"
      ],
  "health_reasons": [],
  "health_state": "ok",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-076191ba-49c2-4763-94fd-c70de73ee2e6",
  "id": "r006-076191ba-49c2-4763-94fd-c70de73ee2e6",
  "lifecycle_state": "stable",
  "match_resource_type": "volume",
  "match_user_tags": [
    "my-tag-1",
    "my-tag-2"
  ],
  "name": "my-backup-policy",
  "plans": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-076191ba-49c2-4763-94fd-c70de73ee2e6/plans/r006-4d6074c4-3811-4bb3-af4a-1fd6cb38d6fe",
      "id": "r006-4d6074c4-3811-4bb3-af4a-1fd6cb38d6fe",
      "name": "my-backup-plan-1",
      "resource_type": "backup_policy_plan"
    }
  ],
  "resource_group": {
    "crn": "crn:v1:bluemix:public:resource-controller::a/123456::resource-group:678523bcbe2b4eada913d32640909956",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
    "id": "678523bcbe2b4eada913d32640909956",
    "name": "Default"
  },
  "resource_type": "backup_policy",
  "scope": {
    "crn": "crn:v1:bluemix:public:enterprise::a/e92d45e305dc4ee0b13e29be392f1c0c::enterprise:ebc2b430240943458b9e91e1432cfcce",
    "id": "fee82deba12e4c0fb69c3b09d1f12345",
    "resource_type": "account"
  }
}
```
{: codeblock}

The following example shows the details of a backup policy that creates backups of a multi-volume consistency group.

```sh
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/r006-5f45c186-d710-41ed-a4bc-e924a79e47bb?version=2023-12-05&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

```json
{
  "created_at": "2023-11-14T19:36:29Z",
  "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-5f45c186-d710-41ed-a4bc-e924a79e47bb",
  "health_reasons": [],
  "health_state": "ok",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-5f45c186-d710-41ed-a4bc-e924a79e47bb",
  "id": "r006-5f45c186-d710-41ed-a4bc-e924a79e47bb",
  "included_content": [
    "boot_volume",
    "data_volumes"
  ],
  "lifecycle_state": "stable",
  "match_resource_type": "instance",
  "match_user_tags": [
    "vsi11"
  ],
  "name": "policy-1699990587",
  "plans": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-5f45c186-d710-41ed-a4bc-e924a79e47bb/plans/r006-01fb4c96-57db-4fac-98f0-2158dbfd551f",
      "id": "r006-01fb4c96-57db-4fac-98f0-2158dbfd551f",
      "name": "plan-1699990587",
      "resource_type": "backup_policy_plan"
    }
  ],
  "resource_group": {
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/f20bdbd6554d48739ad38717e0511fdd",
    "id": "f20bdbd6554d48739ad38717e0511fdd",
    "name": "Default"
  },
  "resource_type": "backup_policy",
  "scope": {
    "id": "53945f147c1441b0940bc00927863af6",
    "resource_type": "account"
  }
}
```
{: codeblock}

For more information about the values of the `health_state` and `lifecycle_state` fields, see [Monitoring backup policy health states, lifecycle status, and events](/docs/vpc?topic=vpc-backup-vpc-monitoring).

### Listing all plans for a backup policy with the API
{: #backup-view-plans-api}

You can programmatically list the plans of a backup policy by calling the `/backup_policies/{backup_policy_id}/plans` method in the [VPC API](/apidocs/vpc/latest#get-backup-policy){: external} as shown in the following sample request.

```sh
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

```sh
curl -X GET "$vpc_api_endpoint/v1/backup_policies/{backup_policy_id}/plans/{plan_id}?version=2022-12-16&generation=2"
```
{: codeblock}

## Retrieving a backup plan with the fast restore option
{: #baas-view-plan-fast-restore}
{: api}

You can see information about the zone in which fast restore backup snapshots are being created and the number of snapshots in each zone.

Make a `GET /backup_policy/{backup_policy_id}/plans/{plan_id}` call. In the response, the `clone_policy` property shows the zone in which the snapshot clone for fast restore is created and the maximum number of recent snapshots per volume that keeps clones.

You can also retrieve all plans for a backup policy and view plans you set up for backup snapshot clones. For more information, see [List all plans for a backup policy](/apidocs/vpc/latest#list-backup-policy-plans) in the API reference.
{: note}

See the following example.

```sh
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f/plan/4d58eb08-d950-498b-8175-b4d617b6ba6a?version=2023-08-29&generation=2"
   -H "Authorization: $iam_token"
```
{: codeblock}

The response shows the clone policy information. In this example, clones are created for snapshots in us-south-2 with a maximum of two clones per snapshot to be kept.

```json
{
  "active": true,
  "attach_user_tags": ["my-plan-2"],
  "clone_policy": {
    "max_snapshots": 2,
    "zones": [
      {"name": "us-south-2"},
      {"href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1"}]},
  "copy_user_tags": true,
  "created_at": "2022-12-16T15:16:37Z",
  "cron_spec": "45 * * * *",
  "deletion_trigger": {"delete_after": 5},
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f/plans/4d58eb08-d950-498b-8175-b4d617b6ba6a",
  "id": "r006-6da51cfe-6f7b-4638-a6ba-00e9c327b178",
  "lifecycle_state": "stable",
  "name": "my-backup-plan-2",
  "resource_type": "backup_policy_plan"
  }
```
{: codeblock}

## Retrieving a backup plan with the cross-regional copy option
{: #baas-view-plan-crc}
{: api}

Make a `GET /backup_policy/{backup_policy_id}/plans/{plan_id}` request. In the response, the `remote_region_policies` property shows the remote region where a copy of the backup is created.

See the following example.

```sh
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/fb721535-2cc6-45d6-ade7-3ceb95b7f26f/plan/4d58eb08-d950-498b-8175-b4d617b6ba6a?version=2023-05-09&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

The response shows the remote region policy information. In this example, the output shows that when the backup job creates a backup snapshot in the `us-south` region, a copy of the backup is created in the `us-east` region, too.

```json
{
  "active": true,
  "attach_user_tags": ["hourly-backups"],
  "copy_user_tags": false,
  "created_at": "2023-05-09T15:16:37Z",
  "cron_spec": "0 */2 * * *",
  "deletion_trigger": {"delete_after": 5},
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans/6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "id": "6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "lifecycle_state": "stable",
  "name": "my-hourly-plan-2",
  "remote_region_policies": {
    "delete_over_count": 5,
    "encryption_key": "crn:v1:bluemix:public:kms:us-south:a/dffc98a0f1f0f95f6613b3b752286b87:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd617",
    "region": [
      {"name": "us-east"},
      {"href": "https://us-east.iaas.cloud.ibm.com/v1/regions/us-east/zones/us-east-2"}
     ],
    },
  "resource_type": "backup_policy_plan"
}
```
{: codeblock}

## Viewing backup policies and plans with Terraform
{: #backup-view-terraform}
{: terraform}

You can use Terraform to view backup policies and plans.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a specific regional endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
  region = "eu-de"
}
```
{: screen}

### Listing all backup policies with Terraform
{: #backup-view-all-terraform}

Import the details of a collection of backup policies as a read-only data source. You can filter the collection by specifying the name, resource group, or associated tags.

```terraform
data "ibm_is_backup_policies" "example" {
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policies](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_backup_policies){: external}.

### Viewing backup policy details with Terraform
{: #backup-view-details-terraform}

Import the details of a backup policy as a read-only data source. You can specify either the policy ID or the policy name.

```terraform
data "ibm_is_backup_policy" "example" {
  identifier = ibm_is_backup_policy.example.id
}
```
{: codeblock}

```terraform
data "ibm_is_backup_policy" "example" {
  name = ibm_is_backup_policy.example.name
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_backup_policy){: external}.

### Listing all plans for a backup policy with Terraform
{: #backup-view-plans-terraform}

Import the list of the backup plans that are part of a backup policy as a read-only data source. Optionally, you can specify the name of the plan that you want to see.

```terraform
data "ibm_is_backup_policy_plans" "example" {
  backup_policy_id = ibm_is_backup_policy.example.id
}
```
{: codeblock}

```terraform
data "ibm_is_backup_policy_plans" "example" {
  backup_policy_id = ibm_is_backup_policy.example.id
  name             = "my-policy-plan"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy_plans](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_backup_policy_plans){: external}.

### Viewing backup plan details with Terraform
{: #backup-view-plan-details-terraform}

Import the details of a backup plan as a read-only data source. You can specify either the plan ID or the plan name as you can see in the following examples.

```terraform
data "ibm_is_backup_policy_plan" "example" {
  backup_policy_id = ibm_is_backup_policy.example.id
  identifier       = ibm_is_backup_policy_plan.example.id
}
```
{: codeblock}

```terraform
data "ibm_is_backup_policy_plan" "example" {
  backup_policy_id = ibm_is_backup_policy.example.id
  name             = ibm_is_backup_policy_plan.example.name
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy_plan](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/is_backup_policy_plan){: external}.

## Next steps
{: #backup-view-next-steps}

You can do the following actions.

* [Apply tags to your resources for backups](/docs/vpc?topic=vpc-backup-use-policies).
* [Manage your backup policies and plans](/docs/vpc?topic=vpc-backup-service-manage).
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).
* [Enabling event notifications for Backup for VPC](/docs/vpc?topic=vpc-event-notifications-events).
