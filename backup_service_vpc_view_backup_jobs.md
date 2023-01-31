---

copyright:
  years: 2022, 2023
lastupdated: "2023-01-31"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Viewing backup jobs
{: #backup-view-policy-jobs}

When a backup policy is being created or deleted, you can view the backup job status. Use the UI, CLI, or API to list all backup jobs for a policy and view job details.
{: shortdesc}

A job creates a snapshot from a volume, on behalf of a backup policy and plan.

## View backup jobs in the UI
{: #backup-view-jobs-ui}
{: ui}

List all backup jobs. Select a backup job and review its details.

### View a list of backup jobs in the UI
{: #backup-view-jobs-list-ui}

From the backup policy details page, you can list all backup jobs for that policy. The jobs are listed with the newest one first. Backup jobs show all backups that were created in the account for the selected region.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Backup policies**.

2. Click the **Backup jobs** tab. Table 1 describes the information on the Backup jobs page.

| Field | Description |
|-------|-------------|
| Job ID | It's the auto-generated ID of the backup job. |
| Job status | It's the status of the backup job, either `running`, `completed`, or `failed`. |
| Plan | The backup plan that triggered the backup. Hover over the plan name for a summary of the plan. |
| Job type | When a backup is being created, it shows `creation`. When a backup is being deleted, it shows `retention`.|
| Job started | Date and time of when the job began. |
| Job completed | Date and time of when the job finished. |
| Snapshot | The snapshot (backup) that was created when the backup job finishes. Click the name to see the details in the side panel. |
| Source | Source volume from which the backup was created. Click the volume name to see its details. |
{: caption="Table 1. List of backup jobs for the backup policy" caption-side="bottom"}

### View snapshots that were created by a backup job in the UI
{: #backup-view-snap-backup-ui}

From the list of backup jobs, click the name of a snapshot. A side panel provides snapshot details (Table 2).

| Field | Description |
|-------|-------------|
| Status | The status of the snapshot, such as _Stable_. For a list of snapshot statuses, see [Snapshot statuses](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status). |
| Name | The name of the backup policy that created the snapshot. You can change the backup policy settings by clicking the pencil icon. For more information, see [Managing backup policies](/docs/vpc?topic=vpc-backup-service-manage). |
| ID | GUID of the snapshot. |
| Bootable | It indicates whether the snapshot was created from a boot volume. |
| CRN | Cloud resource name of the snapshot. |
| Created | Date and time of when that the snapshot resource creation process started. |
| Captured | The date and time of when this snapshot was taken. If the field is empty, then the snapshot is not yet captured or the snapshot was created before this feature was introduced (January 2022). |
| Size | Size in GBs of the snapshot, it is inherited from the source volume. |
| Source Volume | Source volume from which the first snapshot was taken. Click the link for volume details. If the volume was deleted, the name appears without a link. |
| Encryption | Either provider-managed or customer-managed. |
| Tags |  User tags inherited from the source volume when the snapshot was created. Click the pencil icon to more user tags.|
{: caption="Table 2. Snapshot details side panel" caption-side="bottom"}

## View backup jobs from the CLI
{: #backup-view-jobs-cli}
{: cli}

You can view a list of backup jobs by specifying the ID or name of the backup policy. You can also see details of a backup job with the ID of the backup job.

### View a list of backup jobs from the CLI
{: #backup-view-jobs-list-cli}

Run the `backup-policy-jobs` command to view the backup jobs for your backup snapshots. Status indicates when backup snapshots are being created, failed completion, or succeeded completion. 

```text
ibmcloud is backup-policy-jobs POLICY [--volume VOLUME] [--snapshot SNAPSHOT] [--snapshot-crn SNAPSHOT_CRN] [--status failed | running | succeeded] [--plan PLAN] [--output JSON] [-q, --quiet]
```
{: pre}

In this example, the ID of the backup policy lists jobs for that backup policy. You can also specify the backup policy name.

```json
ibmcloud is backup-policy-jobs bd8e95e1-e63a-468e-9df8-6a8747f03ffc
Listing jobs of backup policy bd8e95e1-e63a-468e-9df8-6a8747f03ffc under account VPC as user myuser@mycompany.com...
ID                                          Auto delete   Auto delete after   Completed at                Created                     Job type   Status
934c7e39-f1ba-4e62-b9ae-d5d3773874c0   true          15                  2022-06-24T17:02:14+05:30   2022-06-24T17:01:16+05:30   creation   succeeded
d3b0d928-1a3a-4302-9b12-cc0a17bdd1d5   true          15                  2022-06-25T17:00:35+05:30
2022-06-25T17:00:20+05:30   creation   succeeded
```
{: screen}

### View details of a backup job from the CLI
{: #backup-view-jobs-details-cli}

Run the `backup-policy-jobs` command and specify the ID of the backup policy and backup job to see the job details. Alternatively, you can specify the backup policy name.

```text
ibmcloud is backup-policy-job POLICY JOB_ID [--output JSON] [-q, --quiet]
```
{: pre}

The following example shows the output when you run the command by using the policy ID and the job ID as parameters.

```json
ibmcloud is backup-policy-job 7003cdf1-48bb-4af2-9ceb-1edbe8fcb818 a0d4eac1-6473-4c3b-a123-5543cdecaf45
Getting job a0d4eac1-6473-4c3b-a123-5543cdecaf45 under account VPC as user myuser@mycompanu.com...

ID                      a0d4eac1-6473-4c3b-a123-5543cdecaf45
Auto delete             true
Auto delete after       15
Plans                   ID                                          Name                         Resource type
                        392d5e3a-4596-407f-9ca6-2c78afce8d6e        test-p1-plan-2               backup_policy_plan
Completed at            2022-06-21T16:59:52+05:30
Created                 2022-06-21T16:59:30+05:30
Source volume           CRN                                                                                                                        ID                                          Name                          Resource type
                        crn:v1:staging:public:is:us-south-3:a/fee5afc483594adaa8325e2b4d1290df::volume:a11e25f2-3fc3-4507-8725-e5f1d07256ea   a11e25f2-3fc3-4507-8725-e5f1d07256ea   volume-2

Status                  succeeded
Completed at            2022-06-21T16:59:52+05:30
Current status reason   Code   Message

Target snapshot         CRN                                                                                                                        ID                                          Name                                                 Resource type
                        crn:v1:staging:public:is:us-south:a/fee5afc483594adaa8325e2b4d1290df::snapshot:cd900168-918e-4386-9527-f1ba13157bba   cd900168-918e-4386-9527-f1ba13157bba   test-p1-plan-2-e49bef87507f-400d   snapshot

Job type                creation
Resource type           backup_policy_job
```
{: screen}

## View backup jobs with the API
{: #backup-view-jobs-api}
{: api}

View a list of backup jobs or details of a single backup job with the API.

### View a list of backup jobs with the API
{: #backup-view-jobs-list-api}

Make a `GET /backup_policies/{backup_policy_id}/jobs` request to list all backup jobs a backup policy.

```curl
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
      "source_volume": {
        "crn": "crn:v1:bluemix:public:is:us-south-1:a/123456::volume:1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
        "deleted": {
          "more_info": "https://cloud.ibm.com/apidocs/vpc#deleted-resources"
        },
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
        "id": "1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
        "name": "my-volume"
      },
      "status": "failed",
      "status_reasons": [
        {
          "code": "source_volume_busy",
          "message": "string",
          "more_info": "https://cloud.ibm.com/docs/__TBD__"
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

```curl
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
  "source_volume": {
    "crn": "crn:v1:bluemix:public:is:us-south-1:a/123456::volume:1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
    "deleted": {
      "more_info": "https://cloud.ibm.com/apidocs/vpc#deleted-resources"
    },
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
    "id": "1a6b7274-678d-4dfb-8981-c71dd9d4daa5",
    "name": "my-volume"
  },
  "status": "failed",
  "status_reasons": [
    {
      "code": "source_volume_busy",
      "message": "string",
      "more_info": "https://cloud.ibm.com/docs/..."
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

### Backup job statuses and reason codes
{: #backup-jobs-status}

When you view details of a backup job by making a `GET/backup_policies/{backup_policy_id}/jobs/{backup_job_id}` request, the `status` property indicates whether the job `failed`, is `running`, or `succeeded`. The API also provides status reasons, with the following codes:

* `internal_error`: Indicates an internal error. Contact IBM support if your see this code.
* `snapshot_pending`: Indicates that backup snapshot in the `pending` [snapshot lifecycle state](/docs/vpc?topic=vpc-snapshots-vpc-manage&interface=ui#snapshots-vpc-status) cannot be deleted.
* `snapshot_volume_limit`: Indicates that the [snapshot limit](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-3) for the source volume is reached.
* `source_volume_busy`: Indicates that the source volume is busy after multiple retries.

## Next steps
{: #backup-jobs-next-steps}

* [Apply tags to your resources for backups](/docs/vpc?topic=vpc-backup-use-policies).
* [Create more backup policies](/docs/vpc?topic=vpc-backup-policy-create&interface=ui).
* [Restore a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).
