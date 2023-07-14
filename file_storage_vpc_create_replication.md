---

copyright:
  years: 2022, 2023
lastupdated: "2023-06-20"

keywords: file share, file storage, source volume, replica share, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating replica file shares
{: #file-storage-create-replication}

Create replica file share in a different zone in your region in the UI, from the CLI, or with the API.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Madrid, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Add replication to a file share in the UI
{: #fs-create-replica-ui}
{: ui}

You can create a replica of your file share from the list of all file shares or file share details page. Replica file shares are created in the same region, but in a different zone than the primary share's zone.

Provision a file share as described in [Create a file share and mount target in the UI](/docs/vpc?topic=vpc-file-storage-create&interface=ui#fs-create-share-target-ui). When the file share appears as "stable" on the File shares for VPC page, click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions"). Then, click **Create replica**.

On the File share replica create page, review the source file share details, and complete the replica details.

1. Name - Provide a unique name for the replica share.
2. Zone - Select the zone in which the replica share is to be created. The UI presents the different available zones in the same region.
3. Resource group - Select the resource group from the list.
4. Tags - Optionally, specify [user tags](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags). The tags that you apply to the replica can be the same as or different from the source share's tags.
5. Mount Targets - Optionally, create a mount target for the replica share. Mount targets depend on the VPC in the zone that you're using. You can specify a mount target later.
   1. Provide a mount target name.
   1. Select a virtual private cloud.
   1. Click **Save**.

6. Profile - Select a [profile](/docs/vpc?topic=vpc-file-storage-profiles) for the replica file share. The profile can be different from the source file share's profile. Selecting a different replica profile determines the performance that you get when you [perform a failover](/docs/vpc?topic=vpc-file-storage-failover). Depending on the source volume size, some profiles might be disabled.
7. Size - The replica size (GBs) is inherited from the source volume.
8. Sync frequency - Specify how often you want to synchronize changes from the primary file share to the replica share. The Summary shows the selections that you made. For **Frequency**, the options are hourly, daily, weekly, monthly, or by `cron-spec` expression:
   * For hourly, enter a value in the range 0 - 60 to specify exactly how many minutes past the hour, every hour, every day the replication is to start.
   * For daily, specify the starting time in hours and minutes in Coordinated Universal Time. Enter a value between 00:00 and 23:59. For your convenience, the Coordinated Universal Time value is converted into your local time.
   * For weekly, specify the days of the week you want replication to run and the start time in Coordinated Universal Time. Enter a value between 00:00 and 23:59.
   * For monthly, choose a day 1 - 28. For the start time, enter a value between 00:00 and 23:59.
   * If you specify a `cron-spec` expression, replications must be scheduled not less than 1 hour. Enter the replication frequency in `cron-spec` format: minute, hour, day, month, and weekday. For example, to replicate every day at 5:30 PM you need to enter `30 17 * * *`.

9. The encryption is inherited from the primary share. If you specified customer-managed encryption, the key management system is shown along with the root key. You can't encrypt a replica share with a different key.

10. In the side panel, you have multiple options.
    * Apply a code - if you have a discount code, you can add it in the field.
    * Create file share - when you're ready to create your replica share, click it.
    * Get sample API call - if you want to get the curl command code to create the replica with these exact settings, click it.
    * Add to estimate - if you want to see how adding a replica impacts your monthly cost, click it. The monthly cost estimate is displayed. You can **Review the estimate** for more billing information and details or **Clear the estimate** and return to the Replica create page.

## Add replication to file share from the CLI
{: #fs-create-replica-cli}
{: cli}

Use the CLI to create a file share with replication, or update a file share to include replication.

As of 30 May 2023, you can use `--mount-targets` instead of `-targets` option. To see and use the updated option, set the feature environment variable `IBMCLOUD_IS_FEATURE_FILESHARE_CHANGE_TO_MOUNT_TARGETS` to true.
{: beta}

   ```text
   export IBMCLOUD_IS_FEATURE_FILESHARE_CHANGE_TO_MOUNT_TARGETS=true
   ```
   {: pre}

### Create a file share with replication from the CLI
{: #fs-create-new-share-replica-cli}

Run the `ibmcloud is share-create` command and specify the following properties to define the replica share for a new file share:

* `--replica-share-iops`- The maximum input/output operation performance bandwidth per second for the file share; applicable for custom profile file share.
* `--replica-share-name`- Specify a name for the replica file share.
* `--replica-share-profile`- The profile the file share uses.
* `--replica-cron-spec`- The cron specification for the file share replication schedule.
* `--replica-mount-target`- TARGETS_JSON|@TARGETS_JSON_FILE, specify file share mount targets in JSON or in a JSON file.
* `--replica-zone`- The zone in which the replica file share is to be created. This zone must be a different zone in the same region as the source share.

Syntax:

```sh
ibmcloud is share-create \
  --zone ZONE_NAME \
  --profile PROFILE \
  [--name NAME] \
  [--iops IOPS] \
  [--mount_targets TARGETS_JSON | @TARGETS_JSON_FILE] \
  [--replica-share-profile REPLICA_SHARE_PROFILE \
  --replica-cron-spec REPLICA_CRON_SPEC \
  --replica-zone ZONE_NAME \
  [--replica-share-iops REPLICA_SHARE_IOPS] \
  [--replica-share-name REPLICA_SHARE_NAME] \
  [--replica-mount-target TARGETS_JSON | @TARGETS_JSON_FILE]] \
  [--size SIZE [--encryption_key ENCRYPTION_KEY]] \
  [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] \
  [--output JSON] [-q, --quiet]
```
{: pre}

In this example, a new share, `p-share-3` is created with a replica file share, `replica-p-share-3`. The replica is created in a different zone in the same region.

```bash
ibmcloud is share-create --name p-share-3 --zone us-south-1 --profile tier-5iops --replica-share-profile tier-5iops --replica-cron-spec '55 09 * * *' --replica-zone us-south-3  --replica-share-name replica-p-share-3 --replica-target '[{"name": "my-target1", "vpc": {"id": "af7c3b16-8a59-4434-8c2c-3230e916d441"}}]' --size 40
Creating file share p-share-3 under account VPC as user myuser@mycompany.com...

ID                   8b08129c-e376-4572-9ef3-68c729b315d5
Name                 p-share-3
CRN                  crn:v1:staging:public:is:us-south-1:a/efe5afc483594adaa8325e2b4d1290df::share:8b08129c-e376-4572-9ef3-68c729b315d5
Lifecycle state      pending
Zone                 us-south-1
Profile              tier-5iops
Size(GB)             40
IOPS                 3000
Encryption           provider_managed
Mount targets        ID                          Name   VPC ID   VPC Name
                     No mounted targets found.

Resource group       ID                                 Name
                     11caaa983d9c4beb82690daab08717e9   Default

Created              2022-09-26T03:19:58+05:30
Last sync at         2022-09-21T05:53:28+05:53
Replication share    ID              Name                                        Resource type
                     ID              de1e70af-c3cb-44c6-a96d-2ece09b51ae3
                     Name            replica-p-share-3
                     Resource type   share

Replication role     source
Replication status   none
Source share         ID   Name   Resource type
```
{: screen}

### Update an existing file share for replication from the CLI
{: #fs-create-share-replica-cli}

Run the `ibmcloud is share-create` command and specify the source share by ID or name.

```sh
ibmcloud is share-replica-create \
   --zone ZONE_NAME \
   -profile PROFILE \
   [--name NAME] \
   [--iops IOPS] \
   [--mount-targets TARGETS_JSON | @TARGETS_JSON_FILE] \
   [--replication-cron-spec REPLICATION_CRON_SPEC \
   --source-share SOURCE_SHARE] \
   [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] \
   [--output JSON] [-q, --quiet]
```
{: pre}

The following example creates a replica file share for a source file share that is identified by ID.

```bash
ibmcloud is share-replica-create --name replica-share-3 --zone us-south-3 --profile tier-5iops --replication-cron-spec '10 05 * * *' --source-share 2c4c32f9-9d25-43df-9d59-3874a81ec46e

Creating replica file share replica-share-3 under account VPC as user myuser@mycompany.com...

ID                      4de3d510-b0db-4674-9dea-c9b93e26b1c3
Name                    replica-share-3
CRN                     crn:v1:staging:public:is:us-south-3:a/efe5afc483594adaa8325e2b4d1290df::share:4de3d510-b0db-4674-9dea-c9b93e26b1c3
Lifecycle state         pending
Zone                    us-south-3
Profile                 tier-5iops
Size(GB)                40
IOPS                    3000
Encryption              provider_managed
Mount targets           ID                          Name   VPC ID   VPC Name
                        No mounted targets found.

Resource group          ID                                 Name
                        11caaa983d9c4beb82690daab08717e9   Default

Created                 2022-09-28T10:45:13+05:30
Last sync at            2022-09-28T05:53:28+05:53
Latest job              running
Replication share       ID   Name   Resource type

Replication cron spec   10 05 * * *
Replication role        replica
Replication status      initializing
Source share            ID                                          Name        Resource type
                        2c4c32f9-9d25-43df-9d59-3874a81ec46e        share-3     share
```
{: screen}

## Add replication to file share with the API
{: #fs-create-replica-api}
{: api}

Use the API to add replication to new or existing file shares. Before you begin, first set up the [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=api). For more information about the file shares VPC API, see the [VPC API reference](/apidocs/vpc-beta).

### Create a file share with replication with the API
{: #fs-create-new-share-replica-api}

When you create a file share, you can specify that a replica file share is also created in a different zone. Make a `POST /shares` request and specify the `replica_share` property to define the replica file share.

As described in the [Beta VPC API](/apidocs/vpc-beta) reference [versioning](/apidocs/vpc-beta#api-versioning-beta) policy, support for older versions of the beta API is limited to 45 days. Therefore, beta API requests must specify a `version` query parameter date value within the last 45 days. You must also provide `generation` parameter and specify `generation=2`. For more information, see **Generation** in the [Virtual Private Cloud API reference](/apidocs/vpc#api-generation-parameter).
{: requirement}

The following example creates the replica `test-replica-001` for the source share `source-share-001`. Mount targets, which are optional when you create a file share, are specified for the replica file share and source file share.

```sh
curl -X POST\
"$rias_endpoint/v1/shares?version=2023-07-11&generation=2&maturity=beta"\
-H "Authorization: $iam_token"\
-d '{
    "name": "source-share-001",
    "profile":{
       "name":"tier-3iops"
    },
    "size":10,
    "mount-targets":[
       {
          "vpc":{
              "id":"08669c86-4c8a-4bfa-8ddc-37071f955c52"
            }
         }
    ],
    "zone":{
        "name":"us-south-1"
    },
    "replica_share":{
  	   "name": "test-replica-001",
       "profile":{
           "name":"tier-3iops"
        },
        "replication_cron_spec":"*/1 * * * *",
        "zone":{
            "name":"us-south-3"
        },
        "mount-targets":[
           {
              "vpc":{
                  "id":"9380990e-4b3b-4d79-80fe-ee052fb9772a"
              },
              "transit_encryption": "none",
           }
        ]
      }
   }'
```
{: pre}

### Update an existing file share to add replication with the API
{: #fs-create-share-replica-api}

Make a `POST /shares` request to define the replica file share to add to a file share. In the example, `source_share` specifies the ID of the source file share to which you're adding replication. You also need to specify the source share name or CRN.

Other required properties are the `profile`, `zone`, and `replication_cron_spec`, which provides the replication schedule.

```sh
curl -X POST\
"$rias_endpoint/v1/shares?version=2023-07-11&generation=2&maturity=beta"\
-H "Authorization: $iam_token"\
-d '{
    "source_share": {
        "id": "4aafd9c9-5555-4bdb-902d-d63d4dcf5adc"
     },
    "name": "replica-share-1",
    "profile": "tier-3iops",
     "zone": {
        "name": "us-south-1"
     },
     "replication_cron_spec": "0 */5 * * *",
  }'
```
{: pre}

You can use the API to verify that the replication succeeded, is pending, or failed. Make a `GET /shares/{replica_id}` call. Look at the `latest_job` property. For more information, see [Verify replication with the API](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-verify-replica-api).
{: note}

## Add replication to file share with Terraform
{: #fs-create-replica-terraform}
{: terraform}

You can use the `ibm_is_share` resource in Terraform to create a file share with replication, or update a file share to include replication. The following example creates a replica share in the `us-south-3` zone and associates it to the parent share that is specified by its ID, `ibm_is_share.example.id`.

```terraform
resource "ibm_is_share" "example-1" {
  zone                  = "us-south-3"
  source_share          = ibm_is_share.example.id
  name                  = "my-replica1"
  profile               = "tier-3iops"
  replication_cron_spec = "0 */5 * * *"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

## Next steps
{: #fs-repl-next-steps}

* [Manage replication](/docs/vpc?topic=vpc-file-storage-manage-replication).

* [Fail over to the replica site](/docs/vpc?topic=vpc-file-storage-failover).
