---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-24"

keywords: file share, file storage, source volume, replica share, 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating replica file shares
{: #file-storage-create-replication}

Create a replica file share in the console, from the CLI, with the API, or with Terraform. Replica file shares can be created in another zone of the same metro region as the primary share's zone, or a zone of a different metro region in the same geography.
{: shortdesc}

The following table shows which metro regions can replicate with each other within each geography. 

| Americas | Europe  | Asia  |
|----------|---------|-------|
| - Dallas, TX / `us-south` \n - Montreal / `ca-mon` \n - Sao Paulo / `br-sao` \n - Toronto / `ca-tor` \n - Washington, DC / `us-east` |  - Frankfurt / `eu-de` \n - London / `eu-gb` \n - Madrid / `eu-es`| - Osaka/ `jp-osa` \n - Sydney / `au-syd`\n - Tokyo / `jp-tok` |
{: caption="This table shows the metro regions that can replicate with each other in each geography. Every geography is a separate column." caption-side="bottom"}

The specified source file share must not have another replica already, and must not be a replica of another share.
{: requirement}

If you want to create a replica in another region, you need to establish service-to-service authorizations first. Both file service instances must belong to the same account. Cross-account replication is not supported. For more information, see [Establishing service-to-service authorizations for {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-file-s2s-auth).
{: requirement}

[Select availability]{: tag-green} Customers with special access to preview the new regional file share offering can use the **rfs** profile to create file shares with regional availability. When you create file shares with regional availability, data is automatically replicated throughout the region, so you don't need to set up replication pairs. Cross-regional replication of regional file shares is not supported in this release.

Currently, cross-regional replication for zonal file shares is not supported in the Chennai - Airtel region.
{: restriction}

## Adding replication to a file share in the console
{: #fs-create-replica-ui}
{: ui}

You can create a replica of your file share from the list of all file shares or the file share details page. If you don't already have a source file share, provision one as described in [Create a file share and mount target in the console](/docs/vpc?topic=vpc-file-storage-create&interface=ui#fs-create-share-target-ui). When the file share appears as "stable" on the File storage shares for VPC page, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") and click **Create replica**.

On the File share replica create page, review the source file share details, and complete the replica details.

1. Name - Provide a unique name for the replica share.
1. Replica location - The geography is preselected and cannot be changed. Select the metro (region) and zone in which the replica share is to be created. The UI presents the available options in the menu.
1. Resource group - Select the resource group from the list.
1. Tags - Optionally, specify [user tags](/docs/vpc?topic=vpc-file-storage-managing&interface=ui#fs-add-user-tags). The tags that you apply to the replica can be the same as or different from the source share's tags.
1. Profile - The `dp2` profile is preselected, even if the source file share is based on a profile from a previous release. Specify the maximum value for IOPS. It determines the performance that you get on the replica after you [perform a failover](/docs/vpc?topic=vpc-file-storage-failover).
1. Mount Targets - Optionally, create a mount target for the replica share. You can skip this step if you do not want to create a mount target now. Otherwise, click **Create**. You can create one mount target per VPC per file share. Depending on the [mount target access mode](/docs/vpc?topic=vpc-file-storage-vpc-about&interface=ui#fs-mount-access-mode) of the share, the **Create mount target** form looks different.

   - If you selected security group as the access mode, enter the information as described in Table 2. This action creates and attaches a [virtual network interface](/docs/vpc?topic=vpc-vni-about) to your mount target that identifies the file share with a reserved IP address and applies the rules of the selected Security group. This mount target supports encryption-in-transit and cross-zone mounting.

     | Field | Value |
     |-------|-------|
     | **Details** | |
     | Mount target name | Specify a mount target name. The name can be up to 63 lowercase alpha-numeric characters and include the hyphen (-), and must begin with a lowercase letter. You can later edit the name if you want. |
     | Zone | Zone is inherited from the file share (for example, Dallas 2). |
     | VPC | Select an available VPC. The list includes only those VPCs with a subnet in the selected zone. |
     | Subnet | Select a subnet from the list. |
     | **Reserved IP address** | Required for the mount target. The IP address cannot be changed afterward. However, you can delete the mount target and create another one with a different IP address. |
     | Reserving method | You can have the file service select an IP address for you. The reserved IP becomes visible after the mount target is created. Or, specify your own IP. |
     | Auto-release | Releases the IP address when you delete the mount target. Enabled by default. |
     | **Security groups** | The [default security group](/docs/vpc?topic=vpc-updating-the-default-security-group) for the VPC is selected. You can use it or select another security group from the list. |
     {: caption="Values for creating a mount target." caption-side="top"}

   - If you selected VPC as the access mode, provide a name for the mount target and select a VPC from the list. This mount target can be used to mount the file share on any virtual server instance of the selected VPC in the same zone as the file share. Cross-zone mounting is not supported.

1. Sync frequency - Specify how often you want to synchronize changes from the primary file share to the replica share. The Summary shows the selections that you made. For **Frequency**, the options are hourly, daily, weekly, monthly, or by `cron-spec` expression:
   * For hourly replication, enter a value in the range 0 - 60 to specify exactly how many minutes past the hour, every hour, every day the replication is to start.
   * For daily replication, specify the starting time in hours and minutes in Coordinated Universal Time. Enter a value between 00:00 and 23:59. For your convenience, the Coordinated Universal Time value is converted into your local time.
   * For weekly replication, specify the days of the week you want replication to run and the start time in Coordinated Universal Time. Enter a value between 00:00 and 23:59.
   * For monthly replication, choose a Day 1 - 28. For the start time, enter a value between 00:00 and 23:59.
   * If you specify a `cron-spec` expression, replications must be scheduled not less than 15 minutes. Enter the replication frequency in `cron-spec` format: minute, hour, day, month, and weekday. For example, to replicate every day at 5:30 PM you need to enter `30 17 * * *`.

1. Encryption
   * **Encryption in transit** is disabled by default. You can click the toggle to enable it. For more information about this feature, see [Encryption in transit - Securing mount connections between file share and host](/docs/vpc?topic=vpc-file-storage-vpc-eit). |
   * When you replicate to another zone of the same region, the encryption is inherited from the primary share. If you selected customer-managed encryption, the key management system is shown along with the root key. You can't encrypt a replica share with a different key.
   * When you replicate to another region, the encryption type (provider-managed vs customer-managed) of the replica must match the source share. However, it is not inherited from the source, and you must select a Customer Root Key for your replica if the source share is protected by customer-managed encryption.
  
   | Field | Value |
   |------|------|
   | Encryption | Select either {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}. |
   | Encryption service instance | If you provisioned multiple KMS instances in your account, select the one that includes the root key that you want to use for customer-managed encryption. Ensure that [service-to-service authorizations](/docs/vpc?topic=vpc-file-s2s-auth) between the file service and the target KMS are in place. |
   | Key name | Select the root key within the KMS instance that you want to use for encrypting the share. |
   | Key ID | The field shows the key ID that is associated with the data encryption key that you selected. |
   {: caption="Values for customer-managed encryption for file shares." caption-side="bottom"}

1. In the side panel, review your estimated cost, and apply a discount code, if you have one.
1. Click **Create file share**.
   
If you're not ready to order yet or just looking for pricing information, you can add the information that you see in the side panel to an Estimate. For more information about how this feature works, see [Estimating your costs](/docs/account?topic=account-cost).    
{: tip}

## Adding replication to file share from the CLI
{: #fs-create-replica-cli}
{: cli}

You can use the CLI to create a file share with a replica share in another zone, or region, or to create a replica share for an existing file share.

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).

### Creating a file share with a replica from the CLI
{: #fs-create-new-share-replica-cli}

When you use the `ibmcloud is share-create` command to create your share, you can create a replica share at the same time by specifying values for the options `--replica-share-name`, `--replica-share-profile`, `--replica-share-cron-spec`,`--replica-share-zone`. The cron-spec specifies the replication frequency, and you can schedule to replicate your data as often as 15 minutes. For more information about the command options, see [`ibmcloud is share-create`](/docs/vpc?topic=vpc-vpc-reference#share-create).

In the following example, a share `my-source-file-share` is created in `us-south-2` with a replica file share `my-replica-file-share` in `us-south-1`. In this example, only one mount target is created for the source file share. You could also create a mount target for the replica share by using the same JSON syntax with the `--replica-share-mount-targets` option.

```sh
ibmcloud is share-create --name my-source-file-share --zone us-south-2 --profile dp2 --size 1500 --iops 2000  --user-tags env:dev --mount-targets '[{"name":"my-source-mount-target","virtual_network_interface": {"name":"my-source-vni","subnet": {"id":"0717-c66032c9-048d-4c35-aa83-c932e24afdbb"}}}]' --replica-share-name my-replica-file-share --replica-share-profile dp2 --replica-share-cron-spec '55 09 * * *' --replica-share-zone us-south-1
```
{: pre}

```sh
Creating file share my-source-file-share under account Test Account as user test.user@ibm.com...
                                
ID                                 r006-0a3444bc-df55-4c2e-aef4-f37a9a8d8262   
Name                               my-source-file-share   
CRN                                crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-0a3444bc-df55-4c2e-aef4-f37a9a8d8262   
Lifecycle state                    pending   
Access control mode                security_group   
Accessor binding role              none   
Allowed transit encryption modes   user_managed,none   
Zone                               us-south-2   
Profile                            dp2   
Size(GB)                           1500   
IOPS                               2000   
User Tags                          env:dev   
Encryption                         provider_managed   
Mount Targets                      ID                                          Name      
                                   r006-67799c85-09cf-4cfe-811d-effa10c99395   my-source-mount-target      
                                      
Resource group                     ID                                 Name      
                                   6edefe513d934fdd872e78ee6a8e73ef   defaults      
                                      
Created                            2025-08-01T17:25:05+00:00   
Replication share                  ID                                          Name                    Resource type      
                                   r006-2068a709-8c18-4844-8c1d-42b83889dfb3   my-replica-file-share   share      
                                      
Replication role                   source   
Replication status                 none   
Replication status reasons         Status code   Status message      
                                   -             -      
                                      
Snapshot count                     0   
Snapshot size                      0   
Source snapshot                    -   
Allowed Access Protocols           nfs4    
Availability Mode                  zonal   
Bandwidth(Mbps)                    1    
Storage Generation                 1  
```
{: screen}

### Creating a replica for an existing file share from the CLI
{: #fs-create-share-replica-cli}

1. Locate your source file share from the CLI by listing your file shares in the region with the `ibmcloud is shares` command.
   ```sh
   ibmcloud is shares
   ```
   {: pre}

   ```sh
   Listing shares in all resource groups and region us-south under account Test Account as user test.user@ibm.com...
   ID                                          Name                    Lifecycle state   Zone         Profile   Size(GB)   Resource group   Replication role   Accessor binding role   Snapshot count   Snapshot size   
   r006-a8d6af48-0c97-4c6b-bab1-fbefdc1e1e03   my-file-share           stable            us-south-2   dp2       10         defaults         none               none                    0                0   
   r006-aaf4bfe9-358c-4faa-a4ec-0b955090b940   my-file-share-2         stable            us-south-2   dp2       10         defaults         none               none                    0                0   
   r006-a60bfa90-a893-40ad-be34-28ab51a963f9   replica-dal-2           stable            us-south-2   dp2       10         defaults         replica            none                    0                0   
   r006-3f21e3c3-e12d-425f-ab77-810cabfde8df   source-dal-1            stable            us-south-1   dp2       10         defaults         source             none                    0                0   
   r006-455b601c-8fc1-4476-8771-4708c49c8ef7   my-replica-share-dal-1  stable            us-south-1   dp2       10         defaults         replica            none                    0                0   
   r006-4dadac27-cd17-42df-a5fe-1388705d33e0   my-source-share-dal-2   stable            us-south-2   dp2       10         defaults         source             none                    0                0   
   ```
   {: screen}

1. View the details of the file share that you want to use as your source with the `ibmcloud is share` command. You can use the share's name or ID when you create a replica in the same region. If you create the replica in another region, take note of the CRN of the source file share.
   
   ```sh
   ibmcloud is share my-file-share
   ```
   {: pre}

   ```sh
   Getting file share my-file-share under account Test Account as user test.user@ibm.com...
                                
   ID                           r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
   Name                         my-file-share   
   CRN                          crn:v1:bluemix:public:is:us-south-2:a/a1234567::share:r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   
   Lifecycle state              stable   
   Access control mode          security_group   
   Accessor binding role        none 
   Zone                         us-south-2   
   Profile                      dp2   
   Size(GB)                     1000   
   IOPS                         1000  
   User Tags                    docs:test 
   Encryption                   provider_managed   
   Mount Targets                ID                                          Name      
                                r006-dd497561-c7c9-4dfb-af0a-c84eeee78b61   my-cli-share-mount-target-1      
                                
   Resource group               ID                                 Name      
                                db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
   Created                      2023-10-18T22:15:15+00:00   

   Replication role             none   
   Replication status           none   
   Replication status reasons   Status code   Status message      
                                -             -     

   Snapshot count               0
   Snapshot size                0
   Source snapshot              -
   Allowed Access Protocols     nfs4    
   Availability Mode            zonal   
   Bandwidth(Mbps)              1    
   Storage Generation           1  
   ```
   {: screen}

1. Create a replica share by running the `ibmcloud is share-replica-create` command in the target region. If you're not targeting the right region, use the `ibmcloud target -r REGION` command to switch the target region. Specify the source share by name, ID, or CRN. Provide values to define the zone where the replica file share is going to be created, and the profile of the replica share. Specify the replication schedule with a cron expression. If the source file share has `user_managed` encryption, you must provide the `--encryption_key`. The `--encryption_key` property must not be specified otherwise.

   ```sh
   ibmcloud is share-replica-create --name my-replica-share --zone us-south-3 --profile dp2 --replication-cron-spec '10 05 * * *' --source-share my-file-share
   ```
   {: pre}

   ```sh
   Creating replica file share my-replica-share under account Test Account as user test.user@ibm.com...
                                
   ID                               r006-6d1719da-f790-45cc-9f68-896fd5673a1a   
   Name                             my-replica-share   
   CRN                              crn:v1:bluemix:public:is:us-south-3:a/a1234567::share:r006-6d1719da-f790-45cc-9f68-896fd5673a1a   
   Lifecycle state                  pending   
   Access control mode              security_group
   Accessor binding role            origin
   Allowed transit encryption modes ipsec,none
   Zone                             us-south-3   
   Profile                          dp2   
   Size(GB)                         1000   
   IOPS                             100   
   User Tags                        docs:test 
   Encryption                       provider_managed      
   Mount Targets                    ID                          Name      
                                    No mounted targets found.      
                                
   Resource group                   ID                                 Name      
                                    db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
   Created                          2024-06-25T15:13:18+00:00   
   Latest job                       Job status   Job status reasons      
                                    running      -      
                                
   Replication cron spec            10 05 * * *   
   Replication role                 replica   
   Replication status               initializing   
   Replication status reasons       Status code   Status message      
                                    -             -      
                                
   Source share                     ID                                          Name            Resource type      
                                    r006-b696742a-92ee-4f6a-bfd7-921d6ddf8fa6   my-file-share   share 
   Snapshot count                   0
   Snapshot size                    0         
   Source snapshot                  - 
   Allowed Access Protocols         nfs4    
   Availability Mode                zonal   
   Bandwidth(Mbps)                  1    
   Storage Generation               1                   
   ```
   {: screen}

When you create a replica of a file share in another region, you must use the CRN of the source file share. If the source file share has `user_managed` encryption, you must provide the `encryption_key`. The `encryption_key` value must not be specified otherwise. See the following example.

   ```sh
   ibmcloud is share-replica-create --name my-replica-share --zone us-east-1 --profile dp2 --replication-cron-spec '5 * * * *' --source-share crn:v1:bluemix:public:is:us-south-1:a/a1234567::share:r006-d8c8821c-a227-451d-a9ed-0c0cd2358829 --encryption-key crn:v1:bluemix:public:kms:us-south:a/a1234567:1be45161-6dae-44ca-b248-837f98004057:key:3dd21cc5-cc20-4f7c-bc62-8ec9a8a3d1bd
   ```
   {: pre}

   ```sh
   Creating replica file share my-cross-regional-replica-share under account Test Account as user test.user@ibm.com...
                                
   ID                               r006-6d1719da-g687-45ac-9f68-896fd76843a1b    
   Name                             my-cross-regional-replica-share   
   CRN                              crn:v1:bluemix:public:is:us-east-1:a/a1234567::share:r006-6d1719da-g687-45ac-9f68-896fd76843a1b   
   Lifecycle state                  pending   
   Access control mode              security_group
   Accessor binding role            origin
   Allowed transit encryption modes ipsec,none
   Zone                             us-east-1   
   Profile                          dp2   
   Size(GB)                         1000   
   IOPS                             100   
   Encryption                       user_managed   
   Mount Targets                    ID                          Name      
                                    No mounted targets found.      
                                
   Resource group                   ID                                 Name      
                                    db8e8d865a83e0aae03f25a492c5b39e   Default      
                                
   Created                          2024-06-25T15:13:18+00:00   
   Encryption key                   crn:v1:bluemix:public:kms:us-south:a/a1234567:1be45161-6dae-44ca-b248-837f98004057:key:3dd21cc5-cc20-4f7c-bc62-8ec9a8a3d1bd
   Latest job                       Job status   Job status reasons      
                                    running      -      
                                
   Replication cron spec            5 * * * *   
   Replication role                 replica   
   Replication status               initializing   
   Replication status reasons       Status code   Status message      
                                    -             -      
                                
   Source share                     ID                                          Name       Resource type  Remote
                                    r006-d8c8821c-a227-451d-a9ed-0c0cd2358829   my-share   share          us-south

   Snapshot count                   0
   Snapshot size                    0
   Source snapshot                  - 
   Allowed Access Protocols         nfs4    
   Availability Mode                zonal   
   Bandwidth(Mbps)                  1    
   Storage Generation               1  
   ```
   {: screen}

For more information about the command options, see [`ibmcloud is share-replica-create`](/docs/vpc?topic=vpc-vpc-reference#share-replica-create).

## Adding replication to file share with the API
{: #fs-create-replica-api}
{: api}

You can programmatically set up replication by calling the `/shares` method in the [VPC API](/apidocs/vpc/latest#create-share){: external} as shown in the following sample requests.

Before you begin, first set up the [API environment](/docs/vpc?topic=vpc-set-up-environment&interface=api). For more information about the file shares VPC API, see the [VPC API reference](/apidocs/vpc/latest).

### Creating a file share with a replica with the API
{: #fs-create-new-share-replica-api}

When you create a file share, you can specify that a replica file share is also created in a different zone. Make a `POST /shares` request and specify the `replica_share` property to define the replica file share.

The following example creates the replica `test-replica-001` for the source share `source-share-001`. Mount targets, which are optional when you create a file share, are specified for the replica file share and source file share.

```sh
curl -X POST\
"$vpc_api_endpoint/v1/shares?version=2023-08-08&generation=2"\
-H "Authorization: $iam_token"\
-d '{
    "name": "source-share-001",
    "profile":{"name":"dp2"},
    "size":100,
    "iops": 400,
    "mount-targets":[{"vpc":{"id":"08669c86-4c8a-4bfa-8ddc-37071f955c52"}],
    "zone":{"name":"us-south-1"},
    "replica_share":{
  	   "name": "test-replica-001",
      "profile":{"name":"dp2"},
      "replication_cron_spec":"0 */5 * * *",
      "zone":{"name":"us-south-3"},
      "mount_targets": [
        {"virtual_network_interface": {"subnet": {"id": "2302-ea5fe79f-52c3-4f05-86ae-9540a10489f5"}}},
        {"vpc": {"id": "7ec86020-1c6e-4889-b3f0-a15f2e50f87e"}}],
      "resource_group": {},
      "allowed_transit_encryption_modes": "ipsec",
      "user_tags": ["string"]}}]
      }
    "resource_group": {},
    "allowed_transit_encryption_modes": "ipsec",
    "user_tags": ["string"]
   }'
```
{: pre}

### Creating a replica for an existing file share with the API
{: #fs-create-share-replica-api}

To add replication to an existing file share, you need to create a replica share in a different zone of your region. In the `POST /shares` request, specify the replica share's name and profile, and specify the `source_share` by either its name, ID, or CRN. Other required properties are the `zone`, and `replication_cron_spec`, which provides the replication schedule. You can schedule to replicate your data as often as 15 minutes.

The following example shows an API request that creates a replica share in the `us-south-1` zone. In this example, the source share is in another `us-south` zone and is identified by its ID.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/shares?version=2023-08-28&generation=2"\
  -H "Authorization: Bearer $iam_token" \
  -d '{
  "source_share": {"id": "4aafd9c9-5555-4bdb-902d-d63d4dcf5adc"},
  "mount_targets": [],
  "name": "my-replica-share",
  "profile": {"name": "dp2"},
  "size": 100,
  "zone": {"name": "us-south-1"},
  "iops": 300,
  "replication_cron_spec": "00 05 * * 0",
  "resource_group": {"id": "6edefe513d934fdd872e78ee6a8e73ef"},
  "access_control_mode": "security_group",
  "allowed_transit_encryption_modes": "ipsec,none"
}'
```
{: pre}

When you create a replica of a file share in another region, you must use the CRN of the source file share. If the source file share has `user_managed` encryption, you must provide the `encryption_key`. The `encryption_key` value must not be specified otherwise.
{: requirement}

You can use the API to verify that the replication succeeded, is pending, or failed. Make a `GET /shares/{replica_id}` call. Look at the `latest_job` property. For more information, see [Verify replication with the API](/docs/vpc?topic=vpc-file-storage-manage-replication&interface=api#fs-verify-replica-api).
{: note}

## Adding replication to file share with Terraform
{: #fs-create-replica-terraform}
{: terraform}

You can use the `ibm_is_share` resource in Terraform to create a file share with replication, or update a file share to include replication. The following example creates a replica share in the `us-south-3` zone and associates it to the parent share that is specified by its ID `ibm_is_share.example.id`.

```terraform
resource "ibm_is_share" "my-replica1" {
  zone                  = "us-south-3"
  source_share          = ibm_is_share.example.id
  name                  = "my-replica1"
  profile               = "dp2"
  replication_cron_spec = "0 */5 * * *"
}
```
{: screen}

The following example creates a file share in `us-south-1` with a replica in `us-south-3`.

```terraform
resource "ibm_is_share" "my-replica" {
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
{: screen}

When you create a replica of a file share in another region, you must use the CRN of the source file share. If the source file share has `user_managed` encryption, you must provide the `encryption_key`. The `encryption_key` value must not be specified otherwise. See the following example.

```terraform
resource "ibm_is_share" "my-cross-regional-replica" {
  zone    = "us-east-1"
  source_share_crn = "crn:v1:bluemix:public:is:us-south-1:a/a1234567::share:r006-d8c8821c-a227-451d-a9ed-0c0cd2358829"
  encryption_key = "crn:v1:bluemix:public:kms:us-south:a/a1234567:1be45161-6dae-44ca-b248-837f98004057:key:3dd21cc5-cc20-4f7c-bc62-8ec9a8a3d1bd"
  replication_cron_spec = "5 * * * *"
  name    = "my-cross-regional-replica"
  profile = "dp2"
}
```
{: screen}

For more information about the arguments and attributes, see [ibm_is_share](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_share){: external}.

## Next steps
{: #fs-repl-next-steps}

* [Manage replication](/docs/vpc?topic=vpc-file-storage-manage-replication).
* [Fail over to the replica site](/docs/vpc?topic=vpc-file-storage-failover).
