---

copyright:
  years: 2022, 2023
lastupdated: "2023-08-22"

keywords: file share, file storage, source volume, replica share, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating replica file shares
{: #file-storage-create-replication}

Create replica file share in a different zone in your region in the UI, from the CLI, or with the API.
{: shortdesc}

## Adding replication to a file share in the UI
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

## Adding replication to file share from the CLI
{: #fs-create-replica-cli}
{: cli}

Use the CLI to create a file share with replication, or update a file share to include replication.  

### Creating a file share with replication from the CLI
{: #fs-create-new-share-replica-cli}

Run the `ibmcloud is share-create` command and specify the following properties to define the replica share for a new file share:

* `--replica-share-iops`- The maximum input/output operation per second for the file share.
* `--replica-share-name`- Specify a name for the replica file share.
* `--replica-share-profile`- The profile the file share uses.
* `--replica-cron-spec`- The cron specification for the file share replication schedule.
* `--replica-share-mount-target`- TARGETS_JSON|@TARGETS_JSON_FILE, specify file share mount targets in JSON or in a JSON file.
* `--replica-zone`- The zone in which the replica file share is to be created. This zone must be a different zone in the same region as the source share.

Syntax:

```sh
ibmcloud is share-create \
  --zone ZONE_NAME \
  --profile PROFILE \
  [--name NAME] \
  [--iops IOPS] \
  [--mount_targets TARGETS_JSON | @TARGETS_JSON_FILE] \
  [--replica-share-profile REPLICA_SHARE_PROFILE] \
  --replica-cron-spec REPLICA_CRON_SPEC \
  --replica-zone ZONE_NAME \
  [--replica-share-iops REPLICA_SHARE_IOPS] \
  [--replica-share-name REPLICA_SHARE_NAME] \
  [--replica-share-mount-target TARGETS_JSON | @TARGETS_JSON_FILE] \
  [--size SIZE [--encryption_key ENCRYPTION_KEY]] \
  [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] \
  [--output JSON] [-q, --quiet]
```
{: pre}

In this example, a new share, `my-fs-cli-1` is created with a replica file share, `my-fs-cli-1-replicate`. The replica is created in a different zone in the same region.

```sh
$ ibmcloud is share-create --name my-fs-cli-1 --zone br sao-02 --profile dp2 --size 40 --mount-targets `[
   { 
      "name": "my-target1"
      "virtual_network_interface": {
         "name": "my-fs-cli-1-vni",
         "primary_ip": {
            "id": "02u7-177537ff-0732-4cf7-8711-c465a03e680d"
         },
         "resource_group": {
            "id": "bdd96715c2a44f2bb60df4ff14a543f5"
         },
         "security_groups": [
            {
               "id": "r042-15757ab8-9df9-40f2-9210-405a394fc8d0"
            }
         ]
      }
   }]` --replica-share-profile dp2 --replica-cron-spec '55 09 * * *' --replica-zone br-sao-3 --replica-share-name my-fs-cli-1-replica --replica-share-mount-target '[
      {
         "name": "my-target1",
         "virtual_network_interface": {
            "name": "my-fs-cli-1-vni",
            "primary_ip": {
               "address": "10.250.128.12"
               "Auto-delete": true
               "name": "rip-vni-target"
               },
         },      
         "resource_group": {
            "id": "bdd96715c2a44f2bb60df4ff14a543f5"
         },
         "security_groups": [
            {
               "id": "r042-15757ab8-9df9-40f2-9210-405a394fc8d0"
            }
         ],
         "subnet": {
            "id": "02v7_a08ccb9b-7ac2-4d76-ab5e-b414042e5ff2"
         }
      }
]`   
Creating file share my-fs-cli-1 under account VPC as user myuser@mycompany.com...

ID                   r042-90c12f7b-6fbd-489a-9e4-99ad6555ffaf
Name                 my-fs-cli-1
CRN                  crn:v1:bluemix:public:is:br-sao-2:a/efe5afc483594adaa8325e2b4d1290df::share:042-90c12f7b-6fbd-489a-9e4-99ad6555ffaf
Lifecycle state      pending
Access control mode  security_group
Zone                 br-sao-2
Profile              dp2
Size(GB)             40
IOPS                 100
Encryption           provider_managed
Mount targets        ID                                        Name  
                     r042-ae28v388-1ab2-45f6-ad3c-54382395baa7 my-target1

Resource group       ID                                 Name
                     bdd96715c2a44f2bb60df4ff14a543f5   Default

Created              2023-08-08T03:19:58+05:30
Last sync at         -
Replication share    ID                                         Name                 Resource type
                     r042-66b59863-4914-44ea-8dff-bc875c049c0a  my-fs-cli-1-replica  share

Replication role     source
Replication status   none
```
{: screen}

### Updating an existing file share for replication from the CLI
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

For more information about the command options, see [`ibmcloud is share-create`](/docs/vpc?topic=vpc-vpc-reference#share-create).

The following example creates a replica file share for a source file share that is identified by ID.

```sh
$ ibmcloud is share-replica-create --name replica-share-3 --zone us-south-3 --profile tier-5iops --replication-cron-spec '10 05 * * *' --source-share 2c4c32f9-9d25-43df-9d59-3874a81ec46e

Creating replica file share replica-share-3 under account VPC as user myuser@mycompany.com...

ID                      4de3d510-b0db-4674-9dea-c9b93e26b1c3
Name                    replica-share-3
CRN                     crn:v1:bluemix:public:is:us-south-3:a/efe5afc483594adaa8325e2b4d1290df::share:4de3d510-b0db-4674-9dea-c9b93e26b1c3
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

For more information about the command options, see [`ibmcloud is share-replica-create`](/docs/vpc?topic=vpc-vpc-reference#share-replica-create).

## Adding replication to file share with the API
{: #fs-create-replica-api}
{: api}

You can programmatically set up replication by calling the `/shares` method in the [VPC API](/apidocs/vpc/latest#create-share){: external} as shown in the following sample requests.

Before you begin, first set up the [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=api). For more information about the file shares VPC API, see the [VPC API reference](/apidocs/vpc/latest).

### Creating a file share with replication with the API
{: #fs-create-new-share-replica-api}

When you create a file share, you can specify that a replica file share is also created in a different zone. Make a `POST /shares` request and specify the `replica_share` property to define the replica file share.

The following example creates the replica `test-replica-001` for the source share `source-share-001`. Mount targets, which are optional when you create a file share, are specified for the replica file share and source file share.

```sh
curl -X POST\
"$rias_endpoint/v1/shares?version=2023-08-08&generation=2"\
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

### Creating a replica for an existing file share with the API
{: #fs-create-share-replica-api}

To add replication to an existing file share, you need to create a replica share in the target region. Make a `POST /shares` request in the target region for a new share, specify the replica share name or CRN, and include the ID of the `source_share`.
Other required properties are the `profile`, `zone`, and `replication_cron_spec`, which provides the replication schedule.

```sh
curl -X POST\
"$rias_endpoint/v1/shares?version=2023-08-08&generation=2"\
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

## Adding replication to file share with Terraform
{: #fs-create-replica-terraform}
{: terraform}

You can use the `ibm_is_share` resource in Terraform to create a file share with replication, or update a file share to include replication. The following example creates a replica share in the `us-south-3` zone and associates it to the parent share that is specified by its ID, `ibm_is_share.example.id`.

```terraform
resource "ibm_is_share" "example-1" {
  zone                  = "us-south-3"
  source_share          = ibm_is_share.example.id
  name                  = "my-replica1"
  profile               = "dp2"
  replication_cron_spec = "0 */5 * * *"
}
```
{: codeblock}

The following example creates a file share in `us-south-1` with a replica in `us-south-3`.

```terraform
resource "ibm_is_share" "example-2" {
  zone    = "us-south-1"
  size    = 220
  name    = "my-share"
  profile = "dp2"
  replica_share {
    name                  = "my-replica"
    replication_cron_spec = "0 */5 * * *"
    profile               = "dp2"
    zone                  = "us-south-3"
  }
}
```

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

## Next steps
{: #fs-repl-next-steps}

* [Manage replication](/docs/vpc?topic=vpc-file-storage-manage-replication).

* [Fail over to the replica site](/docs/vpc?topic=vpc-file-storage-failover).
