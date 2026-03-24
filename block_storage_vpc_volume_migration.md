---

copyright:
  years: 2025, 2026
lastupdated: "2026-03-24"

keywords: Block Storage, volume migration, volume jobs, storage generations, volume conversion

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Migrating block storage volume
{: #block-storage-vpc-volume-migration}

You can migrate your storage volume from first-generation volume profiles ([tiered](https://cloud.ibm.com/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#tiers) or [custom](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#custom)) to the [second-generation volume profile](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#defined-performance-profile) in the console, from the CLI, with API, or Terraform. During the migration, all used areas of the volume are copied and the source and target volumes are kept in sync by mirroring writes. If the volume is attached to an instance, the VSI's volume attachment is seamlessly converted from source to target volume when the copy operation is complete.
{: shortdesc}

When you create a migration job, you can customize the IOPS and bandwidth limits. If you don’t specify these values, the system applies the default values of the profile. While the migration is in progress, the volume is in the `updating` state. You can cancel a migration while the copy process is running. You can track the status and progress of your migration jobs in the UI, from the CLI, with API and Terraform. When the migration completes, the volume moves to the `available` state and follows the billing model of the new profile.

Encryption at rest is not impacted by the migration process. The volume is encrypted by using the same CRK and WDEK before and after the migration.

Snapshots that were taken before the volume migration are not migrated along with the volume, and their storage generation values remain unchanged. The first snapshot taken of the volume after the migration is a full-copy of the volume and inherits the storage generation value from the volume. Subsequent snapshots that are taken after the first one become incremental snapshots.

If you are not satisfied with the migrated volume’s performance or features, create a [support case](/unifiedsupport/supportcenter) to get added to the allowlist. When your account is on the allowlist, you can initiate a migration to return the volume back to generation 1.
{: preview}

## Limitations
{: #volume-migration-limitations}

* Boot volumes that were created from images that are encrypted with customer-managed keys can't be migrated in this release.
* Within a 24-hour period, an account can initiate a maximum of 10 migrations per region.
* If an offline migration is in progress, any new volume attachment request fails. To proceed with the attachment, you must cancel the migration process.
* If an online migration is in progress, initiating a volume detachment can cancel the migration process.
* New snapshots cannot be created from a volume while it's migrating.
* If a volume is attached to a VSI that uses pooled bandwidth allocation, the volume cannot be migrated. Set the bandwidth allocation to `weighted` to support the volume migration process.
* [Select availability]{: tag-green} Only those second-generation volumes can be migrated back to a first-generation profile that were originally created as first-generation volumes. Volumes that were created with the `sdp` profile cannot be converted to a first-generation profile.
* [Select availability]{: tag-green} If a migrated volume's capacity is increased over 16 TB, it cannot be migrated back to use a first-generation profile. The first-generation, traditional profiles don't support capacities larger than 16 TB.

## Initiating volume migration in the console
{: #volume-migration-initiate-ui}
{: ui}

You can migrate your storage volume to the [`sdp` profile](/docs/vpc?topic=vpc-block-storage-profiles#defined-performance-profile) by selecting **Migrate volume** from the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") in the Block storage volumes for VPC list.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. Select the volume that you want to migrate from the list. Click the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and click **Migrate volume**.

   Volume migration is only generally available for first-generation volumes. For second‑generation volumes, the **Migrate volume** option is visible only to accounts with special access.
   {: note}

1. Select the target profile. Currently, the only available second-generation volume profile is `sdp`. If you want to return a migrated volume to a first-generation profile (`general-purpose`, `5-iops`, `10-iops`, or `custom` profiles), select the profile that supports your capacity and performance needs. The profiles that can't support your volume's current capacity are disabled.

1. Click **Migrate**.

## Initiating volume migration from the CLI
{: #volume-migration-initiate-cli}
{: cli}

You can migrate your storage volume to a [volume profile](/docs/vpc?topic=vpc-block-storage-profiles#block-storage-profile-overview) of another storage generation by using the `volume-job-create` command. Specify the ID or name of the volume that you want to convert, and the target volume profile. Adding the new IOPS and Bandwidth values is optional and dependent on the target volume profile. See the following example.

```sh
ibmcloud is volume-job-create r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3 --profile sdp --name my-volume-migration-job --iops 3000 --bandwidth 1000
```
{: pre}

```sh
Creating volume job for r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3 under account test account as user test.user@ibm.com...
ID                r006-10d88923-1102-4859-a2b8-628bb9fb0d70   
Name              my-volume-migration-job   
Status            queued   
Bandwidth(Mbps)   1000   
IOPS              3000   
Profile           sdp   
Auto Delete       false   
Created at        2026-03-17T14:26:02+05:30   
Href              https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3/jobs/r006-10d88923-1102-4859-a2b8-628bb9fb0d70   
Job type          migrate 
Resource type     volume_job
```
{: screen}

For more information about all available command options, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference).

## Initiating volume migration with the API
{: #volume-migration-initiate-api}
{: api}

Before you begin, make sure that you set up your API environment. For more information, see [Setting up your API and CLI environment](/docs/vpc?topic=vpc-set-up-environment).

You can programmatically [migrate your storage volume](/apidocs/vpc/latest#create_volume_job) to a [volume profile](/docs/vpc?topic=vpc-block-storage-profiles#block-storage-profile-overview) of another storage generation by making a `POST /volumes/{volume_id}/jobs` request to the VPC API. Specify the job type "migrate", and the target volume profile. When the volume is migrated to the `sdp` profile, you can optionally specify new IOPS and bandwidth values. See the following example.

```sh
curl "$vpc_api_endpoint/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs?version=2026-03-17&generation=2" \
  --header "Content-Type: application/json" \
  -H "Authorization: $iam_token" \
  --request POST \
  --data '{
    "job_type": "migrate",
    "parameters": {
      "profile": {
        "name": "sdp"
      }
    },
    "name": "my-volume-migration-job"
  }'
```
{: pre}

A successful response shows that the volume migration job is created.

```json
{
  "auto_delete": false,
  "created_at": "2026-03-17T04:15:00.000Z",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
  "id": "r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
  "job_type": "migrate",
  "name": "my-volume-migration-job",
  "parameters": {
    "bandwidth": 1000,
    "iops": 3000,
    "profile": {
      "name": "sdp"
    }
  },
  "resource_type": "volume_job",
  "status": "queued",
  "status_reasons": []
}
```
{: screen}

## Initiating volume migration with Terraform
{: #volume-migration-initiate-terraform}
{: terraform}

You can migrate your storage volume to the [`sdp` profile](/docs/vpc?topic=vpc-block-storage-profiles#defined-performance-profile) by using the `ibm_is_volume_job` resource. Specify the job_type "migrate", and the name of the volume job. Adding the new IOPS and bandwidth values is optional and dependent on the target volume profile. See the following example.

```terraform
resource "ibm_is_volume_job" "vol-job1" {
  job_type = "migrate"
  name    = "vol-job1"
  parameters {
    profile {
      name = "sdp"
    }
    bandwidth = 2000
    iops      = 5000
  }
}
```
{: codeblock}

## The migration job status progression
{: #volume-migration-status}

The migration process is not instantaneous. The duration of the migration depends on the volume size. A 1‑TiB volume typically takes about 6 hours to migrate. However, migration can take longer when many volumes are being processed at the same time.

The volume job starts in `queued` status, then moves into `running` state, then into `completed` when the operation finishes successfully. During the migration, the [volume lifecycle state](/docs/vpc?topic=vpc-block-storage-vpc-monitoring&interface=terraform#block-storage-vpc-status) is shown as `updating`. The following table shows the status values that you can see in the console, CLI, and API responses when you check how the migration is progressing.

| Volume job status| Activity                             |
|------------------|--------------------------------------|
|`queued`          | The job is queued, waiting to start. |
|`running`         | The job is in progress.              |
|`failed`          | The job was not completed.           |
|`succeeded`       | The job was completed successfully.  |
|`deleting`        | The job is being deleted.            |
|`deleted`         | The job was deleted.                 |
|`canceling`       | The job is being canceled.           |
|`canceled`        | The job was canceled.                |
{: caption="Volume job statuses." caption-side="bottom"}

## Monitoring volume migration in the console
{: #volume-migration-monitor-ui}
{: ui}

You can check the status and progress of the volume migration in the console.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. From the list, click the name of the volume that you started to migrate.
1. Click the Volume jobs tab to display the list of jobs that are associated with the volume.
1. Click the migration job that is in progress to see the status and estimated completion time.

## Monitoring volume migration from the CLI
{: #volume-migration-monitor-cli}
{: cli}

You can check the status and progress of the volume migration from the CLI. First, list all the volume jobs with the `ibmcloud is volume-jobs` command, and select a job from the output. The following example uses the volume's ID to list the associated volume job. You can also use the volume name.

```sh
ibmcloud is volume-jobs r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3
```
{: pre}

```sh
Listing volume job for r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3 under account test account as user test.user@ibm.com...

ID                                         Name       Created at                 Auto Delete  Job type  Status     Bandwidth(Mbps)   IOPS  Profile   
r006-10d88923-1102-4859-a2b8-628bb9fb0d70  my-volume-migration-job   2026-03-17T14:26:02+05:30  false        migrate   succeeded  1000              3000  sdp
```
{: screen}

Then, get the details of the volume job with the `ibmcloud is volume-job` command. Specify either the volume name or ID as the argument, and specify the job name or ID as part of the `--job` option.

```sh
ibmcloud is volume-job r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3 --job r006-10d88923-1102-4859-a2b8-628bb9fb0d70
```
{: pre}

When the migration is in progress, the response looks similar to the following example:

```sh
ID                        r006-10d88923-1102-4859-a2b8-628bb9fb0d70
Name                      my-volume-migration-job
Status                    running
Bandwidth(Mbps)           1000
IOPS                      3000
Profile                   sdp
Auto delete               false
Created at                2026-03-17T10:54:52-06:00
Estimated completion at   2026-03-17T12:03:30-06:00
Href                      https://us-south.cloud.ibm.com/v1/volumes/r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3/jobs/r006-10d88923-1102-4859-a2b8-628bb9fb0d70
Job type                  migrate
Started at                2026-03-17T10:55:07-06:00
Resource type             volume_job
```
{: screen}

Note the estimated completion date and time in the response.

When the migration is in complete, the response looks similar to the following example:

```sh
Getting volume job r006-10d88923-1102-4859-a2b8-628bb9fb0d70 for r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3 under account test account as user test.user@ibm.com...

ID                r006-10d88923-1102-4859-a2b8-628bb9fb0d70   
Name              my-volume-migration-job   
Status            succeeded   
Bandwidth(Mbps)   1000   
IOPS              3000   
Profile           sdp   
Auto Delete       false   
Completed at      2026-03-17T14:26:23+05:30   
Created at        2026-03-17T14:26:02+05:30   
Href              https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3/jobs/r006-10d88923-1102-4859-a2b8-628bb9fb0d70   
Job type          migrate   
Started at        2026-03-17T14:26:18+05:30 
Resource type     volume_job
```
{: screen}

For more information about all available command options, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference).

## Monitoring volume migration with the API
{: #volume-migration-monitor-api}
{: api}

You can check the status and progress of the volume migration programmatically with the API. First, [list the volume jobs](/apidocs/vpc/latest#list_volume_jobs) of the volume, and select a migration job from the API response. Then, [retrieve the selected volume job](/apidocs/vpc/latest#get-volume-job).

1. List the migration jobs of the volume. The following example lists the migration jobs for the volume `r006-567faad4-81f4-440a-95f0-6f4c98f22c25`.

   ```sh
   curl "$vpc_api_endpoint/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs?version=2026-03-17&generation=2" \
     --header "Content-Type: application/json" \
     -H "Authorization: $iam_token" \
    --request GET
   ```
   {: pre}

   A successful response looks like the following example.

   ```json
   {
   "first": {
       "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs?limit=50"
      },
   "jobs": [
    {
      "auto_delete": false,
      "created_at": "2026-03-17T06:47:41.000Z",
      "estimated_completion_at": "2026-03-17T06:51:58.000Z",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r134-45e29fb5-aa4a-4772-a857-da2f31a97726",
      "id": "r134-45e29fb5-aa4a-4772-a857-da2f31a97726",
      "job_type": "migrate",
      "name": "my-volume-migration-job-reverse",
      "parameters": {
        "bandwidth": 1000,
        "iops": 100,
        "profile": {
          "name": "custom"
        }
      },
      "resource_type": "volume_job",
      "started_at": "2026-03-17T06:47:44.000Z",
      "status": "running",
      "status_reasons": [
      ]
    },
    {
      "auto_delete": false,
      "completed_at": "2026-03-17T06:44:29.000Z",
      "created_at": "2026-03-17T06:40:11.000Z",
      "estimated_completion_at": "2026-03-17T06:44:25.000Z",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r006-10d88923-1102-4859-a2b8-628bb9fb0d70",
      "id": "r006-10d88923-1102-4859-a2b8-628bb9fb0d70",
      "job_type": "migrate",
      "name": "my-volume-migration-job",
      "parameters": {
        "bandwidth": 1000,
        "iops": 3000,
        "profile": {
          "name": "sdp"
        }
      },
      "resource_type": "volume_job",
      "started_at": "2026-03-17T06:40:14.000Z",
      "status": "succeeded",
      "status_reasons": [
      ]
    }
   ],
   "limit": 50,
   "total_count": 2
   }
   ```
   {: screen}

1. Retrieve details of the migration job. The following example uses the volume ID `006-567faad4-81f4-440a-95f0-6f4c98f22c25` and the job ID `r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d` from the previous API request.

   ```sh
   curl "$vpc_api_endpoint/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d?version=2026-03-17&generation=2" \
     --header "Content-Type: application/json" \
     -H "Authorization: $iam_token" \
     --request GET
   ```
   {: pre}

   A successful response looks like the following example when the job is still running. Note the date and time of estimated completion in the response.

   ```json
   {
   "auto_delete": false,
   "created_at": "2026-03-17T04:15:00.000Z",
   "estimated_completion_at": "2026-03-17T06:51:58.000Z",
   "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
   "id": "r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
   "job_type": "migrate",
   "name": "my-volume-migration-job",
   "parameters": {
       "bandwidth": 1000,
       "iops": 3000,
       "profile": {
         "name": "sdp"
       }
   },
   "resource_type": "volume_job",
   "started_at": "2026-03-17T04:15:15.000Z",
   "status": "running",
   "status_reasons": []
   }
   ```
   {: screen}

   A successful response looks like the following example when the job is complete.

   ```json
   {
   "auto_delete": false,
   "completed_at": "2026-03-17T06:44:29.000Z",
   "created_at": "2026-03-17T04:15:00.000Z",
   "estimated_completion_at": "2026-03-17T06:51:58.000Z",
   "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
   "id": "r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
   "job_type": "migrate",
   "name": "my-volume-migration-job",
   "parameters": {
       "bandwidth": 1000,
       "iops": 3000,
       "profile": {
         "name": "sdp"
       }
   },
   "resource_type": "volume_job",
   "started_at": "2026-03-17T04:15:15.000Z",
   "status": "running",
   "status_reasons": []
   }
   ```
   {: screen}

## Monitoring volume migration with Terraform
{: #volume-migration-monitor-terraform}
{: terraform}

Import the list of volume jobs that belong to a volume as a read-only data source with the `ibm_is_volume_job` resource.

```terraform
`data "ibm_is_volume_job" "example" { 
  volume_job_id = ibm_is_volume_job.example.id 
  }
```
{: codeblock}

## Updating volume migration job in the console
{: #volume-migration-update-ui}
{: ui}

While the migration job is running, you can also update the volume job's name if you want to.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. From the list, click the name of the volume that you started to migrate.
1. Click the Volume jobs tab to display the list of jobs that are associated with the volume.
1. Select the job that you want to update. Click the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Rename**.

## Updating volume migration job from the CLI
{: #volume-migration-update-cli}
{: cli}

While the migration job is running, you can also update the volume job's name if you want to by using the `volume-job-update` command. Specify the ID of the volume job that you want to update, and the new name.

```sh
ibmcloud is volume-job-update r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3 --volume-job r006-10d88923-1102-4859-a2b8-628bb9fb0d70 --name new-name-for-migration-jon
```
{: pre}

```sh
Updating volume job r006-10d88923-1102-4859-a2b8-628bb9fb0d70 for r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3 under account test account as user test.user@ibm.com...

ID                        r006-10d88923-1102-4859-a2b8-628bb9fb0d70   
Name                      new-name-for-migration-job   
Status                    running   
Bandwidth(Mbps)           1000   
IOPS                      3000   
Profile                   sdp   
Auto Delete               false   
Created at                2026-03-17T10:54:52-06:00
Estimated completion at   2026-03-17T12:03:30-06:00 
Href                      https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-45421f23-df25-4dc7-bb5a-3cbcec26bba3/jobs/r006-10d88923-1102-4859-a2b8-628bb9fb0d70   
Job type                  migrate   
Started at                2026-03-17T10:58:52-06:00     
Resource type             volume_job
```
{: screen}

For more information about all available command options, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference).

## Updating volume migration job with the API
{: #volume-migration-update-api}
{: api}

While the migration job is running, you can also update the volume job's name if you want to with a `PATCH/v1/volumes/{volume_id}/jobs/{job_id}` request. See the following example.

```sh
curl "$vpc_api_endpoint/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d?version=2026-03-17&generation=2" \
  --header "Content-Type: application/json" \
  -H "Authorization: $iam_token" \
  --request PATCH \
  --data '{
    "name": "my-updated-migration-job"
  }'
```
{: pre}

A successful response shows that the volume job name is updated.

```json
{
  "auto_delete": false,
  "created_at": "2026-03-17T04:15:00.000Z",
  "estimated_completion_at": "2026-03-17T06:51:58.000Z",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
  "id": "r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
  "job_type": "migrate",
  "name": "my-updated-migration-job",
  "parameters": {
    "bandwidth": 1000,
    "iops": 3000,
    "profile": {
      "name": "sdp"
    }
  },
  "resource_type": "volume_job",
  "started_at": "2026-03-17T04:15:15.000Z",
  "status": "running",
  "status_reasons": []
}
```
{: screen}

## Updating volume migration job with Terraform
{: #volume-migration-update-terraform}
{: terraform}

While the migration job is running, you can also update the volume job's name if you want to, by using the `ibm_is_volume_job` resource.

```terraform
`resource "ibm_is_volume_job" "example" { 
  volume_job_id = ibm_is_volume_job.example.id
  name          = 
  }
```
{: codeblock}

## Canceling volume migration in the console
{: #volume-migration-cancel-ui}
{: ui}

You can cancel a migration job if it’s still within the migrating phase in the `queued` or `running`.

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
1. From the list, click the name of the volume that you started to migrate.
1. Click the Volume jobs tab to display the list of jobs that are associated with the volume.
1. Select the job that you want to update. Click the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Cancel migration**.

## Canceling volume migration from the CLI
{: #volume-migration-cancel-cli}
{: cli}

You can cancel a migration job if it’s still within the migrating phase by using the `volume-job-cancel` command. Specify the ID of the volume job that you want to cancel.

```sh
ibmcloud is volume-job-cancel r006-ef750f93-70f3-45db-bb10-71901d5371e2 --volume-job r006-55e8105d-61bd-46cc-b714-042eadfc6e17
```
{: pre}

```sh
Cancelling volume job r006-55e8105d-61bd-46cc-b714-042eadfc6e17 for r006-ef750f93-70f3-45db-bb10-71901d5371e2 under account test account as user test.user@ibm.com...

ID                        r006-55e8105d-61bd-46cc-b714-042eadfc6e17   
Name                      my-volume-migration-job   
Status                    canceling   
Bandwidth(Mbps)           1000   
IOPS                      3000   
Profile                   sdp   
Auto delete               false   
Created at                2026-03-17T17:24:53+05:30   
Estimated completion at   2026-03-17T17:38:39+05:30   
Href                      https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-ef750f93-70f3-45db-bb10-71901d5371e2/jobs/r006-55e8105d-61bd-46cc-b714-042eadfc6e17  
Job type                  migrate   
Started at                2026-03-17T17:25:00+05:30      
Resource type             volume_job
```
{: screen}

For more information about all available command options, see the [VPC CLI reference](/docs/vpc?topic=vpc-vpc-reference).

## Canceling volume migration with the API
{: #volume-migration-cancel-api}
{: api}

You can cancel a migration if it’s still within the migrating phase programmatically by making a `POST /v1/volumes/{id}/jobs/{job-id}/cancel` request to the VPC API.

```sh
curl "$vpc_api_endpoint/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d/cancel?version=2026-03-17&generation=2" \
  --header "Content-Type: application/json" \
  -H "Authorization: $iam_token" \
  --request POST
```
{: pre}

A successful response shows that the volume migration job is canceled.

```json
{
  "auto_delete": false,
  "created_at": "2026-03-17T04:15:00.000Z",
  "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/r006-567faad4-81f4-440a-95f0-6f4c98f22c25/jobs/r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
  "id": "r006-03edeeed-fae7-439e-b31c-fbdbec6f9e7d",
  "job_type": "migrate",
  "name": "my-volume-migration-job",
  "parameters": {
    "bandwidth": 1000,
    "iops": 3000,
    "profile": {
      "name": "sdp"
    }
  },
  "resource_type": "volume_job",
  "started_at": "2026-03-17T04:15:15.000Z",
  "status": "canceling",
  "status_reasons": []
}
```
{: screen}

## Canceling volume migration with Terraform
{: #volume-migration-cancel-terraform}
{: terraform}

You can cancel a migration if it’s still within the migrating phase by using the `ibm_is_volume_job_operations` resource and adding the `cancel = true` argument.

```terraform
resource "ibm_is_volume_job_operations" "cancel" {
  cancel = true
  volume_job = ibm_is_volume_job.example.id
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_volume_job_operations](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_volume_job_operations){: external}.
