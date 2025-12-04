---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-04"

keywords: snapshots, Block Storage, snapshot clone, remote copy, fast restore, Block Storage snapshot, cross-regional snapshot

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating {{site.data.keyword.block_storage_is_short}} snapshots
{: #snapshots-vpc-create}

With the UI, CLI, API, or Terraform, you can create a snapshot of a first-generation {{site.data.keyword.block_storage_is_short}} volume that is attached to a running virtual server instance. You can create a snapshot of a boot or a data volume. If the volume is not attached to a server instance, you can't create a snapshot of it.
{: shortdesc}

In the current release of second-generation block storage volumes, you can take a snapshot of a second-generation storage volume even if it is not attached to a running virtual server instance. The feature is available in Dallas (`us-south`), Frankfurt (`eu-de`), London (`eu-gb`), Madrid (`eu-es`), Osaka (`js-osa`), Sao Paulo (`br-sao`), Sydney (`au-syd`), Tokyo (`jp-tok`), Toronto (`ca-tor`), and Washington (`us-east`) regions. Consistency group snapshots are not supported. 

[Beta]{: tag-cyan} Customers with special access can create cross-regional copies of the second-generation volume backups if the source volume exceeds 10 TB.

Before you take a snapshot, make sure that all cached data is present on disk, especially when you're taking a snapshot of instances with Windows and Linux&reg; operating systems. For example, on Linux&reg; operating systems, run the `sync` command to force an immediate write of all cached data to disk.
{: note}

You can create a consistency group that contains snapshots of multiple Gen 1 volumes that are attached to a virtual server instance. All snapshots in the consistency group are created at the same time and are loosely coupled. For more information, see [Creating snapshot consistency groups](/docs/vpc?topic=vpc-snapshots-vpc-create-consistency-groups).

## Creating a snapshot in the console
{: #snapshots-vpc-create-ui}
{: ui}

In the console, you can create a snapshot of a {{site.data.keyword.block_storage_is_short}} volume that is attached to a running virtual server instance.

1. You can access the Block Storage snapshot for VPC provisioning screen in the [{{site.data.keyword.cloud}} console](/login){: external} in multiple ways.

   - From the **[Block Storage snapshots for VPC](/infrastructure/storage/snapshots)** list,
      1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage snapshots**.
      2. From the list of snapshots that is initially empty, click **Create**.

   - From the **[Block Storage volumes for VPC](/infrastructure/storage/storageVolumes)** list,
      1. Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**.
      2. From the list of volumes, locate a boot or data volume that is attached to an instance.
      3. Click the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Create snapshot**.

   - From the **Block Storage volume details** screen,
     1. Go to the volume details page in one of these ways.

         - Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Compute > Virtual server instances**. Select the instance that contains the volume that you want to make a snapshot of. From the [instance details page](/infrastructure/compute/vs), scroll to the list of attached volumes and click the name of the volume.
         - Click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage volumes**. From the list of Block Storage volumes, select the volume that you want to make a snapshot of.

     2. On the volume details page, select **Create snapshot** from the **Actions** menu.

2. Enter the required information to define your snapshot and select the {{site.data.keyword.block_storage_is_short}} volume that you want to take a snapshot of.

   | Field | Value |
   |-------|-------|
   | Location | Specify the geography and region for this snapshot. |
   | Snapshot type | Select **Single volume**. |
   | Name  | Provide a unique name for the snapshot. The UI verifies the name for proper conventions and identifies duplicate names. For suggestions about how to name your snapshots, see [Naming snapshots](/docs/vpc?topic=vpc-snapshots-vpc-manage#snapshots-vpc-naming). |
   | Resource group | Select a [Resource group](/docs/vpc?topic=vpc-iam-getting-started&interface=ui#iam-resource-groups) for the snapshot, or use the default. You can't change the resource group after the snapshot is created. |
   | Tags | Specify any user tags that you want to identify this resource. |
   | Access management tags | Specify any [access management tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags) for this resource. |
   | Volume | Select a volume from the list. The boot or data volume must be attached to a running virtual server instance. |
   | Encryption | Encryption information for the volume that you selected, either [provider-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-provider-managed-encryption) or [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about&interface=ui#vpc-customer-managed-encryption). The snapshot inherits the encryption of the source volume. You can't change the encryption type. |
   | Optional configurations | Cross-region snapshot copy. Select Copy Snapshot to a different region. Click **Create**. \n  This feature is not supported for snapshots with source volumes larger than 10 TB.|
   {: caption="Selections for creating a snapshot" caption-side="bottom"}

3. Click **Create**. You're returned to the screen that you started from. Messages are displayed while the snapshot is being created and when it's ready, the snapshot is displayed in the list of snapshots. For more information, see [View snapshot details in the console](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=ui#snapshots-vpc-view-snapshot-ui).
   
If you're not ready to order yet or just looking for pricing information, you can add the information that you see in the side panel to an Estimate. For more information about how this feature works, see [Estimating your costs](/docs/account?topic=account-cost).    
{: tip}

## Enabling fast restore snapshot clones in the console
{: #frsnapshots-vpc-create-ui}
{: ui}

1. To enable the [fast restore feature](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-fr), click the new snapshot to [view its details](/docs/vpc?topic=vpc-snapshots-vpc-view#snapshots-vpc-view-snapshot-ui).
2. Scroll to the **Fast restore** card and click **Edit**.
3. In the side panel, select the zones where you want to enable fast restore clones.
4. Click **Save**.

The fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots.
{: note}

## Create a cross-regional copy from the Snapshots for VPC list
{: #crsnapshots-vpc-create-ui}
{: ui}

In the previous section, you saw how to create a cross-regional snapshot copy when you take a new snapshot in the console. You can also create cross-regional copies of existing snapshots.

1. In the console, click the **Navigation menu** icon ![menu icon](../icons/icon_hamburger.svg) **> Infrastructure** ![VPC icon](../icons/vpc.svg) **> Storage > Block Storage snapshots**.
1. In the list of snapshots, locate that snapshot that you want to copy. Make sure that the snapshot is in Stable status.
1. Click the Actions menu ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Copy snapshot**.
1. Select the region where you want to create the copy.

   In the current release, only accounts with special access can create a cross-regional copy of a second-generation snapshot if the source volume exceeds 10 TB.
   {: restriction}

1. Click **Create**.

## Creating a snapshot from the CLI
{: #snapshots-vpc-create-cli}
{: cli}

### Before you begin
{: #snapshots-vpc-create-procedure-cli}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to {{site.data.keyword.cloud}}.

   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

Before you start, gather the following information:

- A unique name for the snapshot.
- The ID of the source volume.
- The resource group ID. You can't change the resource group after the snapshot is created.
- Any tags that you want to attach to the snapshot.

Use the following CLI commands to collect the information that you need.

-`ibmcloud is volumes` - The command lists all available volumes in the region that you selected. Locate the volume in the list, verify the status (`available`), the attachment type (`boot` or `data`), and the resource group.
-`ibmcloud is volume VOLUME_ID` - Use this command with the volume ID from the output of the previous command to review the details of the volume. If the output shows that the volume is available, attached to an instance and not busy, you can create a snapshot.

### Creating a snapshot from the CLI
{: #snapshot-create-cli}
{: cli}

To create a snapshot, run the `ibmcloud is snapshot-create` command.

```sh
ibmcloud is snapshot-create --volume VOLUME [--name NAME] [--clone-zones CLONE_ZONES] [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] [--tags  TAG_NAME1,TAG_NAME2,...] [--output JSON] [-q, --quiet]
```
{: pre}

For more information about available command options, see [`ibmcloud is snapshot-create`](/docs/cli?topic=cli-vpc-reference#snapshot-create).

The following example creates a snapshot with the name `cli-snapshot-test` of the data volume `block-test1` in the `eu-de-2` region. The snapshot is tagged with `env:test` and `env:prod`, and it has a fast restore snapshot clone in the `eu-de-1` zone.

```sh
ibmcloud is snapshot-create --volume r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa --name cli-snapshot-test --tags env:test,env:prod --clone-zones eu-de-1
```
{: pre}

```sh
Creating snapshot cli-snapshot-test under account Test Account as user test.user@ibm.com...

ID                     r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
Name                   cli-snapshot-test
CRN                    crn:v1:bluemix:public:is:eu-de:a/a1234567::snapshot:r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
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
ibmcloud is snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
```
{: pre}

```sh
Getting snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...

ID                     r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
Name                   cli-snapshot-test
CRN                    crn:v1:bluemix:public:is:eu-de:a/a1234567::snapshot:r138-4463eb2c-4913-43b1-b9bf-62a94f74c146
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

## Creating a fast restore snapshot clone from the CLI
{: #snapshots-vpc-create-frclone-cli}
{: cli}

You can create a fast restore clone by using the `ibmcloud is snapshot-clc` command with the snapshot ID and the target zone specification. For more information about available command options, see [`ibmcloud is snapshot-clone-create`](/docs/cli?topic=cli-vpc-reference#snapshot-clone-create).

The following example creates a fast restore snapshot clone of the snapshot `r138-4463eb2c-4913-43b1-b9bf-62a94f74c146` in the `eu-de-3` zone.

```sh
ibmcloud is snapshot-clc r138-4463eb2c-4913-43b1-b9bf-62a94f74c146  --zone eu-de-3
```
{: pre}

```sh
Creating zonal clone of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...

Zone        eu-de-3
Available   false
Created     2023-02-17T20:29:21+00:00
Href        https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-3
```
{: screen}

The snapshot clone appears as unavailable while the snapshot clone is created. Issue the `ibmcloud is snapshot-cl` command with the snapshot ID and the clone target zone to see the new snapshot clone as available.

```sh
ibmcloud is snapshot-cl r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 eu-de-3
```
{: pre}

```sh
Creating zonal clone of snapshot r138-4463eb2c-4913-43b1-b9bf-62a94f74c146 under account Test Account as user test.user@ibm.com...

Zone        eu-de-3
Available   false
Created     2023-02-17T20:29:21+00:00
Href        https://eu-de.iaas.cloud.ibm.com/v1/regions/eu-de/zones/eu-de-3
```
{: screen}

The fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots.
{: note}

## Creating a cross-regional copy of a snapshot from the CLI
{: #snapshots-vpc-create-crcopy}
{: cli}

To create a copy of a snapshot in another region, run the `ibmcloud is snapshot-create` command with the `--source-snapshot-crn` option and the source snapshot CRN. 

If the source snapshot is not encrypted with a customer key, the encryption of the copy remains provider-managed. If the source snapshot is protected by a customer-managed key, you need to specify the customer-managed key that you want to use to encrypt the new copy. The source snapshot must be in Stable status for the copy to be created successfully.
{: important}

The following example creates a snapshot in the target region (`us-south`) by using the CRN of a snapshot from the source region (`us-east`).

```sh
ibmcloud is snapshot-create --name my-cli-snapshot-crc --source-snapshot-crn crn:v1:bluemix:public:is:us-east:a/a1234567::snapshot:r014-14aae86e-f03d-4978-a4da-ab02e69bb2f9
```
{: pre}

```sh
Creating snapshot my-cli-snapshot-crc under account Test Account as user test.user@ibm.com...
                          
ID                     r006-daefc524-2643-4444-a22d-7c38144cc529   
Name                   my-cli-snapshot-crc   
CRN                    crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-daefc524-2643-4444-a22d-7c38144cc529   
Status                 stable   
Clones                 Zone   Available   Created      
                          
Source volume          ID                                          Name                   Remote Region   CRN                                                                                                                       Resource type      
                       r014-26fce2ff-8177-47ce-8182-49d8aed33063   -remote-49d8aed33063   us-east         crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-26fce2ff-8177-47ce-8182-49d8aed33063   volume      
                          
Backup policy plan     -   
Snapshot Copies        -   
Bootable               true   
Encryption             provider_managed   
Encryption key         -   
Source Snapshot        ID                                          Name                   Remote Region   CRN                                                                                                                       Resource type      
                       r014-14aae86e-f03d-4978-a4da-ab02e69bb2f9   my-bootable-snapshot   us-east         crn:v1:bluemix:public:is:us-east:a/a1234567::snapshot:r014-14aae86e-f03d-4978-a4da-ab02e69bb2f9   snapshot      
                          
Minimum capacity(GB)   100   
Size(GB)               3   
Source Image           ID                                          Name                   Remote Region   CRN                                                                                                                    Resource type      
                       r014-da69503f-30d4-4f1d-b03f-1f4a7cd29214   -remote-1f4a7cd29214   us-east         crn:v1:bluemix:public:is:us-east:a/811f8abfbd32425597dc7ba40da98fa6::image:r014-da69503f-30d4-4f1d-b03f-1f4a7cd29214   image      
                          
Operating system       Name                    Vendor   Version   Family          Architecture   Display name      
                       centos-stream-9-amd64   CentOS   9         CentOS Stream   amd64          CentOS Stream 9 - Minimal Install (amd64)      
                          
Resource group         ID                                 Name      
                       6edefe513d934fdd872e78ee6a8e73ef   defaults      
                          
Created                2025-01-21T21:27:30+00:00   
Captured at            2025-01-21T21:17:14+00:00   
Tags                   -   
Service Tags           -   
```
{: codeblock}

For more information about available command options, see [`ibmcloud is snapshot-create`](/docs/cli?topic=cli-vpc-reference#snapshot-create).

## Creating a snapshot with the API
{: #snapshots-vpc-create-api}
{: api}

You can create a snapshot by using the API.

### Prerequisites for creating a snapshot with the API
{: #snapshots-vpc-create-procedure-api}

You can create a snapshot by calling the [VPC API](/apidocs/vpc). Before you start, gather the following information:

- A unique name for the snapshot.
- The ID of the source volume.
- The resource group ID. You can't change the resource group after the snapshot is created.
- Any tags that you want to attach to the snapshot.

### Creating a snapshot with the API
{: #snapshots-vpc-create-snapshot-api}

To create a snapshot of a boot or data volume, make a `POST /snapshots`. The following example creates a snapshot of a boot volume by using the volume ID, and specifies user tags that can be associated with a [backup policy](/docs/vpc?topic=vpc-backup-service-about).

```sh
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2025-02-18&generation=2" \
-H "Authorization: $iam_token" \
-d '{
      "name": "my-bootable-snapshot",
      "source_volume": {"id": "r014-26fce2ff-8177-47ce-8182-49d8aed33063"},
      "resource_group": {"id": "6edefe513d934fdd872e78ee6a8e73ef"},
      "user_tags": ["env:test","env:prod"]
    }'
```
{: codeblock}
{: curl}

A successful response looks like the following example. The snapshot lifecycle state is `pending` while the snapshot is created. When it is successfully created, the status changes to `stable`.

```json
{
    "bootable": true,
    "captured_at": "2025-01-21T21:17:14.000Z",
    "clones": [],
    "copies": [],
    "created_at": "2025-01-21T21:17:13.000Z",
    "crn": "crn:v1:bluemix:public:is:us-east:a/a1234567::snapshot:r014-14aae86e-f03d-4978-a4da-ab02e69bb2f9",
    "deletable": true,
    "encryption": "provider_managed",
    "href": "https://us-east.iaas.cloud.ibm.com/v1/snapshots/r014-14aae86e-f03d-4978-a4da-ab02e69bb2f9",
    "id": "r014-14aae86e-f03d-4978-a4da-ab02e69bb2f9",
    "lifecycle_state": "stable",
    "minimum_capacity": 100,
    "name": "my-bootable-snapshot",
    "operating_system": {
        "allow_user_image_creation": true,
        "architecture": "amd64",
        "dedicated_host_only": false,
        "display_name": "CentOS Stream 9 - Minimal Install (amd64)",
        "family": "CentOS Stream",
        "href": "https://us-east.iaas.cloud.ibm.com/v1/operating_systems/centos-stream-9-amd64",
        "name": "centos-stream-9-amd64",
        "user_data_format": "cloud_init",
        "vendor": "CentOS",
        "version": "9"
    },
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/6edefe513d934fdd872e78ee6a8e73ef",
        "id": "6edefe513d934fdd872e78ee6a8e73ef",
        "name": "defaults"
    },
    "resource_type": "snapshot",
    "service_tags": [],
    "size": 2,
    "source_image": {
        "crn": "crn:v1:bluemix:public:is:us-east:a/a1234567:image:r014-da69503f-30d4-4f1d-b03f-1f4a7cd29214",
        "href": "https://us-east.iaas.cloud.ibm.com/v1/images/r014-da69503f-30d4-4f1d-b03f-1f4a7cd29214",
        "id": "r014-da69503f-30d4-4f1d-b03f-1f4a7cd29214",
        "name": "ibm-centos-stream-9-amd64-9",
        "resource_type": "image"
    },
    "source_volume": {
        "crn": "crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-26fce2ff-8177-47ce-8182-49d8aed33063",
        "href": "https://us-east.iaas.cloud.ibm.com/v1/volumes/r014-26fce2ff-8177-47ce-8182-49d8aed33063",
        "id": "r014-26fce2ff-8177-47ce-8182-49d8aed33063",
        "name": "my-test-vm-boot-1",
        "resource_type": "volume"
    },
    "user_tags": ["env:test","env:prod"]
}
```
{: codeblock}

## Creating a snapshot and a fast restore snapshot clone with the API
{: #snapshots-vpc-create-snapshot-clone-api}
{: api}
 
When you create a snapshot, you can also create a fast restore snapshot clone in another zone. By cloning a snapshot and keeping it in another zone, you can later use the fast restore feature to quickly provision a new volume with data from a snapshot. For more information, see [Snapshots fast restore feature](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=api#snapshots_vpc_fast_restore).

Make a `POST/snapshots` request to create a snapshot of a boot or data volume and specify the `clones` property. Indicate a different zone or zones in your region from the zone in which you're creating the snapshot. In the following example, a clone is created in us-south-2, specified by name.

```sh
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2022-12-18&generation=2" \
-H "Authorization: $iam_token" \
-d '{
    "clones": [{"zone": {"name": "us-south-2"}}],
    "name": "my-snapshot-1",
    "source_volume": {"id": "1a6b7274-678d-4dfb-8981-c71dd9d4daa5"},
    "resource_group": {"id": "a342dbfb-3ea7-48d1-96e8-2825ec5feab4"},
    "user_tags": ["env:test","env:prod"]
    }
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
				"href": "https:ibm.com/v1/regions/us-south/zones/us-south-2",
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
}
```
{: codeblock}

The fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots.
{: note}

## Creating a cross-regional copy of a snapshot with the API
{: #snapshots-vpc-create-snapshot-copy-api}
{: api}

When you create a snapshot, you can also copy the snapshot to another region. By creating a snapshot copy in another region and create new instances with the help of that snapshot, you can expand your VPC to a different region. This feature can be used in disaster recovery scenarios, as well. For more information, see [Cross-regional snapshot copies](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=api#snapshots_vpc_crossregion_copy).

Make a `POST /snapshots` request to create a snapshot copy in the target region, specify the name and CRN of the source snapshot. In the following example, the target region is us-east and the source region is us-south. The `name` and `source_snapshot` subproperties are required. The `resource_group` is optional. The `encryption_key` subproperty is required when the source snapshot is encrypted with a customer-managed key.

If the source snapshot is not encrypted with a customer key, the encryption of the copy remains provider-managed. If the source snapshot is protected by a customer-managed key, you must specify the customer-managed key that you want to use to encrypt the new copy. The source snapshot must be in Stable status for the copy to be created successfully.
{: important}

```sh
curl -X POST \
"$vpc_api_endpoint/v1/snapshots?version=2023-05-10&generation=2" \
-H "Authorization: $iam_token" \
-d '{
     "name": "my-api-snapshot-crc",    // required
     "source_snapshot": {      // required
      	"crn": "crn:[crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-daefc524-2643-4444-a22d-7c38144cc529]"
     },
     "resource_group": {       // optional
       "id": "6edefe513d934fdd872e78ee6a8e73ef"
     },
     "encryption_key"; "crn:[...]"     // required when source has customer-managed encryption
}
```
{: codeblock}

A successful response indicates that the snapshot copy was created in the target region.

```json
{
    "bootable": true,
    "captured_at": "2025-01-21T21:17:14.000Z",
    "clones": [],
    "copies": [],
    "created_at": "2025-01-21T21:27:30.000Z",
    "crn": "crn:v1:bluemix:public:is:us-south:a/a1234567::snapshot:r006-daefc524-2643-4444-a22d-7c38144cc529",
    "deletable": true,
    "encryption": "provider_managed",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/snapshots/r006-daefc524-2643-4444-a22d-7c38144cc529",
    "id": "r006-daefc524-2643-4444-a22d-7c38144cc529",
    "lifecycle_state": "stable",
    "minimum_capacity": 100,
    "name": "my-api-snapshot-crc",
    "operating_system": {
        "allow_user_image_creation": true,
        "architecture": "amd64",
        "dedicated_host_only": false,
        "display_name": "CentOS Stream 9 - Minimal Install (amd64)",
        "family": "CentOS Stream",
        "href": "https://us-east.iaas.cloud.ibm.com/v1/operating_systems/centos-stream-9-amd64",
        "name": "centos-stream-9-amd64",
        "user_data_format": "cloud_init",
        "vendor": "CentOS",
        "version": "9"
    },
    "resource_group": {
        "href": "https://resource-controller.cloud.ibm.com/v2/resource_groups/6edefe513d934fdd872e78ee6a8e73ef",
        "id": "6edefe513d934fdd872e78ee6a8e73ef",
        "name": "defaults"
    },
    "resource_type": "snapshot",
    "service_tags": [],
    "size": 3,
    "source_image": {
        "crn": "crn:v1:bluemix:public:is:us-east:a/811f8abfbd32425597dc7ba40da98fa6::image:r014-da69503f-30d4-4f1d-b03f-1f4a7cd29214",
        "href": "https://us-east.iaas.cloud.ibm.com/v1/images/r014-da69503f-30d4-4f1d-b03f-1f4a7cd29214",
        "id": "r014-da69503f-30d4-4f1d-b03f-1f4a7cd29214",
        "name": "-remote-1f4a7cd29214",
        "remote": {
            "region": {
                "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-east",
                "name": "us-east"
            }
        },
        "resource_type": "image"
    },
    "source_snapshot": {
        "crn": "crn:v1:bluemix:public:is:us-east:a/a1234567::snapshot:r014-14aae86e-f03d-4978-a4da-ab02e69bb2f9",
        "href": "https://us-east.iaas.cloud.ibm.com/v1/snapshots/r014-14aae86e-f03d-4978-a4da-ab02e69bb2f9",
        "id": "r014-14aae86e-f03d-4978-a4da-ab02e69bb2f9",
        "name": "my-bootable-snapshot",
        "remote": {
            "region": {
                "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-east",
                "name": "us-east"
            }
        },
        "resource_type": "snapshot"
    },
    "source_volume": {
        "crn": "crn:v1:bluemix:public:is:us-east-1:a/a1234567::volume:r014-26fce2ff-8177-47ce-8182-49d8aed33063",
        "href": "https://us-east.iaas.cloud.ibm.com/v1/volumes/r014-26fce2ff-8177-47ce-8182-49d8aed33063",
        "id": "r014-26fce2ff-8177-47ce-8182-49d8aed33063",
        "name": "-remote-49d8aed33063",
        "remote": {
            "region": {
                "href": "https://us-south.iaas.cloud.ibm.com/v1/regions/us-east",
                "name": "us-east"
            }
        },
        "resource_type": "volume"
    },
    "user_tags": []
}
```
{: screen}

## Creating a snapshot with Terraform
{: #snapshots-vpc-create-terraform}
{: terraform}

You can create a snapshot by using Terraform.

Before you start, gather the following information:

- A unique name for the snapshot.
- The ID of the source volume.
- The resource group ID. You can't change the resource group after the snapshot is created.
- Any tags that you want to attach to the snapshot.

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

VPC infrastructure services use a specific regional endpoint, which targets to `us-south` by default. If your VPC is created in another region, make sure to target the appropriate region in the provider block in the `provider.tf` file.

See the following example of targeting a region other than the default `us-south`.

```terraform
provider "ibm" {
   region = "eu-de"
}
```
{: screen}

To create a snapshot, use the `ibm_is_snapshot` resource. The following example creates a snapshot of the volume with the ID `r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa`. The snapshot is named `snapshot-test`.

```terraform
resource "ibm_is_snapshot" "example" {
   name          = "snapshot-test"
   source_volume = "r010-df8ffd90-f2e5-470b-83d7-76e64995a1aa"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

## Creating a snapshot and a fast restore snapshot clone with Terraform
{: #snapshots-vpc-create-snapshot-clone-terraform}
{: terraform}

To create a snapshot with fast restore clones, use the `ibm_is_snapshot` resource. The following example creates a snapshot of the volume with the ID `r010-bdb8fc70-8afb-4622-826a-d65a9fc477a4`. The snapshot is named `example-snapshot`. In addition, two fast restore clones of the snapshot are created in `eu-de-1` and `eu-de-3` regions.

```terraform
resource "ibm_is_snapshot" "example_clones" {
   name            = "example-snapshot-clone"
   source_volume   = "r010-bdb8fc70-8afb-4622-826a-d65a9fc477a4"
   clones          = ["eu-de-1", "eu-de-3"]
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

The fast restore feature is billed at an extra hourly rate for each zone that it is enabled in regardless of the size of the snapshot. Maintaining fast restore clones is considerably more costly than keeping regular snapshots.
{: note}

## Create a cross-regional copy of a snapshot with Terraform
{: #snapshots-vpc-create-snapshot-copy-terraform}
{: terraform}

To create a copy of the snapshot in a remote region, use the `ibm_is_snapshot` resource. The following example creates a copy in the target region by using the CRN of the source snapshot. The copy is going to be encrypted by the encryption key that is specified by its CRN.

```terraform
resource "ibm_is_snapshot" "snapshot" {
   name 		   = "my-cross-regional-snapshot"
   source_snapshot = "r138-4463eb2c-4913-43b1-b9bf-62a94f74c146"
   encryption_key  = "crn:bluemix:public:kms:us-south:a/df0564dd126042ebb03e0224728ce939:4957299d-0ba0-487f-a1a0-c724a729b8b4:key:0cb88b98-9261-4d07-8329-8f594b6641b5"
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_is_snapshot](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/is_snapshot){: external}.

The source snapshot must be in Stable status for the copy to be created successfully.
{: important}

## Next steps
{: #bs_snapshots_create_next_steps}

After you create a snapshot, you can view more details about it or restore a volume from the snapshot.

- [View](/docs/vpc?topic=vpc-snapshots-vpc-view) all the snapshots that you created, or details about a single snapshot.
- [Restore a volume](/docs/vpc?topic=vpc-snapshots-vpc-restore) from a snapshot.
