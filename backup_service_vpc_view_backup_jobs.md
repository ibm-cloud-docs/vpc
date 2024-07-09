---

copyright:
  years: 2022, 2024
lastupdated: "2024-07-09"

keywords: Backup for VPC, backup service, backup plan, backup policy, restore, restore volume, restore data, view backup lists,

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing backup jobs
{: #backup-view-policy-jobs}

Backup jobs create or delete backup snapshots based on the backup plan's frequency and retention settings. You can use the UI, CLI, API, or Terraform to check on the status of the backup jobs.
{: shortdesc}

## View backup jobs in the console
{: #backup-view-jobs-ui}
{: ui}

List all backup jobs. Select a backup job and review its details.

### View a list of backup jobs in the console
{: #backup-view-jobs-list-ui}

From the backup policy details page, you can list all backup jobs for that policy. The jobs are listed with the oldest one first. Backup jobs show all backups that were created by the backup policy for the selected region.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> VPC Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Backup policies**.
1. Click a policy name.
1. Click the **Backup jobs** tab. Table 1 describes the information on the Backup jobs page.

   | Field         | Description |
   |---------------|-------------|
   | Job ID        | The auto-generated ID of the backup job. |
   | Job status    | Status of the backup job, either `running`, `completed`, or `failed`. |
   | Plan          | The backup plan that triggered the backup. Hover over the plan name for a summary of the plan. |
   | Job type      | When a backup is being created, it shows `creation`. When a backup is being deleted, it shows `deletion`.|
   | Job started   | Date and time of when the job began. |
   | Job completed | Date and time of when the job was finished. |
   | Snapshot      | The snapshot (backup) that was created when the backup job finishes. Click the name to see the details in the side panel. |
   | Source        | The source can be the source volume from which the first snapshot was taken, or the source instance of the snapshot consistency group that the backup is a member of. |
   {: caption="Table 1. Information provided by the list of backup jobs for the backup policy" caption-side="bottom"}

### View snapshots that were created by a backup job in the console
{: #backup-view-snap-backup-ui}

From the list of backup jobs, click the Actions icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **View created snapshots**. A side panel provides snapshot details (Table 2).

| Field    | Description |
|----------|-------------|
| Name     | The name of the backup policy that created the snapshot. You can change the backup policy settings by clicking the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit"). For more information, see [Managing backup policies](/docs/vpc?topic=vpc-backup-service-manage). |
| Status   | The status of the snapshot, such as _Stable_. For a list of snapshot statuses, see [Snapshot statuses](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status). |
| Size     | Size in GBs of the snapshot, it is inherited from the source volume. |
| Source   | This field shows the source volume   from which the snapshot was taken. If the source volume was deleted, the name appears without a link. |
| Bootable | It indicates whether the snapshot was created from a boot volume. |
{: caption="Table 2. Snapshot details side panel" caption-side="bottom"}

## View backup jobs from the CLI
{: #backup-view-jobs-cli}
{: cli}

You can view a list of backup jobs by specifying the ID or name of the backup policy. You can also see details of a backup job with the ID of the backup job.

### View a list of backup jobs from the CLI
{: #backup-view-jobs-list-cli}

Run the `backup-policy-jobs` command to view the backup jobs for your backup snapshots. Status indicates when the backup job is running, if the backup job failed or succeeded.

```text
ibmcloud is backup-policy-jobs POLICY [--volume VOLUME] [--snapshot SNAPSHOT] [--snapshot-crn SNAPSHOT_CRN] [--status failed | running | succeeded] [--plan PLAN] [--output JSON] [-q, --quiet]
```
{: pre}

In the first example, the name of the backup policy is used to list the jobs of that backup policy.

```sh
cloudshell:~$ ibmcloud is backup-policy-jobs new-policy-23
Listing jobs of backup policy new-policy-23 under account Test Account as user test.user@ibm.com...
ID                                          Auto delete   Auto delete after   Completed at                Created at                  Job type   Status
r138-4611c14d-69bd-4522-bc5f-4d3352df9b7b   true          15                  2024-02-06T01:47:39+00:00   2024-02-06T01:45:25+00:00   deletion   succeeded   
r138-58a33e41-472f-431c-8c74-5270ac3e48fd   true          15                  2024-01-23T01:48:07+00:00   2024-01-23T01:45:25+00:00   deletion   succeeded   
r138-f800fe99-455f-4b30-95ad-98293eb3b3e2   true          15                  2024-01-17T10:13:13+00:00   2024-01-17T10:06:21+00:00   deletion   succeeded   
r138-60497743-43b4-4e7d-a221-0f6927653c6b   true          15                  2024-01-17T10:06:37+00:00   2024-01-17T10:06:19+00:00   creation   succeeded   
r138-3fe98f73-56ee-425b-842f-2a4b2a967241   true          15                  2024-01-16T10:12:59+00:00   2024-01-16T10:06:22+00:00   deletion   succeeded   
r138-bd881c52-4f2e-4644-8072-fadcd6c8d3df   true          15                  2024-01-16T10:06:38+00:00   2024-01-16T10:06:21+00:00   creation   succeeded 
```
{: screen}

In the second example, a different policy is identified by its ID, and the JSON output option is used.

```json
cloudshell:~$ ibmcloud is backup-policy-jobs r006-0723c648-9a47-4d51-b1ba-349e21e715b6  -json
  [
    {
        "auto_delete": true,
        "auto_delete_after": 30,
        "backup_policy_plan": {
            "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-0723c648-9a47-4d51-b1ba-349e21e715b6/plans/r006-e888bb31-7bf2-4885-a9f3-d448c1c37326",
            "id": "r006-e888bb31-7bf2-4885-a9f3-d448c1c37326",
            "name": "my-plan-b",
            "resource_type": "backup_policy_plan"
        },
        "completed_at": "2024-01-17T10:06:37.000Z",
        "created_at": "2024-01-17T10:06:19.000Z",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-0723c648-9a47-4d51-b1ba-349e21e715b6/jobs/r006-60497743-43b4-4e7d-a221-0f6927653c6b",
        "id": "r006-60497743-43b4-4e7d-a221-0f6927653c6b",
        "job_type": "creation",
        "resource_type": "backup_policy_job",
        "source": {
            "crn": "crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-6afe1361-b592-45ab-b23b-6cca9982e371",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-6afe1361-b592-45ab-b23b-6cca9982e371",
            "id": "r006-6afe1361-b592-45ab-b23b-6cca9982e371",
            "name": "my-block-test3",
            "resource_type": "volume"
        },
        "status": "succeeded",
        "status_reasons": [],
        "target_snapshots": [
            {
                "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-d609337c-a25e-4b6c-bcfd-4cfdf3d96af3",
                "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-d609337c-a25e-4b6c-bcfd-4cfdf3d96af3",
                "id": "r006-d609337c-a25e-4b6c-bcfd-4cfdf3d96af3",
                "name": "my-plan-b-8c3b424fd90b-4d05",
                "resource_type": "snapshot"
            }
        ]
    },
    {
        "auto_delete": true,
        "auto_delete_after": 30,
        "backup_policy_plan": {
            "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-0723c648-9a47-4d51-b1ba-349e21e715b6/plans/r006-309d6d45-8b1c-4cb6-a749-c37deec3252a",
            "id": "r006-309d6d45-8b1c-4cb6-a749-c37deec3252a",
            "name": "my-weekly-plan",
            "resource_type": "backup_policy_plan"
        },
        "completed_at": "2024-01-09T01:47:41.000Z",
        "created_at": "2024-01-09T01:45:19.000Z",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-0723c648-9a47-4d51-b1ba-349e21e715b6/jobs/r006-fab85148-ea61-460b-9d8e-6d3f94ed2c70",
        "id": "r006-fab85148-ea61-460b-9d8e-6d3f94ed2c70",
        "job_type": "deletion",
        "resource_type": "backup_policy_job",
        "source": {
            "crn": "crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-6afe1361-b592-45ab-b23b-6cca9982e371",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-6afe1361-b592-45ab-b23b-6cca9982e371",
            "id": "r006-6afe1361-b592-45ab-b23b-6cca9982e371",
            "name": "my-block-test3",
            "resource_type": "volume"
        },
        "status": "succeeded",
        "status_reasons": [],
        "target_snapshots": [
            {
                "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-1ac68268-35e5-4eb0-8e9f-82fbf06cf192",
                "deleted": {
                    "more_info": "https://cloud.ibm.com/apidocs/vpc#deleted-resources"
                },
                "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-1ac68268-35e5-4eb0-8e9f-82fbf06cf192",
                "id": "r006-1ac68268-35e5-4eb0-8e9f-82fbf06cf192",
                "name": "-deleted-82fbf06cf192",
                "resource_type": "snapshot"
            }
        ]
    }
  ]  
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-jobs`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-jobs-list).

### View details of a backup job from the CLI
{: #backup-view-jobs-details-cli}

Run the `backup-policy-jobs` command and specify the ID of the backup policy and backup job to see the job details. Alternatively, you can specify the backup policy name.

```text
ibmcloud is backup-policy-job POLICY JOB_ID [--output JSON] [-q, --quiet]
```
{: pre}

The following example shows the output when you run the command by using the policy name and the job ID as arguments, and the `--output JSON` option.

```json
$ ibmcloud is backup-policy-job my-backup-policy-v2 r006-60497743-43b4-4e7d-a221-0f6927653c6b -json
{
    "auto_delete": true,
    "auto_delete_after": 30,
    "backup_policy_plan": {
        "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-0723c648-9a47-4d51-b1ba-349e21e715b6/plans/r006-e888bb31-7bf2-4885-a9f3-d448c1c37326",
        "id": "r006-e888bb31-7bf2-4885-a9f3-d448c1c37326",
        "name": "my-plan-b",
        "resource_type": "backup_policy_plan"
    },
    "completed_at": "2024-01-17T10:06:37.000Z",
    "created_at": "2024-01-17T10:06:19.000Z",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/r006-0723c648-9a47-4d51-b1ba-349e21e715b6/jobs/r006-60497743-43b4-4e7d-a221-0f6927653c6b",
    "id": "r006-60497743-43b4-4e7d-a221-0f6927653c6b",
    "job_type": "creation",
    "resource_type": "backup_policy_job",
    "source": {
        "crn": "crn:v1:bluemix:public:is:us-south-2:a/a1234567::volume:r006-6afe1361-b592-45ab-b23b-6cca9982e371",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-6afe1361-b592-45ab-b23b-6cca9982e371",
        "id": "r006-6afe1361-b592-45ab-b23b-6cca9982e371",
        "name": "my-block-test3",
        "resource_type": "volume"
    },
    "status": "succeeded",
    "status_reasons": [],
    "target_snapshots": [
        {
            "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-d609337c-a25e-4b6c-bcfd-4cfdf3d96af3",
            "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-d609337c-a25e-4b6c-bcfd-4cfdf3d96af3",
            "id": "r006-d609337c-a25e-4b6c-bcfd-4cfdf3d96af3",
            "name": "my-plan-b-8c3b424fd90b-4d05",
            "resource_type": "snapshot"
        }
    ]
```
{: screen}

For more information about available command options, see [`ibmcloud is backup-policy-jobs`](/docs/vpc?topic=vpc-vpc-reference#backup-policy-jobs-list).

## View backup jobs with the API
{: #backup-view-jobs-api}
{: api}

View a list of backup jobs or details of a single backup job with the API.

### View a list of backup jobs with the API
{: #backup-view-jobs-list-api}

Make a `GET /backup_policies/{backup_policy_id}/jobs` request to list all backup jobs of a backup policy.

```sh
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/{backup_policy_id}/jobs?version=2022-06-22&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "first": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/7241e2a8-601f-11ea-8503-000c29475bed/jobs?limit=20"
  },
  "jobs": [
    {
      "auto_delete": true,
      "auto_delete_after": 90,
      "backup_policy_plan": {
        "deleted": {
          "more_info": "https://cloud.ibm.com/apidocs/vpc#deleted-resources"
        },
        "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/0fe9e5d8-0a4d-4818-96ec-e99708644a58/plans/4cf9171a-0043-4434-8727-15b53dbc374c",
        "id": "4cf9171a-0043-4434-8727-15b53dbc374c",
        "name": "my-policy-plan",
        "resource_type": "backup_policy_plan"
      },
      "completed_at": "2022-06-23T13:37:11.549Z",
      "created_at": "2022-06-23T13:37:11.549Z",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/0fe9e5d8-0a4d-4818-96ec-e99708644a58/jobs/4cf9171a-0043-4434-8727-15b53dbc374c",
      "id": "4cf9171a-0043-4434-8727-15b53dbc374c",
      "job_type": "creation",
      "resource_type": "backup_policy_job",
      "source": {
        "crn": "crn:v1:bluemix:public:is:us-south-1:a/123456::volume:r006-1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
        "deleted": {
          "more_info": "https://cloud.ibm.com/apidocs/vpc#deleted-resources"
        },
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
        "id": "1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
        "name": "my-volume",
        "resource_type": "volume"
      },
      "status": "failed",
      "status_reasons": [
        {
          "code": "source_volume_busy",
          "message": "string",
          "more_info": "https://cloud.ibm.com/docs/vpc?..."
        }
      ],
      "target_snapshot": {
        "crn": "crn:v1:bluemix:public:is:us-south:a/123456::snapshot:f6bfa329-0e36-433f-a3bb-0df632e79263",
        "deleted": {
          "more_info": "https://cloud.ibm.com/apidocs/vpc#deleted-resources"
        },
        "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/f6bfa329-0e36-433f-a3bb-0df632e79263",
        "id": "f6bfa329-0e36-433f-a3bb-0df632e79263",
        "name": "my-snapshot",
        "resource_type": "snapshot"
      }
    }
  ],
  "limit": 20,
  "next": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/7241e2a8-601f-11ea-8503-000c29475bed/jobss?start=9d5a91a3e2cbd233b5a5b33436855ed1&limit=20"
  },
  "total_count": 132
}
```
{: codeblock}

### View details of a backup job with the API
{: #backup-view-jobs-details-api}

Make a `GET /backup_policies/{backup_policy_id}/jobs/{backup_job_id}` request to view details of a backup job, which is specified by ID.

```sh
curl -X GET\
"$vpc_api_endpoint/v1/backup_policies/{backup_policy_id}/jobs{backup_job_id}?version=2022-06-22&generation=2"\
   -H "Authorization: $iam_token"
```
{: codeblock}

A successful response looks like the following example.

```json
{
  "auto_delete": true,
  "auto_delete_after": 90,
  "backup_policy_plan": {
    "deleted": {
      "more_info": "https://cloud.ibm.com/apidocs/vpc#deleted-resources"
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/0fe9e5d8-0a4d-4818-96ec-e99708644a58/plans/4cf9171a-0043-4434-8727-15b53dbc374c",
    "id": "4cf9171a-0043-4434-8727-15b53dbc374c",
    "name": "my-policy-plan",
    "resource_type": "backup_policy_plan"
  },
  "completed_at": "2022-06-30T01:58:32.163Z",
  "created_at": "2022-06-30T01:58:32.163Z",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/backup_policies/0fe9e5d8-0a4d-4818-96ec-e99708644a58/jobs/4cf9171a-0043-4434-8727-15b53dbc374c",
  "id": "4cf9171a-0043-4434-8727-15b53dbc374c",
  "job_type": "creation",
  "resource_type": "backup_policy_job",
  "source": {
    "crn": "crn:v1:bluemix:public:is:us-south-1:a/123456::volume:r006-1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
    "deleted": {
      "more_info": "https://cloud.ibm.com/apidocs/vpc#deleted-resources"
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
    "id": "1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
    "name": "my-volume",
    "resource_type": "volume"
  },
  "status": "failed",
  "status_reasons": [
    {
      "code": "source_volume_busy",
      "message": "string",
      "more_info": "https://cloud.ibm.com/docs/vpc?...."
    }
  ],
  "target_snapshot": {
    "crn": "crn:v1:bluemix:public:is:us-south:a/123456::snapshot:f6bfa329-0e36-433f-a3bb-0df632e79263",
    "deleted": {
      "more_info": "https://cloud.ibm.com/apidocs/vpc#deleted-resources"
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/f6bfa329-0e36-433f-a3bb-0df632e79263",
    "id": "f6bfa329-0e36-433f-a3bb-0df632e79263",
    "name": "my-snapshot",
    "resource_type": "snapshot"
  }
}
```
{: codeblock}

## View backup jobs with Terraform
{: #backup-view-jobs-terraform}
{: terraform}

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

### View a list of backup jobs with Terraform
{: #backup-view-jobs-list-terraform}

Import the list of a collection of backup jobs as a read-only data source.

```terraform
data "ibm_is_backup_policy_jobs" "example" {
  backup_policy_id = ibm_is_backup_policy.example.id
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy_jobs](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/ibm_is_backup_policy_jobs){: external}.

### View details of a backup job with Terraform
{: #backup-view-jobs-details-terraform}

Import the details of a backup job as a read-only data source.

```terraform
data "ibm_is_backup_policy_job" "example" {
    backup_policy_id = ibm_is_backup_policy.example.id
    identifier = "r138-25828175-2b51-4247-ba01-79c538f68282"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_backup_policy_job](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/ibm_is_backup_policy_job){: external}.

## Backup job statuses and reason codes
{: #backup-jobs-status}

When you view details of a backup job from the CLI or by making a `GET /backup_policies/{backup_policy_id}/jobs/{backup_job_id}` request to the API, the `status` property indicates whether the job `failed`, is `running`, or `succeeded`. The CLI and API responses also provide status reasons with the following codes:

* `internal_error` - The code indicates an internal error. Contact IBM support if you see this code.
* `snapshot_pending` - The code indicates that a backup snapshot in the `pending` [snapshot lifecycle state](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status) cannot be deleted.
* `snapshot_volume_limit` - The code indicates that the [snapshot limit](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-3) for the source volume is reached.

## Next steps
{: #backup-jobs-next-steps}

* [Apply tags to your resources for backups](/docs/vpc?topic=vpc-backup-use-policies).
* [Create more backup policies](/docs/vpc?topic=vpc-create-backup-policy-and-plan&interface=ui).
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).
