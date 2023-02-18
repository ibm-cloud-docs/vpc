---

copyright:
  years: 2021, 2023
lastupdated: "2023-02-17"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating snapshots
{: #snapshots-vpc-create}

With the UI, CLI, or API, you can create a snapshot of a {{site.data.keyword.block_storage_is_short}} volume that is attached to a virtual server instance. You can create a snapshot of a boot or a data volume. If the volume is not attached to a server instance, you cannot create a snapshot of it.
{: shortdesc}

Before you take a snapshot, make sure that all cached data is present on disk, especially when you're taking a snapshot of instances with Windows and Linux&reg; operating systems. For example, on Linux&reg; operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{: note}

## Create a snapshot in the UI
{: #snapshots-vpc-create-ui}
{: ui}

In the console, you can create a snapshot of a {{site.data.keyword.block_storage_is_short}} volume that is attached to a running virtual server instance.

1. You can access the Block storage snapshot for VPC provisioning screen in the [{{site.data.keyword.cloud_notm}} console](/login){: external} in multiple ways. 

   - From the **[Block storage snapshots for VPC](/vpc-ext/storage/snapshots)** list: 
     1. Go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**. 
     2. From the list of snapshots that is initially empty, click **Create**.

   - From the **[Block storage volumes for VPC](/vpc-ext/storage/storageVolumes)** list: 
     1. Go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
     2. From the list of volumes, locate a boot or data volume that is attached to an instance.
     3. Click the overflow menu (...) and select **Create snapshot**.

   - From the **Block storage volume details** screen: 
     1. Go to the volume details page in one of these ways:
        * Go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**. Select the instance that contains the volume that you want to make a snapshot of. From the [instance details page](/vpc-ext/compute/vs), scroll to the list of attached volumes and click the name of the volume.
        * Go to **menu icon ![menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**. From the list of block storage volumes, select the volume that you want to make a snapshot of.
     2. On the volume details page, select **Create snapshot** from the **Actions** menu.

2. Enter the required information to define your snapshot and select the {{site.data.keyword.block_storage_is_short}} volume that you want to take a snapshot of.

   | Field | Value |
   |-------|-------|
   | Location | Specify the geography and region for this snapshot. |
   | Name  | Provide a unique name for the snapshot. The UI verifies the name for proper conventions and identifies duplicate names. For suggestions about how to name your snapshots, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
   | Resource group | Select a [resource group](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups) for the snapshot, or use the default. You can't change the resource group after the snapshot is created. |
   | Tags | Specify any [user tags that you want to identify this resource. |
   | Access management tags | Specify any [access management tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags) for this resource. |
   | Block storage volume | Select a volume from the list. The boot or data volume must be attached to a running virtual server instance. |
   | Encryption | Encryption information for the volume that you selected, either [provider-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-customer-managed-encryption). The snapshot inherits the encryption of the source volume. You can't change the encryption type.
   {: caption="Table 1. Selections for creating a snapshot" caption-side="bottom"}

4. Click **Create block storage snapshot**. You're returned to the screen that you started from. Messages are displayed while the snapshot is being created and when it's ready, the snapshot is displayed in the list of snapshots. For more information, see [View snapshot details in the UI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-snapshot-ui).

## Enable fast restore snapshot clones in the UI
{: #frsnapshots-vpc-create-ui}
{: ui}

1. To enable the [fast restore feature](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-fr), click the new snapshot to [view its details](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).
2. Scroll to the **Fast restore** card and click **Edit**. 
3. In the side panel, select the zones where you want to enable fast restore clones. 
4. Click **Save**.

## Create a snapshot from the CLI
{: #snapshots-vpc-create-cli}
{: cli}

### Before you begin
{: #snapshots-vpc-create-procedure-cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
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

3. Set the `IBMCLOUD_IS_FEATURE_SNAPSHOT` environment variable to `true`.

   ```zsh
   export IBMCLOUD_IS_FEATURE_SNAPSHOT=true
   ```
   {: pre}

Before you create a snapshot, gather the following information:
- A unique name for the snapshot to be created.
- The ID of the source volume.
- The resource group ID. You can't change the resource group after the snapshot is created.
- Any tags that you want to attach to the snapshot.

Use the following CLI commands to collect the information that you need.
- `ibmcloud is volumes`: Lists all available volumes in the region that you selected. Locate the volume in the list, verify the status (`available`), the attachment type (`boot` or `data`), and resource group.
- `ibmcloud is volume VOLUME_ID`: Use this command with the volume ID from the output of the previous command to review the details of the volume. If the output shows that the volume is available, attached to an instance and not busy, you can create a snapshot.

### Create a snapshot from the CLI
{: #snapshot-create-cli}

To create a snapshot, run the `imbcloud is snapshot-create` command.

```zsh
ibmcloud is snapshot-create --volume VOLUME_ID [--name NAME] [--resource-group-id RESOURCE_GROUP_ID --resource-group-name RESOURCE_GROUP_NAME] [--tags  TAG_NAME1,TAG_NAME2,...] [--clone-zones ZONE1,ZONE2] [--output JSON] [-q, --quiet]
```
{: pre}

For more information about available command options, see [`ibmcloud is snapshot-create`](/docs/cli?topic=cli-vpc-reference#snapshot-create).

The following example creates a snapshot with the name `cli-snapshot-test` of the data volume `block-test1` in the `eu-de-2` region. The snapshot is tagged with `env:test` and `env:prod`, and it has a fast restore snapshot clone in the `eu-de-1` zone.

```sh
cloudshell:~$ ibmcloud is snapshot-create --volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa --name cli-snapshot-test --tags env:test,env:prod --clone-zones eu-de-1
Creating snapshot cli-snapshot-test under account Test Account as user test.user@ibm.com...
                          
ID                     r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   
Name                   cli-snapshot-test   
CRN                    crn:v1:bluemix:public:is:eu-de:a/a10d63fa66daffc9b9b5286ce1533080::snapshot:r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   
Status                 pending   
Clones                 Zone      Available   Created      
                       eu-de-1   false       2023-02-17T20:15:46+00:00      
                          
Source volume          ID                                          Name      
                       r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   block-test1      
                          
Bootable               false   
Encryption             provider_managed   
Encryption key         -   
Minimum capacity(GB)   20   
Size(GB)               1   
Resource group         ID                                 Name      
                       6edefe513d934fdd872e78ee6a8e73ef   defaults      
                          
Created                2023-02-17T20:15:43+00:00   
Captured at            0001-01-01T00:00:00+00:00   
Tags                   env:prod,env:test   
```
{: screen}

The status shows `pending` while the snapshot is created. Issue the `ibmcloud is snapshot` command with the snapshot ID to see the new snapshot in `stable` status.

```sh
cloudshell:~$ ibmcloud is snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
Getting snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.userd@ibm.com...
                          
ID                     r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   
Name                   cli-snapshot-test   
CRN                    crn:v1:bluemix:public:is:eu-de:a/a10d63fa66daffc9b9b5286ce1533080::snapshot:r138-4463eb2c-4913-43b1-b9bf-62a94f74c146   
Status                 stable   
Clones                 Zone      Available   Created      
                       eu-de-1   true        2023-02-17T20:15:46+00:00      
                          
Source volume          ID                                          Name      
                       r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa   block-test1      
                          
Bootable               false   
Encryption             provider_managed   
Encryption key         -   
Minimum capacity(GB)   20   
Size(GB)               1   
Resource group         ID                                 Name      
                       6edefe513d934fdd872e78ee6a8e73ef   defaults      
                          
Created                2023-02-17T20:15:43+00:00   
Captured at            2023-02-17T20:15:44+00:00   
Tags                   env:prod,env:test
```
{: screen}

## Create a fast restore snapshot clone from the CLI
{: #snapshots-vpc-create-frclone}
{: cli}

You can create a fast restore clone by using the `ibmcloud is snapshot-clc` command with the snapshot ID and the target zone specification. For more information about available command options, see [`ibmcloud is snapshot-clone-create`](/docs/cli?topic=cli-vpc-reference#snapshot-clone-create).

The following example creates a fast restore snapshot clone of the snapshot `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146` in the `eu-de-3` zone.

```sh
cloudshell:~$ ibmcloud is snapshot-clc r138-4463eb2c-4913-43b1-b9bf-62a94f74c146  --zone eu-de-3
Creating zonal clone of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
               
Zone        eu-de-3   
Available   false   
Created     2023-02-17T20:29:21+00:00   
Href        https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-3 
```
{: screen}

The snapshot clone appears to be unavailable while the snapshot clone is created. Issue the `ibmcloud is snapshot-cl` command with the snapshot ID and the clone target zone to see the new snapshot clone as available.

```sh
cloudshell:~$ ibmcloud is snapshot-cl r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 eu-de-3
Getting zonal clone eu-de-3 of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...
               
Zone        eu-de-3   
Available   true   
Created     2023-02-17T20:29:21+00:00   
```
{: screen}

## Create a snapshot with the API
{: #snapshots-vpc-create-api}
{: api}

### Prerequisites for creating a snapshot with the API
{: #snapshots-vpc-create-procedure-api}

You can create a snapshot by calling the [VPC API](/apidocs/vpc). Before you create the snapshot, gather the following information:
- A unique name for the snapshot to be created.
- The ID of the source volume.
- The resource group ID. You can't change the resource group after the snapshot is created.
- Any tags that you want to attach to the snapshot.

### Create a snapshot with the API
{: #snapshots-vpc-create-snaphot-api}

To create a snapshot of a boot or data volume, make a `POST /snapshots`. The following example creates a snapshot of a boot volume by using the volume ID, and specifies user tags that can be associated with a [backup policy](/docs/vpc?topic=vpc-backup-service-about).

```curl
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2022-12-12&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "boot-snapshot-1",
      "source_volume": {
        "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8"
      },
      "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"
      },
      "user_tags": [
         "env:test",
         "env:prod"
      ]
    }'
```
{: codeblock}
{: curl}

A successful response looks like the following example. The snapshot lifecycle state is `pending` while the snapshot is created. When it is successfully created, the status changes to `stable`.

```json
{
  "bootable": true,
  "clones": [],
  "created_at": "2022-12-12T20:18:18Z",
  "crn": "crn:[...]",
  "deletable": false,
  "encryption": "user_managed",
  "encryption_key": {
    "crn": "crn:[...]"
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/12917904-3771-424d-8391-53ec9e305d52",
  "id": "12917904-3771-424d-8391-53ec9e305d52",
  "lifecycle_state": "pending",
  "minimum_capacity": 100,
  "name": "boot-snapshot1",
  "operating_system": {
    "architecture": "amd64",
    "dedicated_host_only": false,
    "display_name": "Ubuntu Linux&reg; 20.04 LTS Focal Fossa Minimal Install (amd64)",
    "family": "Ubuntu Linux",
    "gpu_supported": [],
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/ubuntu-20-04-amd64",
    "name": "ubuntu-20-04-amd64",
    "resource_type": "operating_system",
    "vendor": "Canonical",
    "version": "20.04 LTS Focal Fossa Minimal Install"
  },
  "resource_group": {
    "crn": "crn:[...]",
    "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
    "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4",
    "name": "Default"
  },
  "resource_type": "snapshot",
  "service_tags": [],
  "size": 1,
  "source_image": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/images/5e9c6b3c-c8e4-4c14-a6d7-08c1395d9905",
    "id": "5e9c6b3c-c8e4-4c14-a6d7-08c1395d9905",
    "name": "ibm-ubuntu-20-04-minimal-amd64-1",
    "resource_type": "image"
  },
  "source_volume": {
    "crn": "crn:[...]",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "id": "8948ad59-bc0f-7510-812f-5dc64f59fab8",
    "name": "my-instance-data",
    "resource_type": "volume"
  },
  "user_tags": [
    "env:test",
    "env:prod"
  ]
}
```
{: codeblock}

## Create a snapshot and a fast restore snapshot clone with the API
{: #snapshots-vpc-create-snaphot-clone-api}
{: api}

When you create a snapshot, you can also create a fast restore snapshot clone in another zone. By cloning a snapshot and keeping it in another zone, you can later use the fast restore feature to quickly provision a new volume with data from a snapshot. For more information, see [Snapshots fast restore feature](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=ui#snapshots_vpc_fast_restore).

Make a `POST/snapshots` request to create a snapshot of a boot or data volume and specify the `clones` property. Indicate a different zone or zones in your region from the zone in which you're creating the snapshot. In the following example, a clone is created in us-south-2, specified by name.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2022-12-18\&generation=2" \
-H "Authorization: $iam_token" \
-d '{
    "clones": [
      {
        "zone": {
            "name": "us-south-2"
        }
      }
    ],
    "name": "my-snapshot-1",
    "source_volume": {
        "id": "1a6b7274-678d-4dfb-8981-c71dd9d4daa5"
    "resource_group": {
        "id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"
    },
      "user_tags": [
         "env:test",
         "env:prod"
    ]
  }'
```
{: codeblock}
{: curl}

A successful response indicates that the clone was created in the specified zone.

```json
{
  "bootable": true,
  "clones": [
		{
			"available": false,
			"created_at": "2022-12-18T14:58:32Z",
			"zone": {
				"name": "us-south-2",
				"href": "https:ibm.com/v1/regions/us-south/zones/us-south-1",
				"resource_type": "zone"
			}
		}
	],
  "created_at": "2022-12-18T20:18:18Z",
  "crn": "crn:[...]",
  "deletable": false,
  "encryption": "user_managed",
  "encryption_key": {
     "crn": "crn:[...]"
  },
  "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/12917904-3771-424d-8391-53ec9e305d52",
  "id": "12917904-3771-424d-8391-53ec9e305d52",
  "lifecycle_state": "pending",
  .
  .
  .
}  
```
{: codeblock}

## Next steps
{: #snapshots_vpc_create_next_steps}

[View](/docs/vpc?topic=vpc-snapshots-vpc-view) all the snapshots that you created, or details about a single snapshot.

[Restore a volume](/docs/vpc?topic=vpc-snapshots-vpc-restore) from a snapshot.
