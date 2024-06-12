---

copyright:
  years: 2022, 2024
lastupdated: "2024-03-27"

keywords: Backup, backup service, backup plan, backup policy, restore, restore volume, restore data

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing backup policies
{: #backup-service-manage}

You can manage backup policies and their associated plans that were created for your resources in the console, from the CLI, with the API, or Terraform. You can delete policies and plans that you no longer need. You can update the backup policy and plans when your needs change. You can add or remove tags. You can check the status of your backup policies. Audit your policies by integrating Activity Tracker events.
{: shortdesc}

## Managing backup policies in the UI
{: #backup-manage-policy-ui}
{: ui}

You can rename a backup policy, delete a plan within a backup policy, delete the policy, and edit user tags in the console.

### Renaming a backup policy in the UI
{: #backup-rename-policy-ui}

You can rename a policy from the list of backup policies or from the backup policies details page.

* From the list of backup policies, click the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Rename**.

* From the policy details page, click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") next to the policy name.

### Editing tags for target resources in the UI
{: #backup-edit-tags}

From the Backup policy details page, you can edit the tags for your {{site.data.keyword.block_storage_is_short}} volumes target resources.

1. Go to the [details page for a backup policy](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-policy).

2. For **Tags for target resources**, click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit").

3. In the new window, enter a user tag name. For existing {{site.data.keyword.block_storage_is_short}} volumes with user tags, enter the tag name exactly as it appears in the volume. You can also add a user tag here and then go back and add it to a volume. You need only one tag for a volume to create a backup.

4. Click **Next**.

5. Review the information on the next window. If you're satisfied, check the disclaimer and click **Save changes**.

### Editing consistency group members in the UI
{: #backup-consistency-group-update-ui}

From the Backup policy details page, you can include or exclude {{site.data.keyword.block_storage_is_short}} boot volumes in the backup policy.

1. Go to the [details page for a backup policy](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-policy).

2. On the Overview tab, look for **Boot volume** in the Policy details, and click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") to change the value.

### Editing a backup plan in the UI
{: #backup-edit-delete-plan-ui}

After you provisioned a backup policy and created a backup plan, you can edit the plan details or delete the plan. You must have at least one remaining plan for the policy to create backups.

1. From the [backup plan details](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-view-policy) page, expand the Actions menu for the plan.

2. Select **Edit**. The plan details appear in the side panel. You can modify the same information that you specified when you created the plan, such as the name and backup frequency. You can enable or disable fast restore, or change which regions fast restore is available in. You can also enable or disable the creation and retention of copies in other regions.

3. Confirm your selections when you're finished.

## Managing backup policies and plans from the CLI
{: #backup-manage-policy-cli}
{: cli}

Delete a backup policy, rename a policy, and edit user tags from the CLI.

### Before you begin
{: #backup-manage-policy-cli-prereq}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to {{site.data.keyword.cloud}}.

   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

2. Select the current generation of VPC.

   ```sh
   ibmcloud is target --gen 2
   ```
   {: pre}

### Renaming a backup policy from the CLI
{: #backup-rename-policy-cli}

Run the `ibmcloud is backup-policy-update` command and specify the policy ID or policy name, and a new name for the backup policy.

```sh
ibmcloud is backup-policy-update POLICY [--name NEW_NAME] [--output JSON] [-q, --quiet]
```
{: pre}

The following example renames a backup policy that is identified by name:

```sh
cloudshell:~$ ibmcloud is backup-policy-update backup-policy-v1 --name new-policy-23
Updating backup policy backup-policy-v1 under account Test Account as user test.user@ibm.com...

ID                      r138-0521986d-963c-4c18-992d-d6a7a99d115f
Name                    new-policy-23
CRN                     crn:v1:bluemix:public:is:eu-de:a/a1234567::backup-policy:r138-0521986d-963c-4c18-992d-d6a7a99d115f
Status                  stable
Last job completed at   2023-02-22T20:12:44.000Z
Plans                   ID                                          Name               Resource type
                        r138-2129a79a-5629-4069-bf79-7bb0af3b0bd3   my-policy-plan-a   backup_policy_plan
                        r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9   my-policy-plan-c   backup_policy_plan

Backup tags             dev:test
Backup resource type    volume
Resource group          defaults
Created at              2023-02-21T22:42:10+00:00
```
{: screen}

The following example updates an Enterprise backup policy that is identified by ID:

```sh
cloudshell:~$ ibmcloud is backup-policy-update r006-0bc533ed-4796-407a-982e-693b418f3de3 --name cli-updated-2
Updating backup policy r006-0bc533ed-4796-407a-982e-693b418f3de3 under account Enterprise Test as user test.user@ibm.com...

ID                     r006-0bc533ed-4796-407a-982e-693b418f3de3
Name                   cli-updated-2
CRN                    crn:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-0bc533ed-4796-407a-982e-693b418f3de3
Status                 stable
Plans                  ID                                          Name           Resource type
                       r006-0741b600-e8d5-41b4-88a7-c19b6fbf89ca   scope-plan-2   backup_policy_plan

Backup tags            dev:test
Backup resource type   volume
Resource group         ID                                 Name
                       e579217258f74f42974e6ec4da287fc5   Default

Scope                  ID                                 CRN                                                                                                                 Resource type
                       7e44cb4667ba4b88b1b1f8dcc15e33b3   crn:v1:bluemix:public:enterprise::a/a1234567::enterprise:7e44cb4667ba4b88b1b1f8dcc15e33b3   -

Health State           ok
Created at             2023-08-30T13:39:10+05:30
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-update`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-update){: external}.

### Editing tags for target resources from the CLI
{: #backup-edit-tags-cli}

Run the `ibmcloud is backup-policy-update` command and specify the policy ID or policy name, and the tags that you want to modify.

```sh
ibmcloud is backup-policy-update POLICY [--match-tags MATCH_TAGS] [--output JSON] [-q, --quiet]
```
{: pre}

The following example updates backup policy user tags of the policy that is identified by ID.

```sh
cloudshell:~$ ibmcloud is backup-policy-update r138-0521986d-963c-4c18-992d-d6a7a99d115f --match-tags dev:env,bck:test
Updating backup policy r138-0521986d-963c-4c18-992d-d6a7a99d115f under account Test Account as user test.user@ibm.com...

ID                      r138-0521986d-963c-4c18-992d-d6a7a99d115f
Name                    new-policy-23
CRN                     crn:v1:bluemix:public:is:eu-de:a/a1234567::backup-policy:r138-0521986d-963c-4c18-992d-d6a7a99d115f
Status                  stable
Last job completed at   2023-02-22T20:12:44.000Z
Plans                   ID                                          Name               Resource type
                        r138-2129a79a-5629-4069-bf79-7bb0af3b0bd3   my-policy-plan-a   backup_policy_plan
                        r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9   my-policy-plan-c   backup_policy_plan

Backup tags             dev:env,bck:test
Backup resource type    volume
Resource group          ID                                 Name
                        11caaa983d9c4beb82690daab08717e9   Default

Scope                   ID                                 Resource type
                        a1234567   account

Health State            ok
Created at              2023-02-21T22:42:10+00:00
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-update`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-update){: external}.

### Editing consistency group members from the CLI
{: #backup-consistency-group-update-cli}

Run the `ibmcloud is backup-policy-update` command and specify the policy ID or policy name, and the tags that you want to modify.

```sh
ibmcloud is backup-policy-update POLICY [--included_content data_volumes|boot_volume ] [--output JSON] [-q, --quiet]
```
{: pre}

The following example updates backup policy to be applied to only the data volumes that are attached to the virtual server instance with the matching tag.

```sh
ibmcloud is backup-policy-create --match-tags dev:test --name my-cr-backup-policy-v1 --match-resource-type instance --included-content data_volumes
Updating backup policy my-cr-backup-policy-v1 under account Test Account as user test.user@ibm.com...

ID                    r006-e0713176-37b6-4168-88ab-ad92f8a544f9
Name                  my-cr-backup-policy-v1
CRN                   crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-e0713176-37b6-4168-88ab-ad92f8a544f9
Status                stable
Plans                 ID   Name   Resource type

Backup tags           dev:test
Match resource type   instance
Included Content      data_volumes
Resource group        ID                                 Name
                      11caaa983d9c4beb82690daab08717e9   Default

Scope                 ID                                 Resource type
                      a1234567                           account

Health State          ok
Created at            2023-12-05T19:29:35+05:30
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-update`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-update){: external}.

### Updating a backup plan from the CLI
{: #backup-update-plan-cli}

Run the `ibmcloud is backup-policy-plan-update` command to update a backup plan. Identify the policy and plan by ID or name in the command line. You can update the plan name, `cron-spec` backup schedule, attach user tags, update backup retention period, add, or remove fast restore zones.

The following code snippet shows the command syntax.

```sh
ibmcloud is backup-policy-plan-update POLICY PLAN --cron-spec CRON_SPEC [--name NAME] [--active] [--attach-tags ATTACH_TAGS] [--copy-tags true | false] [--delete-after DELETE_AFTER] [--delete-over-count DELETE_OVER_COUNT] [--clone-policy-zones ZONES] [--clone-policy-max-snapshots MAX_SNAPSHOTS] [--output JSON] [-q, --quiet] [--copy-remote-regions TBD] [--encryption-key TBD]
```
{: pre}

To rename the backup plan, run the command like the following example. It specifies the backup policy and plan by name, and changes the name of the plan from `my-policy-plan-c` to `my-policy-plan-b`.

```sh
cloudshell:~$ ibmcloud is backup-policy-plan-update new-policy-23 my-policy-plan-c --name my-policy-plan-b
Updating plan my-policy-plan-c of the backup policy r138-0521986d-963c-4c18-992d-d6a7a99d115f under account Test Account as user test.user@ibm.com...

ID                   r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9
Name                 my-policy-plan-b
Active               true
Lifecycle state      updating
Clone policy         Max snapshots   Zones
                     3               eu-de-1,eu-de-2

Deletion trigger     Delete after   Delete over count
                     20             20

Attached tags        daily-backup-plan
Copy tags            true
Cron specification   10 20 * * *
Created at           2023-02-21T22:42:32+00:00
Resource type        backup_policy_plan
```
{: screen}

To update locations of fast restore clones of backup snapshots, run the command like the following example. It specifies the backup policy and plan by their IDs. Because only `eu-de-1` is specified in the `--clone-policy-zones` option, fast restore snapshot clones are no longer retained in `eu-de-2`. The update also changes the number of backup clones to be created and stored in that zone from three to five.

```sh
cloudshell:~$ ibmcloud is backup-policy-plan-update r138-0521986d-963c-4c18-992d-d6a7a99d115f r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9  --clone-policy-max-snapshots 5 --clone-policy-zones eu-de-1
Updating plan r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9 of the backup policy r138-0521986d-963c-4c18-992d-d6a7a99d115f under account Test Account as user test.user@ibm.com...

ID                   r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9
Name                 my-policy-plan-b
Active               true
Lifecycle state      updating
Clone policy         Max snapshots   Zones
                     5               eu-de-1

Deletion trigger     Delete after   Delete over count
                     20             20

Attached tags        daily-backup-plan
Copy tags            true
Cron specification   10 20 * * *
Created at           2023-02-21T22:42:32+00:00
Resource type        backup_policy_plan
```
{: screen}

To update the backup plan to include the creation of a remote copy of the backup snapshot in a different region, run the `backup-policy-plan-update` command with `--remote-region-policies` option. The following example updates the _my-policy-plan-b_ plan that is part of the _new-policy-23_. It specifies the `us-east` region as the destination of the remote region copy. Optionally, you can add the `--encryption-key` option to provide the CRN of the root key to be used to decrypt the snapshot in the remote region.

```sh
cloudshell:~$ ibmcloud is backup-policy-plan-update new-policy-23 my-policy-plan-b --remote-region-policies '[
  {"delete_over_count": 99,"region": {"name": "us-east"}}
]'

Updating plan r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9 of the backup policy r138-0521986d-963c-4c18-992d-d6a7a99d115f under account Test Account as user test.user@ibm.com...

ID                   r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9
Name                 my-policy-plan-b
Active               true
Lifecycle state      updating
Clone policy         Max snapshots   Zones
                     5               eu-de-1

Deletion trigger     Delete after   Delete over count
                     20             20

Remote Region Policies   Region    Encryption Key   Delete over count
                         us-east   -                99

Attached tags        daily-backup-plan
Copy tags            true
Cron specification   10 20 * * *
Created at           2023-05-05T22:42:32+00:00
Resource type        backup_policy_plan
```
{: codeblock}

For more information about available command options, see [`ibmcloud is backup-policy-plan-update`](/docs/cli?topic=cli-vpc-reference#backup-policy-plan-update){: external}.

## Managing backup policies and plans with the API
{: #backup-manage-policy-api}
{: api}

You can rename and update a policy, edit user tags, and delete a backup policy programmatically with the API.

### Renaming a backup policy with the API
{: #backup-rename-policy-api}

To change the name of a backup policy, make a `PATCH /backup_policies/{backup_policies_id}` request and specify a new name for a backup policy. The following example shows how to rename policy `5063bfe5-c16f-4606-ba26-fba0f099b97d` to `my-backup-policy`.

```sh
curl -X PATCH\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d?version=2022-06-28&generation=2"\
  -H "Authorization: $iam_token"\
  -d {"name": "my-backup-policy"}
```
{: codeblock}

### Editing tags for target resources with the API
{: #backup-edit-tags-api}

To update the user tags for target resources, make a `PATCH /backup_policies/{backup_policies_id}` request and specify the new tag like the following example.

```sh
curl -X PATCH\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d?version=2022-06-28&generation=2"\
  -H "Authorization: $iam_token"\
  -d {"match_user_tags": ["my-daily-backup-policy"]}
```
{: codeblock}

### Editing consistency group members with the API
{: #backup-consistency-group-update-api}

When you want to update a multi-volume backup policy to include or exclude the boot volume from the consistency group, you can make a `PATCH /backup_policies/{backup_policies_id}` request and specify the value for `included-content` property as either `data_volumes`, `boot_volume`, or just `data_volumes`.

```sh
curl -X PATCH\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d?version=2022-06-28&generation=2"\
  -H "Authorization: $iam_token"\
  -d {"included_content": ["data_volumes"]}
```
{: codeblock}

### Updating a backup plan with the API
{: #backup-update-plan-api}

Make a `PATCH /backup_policies/{backup_policy_id}/plans/{plan_id}` request to update the backup plan by specifying the following properties:

* `attach_user_tags` specifies the user tags from the backup policy that are to be attached to backups created by the plan. Updating this value doesn't change the user tags for backups that were already created by this plan.

* `copy_user_tags` specifies whether source volume tags are to be copied to the backup snapshot. The value is a Boolean, it's either `true` or `false`.

* `deletion_trigger` has two properties. The `delete_after` property sets the number of days to keep a backup. The `delete_over_count` property sets the maximum number of recent backups to keep. When the maximum number is reached, the oldest backup is deleted to make space for a new one. Specify `null` to remove the existing maximum.

```sh
curl -X PATCH\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d/plans/78bb42ae-b82e-4560-861b-5bc4bca3fba6?version=2022-06-28&generation=2"\
  -H "Authorization: $iam_token"\
  -d '{
      "active": true,
      "attach_user_tags": ["my-daily-backup-plan"],
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

### Updating a backup policy plan for the fast restore option with the API
{: #baas-update-plan-fast-restore-api}

Make a `POST /backup_policies/{backup_policy_id}/plans/{plan_id}` request to update a backup plan. Specify the `clone_policy` property and `zones` subproperty to indicate which zone in your region you want to create the backup snapshot clone. The zone must be different than your source zone. You can specify multiple zones. The plan includes keeping a maximum of two cached backup snapshots in each zone.

This example updates a backup policy to create a backup plan. The new plan defines a clone policy to create snapshot clones in zone _us-south-3_. The plan specifies keeping a maximum of three cached backup snapshots.

```json
curl -X POST "$vpc_api_endpoint/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans?version=2022-12-09&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
        "active": true,
        "attach_user_tags": ["daily-backups"],
        "clone_policy": {
          "max_snapshots": 3,
          "zones": [
            {"name": "us-south-3"},
            {"href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3"}
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
  "attach_user_tags": ["daily-backups"],
  "clone_policy": {
    "max_snapshots": 3,
    "zones": [
      {"name": "us-south-2"},
      {"href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3"}
      ]
    },
  "copy_user_tags": false,
  "created_at": "2022-12-09T15:16:37Z",
  "cron_spec": "30 */2 * * 1-5",
  "deletion_trigger": {"delete_after": 5},
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans/6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "id": "6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "lifecycle_state": "stable",
  "name": "my-daily-plan-1",
  "resource_type": "backup_policy_plan"
}
```
{: codeblock}

### Updating a backup policy plan to include creation of a copy in another region with the API
{: #baas-update-plan-crc-api}

Make a `PATCH /backup_policies/{backup_policy_id}/plans/{plan_id}` request to update a backup plan. Specify the `remote_region_policies` property and `region` subproperty to indicate the region where you want to create the backup copy.

This example updates a backup policy to create a backup plan that creates backups in the `us-south` region and includes the creation of a remote copy. The following example specifies that the copies are to be created in the `us-east` region.

```json
curl -X PATCH "$vpc_api_endpoint/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans?version=2023-05-09&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
        "active": true,
        "attach_user_tags": ["daily-backups"],
        "copy_user_tags": true,
        "cron_spec": "30 */2 * * 1-5",
        "deletion_trigger": {
          "delete_after": 20,
          "delete_over_count": 20
          },
        "remote_region_policies": {
          "delete_over_count": 5,
          "encryption_key": [{"CRN": "crn:v1:bluemix:public:kms:us-south:a/a1234567:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd617"}],
          "region": [{"name":"us-east"}]
          },
        "name": "my-daily-plan-2"
      }'
```
{: codeblock}

A successful response shows that the remote region policy is created.

```json
{
  "active": true,
  "attach_user_tags": [
    "hourly-backups"
  ],
  "copy_user_tags": false,
  "created_at": "2023-05-09T15:16:37Z",
  "cron_spec": "0 */2 * * *",
  "deletion_trigger": {
    "delete_after": 5
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans/6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "id": "6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "lifecycle_state": "stable",
  "name": "my-hourly-plan-2",
  "remote_region_policies": {
    "delete_over_count": 5,
    "encryption_key": "crn:v1:bluemix:public:kms:us-south:a/a1234567:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd617" ,
    "region": [
      {"name": "us-east"},
      {"href": "https://us-east.iaas.cloud.ibm.com/v1/regions/us-east/zones/us-east-2"}
     ],
    },
  "resource_type": "backup_policy_plan"
}
```
{: codeblock}

## Managing backup policies and plans with Terraform
{: #backup-manage-policy-terraform}
{: terraform}

You can manage backup policies by using Terraform.

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

### Updating a backup policy with Terraform
{: #backup-rename-policy-terraform}

To update a backup policy, use the `ibm_is_backup_policy` resource. The following example identifies the name of the backup policy and the associated tags.

```terraform
resource "ibm_is_backup_policy" "example" {
  match_user_tags = ["dev:env","bck:test",]
  name            = "example-backup-policy"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_backup_policy){: external}.

### Updating a backup plan with Terraform
{: #backup-update-plan-terraform}

To update a backup policy, use the `ibm_is_backup_policy_plan` resource. The following example resource identifies the policy by ID, and you can change the plan's name or its schedule by updating the name or the cron_spec arguments.

```terraform
resource "ibm_is_backup_policy_plan" "example" {
  backup_policy_id = "r138-0521986d-963c-4c18-992d-d6a7a99d115f"
  cron_spec        = "10 20 * * *"
  name             = "example-backup-policy-plan"
}
```
{: codeblock}

The following example is a resource definition that you can use to update the backup plan with fast restore clones in two eu-de zones.

```terraform
resource "ibm_is_backup_policy_plan" "example" {
  backup_policy_id = "r138-0521986d-963c-4c18-992d-d6a7a99d115f"
  cron_spec        = "10 20 * * *"
  name             = "example-backup-policy-plan"
  clone_policy {
    zones          = ["eu-de-1", "eu-de-2"]
    max_snapshots  = 4
  }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy_plan](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_backup_policy_plan){: external}.

## Backup policy deletion overview
{: #backup-delete}

You can delete a plan or multiple plans within a policy. When you delete a backup policy, you also delete all the plans that are associated with it.

When you delete a backup policy, it no longer creates new backups. However, the existing backups that it created remain intact as their lifecycle is independent from the policy. Existing backups are retained until their expiration date.

### Deleting a backup plan in the UI
{: #backup-delete-plan}
{: ui}

1. Go to the [list of backup policies](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-list-all-policies) and locate the backup policy.

2. On the backup policy details page, select the plan that you want to delete. Expand the Actions menu for the plan, and select **delete**. You can have up to four backup plans per policy.

### Deleting a backup policy in the UI
{: #backup-delete-policy}
{: ui}

1. Go to the [list of backup policies](/docs/vpc?topic=vpc-backup-view-policies&interface=ui#backup-list-all-policies).

2. Locate the policy that you want to delete.

3. From the Actions menu, select **Delete**.

   Confirm that the volumes that are associated with the backup policy are no longer to be backed up, and that backups that were created by the policy continue to exist. You must go to the [list of snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-delete-snapshot-ui) and delete each one manually.

4. Enter _delete_ to confirm, the click **Delete policy**.

### Deleting a backup plan from the CLI
{: #backup-delete-plan-cli}
{: cli}

Run the `ibmcloud is backup-policy-plan-delete` command and specify the backup policy and the plan or plans that you want to delete. You can specify the backup policy and plans by ID or by name.

The following example shows the command syntax.

```sh
ibmcloud is backup-policy-plan-delete POLICY (PLAN1 PLAN2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

The following example deletes the backup plan `my-policy-plan-a` from the `my-new-policy-23` policy that is identified by its ID.

```sh
$ ibmcloud is backup-policy-plan-delete r138-0521986d-963c-4c18-992d-d6a7a99d115f my-policy-plan-a
This will delete plan my-policy-plan-a and cannot be undone. Continue [y/N] ?> y
Deleting plan my-policy-plan-a under account Test Account as user test.user@ibm.com...
OK
Deletion request for plan my-policy-plan-a has been accepted.
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-plan-delete`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-plan-delete){: external}.

### Deleting a backup policy from the CLI
{: #backup-delete-policy-cli}
{: cli}

Run the `backup-policy-delete` command and specify the policy ID or policy name for one or more backup policies. Backup plans that are associated with the policy are also deleted. For more information about backup policy deletion, see [Backup policy deletion overview](#backup-delete).

```sh
ibmcloud is backup-policy-delete (POLICY1 POLICY2 ...) [--output JSON] [-f, --force] [-q, --quiet]
```
{: pre}

The following example is a request to delete a backup policy by name.

```sh
$ ibmcloud is backup-policy-delete  my-backup-policy-v1
This will delete backup policy my-backup-policy-v1 and can't be undone. Continue [y/N] ?> y
Deleting backup policy my-backup-policy-v1 under account Test Account as user test.user@ibm.com...
OK
Deletion request for backup policy my-backup-policy-v1 has been accepted.
```
{: screen}

The following example is a request to delete a backup policy by ID and provides the response in JSON format.

```json
$ ibmcloud is backup-policy-delete r138-5c719085-cf26-456e-9216-984866659e29 --output JSON
[
    {
        "id": "r138-5c719085-cf26-456e-9216-984866659e29",
        "result": true,
        "Error": null
    }
]
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-delete`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-delete).

### Deleting a backup plan with the API
{: #backup-delete-plan-api}
{: api}

Make a `DELETE /backup_policies/{backup_policy_id}/plans/{plan_id}` request to delete a specific backup plan from a backup policy. Resources that were created by the plan remain but they are no longer subject to the plan's deletion trigger. During the deletion, the status shows `deleting`. When the deletion is complete (status = `deleted`), the backup plan can't be recovered.

See the following example.

```sh
curl -X DELETE\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d/plans/4cf9171a-0043-4434-8727-15b53dbc374c?version=2022-06-28&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

The response shows that the backup plan was deleted.

```json
{
  "active": true,
  "attach_user_tags": ["my-daily-backup-plan"],
  "copy_user_tags": true,
  "created_at": "2022-06-22T01:44:49.070Z",
  "cron_spec": "*/5 1,2,3 * * *",
  "deletion_trigger": {"delete_after": 20},
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/0fe9e5d8-0a4d-4818-96ec-e99708644a58/plans/4cf9171a-0043-4434-8727-15b53dbc374c",
  "id": "4cf9171a-0043-4434-8727-15b53dbc374c",
  "lifecycle_state": "deleted",
  "name": "my-policy-plan",
  "resource_type": "backup_policy_plan"
  }
```
{: screen}

### Deleting a backup policy and plans with the API
{: #backup-delete-policy-api}
{: api}

Make a `DELETE /backup_policies/{backup_policy_id}` request to delete a backup policy and all associated backup plans. During the deletion, the status shows `deleting`. When the deletion is complete (status = `deleted`), the backup policy and backup plans can't be recovered.

See the following example.

```sh
curl -X DELETE\
"$vpc_api_endpoint/v1/backup_policies/5063bfe5-c16f-4606-ba26-fba0f099b97d?version=2022-06-28&generation=2"\
  -H "Authorization: $iam_token"
```
{: codeblock}

The response indicates that all backup plans are deleted.

### Deleting a backup plan or a backup policy with Terraform
{: #backup-delete-plan-terraform}
{: terraform}

Use the `terraform destroy` command to conveniently delete a remote object such as a single backup plan or backup policy. The following example erases the `example-backup-policy-plan`.

```terraform
terraform destroy --target ibm_is_backup_policy_plan.example.id
```
{: codeblock}

The following example deletes the `example-backup-policy`.

```terraform
terraform destroy --target ibm_is_backup_policy.example.id
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

## Next steps
{: #backup-next-steps-manage}

You can do the following actions.

* [Apply tags to your resources for backups.](/docs/vpc?topic=vpc-backup-use-policies)
* [Create more backup policies.](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=ui)
* [Restore a volume from a backup snapshot.](/docs/vpc?topic=vpc-baas-vpc-restore)
