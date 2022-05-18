---

copyright:
  years: 2022
lastupdated: "2022-05-18"

keywords:

subcollection: vpc

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:preview: .preview}
{:external: target="_blank" .external}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating replica file shares
{: #file-storage-create-replication}

Create replica file share in a different zone in your region in the UI, CLI, or API. 
{: shortdesc}

File Storage for VPC is available for customers with special approval to preview this service in the Washington, Dallas, Frankfurt, London, Sydney, Sao Paulo, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## Add replication to a file share in the UI
{: #fs-create-replica-ui}
{: ui}

Add replication to a new file share you create or update an existing file share to include replication. In both cases, replica file shares are created in the designated zone. The zone must be different than the primary file share zone and in the same region.

### Create a replica file share when creating a new file share in the UI
{: #fs-create-new-share-replica-ui}

Creating a replica file share during file share provisioning adds a few extra steps to the regular provisioning process.
Specify a different zone for the replica file share and configure replication.

1. Provision a new file share as in [Create a file share and mount target in the UI](/docs/vpc?topic=vpc-file-storage-create&interface=ui#fs-create-share-target-ui).

2. In the File share replica field, click **Create replica**.

In the side panel, define the replica and settings:

1. Provide a name for the replica share.

2. Optionally specify [user tags](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags). Tags you apply to the replica can be the same or different than the source share.

3. Specify a resource group or use the default.

4. Specify the zone in which it will be created. The UI presents a different zone from the source share in the same region.

5. Optionally, create a mount target for the replica share. Mount targets are dependent on the VPC in the zone you're usng. You can specify a mount target later.
    a. Provide a mount target name.
    b. Select a virtual private cloud.
    c. Click **Save**.

6. Select a [profile](/docs/vpc?topic=vpc-file-storage-profiles) for the replica file share. The profile can be different than the source file share. Selecting a different replia profile determines the performance you'll get when you [perform a failover](/docs/vpc?topic=vpc-file-storage-failover). Note that dpending on the source volume size, some profiles might be disabled. 

    The replica size (GBs) is inherited from the source volume. 

7. Specify how often you'd like to sync changes from the primary file share to the replica share. The Summary shows the selections you made. For **Frequency**, options are hourly, daily, weekly, monthly, or by `cron-spec` expression:

    * For hourly, specify 0 to signify hourly replication.
    * For daily, specify the starting time in hours and minutes. UTC is converted into your local time.
    * For weekly, specify the days of the week you want replication to run. 
    * For monthly, choose a day from 1 to 28.
    * If you specify a `cron-spec` expression, replications must be scheduled not less than one hour. 

8. The encryption is inherited from the primary share. If you specified customer-managed encryption, the key management system is shown along with the root key. You can't encrypt a replica share with a different key.

9. Click **Create replica**. The side panel closes and the replica information is stored in memory. On the create file share page, replica details card summarizes the information you provided.

    After you create the replica, if you change anything on the source file share page (for example, the share size), the replica is invalidated, indicated by a red dot next to the setting. From the replica details card, click the overflow menu (...) and select **Edit**. The replica side panel displays and you can make changes, such as updating the profile for the new size. You can also delete the replica at this point.
    {: note}

10. Click **Add to estimate** in the side panel to save the billing information.

11. Click **Create file share** to create the file share and replica. Billing starts at the next interval.  

### Update an existing file share for replication in the UI
{: #fs-update-share-replica-ui}

Navigate to te list of file shares to set up replication.

1. In the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/vpc-ext){: external}, go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > File Shares**.

2. The File Shares for VPC list page shows all file shares that are created in that zone. Identify the file share for which you want to set up replication. The file share should be in a `stable` state.

3. From the overflow menu at the end of the file share row, select **Create replica**. The File share replica page displays and shows information about the source file share. The replica inherits the profile, size, and encryption of the source share.

4. Provide a name for the replica share and then specify the zone in which it will be created. The UI presents a different zone from the source share in the same region.

5. Optionally, specify a resource group and [user tags](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags).

6. Optionally, create mount targets for the replica share. 
    a. Provide a mount target name.
    b. Select a virtual private cloud.
    c. Click **Create**.

7. Select a [profile](/docs/vpc?topic=vpc-file-storage-profiles) for the replica file share. The profile can be different than the source file share.

  The file share size is inherited from the source file share. The first replication will be the entire contents of the source share.

8.  Specify how often you'd like to sync changes from the primary file share to the replica share. The Summary shows the selections you made. For **Frequency**, options are hourly, daily, weekly, monthly, or by `cron-spec` expression.

    * For hourly, specify 0 to signify hourly replication.
    * For daily, specify the starting time in hours and minutes. UTC is converted into your local time.
    * For weekly, specify the days of the week you want replication to run. 
    * For monthly, choose a day from 1 to 28.
    * If you specify a `cron-spec` expression, replications must be scheduled not less than one hour. 

9. The encryption is inherited from the primary share. If you specified customer-managed encryption, the key management system is shown along with the root key. You can't encrypt a replica share with a different key.

10. Click **Create replica**. Summary information shows the following details:

    Source file share information: 
      * Name, region, and resource group
      * Mount points and zone
      * Encryption type

    Replica file share information:
      * Name, zone within the region, resource group, size, and profile
      * Mount points
      * Encryption type

      Total monthly cost is estimated for the share with replication.

## Add replication to file share from the CLI
{: #fs-create-replica-cli}
{: cli}

Use the CLI to create a file share with replication, or update a file share to include replication.

### Create a new file share with replication from the CLI
{: #fs-create-new-share-replica-cli}

Run the `ibmcloud is share-create` command and specify the following properties to define the replica share for a new file share:

* `--replica-share-iops`: The maximum input/output operation performance bandwidth per second for the file share; applicable for custom profile file share.
* `--replica-share-name`: Specify a name for the replica file share.
* `--replica-share-profile`:` The profile the file share uses.
* `--replica-cron-spec`: The cron specification for the file share replication schedule.
* `--replica-target`: TARGETS_JSON|@TARGETS_JSON_FILE, specify file share targets in JSON or in a JSON file.
* `--replica-zone`: The zone in which th  replica file share will reside. It must be a different zone in the same region as the source share.

Syntax:

```zsh
ibmcloud is share-create --zone ZONE_NAME --profile PROFILE [--name NAME] [--iops IOPS] [--targets TARGETS_JSON | @TARGETS_JSON_FILE] [--replica-share-profile REPLICA_SHARE_PROFILE --replica-cron-spec REPLICA_CRON_SPEC --replica-zone ZONE_NAME [--replica-share-iops REPLICA_SHARE_IOPS] [--replica-share-name REPLICA_SHARE_NAME] [--replica-target TARGETS_JSON | @TARGETS_JSON_FILE]] [--size SIZE [--encryption_key ENCRYPTION_KEY]] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
```
{: pre}

In this example, a new share named `p-share-3` is created with a replica file share, `replica-p-share-3`. The replica is created in a different zone in the same region.

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
Targets              ID                          Name   VPC ID   VPC Name
                     No mounted targets found.
                        
Resource group       ID                                 Name
                     11caaa983d9c4beb82690daab08717e9   Default
                        
Created              2022-04-26T03:19:58+05:30
Last sync at         2022-04-21T05:53:28+05:53
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

```zsh
ibmcloud is share-replica-create --zone ZONE_NAME --profile PROFILE [--name NAME] [--iops IOPS] [--targets TARGETS_JSON | @TARGETS_JSON_FILE] [--replication-cron-spec REPLICATION_CRON_SPEC --source-share SOURCE_SHARE] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--output JSON] [-q, --quiet]
```
{: pre}

This example creates a replica file share for a source file share identified by ID:

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
Targets                 ID                          Name   VPC ID   VPC Name
                        No mounted targets found.
                           
Resource group          ID                                 Name
                        11caaa983d9c4beb82690daab08717e9   Default
                           
Created                 2022-04-28T10:45:13+05:30
Last sync at            2022-04-28T05:53:28+05:53
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

When you create a new file share, you can specify that a replica file share be created in a different zone. Make a `POST/shares` request and specify the `replica_share` property to define the replica file share.

This example creates the replica `test-replica-001` for the source share `source-share-001`. Mount targets, optional when creating a file share, are specified for the replica file share and source file share.

```curl
curl -X POST\
"$rias_endpoint/v1/shares?version=2022-05-03&generation=2\
-H "Authorization: $iam_token"\
-d '{
    "name": "source-share-001",
    "profile":{
       "name":"tier-3iops"
    },
    "size":10,
    "targets":[
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
        "targets":[
           {
              "vpc":{
                "id":"9380990e-4b3b-4d79-80fe-ee052fb9772a"
              }
           }
        ]
      }
   }'
```
{: pre}


### Update an existing file share to add replication with the API
{: #fs-create-share-replica-api}

Make a `POST/shares` request to define the replica file share to add to a file share. In the example, `source_share` specifies the ID of the source file share to which you're adding replication. You also specify the source share name or CRN.

Other required properties are the `profile`, `zone`, and `replication_cron_spec`, which provides the replication schedule. 

```curl
curl -X POST\
"$rias_endpoint/v1/shares?version=2022-05-03&generation=2\
-H "Authorization: $iam_token"\
-d '{
    "source_share": {
        "id": "4aafd9c9-5555-4bdb-902d-d63d4dcf5adc"
     },
    "name": "replica-share-1",
        "profile": "tier-3iops"
     },
     "zone": {
        "name": "us-south-1"
     },
     "replication_cron_spec": "0 */5 * * *",
  }'
```
{: pre}

## Next steps
{: #fs-repl-next-steps}

* [Manage replication](/docs/vpc?topic=vpc-file-storage-manage-replication).

* [Fail over to the replica site](/docs/vpc?topic=vpc-file-storage-failover).
