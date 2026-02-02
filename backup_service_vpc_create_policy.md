---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-02"

keywords: Backup, backup snapshot, create backups, backup service, backup plan, backup policy, restore, restore volume, restore data, restore share

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating backup policies and plans
{: #create-backup-policy-and-plan}

You can create backup policies for your {{site.data.keyword.block_storage_is_short}} volumes and {{site.data.keyword.filestorage_vpc_short}} shares in the console, from the CLI, with the API, or Terraform. You can create up to four backup plans to schedule backup creation and retention. Specify user tags in the policy to make sure that your data is backed up regularly. Create backups on schedule when the tags that are applied to a resource match the tags in a backup policy.
{: shortdesc}

## Before you begin
{: #backup-prereqs}

1. Establish IAM user roles to grant [service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth#backup-s2s-auth-overview) so the backup service can detect tags on resources and create backups.

2. Create user tags for new or existing resources (storage volumes, shares, or virtual server instances) that you can associate with a backup policy. For more information about adding tags, see [Apply tags to resources for backup policies](/docs/vpc?topic=vpc-backup-use-policies). For more information about creating tags, see [Working with tags](/docs/account?topic=account-tag).

You're not required to create a backup plan when you create a backup policy, but it's good practice to create at least one backup plan with your policy.
{: tip}

## Creating a backup policy and plan in the console
{: #backup-policy-create-ui}
{: ui}

You can use the UI to create a backup policy and plan.

### Creating a backup policy in the console
{: #backup-provisioning-page-ui}

Use the following steps to create a backup policy by using the UI.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Backup policies**. The **Create** tab is selected by default.

   The UI displays a notification message when service-to-service authorizations are incorrect or missing on your account.

   If your account doesn't allow for the required service-to-service authorizations and user access roles to create a backup policy, [contact IBM support](/docs/vpc?topic=vpc-getting-help-and-support-for-vpc&interface=ui) for help.
   {: note}

1. Enter information for each section of the provisioning form. Optionally create a backup plan for the policy.

   | Field        | Description |
   |--------------|-------------|
   | **Location** | Select the location where you want to create the backup policy. |
   | - Geography  | Select the appropriate value from the list of available geographies. |
   | - Region     | Select the appropriate value from the list of available regions in the selected geography. |
   | **Details**  | Enter details to define the policy. A policy defines what volumes are backed up. |
   | - Name       | Provide a unique name for your backup policy that easily identifies the policy. Standard naming conventions apply to policies, plans, and backups. For example, see the [naming conventions for Snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-naming). |
   | - Resource group | Optionally, specify a resource group for your policy. It can't be changed after you enter it. \n For more information, see [best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup). |
   | - Resource tags  | Optional tags to help you group and manage your backup policy. Consider writing tags as key:value pairs. For more information, see [Working with tags](/docs/account?topic=account-tag&interface=ui).|
   | **Target resource type**  | Choose between Individual block volumes, or multiple volumes that are attached to the same virtual server instance, or File shares. \n When you choose to create a policy to back up volumes that are attached to the same virtual server instance you can choose to include the boot volume, too. |
   | **Tags for target resources** | Specify the user tags to apply to your target resources (volumes, virtual server instances, or shares) in your selected region. If multiple resources use the same tag, backups are created for all tagged resources. If a resource has multiple tags, it needs to match only one tag that is associated with the backup policy. After the backup policy is created, existing resources with any of the tags for target resources are automatically associated. |
   | **Scope**    | This option is applicable only to Enterprise accounts. As an Enterprise account administrator, you can specify whether the backup policy applies to the Enterprise account alone or the Enterprise account and all of its subaccounts. Check the box to enable the policy for all accounts of the Enterprise. |
   | **Plan**     | Click **Create** to create backup plan for this policy. In the side panel, specify the plan details. When finished, click **Create**. The page refreshes with a summary of the plan details. You can create up to four backup plans. All apply to the volumes with tags that match the backup policy. \n For more information about options, see the next section. |
   {: caption="Backup policy provisioning selections" caption-side="bottom"}

1. Click **Create backup policy**. The order summary side panel shows the backup policy and all plans that were created for it.

If you're not ready to order yet or just looking for pricing information, you can add the information that you see in the side panel to an Estimate. For more information, see [Estimating your costs](/docs/account?topic=account-cost).
{: tip}

### Creating a backup plan in the console
{: #backup-plan-ui}

You can schedule backups in your plan on a daily, weekly, or monthly basis by using predefined settings, or by way of a `cron-spec` expression. The following steps describe the Create backup plan side panel.

1. In the Create plane side panel, the plan status toggle is set to "enabled", by default.

1. Enter a name for the plan (for example, _daily-dallas-vol1_). The plan name must be unique within the policy.

1. Specify the **Frequency**. Select one of the following options from the list.
   * Daily
      * For a Daily plan, enter the **starting time (UTC)** in hours and minutes, Coordinated Universal Time. For example, 12 noon is 12:00. Local time conversion is automatically provided on the screen, for example, 12 PM Central Daylight Time.
   * Weekly
      * For a Weekly plan, select the days of the week that you want backups to run. For example, you can select Monday, Wednesday, and Friday. Specify the starting time in the same manner as a Daily plan.
   * Monthly
      * For a Monthly plan, select the day of the month that you want backups to run. For example, "1" schedules a backup every first of the month. Specify the starting time in the same manner as a Daily plan.
   * Specify by using cron expression
      * Under **Cron expression (UTC)**, enter the backup creation frequency in `cron-spec` format: minute, hour, day, month, and weekday. For example, to create a backup every day at 5:30 PM, you need to enter `30 17 * * *`.

   The **Backup destination** shows the target resource's region. The **Backup resource group** is the resource group that is associated with the target resource.
   {: note}

1. Specify a **Retention type** for the backups. You can specify how long to keep them by the number of days and the total number to retain.

   * For **Age**, specify the number of days that you want to retain the backups in the range of 1 to 1000.
   * For **Count**, provide the number of backups that you want to keep.

   To keep costs down, set a retention period or snapshots count adequate to your needs. For example, setting "7" for **Age** retains a week's worth of backups.

1. Under **Optional**, you can configure two options for individual volume backups. When you're creating a plan for a policy that is for multi-volume backups or share backups, fast snapshot restore is not available.

   * **Fast snapshot restore** - When you enable this feature, you must specify the zone or zones where you want [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-about#snapshots_vpc_fast_restore) enabled. You can also specify the maximum number of fast restore snapshots that you want to retain. The fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots.

   * **Tagging**, specify more tags that apply to the backup when the plan runs.
      * Select the box to copy all tags from the source resource to all backups.
      * Under **Tags for backups**, you can manually add any extra plan tags in this field. This option is not available for File share backups.

1. If you're creating a backup plan for multi-volume backups or share backups, you can click **Create** and return to the backup policy page. If you're creating a backup plan for individual volumes, you can click **Next** to proceed to configure remote copies, which are an optional part of the plan.
   1. To create cross-regional copies of your backup, select the geography and regions where you want to have a copy. Remember, you can have only one copy per region.
   1. Click the toggle to enable remote copy in the selected region.
   1. If the source snapshot is encrypted by using a customer-managed key, you must select the encryption service instance and provide the key name. If you prefer, you can create a service instance or encryption key by following the links.
      * {{site.data.keyword.keymanagementserviceshort}} - it can be used when the original volume is encrypted by using the {{site.data.keyword.keymanagementserviceshort}} service.
      * {{site.data.keyword.hscrypto}} - it can be used when the original volume is encrypted by using the {{site.data.keyword.hscrypto}} service.
   1. Click **Apply changes** to save the new plan. The list of plans is updated in the policy details page.

1. If you want to make any changes, click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") for that plan. If you want to delete the plan, click the delete icon.

### Estimating your expected usage and costs
{: #backup-cost-estimator-ui}

Use the cost estimator to see what your backups might cost based on the rate of expected change in your {{site.data.keyword.block_storage_is_short}} volumes.

1. After you create your backup policy and plan, on the side panel of the Backup summary, click **Add to estimate**.

2. On the Estimate side panel, enter your expected usage to the initial costs. The backup policy is without charge. You pay for the amount of backup storage that is used. Provide the following estimates:

   * Number of volumes or shares that you want to associate with the backup policy.
   * Average amount of data per volume or share (in GBs). For example, you might associate two volumes with a policy. The first volume has 4 GB of data and the second 20 GB. An average of the two would be 12 GB.
   * Number of backups per volume or share per month. You can take a maximum of 750 backup snapshots per volume and a maximum of 750 backups per share.
   * Percentage of incremental change after the initial backup. For example, a 15% increase in size for each subsequent backup.

3. When you're finished, click **Calculate cost**.

The cost estimate summary shows how the costs are calculated and breaks down the storage costs, providing a monthly estimate. Click **Save** to see the details on the cost estimator panel.

## Creating backup policies and plans from the CLI
{: #backup-policy-create-cli}
{: cli}

You can create a backup policy from the command-line interface (CLI). The following examples show how to create a policy with no plan, a policy and a plan together, and a separate backup plan for an existing policy. They also show how to create multiple plans for an existing policy and how to create a plan with fast restore for backups.

For more information about available command options, see [`ibmcloud is backup-policy-create`](/docs/cli?topic=cli-vpc-reference#backup-policy-create).

### Before you begin
{: #before-creating-backup-cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to {{site.data.keyword.cloud}}.

   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

1. If you are an Enterprise account administrator who wants to create a backup policy and plan for your enterprise account and subaccounts, you need to fetch your enterprise account CRN. Run the following command to see the enterprise account name, ID, and CRN.

   ```sh
   ibmcloud enterprise show
   ```
   {: pre}

### Creating a backup policy for individual volumes from the CLI for an account
{: #backup-create-policy-noplan}

Run the `ibmcloud is backup-policy-create` command to create a backup policy without a backup plan. Use the options `--match-tags` and `--name` to give your policy a name and identify the tag that you want to use for your target resources. After the policy is created, you can [add backup plans](#backup-create-plan-cli) to it later on.

```sh
ibmcloud is backup-policy-create --match-tags dev:test --name my-backup-policy-v1
```
{: pre}

```sh
Creating backup policy my-backup-policy-v1 under account Test Account as user test.user@ibm.com...

ID                     r006-d6052504-516f-4923-938b-9e9def977428
Name                   my-backup-policy-v1
CRN                    crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-d123456
Status                 pending
Plans                  ID   Name   Resource type

Backup tags            dev:test
Match resource type    volume
Resource group         ID                                 Name
                       11caaa983d9c4beb82690daab08717e9   Default

Scope                  ID                                 Resource type
                       efe5afc483594adaa8325e2b4d1290df   account

Health State           ok
Created at             2023-12-05T19:27:28+05:30
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-create`](/docs/cli?topic=cli-vpc-reference#backup-policy-create).

### Creating a backup policy for individual volumes from the CLI for an enterprise
{: #backup-create-policy-noplan-enterprise}

Run the `ibmcloud is backup-policy-create` command to create a backup policy without a backup plan. Specify the enterprise CRN to create a policy for the enterprise account and its subaccount. After the policy is created, you can [add backup plans](#backup-create-plan-cli) to it later on.

```sh
ibmcloud is backup-policy-create --match-tags dev:test --name backup-enterprise-scope --scope  crn:v1:bluemix:public:enterprise::a/a1234567::enterprise:7e44cb4667ba4b88b1b1f8dcc15e33b3
```
{: pre}

```sh
Creating backup policy backup-scope-1 under account Enterprise Test as user test.user@ibm.com...

ID                     r006-a1b46efe-12bd-403a-9f09-bede1ad3766f
Name                   backup-enterprise-scope
CRN                    crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-a1b46efe-12bd-403a-9f09-bede1ad3766f
Status                 pending
Plans                  ID   Name   Resource type

Backup tags            dev:test
Match resource type    volume
Resource group         ID                                 Name
                       e579217258f74f42974e6ec4da287fc5   Default

Scope                  ID                                 CRN                                                                                         Resource type
                       7e44cb4667ba4b88b1b1f8dcc15e33b3   crn:v1:bluemix:public:enterprise::a/a1234567::enterprise:7e44cb4667ba4b88b1b1f8dcc15e33b3   -

Health State           ok
Created at             2023-09-26T13:34:56+05:30

```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-create`](/docs/cli?topic=cli-vpc-reference#backup-policy-create).

### Creating a backup policy for consistency group from the CLI for an account
{: #backup-create-policy-noplan-instance}

Run the `ibmcloud is backup-policy-create` command to create a backup policy without a backup plan. Use the options `--match-resource-type`, `--included-content`,`--match-tags`, and `--name` to give your policy a name and identify the tag that you want to use for your target resources. After the policy is created, you can [add backup plans](#backup-create-plan-cli) to it later on.

The following example creates a backup policy for the boot and data volumes of tagged instances.

```sh
ibmcloud is backup-policy-create --match-tags dev:test --name my-cr-backup-policy-v1 --match-resource-type instance --included-content data_volumes,boot_volume
```
{: pre}

```sh
Creating backup policy my-cr-backup-policy-v1 under account Test Account as user test.user@ibm.com...

ID                    r006-e0713176-37b6-4168-88ab-ad92f8a544f9
Name                  my-cr-backup-policy-v1
CRN                   crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-e0713176-37b6-4168-88ab-ad92f8a544f9
Status                pending
Plans                 ID   Name   Resource type

Backup tags           dev:test
Match resource type   instance
Included Content      data_volumes,boot_volume
Resource group        ID                                 Name
                      11caaa983d9c4beb82690daab08717e9   Default

Scope                 ID                                 Resource type
                      a1234567                           account

Health State          ok
Created at            2023-12-05T19:29:35+05:30
```
{: screen}

The following example creates a backup policy for the data volumes of the tagged instances.

```sh
ibmcloud is backup-policy-create --match-tags dev:test --name my-cr-backup-policy-v2 --match-resource-type instance --included-content data_volumes
```
{: pre}

```sh
Creating backup policy my-cr-backup-policy-v2 under account Test Account as user test.user@ibm.com...

ID                    r006-e773722f-d61e-487a-ac88-b1800395aa92
Name                  my-cr-backup-policy-v2
CRN                   crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-e773722f-d61e-487a-ac88-b1800395aa92
Status                pending
Plans                 ID   Name   Resource type

Backup tags           dev:test
Match resource type   instance
Included Content      data_volumes
Resource group        ID                                 Name
                      11caaa983d9c4beb82690daab08717e9   Default

Scope                 ID                                 Resource type
                      a1234567                           account

Health State          ok
Created at            2023-10-20T19:31:16+05:30
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-create`](/docs/cli?topic=cli-vpc-reference#backup-policy-create).

### Creating a backup policy for file shares from the CLI
{: #backup-create-policy-noplan-share}

Run the `ibmcloud is backup-policy-create` command to create a backup policy without a backup plan. Use the options `--match-resource-type`, `--match-tags`, and `--name` to give your policy a name and identify the tag that you want to use for your target resources. After the policy is created, you can [add backup plans](#backup-create-plan-cli) to it later on.

The following example creates a backup policy for file shares of tagged instances.

```sh
ibmcloud is backup-policy-create --match-tags dev:test --name my-share-backup-policy --match-resource-type share
```
{: pre}

```sh
Creating backup policy my-cr-backup-policy-v1 under account Test Account as user test.user@ibm.com...

ID                    r006-e0713176-37b6-4168-88ab-ad92f8a544f9
Name                  my-share-backup-policy
CRN                   crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-e0713176-37b6-4168-88ab-ad92f8a544f9
Status                pending
Plans                 ID   Name   Resource type

Backup tags           dev:test
Match resource type   share
Included Content
Resource group        ID                                 Name
                      11caaa983d9c4beb82690daab08717e9   Default

Scope                 ID                                 Resource type
                      a1234567                           account

Health State          ok
Created at            2023-12-05T19:29:35+05:30
```
{: screen}

### Creating a backup policy with a plan from the CLI for an account
{: #backup-create-policy-cron-cli}

Run the `backup-policy-create` command to create a backup policy and a backup plan in the same command.

The following example uses the `--match_tags` option to match tags to volumes with the user tag `dev:test` and specifies the frequency of the backup plan as a `cron-spec` expression. The `--plan-attach-user-tags` option indicates that the backup plan's user tags are to be attached to the backup snapshot. Setting the `--plan-copy-user-tags` option to false indicates that the source volume's user tags are not copied to the backup. The `--plan-delete-after` option indicates the maximum number of days that the backups are to be kept and the `--plan-delete-over-count` option defines the maximum number of recent backups to keep. The `-plan-clone-policy-zones` option specifies that after the backup snapshot is created and stored in a separate regional storage repository, a full copy of the backup is stored in the `us-south-1` region of the availability zone. The `--plan-clone-policy-max-snapshots` option changes the number of cached backups that are stored in the AZ to 4. The default value is 5.

```sh
ibmcloud is backup-policy-create --match-tags dev:test --name my-backup-policy-v2 --plan-name my-plan-b  --plan-attach-tags bkp:test --plan-copy-tags false --plan-delete-after 60 --plan-cron-spec '45 09 * * *' --plan-active --plan-clone-policy-max-snapshots 4 --plan-clone-policy-zones us-south-1,us-south-2 --plan-delete-over-count 2
```
{: pre}

```sh
Creating backup policy my-backup-policy-v2 under Test Account as user test.user@ibm.com...

ID                     r006-0723c648-9a47-4d51-b1ba-349e21e715b6
Name                   my-backup-policy-v2
CRN                    crn:v1:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-0723c648-9a47-4d51-b1ba-349e21e715b6
Status                 pending
Plans                  ID                                          Name        Resource type
                       r006-e888bb31-7bf2-4885-a9f3-d448c1c37326   my-plan-b   backup_policy_plan

Backup tags            dev:test
Match resource type    volume
Resource group         ID                                 Name
                       6edefe513d934fdd872e78ee6a8e73ef   defaults
Scope                  ID                                 Resource type
                       a1234567                           account

Health State           ok
Created at             2023-12-05T19:27:28+05:30
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-create`](/docs/cli?topic=cli-vpc-reference#backup-policy-create).

### Creating a backup policy with a plan from the CLI for an enterprise
{: #backup-create-policy-cron-cli-enterprise}

Run the `backup-policy-create` command to create a backup policy and a backup plan in the same command. Specify the enterprise CRN in the `scope` to create a policy for the enterprise account and its subaccount.

```sh
ibmcloud is backup-policy-create --match-tags dev:test --name backup-scope-2 --plan-name scope-plan-2 --plan-attach-tags dev:test --plan-copy-tags false --plan-delete-after 60 --plan-cron-spec '45 09 * * *' --plan-active  --plan-delete-over-count 2 --scope  crn:v1:bluemix:public:enterprise::a1234567::enterprise:7e44cb4667ba4b88b1b1f8dcc15e33b3
```
{: pre}

```sh
Creating backup policy backup-scope-2 under account Enterprise Test as user test.user@ibm.com...

ID                     r006-0bc533ed-4796-407a-982e-693b418f3de3
Name                   backup-scope-2
CRN                    crn:bluemix:public:is:us-south:a/a1234567::backup-policy:r006-0bc533ed-4796-407a-982e-693b418f3de3
Status                 pending
Plans                  ID                                          Name           Resource type
                       r006-0741b600-e8d5-41b4-88a7-c19b6fbf89ca   scope-plan-2   backup_policy_plan

Backup tags            dev:test
Match resource type    volume
Resource group         ID                                 Name
                       e579217258f74f42974e6ec4da287fc5   Default

Scope                  ID         CRN                                                                 Resource type
                       e7654321   crn:v1:bluemix:public:enterprise::a/a1234567::enterprise:e7654321   -

Health State           ok
Created at             2023-08-30T13:39:10+05:30
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-create`](/docs/cli?topic=cli-vpc-reference#backup-policy-create).

### Creating a backup policy with multiple plans from the CLI
{: #backup-create-policy-multiplan-cli}

Run the `ibmcloud is backup-policy-create` command and define multiple plans in a JSON format. This example creates two plans, `my-policy-plan-a` and `my-policy-plan-b`.

```json
$ ibmcloud is backup-policy-create --match-tags dev:test --name backup-policy-v1  --plans '[{
   "active": true,
   "attach_user_tags": ["daily-backup-plan"],
   "copy_user_tags": true,
   "cron_spec": "05 15 * * *",
   "clone_policy": {
     "max_snapshots": 4,
      "zones": [
        {"name": "eu-de-1"},
        {"name": "eu-de-2"}
       ]
     },
   "deletion_trigger": {
     "delete_after": 20,
     "delete_over_count": 20
   },
   "name": "my-policy-plan-a"
 },{
   "active": true,
   "attach_user_tags": ["daily-backup-plan"],
   "copy_user_tags": true,
   "cron_spec": "10 20 * * *",
    "clone_policy": {
     "max_snapshots": 3,
      "zones": [
        {"name": "eu-de-1"},
        {"name": "eu-de-2"}
       ]
     },
   "deletion_trigger": {
     "delete_after": 20,
     "delete_over_count": 20
   },
   "name": "my-policy-plan-c"
 }]'
```
{: codeblock}

The result shows that two plans were created.

```sh
Creating backup policy backup-policy-v1 under account Test Account as user test.user@ibm.com...

ID                     r138-0521986d-963c-4c18-992d-d6a7a99d115f
Name                   backup-policy-v1
CRN                    crn:v1:bluemix:public:is:eu-de:a/a1234567::backup-policy:r138-0521986d-963c-4c18-992d-d6a7a99d115f
Status                 pending
Plans                  ID                                          Name               Resource type
                       r138-2129a79a-5629-4069-bf79-7bb0af3b0bd3   my-policy-plan-a   backup_policy_plan
                       r138-6f4f08ba-e0bb-470f-bbfb-f3a22aebbfa9   my-policy-plan-c   backup_policy_plan

Backup tags            dev:test
Match resource type    volume
Resource group         defaults
Created at             2023-02-21T22:42:10+00:00
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-create`](/docs/cli?topic=cli-vpc-reference#backup-policy-create).

### Creating a backup plan from the CLI
{: #backup-create-plan-cli}

Run the `backup-policy-plan-create` command to create a backup plan and attach it to an existing policy. Identify the policy by ID or name.

Syntax:

```sh
ibmcloud is backup-policy-plan-create POLICY --cron-spec CRON_SPEC [--name NAME] [--active] [--attach-tags ATTACH_TAGS] [--copy-tags true | false] [[--delete-after DELETE_AFTER] [--delete-over-count DELETE_OVER_COUNT]] [[--clone-policy-zones  ZONE1,ZONE2,...] [--clone-policy-max-snapshots CLONE_POLICY_MAX_SNAPSHOTS]] [--remote-region-policies REMOTE_REGION_POLICY_JSON | @REMOTE_REGION_POLICY_JSON] [--output JSON] [-q, --quiet]
```
{: pre}

The following example creates a backup plan for an existing policy, which is identified by name `my-backup-policy-v1`. It attaches backup policy tags to the backup snapshots that are created by the new plan that is called `not-just-another-plan`. The backup job runs at 01:05 every morning, and copies source volume tags to the backup snapshot. Fast restore clones are not enabled and the oldest backup is deleted after 80 backup snapshots are taken.

```sh
ibmcloud is backup-policy-plan-create my-backup-policy-v1  --attach-tags dev:test --copy-tags true --cron-spec '05 01 * * *' --delete-after 80 --name not-just-another-plan
```
{: pre}

```sh
Creating plan not-just-another-plan of backup policy my-backup-policy-v1 under account Test Account as user test.user@ibm.com...

ID                   r138-4d77d84c-929c-49e9-9f05-952be9486406
Name                 not-just-another-plan
Active               true
Lifecycle state      pending
Clone policy         Max snapshots   Zones
                     0

Deletion trigger     Delete after   Delete over count
                     80             -

Attached tags        dev:test
Copy tags            true
Cron specification   05 01 * * *
Created at           2023-02-21T22:19:22+00:00
Resource type        backup_policy_plan
```
{: codeblock}

### Creating a backup plan with fast restore option from the CLI
{: #backup-create-plan-with-fr-cli}

To create a backup plan for an existing policy, use the `ibmcloud is backup-policy-plan-create` command and specify the policy ID. Then, use the following options to flesh out the plan.

* `--cron-spec` followed by the cron expression that defines when the backup job is to run.
* `--active` to indicate that the plan is active.
* `--name` followed by the name that you chose for the new plan.
* `--attach-tags` followed by the list of tags that you want to attach to your backup snapshots.
* `--copy-tags` followed by true or false to indicate whether the backup snapshots are to inherit the tags from the parent volume.
* `--delete-after` followed by the number of days that you want to keep the backups for.
* `--delete-over` followed by the maximum number of backups that you want to keep of the volume.
* `--clone-policy-max-snapshots` following by the number of clones you want to keep.
* `--clone-policy-zones` followed by the list of zones, where you want to keep a copy of the backup snapshot.

The following example creates a backup plan for an existing policy that includes the fast restore option in two zones within the eu-de region. The plan includes keeping a maximum of two cached backups in each zone. The backup job starts creating backup snapshots at 7:15 PM every day.

```sh
ibmcloud is backup-policy-plan-create r138-8c494618-9e4f-4b67-9a08-ee3491404f3b  --cron-spec '15 19 * * *' --active --name my-policy-plan --attach-tags my-daily-backup-plan --copy-tags true --delete-after 10 --delete-over-count 2 --clone-policy-max-snapshots 2 --clone-policy-zones eu-de-1,eu-de-3
```
{: pre}

```sh
Creating plan my-policy-plan of backup policy r138-8c494618-9e4f-4b67-9a08-ee3491404f3b under account Test Account as user ibm.user@ibm.com...

ID                   r138-7734be40-e2a5-4ee6-b4bd-75763639092b
Name                 my-policy-plan
Active               true
Lifecycle state      pending
Clone policy         Max snapshots   Zones
                     2               eu-de-1,eu-de-3

Deletion trigger     Delete after   Delete over count
                     10             2

Attached tags        my-daily-backup-plan
Copy tags            true
Cron specification   15 19 * * *
Created at           2023-02-21T19:21:28+00:00
Resource type        backup_policy_plan
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-plan-create`](/docs/cli?topic=cli-vpc-reference#backup-policy-plan-create).

The fast restore feature is billed at an extra rate per hour for each zone in which it is enabled. Maintaining fast restore clones is considerably more costly than keeping regular backup snapshots.
{: note}

The fast restore feature is not available for multi-volume or share backups.
{: important}

### Creating a backup plan with cross-regional copy option from the CLI
{: #backup-create-plan-with-crc-cli}

To create a backup plan that also saves a copy of the backup snapshot in another region, run the `ibmcloud is backup-policy-plan-create` command with the `--remote-region-policies` option.

If the source snapshot is not encrypted with a customer key, the encryption of the copy remains provider-managed. If the source snapshot is protected by a customer-managed key, you must specify the customer-managed key that you want to use to encrypt the new copy with the `--encryption-key` option. See the following example.

```sh
ibmcloud is backup-policy-plan-create my-backup-policy-v1 --cron-spec '0 0 * * *' --name my-crc-plan1 --remote-region-policies '[
  {
    "delete_over_count": 10,
    "region": {"name": "us-east"}
  }
]'
```
{: pre}

```sh
Creating plan my-crc-plan1 of backup policy my-backup-policy-v1 under account Test Account as user test.user@ibm.com...
ID                       r006-f0d881c9-213e-471b-bba7-999ee2eee3ff
Name                     my-crc-plan1
Active                   true
Lifecycle state          pending
Clone policy             Max snapshots   Zones
                         0

Deletion trigger         Delete after   Delete over count
                         30             -

Remote Region Policies   Region    Encryption Key   Delete over count
                         us-east   -                10

Attached tags            -
Copy tags                true
Cron specification       0 0 * * *
Created at               2023-05-05T15:27:01+05:30
Resource type            backup_policy_plan
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-plan-create`](/docs/cli?topic=cli-vpc-reference#backup-policy-plan-create).

The cross-regional copy feature is not applicable for multi-volume or share backups.
{: important}

## Creating backup policies and plans with the API
{: #backup-policy-create-policy-plan-api}
{: api}

You can programmatically create a backup policy by calling the `/backup_policies` method in the [VPC API](/apidocs/vpc/latest#create-backup-policy){: external} as shown in the following sample requests. A `POST /backup_policies` request creates a backup policy with tags that you provide to identify {{site.data.keyword.block_storage_is_short}} volume resources that are to be backed up. The backup policy accepts a backup plan, where you define backup schedules and deletion rules.

If you are an Enterprise account administrator who wants to create a backup policy and plan for your enterprise account and child accounts, you need to fetch your enterprise account CRN. Make an API request to the [Enterprise Management API](/apidocs/enterprise-apis/enterprise#list-enterprises) like the following example.

```sh
curl -X GET "https://enterprise.cloud.ibm.com/v1/accounts/$ACCOUNT_ID" -H "Authorization: Bearer <IAM_Token>" -H 'Content-Type: application/json'
```
{: pre}

In the response, look for the "parent" CRN. This CRN contains the enterprise ID and the account ID.
```json
{
	"url": "/v1/accounts/ea123456",
	"id": "ea1ef237216e4f9e8e6f76eccec761f8",
	"parent": "crn:v1:bluemix:public:enterprise::a/ea123456::enterprise:b0398194",
	"enterprise_account_id": "ea123456",
	"enterprise_id": "b0398194",
	"enterprise_path": "enterprise:b0398194",
	"name": "Enterprise for VPC",
	"state": "ACTIVE",
	"paid": true,
	"owner_iam_id": "IBM_ID",
	"owner_email": "test.user@ibm.com",
	"created_at": "2022-08-03T16:26:59.175Z",
	"created_by": "iam-ServiceId-3a65c352-34b4-40ca-9360-93a0045fa76a",
	"updated_at": "2023-03-30T23:25:55.556Z",
	"updated_by": "iam-ServiceId-3b123876-4cf2-4565-ba37-2f0a0a8faab1",
	"crn": "crn:v1:bluemix:public:enterprise::a/ea123456:account:ea123456",
	"is_enterprise_account": true
}
```
{: codeblock}

### Creating a backup policy and plan for individual volumes with the API for an account
{: #backup-policy-create-api}

Make a `POST /backup_policies` request to create a backup policy. The value of `match_resource_type` is `volume`. The `match_user_tags` property identifies the backup tags on the {{site.data.keyword.block_storage_is_short}} volume resource and associates it with this plan. In this example, the frequency of the backup plan is defined with a `cron_spec`.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies?version=2022-04-19&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
      "match_resource_type": "volume",
      "match_user_tags": "my-daily-backup-policy",
      "name": "my-backup-policy",
      "plans": [
       {
         "attach_user_tags": "my-daily-backup-plan",
         "copy_user_tags": true,
         "cron_spec": "*/5 1,2,3 * * *",
         "deletion_trigger": {"delete_after": 20},
         "name": "my-backup-plan"
       }
     ],
     "resource_group": {
       "crn": "crn:v1:bluemix:public:resource-controller::a/123456::resource-group:678523bcbe2b4eada913d32640909956",
       "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
       "id": "678523bcbe2b4eada913d32640909956",
       "name": "Default"
     }
   }'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "created_at": "2022-04-19T18:10:58.060Z",
  "crn": "crn:v1:bluemix:public:is:us-south:a/123456::backup-policy:eca6556f-f67d-4a3e-8428-3db8819fc60c",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/eca6556f-f67d-4a3e-8428-3db8819fc60c",
  "id": "eca6556f-f67d-4a3e-8428-3db8819fc60c",
  "lifecycle_state": "stable",
  "match_resource_type": "volume",
  "match_user_tags": "my-daily-backup-policy",
  "name": "my-backup-policy",
  "plans": [
    {
      "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/eca6556f-f67d-4a3e-8428-3db8819fc60c/plans/f0b3740b-83b6-4426-af99-140b90ad6f33",
      "id": "f0b3740b-83b6-4426-af99-140b90ad6f33",
      "name": "my-policy-plan",
      "resource_type": "backup_policy_plan"
    }
  ],
  "resource_group": {
    "crn": "crn:v1:bluemix:public:resource-controller::a/123456::resource-group:fee82deba12e4c0fb69c3b09d1f12345",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/fee82deba12e4c0fb69c3b09d1f12345",
    "id": "fee82deba12e4c0fb69c3b09d1f12345",
    "name": "my-resource-group"
  },
  "resource_type": "backup_policy"
}
```
{: codeblock}

### Creating a backup policy and plan for individual volumes with the API for an enterprise
{: #backup-policy-create-api-enterprise}

Make a `POST /backup_policies` request to create a backup policy. The value of `match_resource_type` is `volume`. The `match_user_tags` property identifies the backup tags on the {{site.data.keyword.block_storage_is_short}} volume resource and associates it with this plan. In this example, the frequency of the backup plan is defined with a `cron_spec`. In the `scope`, specify the enterprise account CRN.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies?version=2023-08-12&generation=2"
-H "accept: application/json" -H "Content-Type: application/json" -H "Authorization: $iam_token"\
-d "{
  {
      "match_resource_type": "volume",
      "match_user_tags": "my-daily-backup-policy",
      "name": "my-backup-policy",
      "plans": [
       {
         "attach_user_tags": ["my-daily-backup-plan"],
         "copy_user_tags": true,
         "cron_spec": "*/5 1,2,3 * * *",
         "deletion_trigger": {"delete_after": 20},
         "name": "my-backup-plan"
       },
       {
         "attach_user_tags": "my-daily-backup-plan",
         "copy_user_tags": true,
         "cron_spec": "*/5 1,2,3 * * *",
         "deletion_trigger": {"delete_after": 20},
         "name": "my-backup-plan"
       }
     ],
     "resource_group": {
       "crn": "crn:v1:bluemix:public:resource-controller::a/123456::resource-group:678523bcbe2b4eada913d32640909956",
       "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
       "id": "678523bcbe2b4eada913d32640909956",
       "name": "Default"
     },
     "scope": {"crn":"crn:v1:bluemix:public:enterprise::a/ea123456::enterprise:b0398194"}
  }'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "created_at": "2022-04-21T15:06:03.000Z",
  "crn": "crn:v1:bluemix:public:is:us-south:a/123456::backup-policy:r006-076191ba-49c2-4763-94fd-c70de73ee2e6",
  "health_reasons": [],
  "health_state": "ok",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-076191ba-49c2-4763-94fd-c70de73ee2e6",
  "id": "r006-076191ba-49c2-4763-94fd-c70de73ee2e6",
  "included_content": "data_volume",
  "lifecycle_state": "pending",
  "match_resource_type": "volume",
  "match_user_tags": ["my-tag-1", "my-tag-2"],
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
    "resource_type": "account",
    "scope": {"crn":"crn:v1:bluemix:public:enterprise::a/ea123456::enterprise:b0398194"}
  }
}
```
{: codeblock}

### Creating a backup policy and plan for a consistency group of Block Storage volumes
{: #backup-policy-create-api-instance}

Make a `POST /backup_policies` request to create a backup policy. The value of `match_resource_type` is `instance`. To create backups of only the data volumes, specify the `included_content` option as `data volumes`. If you want to include the boot volume in the backup operation, specify `boot-volume` as part of the `included_content` option as well. The `match_user_tags` property identifies the backup tags on the virtual server instance resources and associates the attached {{site.data.keyword.block_storage_is_short}} volumes with this policy and plan. In this example, the frequency of the backup plan is defined with a `cron_spec`, and only the data volumes are included in the backup.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies?version=2023-12-05&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
      "match_resource_type": "instance",
      "included_content": "data_volumes",
      "match_user_tags": "my-daily-backup-policy",
      "name": "my-backup-policy-for-consistency-group",
      "plans": [
       {
         "name": "my-backup-plan-for-cg",
         "attach_user_tags": ["my-daily-backup-plan"],
         "copy_user_tags": true,
         "cron_spec": "*/5 1,2,3 * * *",
         "deletion_trigger": {"delete_after": 20}
       }
      ],
      "resource_group": {
        "crn": "crn:v1:bluemix:public:resource-controller::a/a1234567::resource-group:678523bcbe2b4eada913d32640909956",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
        "id": "678523bcbe2b4eada913d32640909956",
        "name": "Default"
      },
     }'
```
{: codeblock}

A successful response looks like the following example:

```json
{
  "included_content": ["data_volumes"],
  "match_resource_type": ["instance"],
  "match_user_tags": ["my-daily-backup-policy"],
  "name": "my-backup-policy-for-consistency-group",
  "plans": [
    {
      "active": true,
      "attach_user_tags": ["my-daily-backup-plan"],
      "clone_policy": {
        "max_snapshots": 0,
        "zones": [
          {"name": "us-south-1"},
          {"href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-1"}
        ]
       },
      "copy_user_tags": true,
      "cron_spec": "30 */2 * * 1-5",
      "deletion_trigger": {
        "delete_after": 20,
        "delete_over_count": 20
       },
      "name": "my-backup-plan-for-cg"
    }
  ],
  "resource_group": {
    "crn": "crn:v1:bluemix:public:resource-controller::a/123456::resource-group:678523bcbe2b4eada913d32640909956",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
    "id": "678523bcbe2b4eada913d32640909956",
    "name": "Default"
  },
}
```
{: screen}

The following example is of a backup policy that creates snapshots of a mult-volume consistency group that includes the boot volume.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies?version=2023-12-05&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
      "match_resource_type": "instance",
      "included_content": ["data_volumes","boot_volume"]
      "match_user_tags": "vsi11",
      "name": "my-consistency-group-policy",
      "plans": [
       {
         "name": "my-backup-plan-for-cg2",
         "attach_user_tags": ["my-daily-backup-plan"],
         "copy_user_tags": true,
         "cron_spec": "*/5 1,2,3 * * *",
         "deletion_trigger": {"delete_after": 20}
       }
      ],
      "resource_group": {
        "crn": "crn:v1:bluemix:public:resource-controller::a/a1234567::resource-group:678523bcbe2b4eada913d32640909956",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/f20bdbd6554d48739ad38717e0511fdd",
        "id": "f20bdbd6554d48739ad38717e0511fdd",
        "name": "Default"
      },
     }'
```
{: screen}

A successful response looks like the following example.

```json
{
  "created_at": "2023-12-05T19:49:02Z",
  "crn": "crn:v1:bluemix:public:is:eu-es:a/a7654321::backup-policy:r050-05135f1e-e1ce-467f-82bd-c46a18ff5d3b",
  "health_reasons": [],
  "health_state": "ok",
  "href": "https://eu-es.iaas.cloud.ibm.com/v1/backup_policies/r050-05135f1e-e1ce-467f-82bd-c46a18ff5d3b",
  "id": "r050-05135f1e-e1ce-467f-82bd-c46a18ff5d3b",
  "included_content": [
    "boot_volume",
    "data_volumes"
  ],
  "lifecycle_state": "pending",
  "match_resource_type": "instance",
  "match_user_tags": [
    "vsi11"
  ],
  "name": "my-consistency-group-policy",
  "plans": [
    {
      "href": "https://eu-es.iaas.cloud.ibm.com/v1/backup_policies/r050-05135f1e-e1ce-467f-82bd-c46a18ff5d3b/plans/r050-63031736-14cc-472c-aa94-d1c790438d93",
      "id": "r050-63031736-14cc-472c-aa94-d1c790438d93",
      "name": "my-backup-plan-for-cg2",
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
{: screen}

### Creating a backup policy and plan for a file share
{: #backup-policy-create-api-share}

Make a `POST /backup_policies` request to create a backup policy. The value of `match_resource_type` is `share`. The `match_user_tags` property identifies the backup tags on the file share resources and associates them with this policy and plan.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies?version=2024-12-10&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
      "match_resource_type": "share",
      "match_user_tags": "my-daily-backup-policy",
      "name": "my-backup-policy-for-file-shares",
      "plans": [
       {
         "name": "my-backup-plan-for-share",
         "attach_user_tags": ["my-daily-backup-plan"],
         "copy_user_tags": true,
         "cron_spec": "*/5 1,2,3 * * *",
         "deletion_trigger": {"delete_after": 20}
       }
      ],
      "resource_group": {
        "crn": "crn:v1:bluemix:public:resource-controller::a/a1234567::resource-group:678523bcbe2b4eada913d32640909956",
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/678523bcbe2b4eada913d32640909956",
        "id": "678523bcbe2b4eada913d32640909956",
        "name": "Default"
      },
     }'
```
{: codeblock}

### Creating a plan for an existing backup policy with the API
{: #backup-policy-create-plan-api}

You can programmatically create a backup plan for an existing ID by calling the `/backup_policies/{id}/plans` method in the [VPC API](/apidocs/vpc/latest#create-backup-policy-plan){: external} as shown in the following sample request.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans?version=2022-04-19&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
     "attach_user_tags": ["my-daily-backup-plan"],
     "copy_user_tags": true,
     "cron_spec": "*/5 1,2,3 * * *",
     "deletion_trigger": {"delete_after": 20},
     "name": "my-backup-plan"
   }'
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "active": true,
  "attach_user_tags": ["my-daily-backup-plan"],
  "copy_user_tags": true,
  "created_at": "2022-04-22T22:51:31.303Z",
  "cron_spec": "*/5 1,2,3 * * *",
  "deletion_trigger": {"delete_after": 20},
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans/4cf9171a-0043-4434-8727-15b53dbc374c",
  "id": "4cf9171a-0043-4434-8727-15b53dbc374c",
  "lifecycle_state": "stable",
  "name": "my-backup-plan",
  "resource_type": "backup_policy_plan"
}
```
{: codeblock}

### Creating a backup plan with fast restore option with the API
{: #backup-create-plan-with-fr-api}

When you create a backup plan for an existing policy, you can specify that clones of backup snapshots are created. Because these clones are created in different zones in your region, you can quickly restore a fully provisioned volume. For more information about this option, see [Restoring a volume by using fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=ui#snapshots-vpc-use-fast-restore).

Make a `POST /backup_policies/{backup_policy_id}/plans` request to create a backup plan for the policy that is identified by ID. Specify the `clone_policy` property and `zones` subproperty to indicate which zone in your region you want to create the backup snapshot clone. The zone must be different than your source zone. You can specify multiple zones. By default, you can keep a maximum of five cached backup snapshots in each zone.

The following example creates a backup plan for an existing policy that includes fast restore in the _us-south-2_ zone. The plan specifies keeping a maximum of two cached backup snapshots.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans?version=2022-12-09&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
        "active": true,
        "attach_user_tags": ["hourly-backups"],
        "clone_policy": {
          "max_snapshots": 2,
          "zones": [
            {"name": "us-south-2"},
            {"href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2"}
          ]
        },
        "copy_user_tags": true,
        "cron_spec": "0 */2 * * *",
        "deletion_trigger": {
          "delete_after": 20,
          "delete_over_count": 20
        },
        "name": "my-hourly-plan-1"
      }'
```
{: codeblock}

A successful response shows that the clone policy is created.

```json
{
  "active": true,
  "attach_user_tags": ["hourly-backups"],
  "clone_policy": {
    "max_snapshots": 2,
    "zones": [
      {"name": "us-south-2"},
      {"href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-2"}
    ]
  },
  "copy_user_tags": false,
  "created_at": "2022-12-09T15:16:37Z",
  "cron_spec": "0 */2 * * *",
  "deletion_trigger": {"delete_after": 5},
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans/6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "id": "6e251cfe-6f7b-4638-a6ba-00e9c327b178",
  "lifecycle_state": "stable",
  "name": "my-hourly-plan-1",
  "resource_type": "backup_policy_plan"
}
```
{: codeblock}

You can also set up the fast restore option when you create a backup policy and plan. Specify `clone_policy` as a subproperty of the `plans` property. For more information, see the [Create a backup policy](/apidocs/vpc/latest#create-backup-policy) in the API reference.

The fast restore feature is billed at an extra rate per hour for each zone where it is enabled. Maintaining fast restore clones is considerably more costly than keeping regular backup snapshots.
{: note}

The fast restore feature is not applicable for multi-volume or share backups.
{: important}

### Creating a backup plan with cross-regional copy option with the API
{: #backup-create-plan-with-crc-api}

When you create a backup plan, you can choose to create a copy of the backup snapshot in a different region.

If the source snapshot is not encrypted with a customer key, the encryption of the copy remains provider-managed. If the source snapshot is protected by a customer-managed key, you must specify the customer-managed key that you want to use to encrypt the new copy with the `encryption_key` subproperty. See the following example.

The following example creates a backup policy in the `us-south` region with a copy of the backup in the `us-east` region.

```sh
curl -X POST "$vpc_api_endpoint/v1/backup_policies/8758bd18-344b-486a-b606-5b8cb8cdd044/plans?version=2023-05-09&generation=2"\
   -H "Authorization: $iam_token"\
   -d '{
        "active": true,
        "attach_user_tags": ["hourly-backups"],
        "copy_user_tags": true,
        "cron_spec": "0 */2 * * *",
        "deletion_trigger": {
          "delete_after": 20,
          "delete_over_count": 20
        },
        "remote_region_policies": {
          "delete_over_count": 5,
          "encryption_key": [
            {"CRN": "crn:v1:bluemix:public:kms:us-south:a/a1234567:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd617"}
          ],
          "region": [{"name":"us-east"}]
        },
        "name": "my-hourly-plan-2"
      }'
```
{: codeblock}

A successful response shows that the clone policy is created.

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
    "encryption_key": "crn:v1:bluemix:public:kms:us-south:a/a1234567:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd617",
    "region": [
      {"name": "us-east"},
      {"href": "https://us-east.iaas.cloud.ibm.com/v1/regions/us-east/zones/us-east-2"}
     ],
    },
  "resource_type": "backup_policy_plan"
}
```
{: codeblock}

The cross-regional copy feature is not applicable for multi-volume or share backups.
{: important}

## Creating backup policies and plans with Terraform
{: #backup-policy-create-policy-plan-terraform}
{: terraform}

You can use Terraform to create backup policies and plans.

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

### Creating a backup policy for individual volumes for an account with Terraform
{: #backup-policy-create-terraform}

To create a backup policy, use the `ibm_is_backup_policy` resource. The following example defines a backup policy with the name `my-backup-policy-v1`. And the new policy applies to resources that have the `dev:test` tag.

```terraform
resource "ibm_is_backup_policy" "example" {
  match_resource_type  = ["volume"]
  match_user_tags      = ["dev:test"]
  name                 = "my-backup-policy-v1"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_backup_policy){: external}.

### Creating a backup policy for a consistency group of volumes with Terraform
{: #backup-policy-create-cg-terraform}

To create a backup policy, use the `ibm_is_backup_policy` resource. The following example defines a backup policy with the name `my-backup-policy-v2`. And the new policy applies to the block storage volumes that are attached to instances, which are tagged with the `dev:test` tag.

```terraform
resource "ibm_is_backup_policy" "example" {
  match_resource_type  = ["instance"]
  included_content     = ["boot_volume", "data_volumes"]
  match_user_tags      = ["dev:test"]
  name                 = "my-backup-policy-v2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_backup_policy){: external}.

### Creating a backup policy for file shares with Terraform
{: #backup-policy-create-share-terraform}

To create a backup policy, use the `ibm_is_backup_policy` resource. The following example defines a backup policy with the name `my-backup-policy-v3`. And the new policy applies to the file shares that are tagged with the `dev:test` tag.

```terraform
resource "ibm_is_backup_policy" "example" {
  match_resource_type  = ["share"]
  match_user_tags      = ["dev:test"]
  name                 = "my-backup-policy-v2"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_backup_policy){: external}.

### Creating a backup policy for an Enterprise with Terraform
{: #backup-policy-create-enterprise-terraform}

To create a backup policy, use the `ibm_is_backup_policy` resource. The following example defines a backup policy with the name `my-backup-policy-v2` and specifies the Enterprise CRN in the scope. And the new policy applies to resources that have the `dev:test` tag.

```terraform
resource "ibm_is_backup_policy" "ent-baas-example" {
  match_resource_type  = ["volume"]
  match_user_tags      = ["dev:test"]
  name                 = "my-backup-policy-v2"
  scope {
    crn = "crn:v1:bluemix:public:is:us-south:a/123456::reservation:7187-ba49df72-37b8-43ac-98da-f8e029de0e63"
  }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_backup_policy){: external}.

### Creating a plan for an existing backup policy with Terraform
{: #backup-policy-create-plan-terraform}

To create a backup plan, use the `ibm_is_backup_policy_plan` resource. The following example defines a plan within the `my-backup-policy-v1` policy. The new plan is called `not-just-another-plan` and it initiates backup jobs every morning at 01:05.

```terraform
 resource "ibm_is_backup_policy_plan" "example" {
  backup_policy_id = ibm_is_backup_policy.example.id
  cron_spec        = "05 01 * * *"
  name             = "my-backup-plan"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy_plan](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_backup_policy_plan){: external}.

### Creating a backup plan with fast restore option with Terraform
{: #backup-create-plan-with-fr-terraform}

To create a backup plan with fast restore option, use the `ibm_is_backup_policy_plan` resource.

The following example defines a backup plan for an existing policy that includes fast restore in two zones within the eu-de region. The plan includes keeping a maximum of two cached backups in each zone. The backup job starts creating backup snapshots at 7:15 PM every day.

```terraform
resource "ibm_is_backup_policy_plan" "example" {
  backup_policy_id = "ibm_is_backup_policy.example.id"
  cron_spec        = "15 19 * * *"
  name             = "my-policy-plan"
  clone_policy {
    zones          = ["eu-de-1", "eu-de-3"]
    max_snapshots  = 2
  }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy_plan](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_backup_policy_plan){: external}.

The fast restore feature is not applicable for multi-volume or share backups.
{: important}

### Creating a backup plan with cross-regional copy option with Terraform
{: #backup-create-plan-with-crc-terraform}

To create a backup plan with cross-regional copy option, use the `ibm_is_backup_policy_plan` resource. If the source snapshot is not encrypted with a customer key, the encryption of the copy remains provider-managed. If the source snapshot is protected by a customer-managed key, you must specify the customer-managed key that you want to use to encrypt the new copy with the `--encryption-key` option. See the following example.

```terraform
resource "ibm_is_backup_policy_plan" "example" {
  backup_policy_id = "ibm_is_backup_policy.example.id"
  cron_spec        = "30 */2 * * 1-5"
  name             = "my-policy-plan"
  deletion_trigger {
    delete_after      = 20
    delete_over_count = 20
  }
  remote_copy_policies {
    delete_over_count = 1
    encryption_key = "crn:v1:bluemix:public:kms:us-south:a/a1234567:e4a29d1a-2ef0-42a6-8fd2-350deb1c647e:key:5437653b-c4b1-447f-9646-b2a2a4cd6179"
    region = "us-south"
  }
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy_plan](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_backup_policy_plan){: external}.

The cross-regional copy feature is not applicable for multi-volume or share backups.
{: important}

## Next steps
{: #backup-create-next-steps}

After you create a backup policy, you can do the following actions.

* [Apply tags to your resources for backups](/docs/vpc?topic=vpc-backup-use-policies).
* [View a list of backup policies](/docs/vpc?topic=vpc-backup-view-policies) that you created.
* [Manage your backup policies and plans](/docs/vpc?topic=vpc-backup-service-manage).
